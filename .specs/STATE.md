# Project state

## Decisions

| ID | Decision | Rationale | Status | Date |
| --- | --- | --- | --- | --- |
| AD-001 | One is built as a React SPA/PWA (Vite + vite-plugin-pwa) with an ASP.NET Core Web API (REST) back end | The group's choice for PEI IV; §2.1.4 of the PEI manual allows frameworks beyond PHP/Java/Node ("entre outras") | active | 2026-10-02 |
| AD-002 | `.claude/scripts/verify.ps1` lists its steps explicitly; the requirements-document test suite is the first one | Detection finds nothing in this repository, and an empty verifier blocks every session | active | 2026-10-02 |
| AD-003 | Course documents identify requirements as `RF-NN`, `RNF-NN` and `UC-NN`, each RF citing the `BR-`/`F` ids of `specs/mvp-business-rules/requirements.md` it derives from | The professor reads RF/RNF; the spec stays the single source | active | 2026-10-02 |

## Handoff

**Feature**: documento-requisitos-abnt
**Where**: plan and checks written; build starting
**In progress**: none
**Next step**: build S1–S3, then dispatch the Verifier
**Blockers**: none
**Uncommitted**: earlier documentation edits outside this feature (`roadmap.md`, `requirements.md`, roteiro, refinamento) and the user's `.gitignore`, `.cursor/`, manual PDF - none go into this feature's commits
**Branch**: feat/documento-requisitos-abnt
