# functionalmlds_assembly_v1

This prompt documents the optional language-model-assisted FunctionalMLDS
assembly variant. The current case-study pipeline constructs the normative
FunctionalMLDS instance deterministically from validated artifacts so that
cardinalities, exclusive-or rules, and runtime separation remain reproducible.

A language model may improve descriptive text in later variants. It must not:

- map `ScenarioStep` directly to `RuntimeAction`;
- place endpoints, tools, or topics in `Capability` or `ScenarioStep`;
- create `Satisfy` relations that reference both a requirement and a use case;
- conflate `Actor` and `Agent`;
- create a `ValidationCase` without expected `StateAssertion` instances.
