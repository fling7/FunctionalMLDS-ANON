# Anonymous-review hygiene

This release discloses implementation and structural evidence without disclosing
author identity, workstation identity, credentials, or development history.

## Scan scope

`verify.py` scans every versioned text/source file in the release tree, including
dotfile ignore rules, HTTP examples, artifact and benchmark files, synthetic case
artifacts, backend and Unity sources, package metadata, tests, and documentation.
The generated verification summary is excluded from its own pre-write scan. The
scan fails on email addresses, Windows or POSIX user-home paths, private-key
headers, common high-confidence API-token formats, and quoted secret assignments.
Findings contain repository-relative paths only.

## Excluded material

The release omits:

- the development repository's Git metadata, commits, branches, tags, and remotes;
- the paper, acknowledgements, author lists, affiliations, and project URLs;
- questionnaires, survey code/responses/exports, participant records, and raffle data;
- reviewer-service tokens, reviews, receipts, and contact addresses;
- environment files, IDE state, Unity caches/licenses, package caches, and local logs;
- screenshots, PDFs, archives, generated 3D assets, characters, and third-party media.

The publication repository starts from a new anonymous commit and has no ancestry in
the development repository. Synthetic fixture identifiers, scene objects, and agent
roles are research content rather than author identity.

The automated scan is a high-confidence hygiene check, not a legal determination.
Reviewers should treat the repository as confidential review material; see
[LICENSE_STATUS.md](LICENSE_STATUS.md).
