# Knowledge management in Interactive Agents

This document explains how the Python runtime stores, retrieves, and transfers
knowledge between agents. It describes the implementation in `backend/` and the
minimal Unity integration included under `InteractivAgents/InteractiveAgents2/`.

## Overview

The runtime distinguishes three kinds of knowledge:

1. **Persistent knowledge base**: text files below `kb/<tag>/...` or a project
   directory at `projects/<project_id>/kb/<tag>/...`.
2. **Agent-specific access**: each agent declares `knowledge_tags`. These tags
   select the folders from which that agent may retrieve snippets.
3. **Session memory**: chat context is either shared by all agents in a session
   or isolated per agent.

Two memory modes are supported:

- `shared_history`: all agents in a session receive the same visible chat
  history.
- `agent_private_history`: each agent receives its own history. Another agent
  sees transferred context only through a handoff brief.

Agents may read different parts of the same file-based knowledge collection,
but the runtime does not perform hidden knowledge synchronization between them.

## Relevant implementation files

- `backend/kb.py` loads local knowledge files, splits them into chunks, and
  performs keyword retrieval.
- `backend/state.py` manages sessions, agent profiles, chat history, retrieval,
  and handoff orchestration.
- `backend/projects.py` manages project directories, room plans, agents, and
  project-specific knowledge entries.
- `backend/server.py` exposes `/setup`, `/chat`, and the project knowledge API.
- `InteractivAgents/InteractiveAgents2/Assets/Scripting/QuickAgentManager.cs`
  is the corresponding Unity-side chat and handoff client.

## Knowledge-base layout

The global knowledge base uses a simple directory structure:

```text
kb/
  common/
    example.md
  pricing/
    pricing.md
  tech/
    integration.md
```

A project-specific knowledge base uses the same structure below its project
directory:

```text
projects/
  <project_id>/
    kb/
      <tag>/
        <entry>.txt
```

The direct child folders of `kb` are the knowledge tags. An agent refers to
these folders through `knowledge_tags`. The name `common` is a convention, not
a reserved tag.

The loader currently accepts `.txt` and `.md` files. Other file types are
ignored.

## Loading and refreshing knowledge

At startup, the backend loads a global `KnowledgeBase` from the `kb_root`
configured in `config.json`. The default is the local `kb` directory.

When `/setup` receives a `project_id`, the backend instead loads:

```text
projects/<project_id>/kb/
```

Project knowledge bases are cached in `SessionStore.kb_cache`. Creating,
updating, or deleting a knowledge entry through the project API calls
`refresh_project_kb(project_id)`.

An existing session retains the knowledge-base instance that was active during
its `/setup` call. Run `/setup` again after editing project knowledge when an
active client must use the refreshed content.

## Chunking and retrieval

`KnowledgeBase` reads all supported files and groups their text into chunks.
`kb_chunk_chars` controls the target chunk size.

The chunker:

- splits text at blank lines;
- combines paragraphs until the target size is reached;
- falls back to fixed-size character blocks when no paragraphs are available;
- tokenizes letters, numbers, and underscores for retrieval.

For every user request, the backend performs deterministic keyword retrieval:

1. Tokenize the question.
2. Select chunks whose tags are listed in the active agent's
   `knowledge_tags`.
3. If the agent has no tags, search all tags in the session knowledge base.
4. Rank chunks by token overlap with a small coverage bonus.
5. Return at most `kb_max_snippets` snippets.

There are no embeddings, vector database, or external retrieval service. The
search is intentionally local, small, and inspectable.

## Memory modes

The default mode is configured in `config.json`:

```json
{
  "memory_mode": "shared_history"
}
```

It can be overridden for an individual `/setup` request:

```json
{
  "memory_mode": "agent_private_history"
}
```

Both `/setup` and `/chat` return the active value as `memory_mode`.

### Shared history

In `shared_history`, all agents share:

- the visible sequence of user and assistant messages;
- the session's loaded knowledge-base instance;
- the same room plan and placement context.

They do not automatically share another agent's internal prompt, retrieved
snippets, hidden model reasoning, or tool internals. An agent may see a previous
agent's visible answer in the common history, but not the private retrieval
context that produced it.

### Private agent history

In `agent_private_history`, the session maintains one history per agent. A
directly addressed agent receives only:

- its own prior messages;
- room and placement context;
- snippets retrieved from its allowed tags;
- compact handoff context explicitly transferred to it.

A handoff does not copy the source agent's full private history. The source
agent supplies `handoff_brief`, and the backend stores that brief in the target
agent's history.

## Agent knowledge configuration

Knowledge-related fields in `agents.json` include:

- `id`: stable technical identifier;
- `display_name`: user-facing name;
- `persona`: role and behavior instruction;
- `expertise`: competence description used for prompting and handoff selection;
- `knowledge_tags`: folders available for retrieval.

Several agents may share a tag. For example, assigning `common` to every agent
makes matching files under `kb/common/` available to all of them, while a
`pricing` tag can remain limited to a pricing specialist.

## Setup flow

`POST /setup` creates a chat session from one of the supported input forms,
including explicit room-plan and agent paths or a materialized `project_id`.
For a project, it loads:

```text
projects/<project_id>/room_plan.json
projects/<project_id>/agents.json
projects/<project_id>/kb/
```

The backend then creates agent specifications, computes spawn placements, and
returns the session identifier plus the Unity-facing agent and placement data.
Persona, expertise, and knowledge tags remain backend context.

## Chat flow

`POST /chat` receives a session identifier, active agent identifier, and user
text. The backend:

1. resolves the session and active agent;
2. retrieves knowledge allowed by the agent's tags;
3. builds the selected shared or private history;
4. calls the configured language model client;
5. validates any requested handoff;
6. optionally invokes one target agent in the same request;
7. appends visible events and trims history to `max_history_turns`.

The response contains the active agent, visible events, memory mode, and an
optional handoff object.

## Handoff behavior

The first agent may return `handoff_to`, `handoff_reason`, and `handoff_brief`.
The structured-output schema restricts `handoff_to` to another known agent or
`null`, and the backend validates the identifier again.

For a valid handoff:

1. The source agent emits a short transfer message and optional reason.
2. The backend builds target context from the user request and handoff brief.
3. The target agent answers in the same `/chat` request.
4. Further handoffs are disabled for that target call, preventing chains.
5. The response's `active_agent_id` becomes the target agent.

`max_handoffs` limits automatic transfers. The current implementation invokes
at most one target agent per request.

## Editing project knowledge

The server exposes project knowledge operations:

- `GET /projects/{id}/knowledge` lists entries.
- `POST /projects/{id}/knowledge/read` reads one entry.
- `POST /projects/{id}/knowledge` creates, updates, or deletes an entry.

Tags and names are slugified before writing. A successful update refreshes the
cached project knowledge base.

## Current limitations

- Retrieval is keyword based and does not perform semantic vector search.
- Root knowledge changed after startup is not reloaded automatically.
- Handoff transfer is explicit and compact; there is no hidden multi-agent
  memory synchronization.
- Private histories are session-local and are not a persistent long-term memory.

## Recommended use

- Put shared facts in a tag such as `common` and assign it to every relevant
  agent.
- Keep specialist material in separate tags such as `pricing`, `tech`, or
  `legal`.
- Write `expertise` descriptions clearly enough for reliable handoff selection.
- Run `/setup` again after project knowledge changes when an active client needs
  the new content immediately.

