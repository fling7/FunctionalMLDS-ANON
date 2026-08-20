# scene_semantics_v1

Analyze a normalized MLDS or RoomPlan scene for a scientific case study. The
analysis must remain spatially grounded: object identifiers may only come from
the supplied list.

Objectives:

- Identify the room's domain and purpose.
- Derive typical visitor goals.
- Form compact semantic zones from object groups and important individual
  objects.
- Mark objects relevant to interaction, explanation, or navigation.
- Propose agent roles that can later be implemented in Interactive Agents.

Rules:

- Do not invent object identifiers.
- Do not create runtime endpoints, requirements, or FunctionalMLDS elements;
  later stages handle them.
- Make every zone semantically explainable and reference at least one existing
  object.
- Derive every role candidate from the room function, visitor goals, and zones.
- Return only the requested JSON schema.
