# Anonymous review artifact

This directory is the entry point for reproducing the release's structural system
evidence. Verification is deterministic, fail-closed, API-free, and network-free.
The paper, reviewer-service material, human-subject data, and raw runtime logs are
intentionally not part of this repository.

## Canonical fixtures

`steinpilz_brand_room` is the canonical walkthrough. Together with
`classroom_dinosaur` and `fitness_career_fair`, it forms a three-fixture structural
benchmark. These are authored synthetic examples, not a user study and not evidence
of population-level generalization.

## One-command verification

Run from the repository root with Python 3.11 or newer:

```text
python -m pip install -r research/iui2027/artifact/requirements.txt
python research/iui2027/artifact/verify.py
```

The verifier checks:

- all expected case inputs and their source-hash manifests;
- deterministic in-memory V2 regeneration and canonical validation;
- the committed structural benchmark and all recorded implementation/input hashes;
- temporary project materialization and backend runtime contracts;
- targeted artifact, benchmark, and frozen-regeneration tests;
- the pinned Unity version, packages, binding/evidence classes, and smoke sources;
- every versioned text/source file for emails, local user paths, private keys, and
  high-confidence embedded secrets, except the verifier's generated output summary.

It atomically writes `verification-summary.json` after the text scan. The summary
is therefore excluded from its own pre-write scan and contains only logical
identifiers, bounded diagnostics, and hashes; it never records a local absolute path,
reviewer address, token, API key, or subprocess log.

## Frozen regeneration

The release does not require an OpenAI configuration. The checked-in normalized
scene, scene-semantics, agent-role, handoff-matrix, and knowledge JSON files are
frozen inputs. The committed FunctionalMLDS JSON files are reproducible evidence.
The authored sparse handoff graph is reused verbatim so regeneration cannot silently
replace it with a different routing policy.

Run the non-mutating preflight:

```text
python research/iui2027/artifact/regenerate_frozen.py --check
```

The command copies only required inputs and backend code to a temporary directory,
blocks socket and URL connections, regenerates and validates all three cases, and
fails if the committed placements, sparse handoff matrix, knowledge files, or three
FunctionalMLDS files differ semantically from the fresh result. The volatile
`generated_at` field in each assembly report is ignored for this comparison. It then
discards the temporary directory. Stage manifests, validation reports, and generated
backend projects are intentionally not committed; the benchmark and contract tests
materialize fresh projects in temporary directories when they run.

## Interpretation boundary

A passing artifact establishes that the checked files agree, fresh regeneration and
validators execute, and the tested structural contracts hold for the three fixtures.
It does not measure generated-answer correctness, human usefulness, usability,
trust, end-to-end network latency, or population-level effects.

See [DATA_DICTIONARY.md](DATA_DICTIONARY.md), [ANONYMITY.md](ANONYMITY.md), and
[LICENSE_STATUS.md](LICENSE_STATUS.md).
