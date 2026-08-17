# Anonymous review artifact

This repository is a compact, history-independent snapshot of the implementation
and structural evidence used for anonymous peer review. It contains only the
current reproducibility surface; it is not a mirror of the development repository.

## Included

- the FunctionalMLDS generation, compatibility, validation, and case-study tools;
- the Python runtime components needed by the executable structural benchmark;
- a minimal Unity source snapshot for model loading, scene binding, dispatch,
  evidence recording, and static smoke-contract inspection;
- three authored, synthetic fixtures: `fitness_career_fair`,
  `classroom_dinosaur`, and `steinpilz_brand_room`;
- deterministic, API-free structural benchmark code and evidence.

The fixtures contain modeled rooms, synthetic agent roles, and generated knowledge
snippets. They contain no participant records or human-subject outcome data.

## Deliberately excluded

The review bundle does **not** contain the paper or its sources, questionnaires,
survey applications or responses, reviewer-service material, author metadata,
development history, archives, raw runtime logs, model/API responses, local
configuration, editor caches, generated 3D models, character assets, screenshots,
or third-party media.

## Verify

Use Python 3.11 or newer from the repository root:

```text
python -m pip install -r research/iui2027/artifact/requirements.txt
python research/iui2027/artifact/verify.py
```

The default verification is deterministic and network-free. It checks the frozen
inputs and hashes, regenerates V2 in memory, validates the committed benchmark,
runs targeted backend/materialization tests, checks the pinned Unity source
contract, and scans the complete release tree for secrets, identity markers, and
local user paths. The generated verification summary is excluded from its own
pre-write scan. Verification does not contact an LLM or start Unity.

To repeat the isolated frozen-input regeneration without publishing files:

```text
python research/iui2027/artifact/regenerate_frozen.py --check
```

This preflight fails if the slim committed artifact surface differs semantically
from fresh regeneration; only the assembly report's volatile `generated_at` field
is normalized.

## Layout

| Path | Content |
| --- | --- |
| `research/iui2027/artifact/` | verification and regeneration entry points |
| `research/iui2027/evaluation/` | structural benchmark and frozen evidence |
| `output/case_studies/` | portable artifacts for the three synthetic fixtures |
| `tools/` | FunctionalMLDS pipeline, schemas, and targeted tests |
| `InteractivAgents/.../InteractiveAgents/backend/` | Python runtime components |
| `InteractivAgents/InteractiveAgents2/` | minimal current Unity source snapshot |

## Distribution status

This bundle is prepared for confidential peer-review inspection. No open-source or
open-data license is granted. See
[`research/iui2027/artifact/LICENSE_STATUS.md`](research/iui2027/artifact/LICENSE_STATUS.md).
