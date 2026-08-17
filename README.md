# Anonymous review repository

This is the anonymized repository for reviewing the FunctionalMLDS artifact.

## Repository guide

| Path | Contents |
| --- | --- |
| `research/iui2027/artifact/` | Verification entry point and artifact documentation |
| `research/iui2027/evaluation/` | Benchmark runner, results, tables, and environment hashes |
| `output/case_studies/` | The three synthetic example cases and their generated artifacts |
| `tools/case_study_pipeline/` | FunctionalMLDS generation and validation pipeline |
| `tools/tests/` | Artifact, benchmark, regeneration, and materialization tests |
| `InteractivAgents/openai_unity_expert_npcs_pycharm/InteractiveAgents/` | Python runtime components |
| `InteractivAgents/InteractiveAgents2/` | Minimal Unity source snapshot |

## Start here

Run the complete verification from the repository root:

```text
python -m pip install -r research/iui2027/artifact/requirements.txt
python research/iui2027/artifact/verify.py
```

For details, see
[`research/iui2027/artifact/README.md`](research/iui2027/artifact/README.md).
