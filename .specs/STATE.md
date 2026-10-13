# Project state

## Decisions

| ID | Decision | Rationale | Status | Date |
| --- | --- | --- | --- | --- |
| AD-001 | One is built as a React SPA/PWA (Vite + vite-plugin-pwa) with an ASP.NET Core Web API (REST) back end | The group's choice for PEI IV; §2.1.4 of the PEI manual allows frameworks beyond PHP/Java/Node ("entre outras") | active | 2026-10-02 |
| AD-002 | `.claude/scripts/verify.ps1` lists its steps explicitly; the requirements-document test suite is the first one | Detection finds nothing in this repository, and an empty verifier blocks every session | active | 2026-10-02 |
| AD-003 | Course documents identify requirements as `RF-NN`, `RNF-NN` and `UC-NN`, each RF citing the `BR-`/`F` ids of `specs/mvp-business-rules/requirements.md` it derives from | The professor reads RF/RNF; the spec stays the single source | active | 2026-10-02 |
| AD-004 | Diagrams of the requirements document are drawn in Mermaid (`.mmd`: classes, logical model) and draw.io (`.drawio`: use cases, conceptual model), replacing PlantUML (door 1) | User request; Mermaid has no use-case or Chen notation, so those two need draw.io's manual layout | active | 2026-10-02 |

## Handoff

**Feature**: documento-requisitos-abnt
**Where**: C1–C34 and C36 closed at `851b7c0`; C35 open
**In progress**: independent Verifier over `0c388a8..HEAD`
**Next step**: add the document test step to `$VerificationSteps` in `.claude/scripts/verify.ps1` (C35), then re-verify C35
**Blockers**: editing `.claude/scripts/verify.ps1` is denied by the user's permission settings
**Uncommitted**: earlier documentation edits outside this feature (`roadmap.md`, `requirements.md`, roteiro, refinamento) and the user's `.gitignore`, `.cursor/`, manual PDF - none go into this feature's commits
**Branch**: feat/documento-requisitos-abnt
