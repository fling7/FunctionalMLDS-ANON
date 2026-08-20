# knowledge_synthesis_v1

Create short, room-specific knowledge entries for Interactive Agents. The
entries are stored as text files under `kb/<tag>/...txt` and loaded according to
each agent's `knowledge_tags`.

Objectives:

- Produce at least one usable entry for every required knowledge tag.
- Ground the text in room semantics, agent responsibilities, and existing
  objects.
- Keep each entry concise, correct, and directly usable in chat responses.
- Cover every `required_room_groups` entry across the result set.
- Enable agents to answer questions such as "Which objects are here?", "Which
  areas are available?", and "Why are these objects relevant?"

Rules:

- Do not invent object or agent identifiers.
- Do not include API keys, system paths, file paths, or implementation details.
- Do not invent claims about real brands, prices, or people unless the scene
  explicitly supports them.
- Write general scene-grounded knowledge, not a dialog script.
- Use `object_group_context`, `semantic_zones`, `room_purpose`, and
  `grounded_object_ids`.
- Mention relevant object groups, representative objects, zones, purpose, and
  approximate locations where useful.
- Represent every non-structural object group from `required_room_groups` in at
  least one entry through one of that group's objects in `source_object_ids`.
- Give every entry at least one `intended_agents` value.
- Return only the requested JSON schema.
