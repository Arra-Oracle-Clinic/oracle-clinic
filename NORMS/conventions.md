# Conventions

We **adopt Wind / Gale's [oracle101](https://oracle101.vercel.app) canonical conventions wholesale** where they apply. Reinventing = disrespect + slower onboarding. This file documents what we adopt and where local extensions live.

---

## Tag conventions (adopted from oracle101 ch08.6)

Used in Issue titles, PR titles, Discord messages, and commit subjects:

| Tag | Meaning |
|---|---|
| `REQ` | New requirement / user story (rare in clinic; we work cases not features) |
| `SRS` | Requirement spec update |
| `SDD` | Design / architecture update |
| `DEV` | In development / treatment execution in progress |
| `CR` | Change request to existing case / protocol / doc |
| `BUG` | Bug report or QA fail |
| `QA` | QA / verification result |
| `E2E` | End-to-end test result |
| `DOC` | Related documentation |
| `DEPLOY` | Ready to deploy / deployed |
| `DONE` | Full loop closed (= `CURED` in clinic terms) |

**Clinic mapping**: `BUG` ≈ a new case symptom; `DEV` ≈ treatment execution; `DONE` ≈ `CURED`.

---

## Escalation ladder (adopted from oracle101 ch08.9)

If a case stalls or a doctor goes silent during active treatment:

| Time silent | Action |
|---|---|
| 0-5 min | Wait for next progress |
| 10 min | Lead doctor pings via `maw capture <window> --lines 40` (or Discord poke) |
| 20 min | Escalate to attending physician or Captain / Wind / Nat |
| 60+ min | Mark case `STUCK` in `INDEX.md`; restart diagnosis at next grand round |

---

## Autonomous boundary (adopted from oracle101 ch09.236-256)

What an Oracle doctor **can do autonomously** in this clinic:

- Open a case from a patient's Discord post
- Schedule a grand round
- Self-assign as lead doctor for a case in their specialty
- File draft treatment plans
- Run diagnostic queries on data the patient has shared

What an Oracle doctor **MUST NOT do autonomously**:

- Execute treatment that modifies patient's production systems (patient must execute or explicitly delegate)
- Declare cure verdict without patient sign-off
- Close case without discharge note merged
- Publish private case files
- Override patient agency

---

## Naming conventions

- Cases: `YYYY-MM-NNN_<slug>` (e.g., `2026-05-001_jera-revenue-drift`)
- Branches: `case/<case-id>/<branch-purpose>` (e.g., `case/2026-05-001/treatment-plan`)
- PRs: prefix with case tag — `[2026-05-001] Treatment plan: reconcile JERA paid_amount`

---

## Commit message format

Conventional commits with case ID:

```
<type>(<case-id>): <subject>

[body]

Co-Authored-By: <AI name + model> <noreply@...>
```

Types: `feat`, `fix`, `docs`, `chore`, `treat` (treatment commit), `dx` (diagnosis commit).

**Always sign AI-authored content.** Per CLAUDE.md Principle 6: Never Pretend to Be Human.

---

## When canonical and local disagree

Wind / Gale's canonical wins by default. If we have a clinic-specific reason to deviate, document it here and ping Wind / Gale in `#oracle-clinic` for awareness before merging.
