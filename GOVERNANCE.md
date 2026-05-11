# Governance

This document is **WIP at V0**. The clinic is just opening. We'll formalize as patterns emerge from real cases.

---

## What's decided at V0 launch (2026-05-11)

### Repo ownership
- Org: `Arra-Oracle-Clinic` (GitHub org, owned by Captain Dr.Do at launch)
- Repo: `oracle-clinic` (this repo) — public
- Private mirror: `oracle-clinic-private` (created on demand for `data_sensitivity: private` cases)

### Founders
- Captain Dr.Do (@dryoungdo) — first patient + founding ops
- พี่นัท (Nat Weerawan) — federation co-founder, framework provider (Nat's Oracle ecosystem)
- พี่วิน (Wind / Gale owner) — federation co-founder, canonical conventions (oracle101)
- GLUEBOY — founding doctor (also co-patient with Captain)

### Decision rules (V0)
- **Who can open a case**: anyone (open Issue or post in Discord)
- **Who can claim a case**: any doctor listed in `DOCTORS/`
- **Who can declare cure verdict**: the lead doctor + patient (must agree)
- **Who can close a case**: lead doctor after discharge note merged
- **Who admits new doctors**: any existing attending physician (PR review + 👍 in Discord)
- **Who can change PROTOCOL.md or ROLES.md**: requires PR + at least 2 attending approvals + 7-day public comment in `#oracle-clinic`

### What we explicitly don't have yet
- Formal voting / quorum rules
- Conflict-of-interest policy
- Compensation / payment model (clinic is free at V0)
- Removal process for doctors
- IP / licensing finalization (see README license note)

---

## What we'll formalize after N cases

| Trigger | Formalize |
|---|---|
| First disagreement between doctors that needs adjudication | Conflict resolution process |
| First patient outside founders | Patient onboarding flow |
| First request for paid consult | Compensation model |
| First doctor needing removal | Removal process |
| 10th closed case | Codify lessons into PROTOCOL.md V2 |
| 10th closed case | Decide whether pre-intake Self-Checkup ratchets from RECOMMENDED → REQUIRED, based on cure-rate data (cases with checkup vs without) |

---

## Amendment process

This GOVERNANCE.md is itself amendable by the same process as PROTOCOL.md (PR + 2 attending approvals + 7-day Discord comment window).

**Append-only changes preferred.** When something changes, add a dated section below. Don't rewrite history.

---

## Changelog

- **2026-05-11** — Initial V0 governance. Founders + decision rules above.
