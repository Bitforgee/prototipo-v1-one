# LESSONS - auto-maintained by scripts/lessons.py

> Machine-owned. Do NOT hand-edit. Changes are overwritten on the next `lessons.py` write.
> Canonical state lives in `.specs/lessons.json`. Edit lessons only via the script.
> promote_threshold=2 distinct features · window_days=45 · quarantine_threshold=2

## Confirmed (load these at Plan/Checks)

Corroborated across multiple features. Safe to apply as guidance.

_none_

## Candidates (under observation - do NOT load as guidance yet)

Seen once or not yet corroborated. Tracked, not trusted.

### L-001 - Prove a failure halfway with a fault injected after the preconditions pass, not only at the precondition check
- signal: `spec_precision_gap` · recurrence: 1 feature(s) · scope: `generators` · harmful: 0
- features: documento-requisitos-abnt
- evidence: verification.md gap 2 - gerar.py:412-427 untested (generators)
- last seen: 2026-10-02T21:36:56Z

### L-002 - Compare case-sensitively when the check fixes the letter case of the text
- signal: `spec_precision_gap` · recurrence: 1 feature(s) · scope: `documents` · harmful: 0
- features: documento-requisitos-abnt
- evidence: verification.md gap 3 - test_documento.py:300 casefold (documents)
- last seen: 2026-10-02T21:36:56Z

### L-003 - Confirm the agent may edit a file before making a check depend on changing it
- signal: `ac_gap` · recurrence: 1 feature(s) · scope: `harness` · harmful: 0
- features: documento-requisitos-abnt
- evidence: verification.md C35 - verify.ps1:54-58 empty (harness)
- last seen: 2026-10-02T21:36:56Z

## Quarantined (failed when applied - ignore)

A confirmed lesson that recurred alongside failure. Kept for the maintainer to review.

_none_
