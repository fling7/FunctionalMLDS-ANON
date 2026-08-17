# Python runtime snapshot

This directory contains the Python backend components used by the anonymous
artifact's executable structural benchmark. It is a source snapshot, not a
preconfigured service deployment.

## Contents

- `backend/`: HTTP service, FunctionalMLDS adapters/runtime, placement, project
  materialization support, knowledge lookup, and trace handling;
- `main.py`: local service entry point;
- `config.example.json`: configuration template with no credentials;
- `examples/` and `kb/`: small synthetic examples and knowledge snippets;
- `docs/`: backend-specific documentation.

Generated backend projects are intentionally absent. The benchmark materializes
fresh projects in temporary directories from the three synthetic fixtures under
`output/case_studies/`.

## Artifact verification

Run the repository-level verifier from the release root:

```text
python research/iui2027/artifact/verify.py
```

That path is deterministic, network-free, and requires no model credentials. To
experiment with the HTTP service separately, copy `config.example.json` to the
ignored `config.json` and provide any required local settings without committing
credentials.

## Unity boundary

The matching minimal Unity source snapshot is located at
`InteractivAgents/InteractiveAgents2/`. This backend directory deliberately does
not duplicate Unity scripts, scenes, generated models, character assets, or editor
caches.
