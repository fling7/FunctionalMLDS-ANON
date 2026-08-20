# agent_roles_v1

Create concrete Interactive Agents roles from a validated semantic room
analysis. The output must be compatible with the backend and suitable for
direct conversion to `agents.json`, knowledge files, FunctionalMLDS agents, and
handoff rules.

Objectives:

- Consolidate the role candidates into two to eight clearly distinct agents.
- Bind every role semantically to zones or objects.
- Give every role a persona, expertise, knowledge tags, `voice`,
  `voice_gender`, `voice_style`, and `tts_model`.
- Define handoffs only between existing agents and justify them by domain.

Rules:

- Agent identifiers and knowledge tags must be slug compatible: lowercase
  letters, numbers, `_`, or `-`.
- Use only `zone_id` and `object_id` values present in the input.
- Set `tts_model` to `gpt-4o-mini-tts`, not `standard`.
- Set `voice_gender` to a simple English value such as `female` or `male`.
- Do not invent runtime endpoints, requirements, or FunctionalMLDS identifiers.
- Treat handoffs as domain transfers, not detailed conversation scripts.
- Return only the requested JSON schema.
