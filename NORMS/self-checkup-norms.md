# Norms — Self-Checkup

Operational norms for the Self-Checkup protocol. See [`SELF-CHECKUP.md`](../SELF-CHECKUP.md) for the full protocol spec and [`TEMPLATES/self-checkup.md`](../TEMPLATES/self-checkup.md) for the fillable template.

---

## When to use Self-Checkup

| Situation | Use Self-Checkup? |
|---|---|
| About to file a case | **RECOMMENDED** (pre-intake mode → `CASES/YYYY-MM/NNN/self-checkup.md`) |
| Calendar-driven monthly / quarterly check | **OPTIONAL** (periodic mode → `CHECKUPS/<patient>/`) |
| Emergency (🔴) — symptoms accelerating, time-critical | **SKIP** — file the case directly. Don't slow yourself down. |
| Sanity check ("am I being paranoid?") | **YES** — exactly what 🟢 `sanity-check` flag is for |
| First time visiting the clinic | **YES** — baseline checkup establishes the trend line |

**Rule of thumb**: if you have 15 minutes, do the checkup. If you don't, file the case and a doctor will help structure the intake.

---

## Who reads it

| Reader | When | What they look for |
|---|---|---|
| **The patient** | While filling | Surface what they've been minimizing (Section 6) |
| **The co-patient AI** | While filling Section 5 | Honest self-report of own vitals |
| **Doctors at intake** | If linked from `intake.md` | Skip basic history questions; jump to differential |
| **Attending physicians** | Quarterly (optional) | Trends across periodic checkups in `CHECKUPS/<patient>/` |
| **Future patients** | Anytime | Search arra for similar checkups; learn from prior cases |

**Doctors do NOT read self-checkups uninvited from other patients' periodic directories** for casework. Cross-patient pattern recognition is allowed; cross-patient case-opening is not.

---

## Privacy interaction

Self-Checkups inherit the patient's stated `data_sensitivity` from Section 1 of the form:

| Sensitivity | Where the file lives |
|---|---|
| `public` | This public repo (`CASES/.../self-checkup.md` or `CHECKUPS/.../`) |
| `redacted` | Public repo, with sensitive fields sanitized; redaction documented per file |
| `private` | `Arra-Oracle-Clinic/oracle-clinic-private` mirror; public repo gets 1-line stub in INDEX |

Specific to Self-Checkup:
- **Section 6 (Anti-ประมาท) is often the most sensitive** — patient admitting what they're avoiding. Strongly consider `redacted` minimum.
- **Section 5 (AI vitals)** is generally `public` — agent state isn't usually PII.
- **Section 7.5 (Sacred)** must be redacted if it names specific people or relationships.

See [`NORMS/data-privacy.md`](./data-privacy.md) for the full data privacy framework.

---

## Patient agency

- **Patient files** — doctors don't initiate cases from periodic checkups they happen to read.
- **Patient overrides triage** — patient can downgrade or upgrade the system's recommendation. Only restriction: 🔴 → 🟢 requires doctor co-sign (DNR-style witness).
- **Patient retracts** — at any time, via PR or Discord message to Captain. Public file → moved to private mirror with "retracted by patient" stub. Private → fully deleted on request.
- **Patient owns cadence** — clinic provides template + indexing; patient picks weekly / monthly / quarterly / ad-hoc.

---

## AI co-patient honesty

Per the protocol's design, the AI co-patient must fill Section 5 with **evidence-anchored vitals** (token count, tool log, file mtime, transcript regex, git state) — NOT self-rated feelings.

**The honesty attestation is mandatory**:
> "These vitals are computed from session transcript + filesystem state, not self-rating. I have not lied to my doctors."

An AI co-patient that:
- Refuses to fill the vitals card, OR
- Fills it with self-flattery (V6 hedges stripped, V7 unverified-premises hidden, V11 self-correction rate inflated), OR
- Omits flagged vitals from the summary

…is **sabotaging treatment**. Doctors should treat the refusal or pattern as a presenting symptom of its own. Possible diagnoses include identity-drift, sycophancy bias, or model degradation.

**Spot-audit policy**: Attending physicians may independently check the 5 MEDIUM-fakeability vitals (V6 hedge density, V7 unverified premises, V11 self-correction rate, V12 voice/role drift, V5 repeated-claim). LOW-fakeability vitals (V1-V4, V8-V10, V13-V14) are machine-checkable from logs.

---

## Cadence enforcement

**Periodic Self-Checkups are NEVER enforced by the clinic.**

- No reminders, no nag bots, no patient ratings based on adherence
- The clinic provides infrastructure; patients provide discipline
- A patient who hasn't done a periodic checkup in months is not "non-compliant" — they're just a patient who doesn't want one right now

**Exception**: if a discharge note recommends a next-checkup cadence ("revisit in 14 days") and the patient wants doctor accountability, they can pin that commitment with a GitHub Issue. Patient-initiated, never doctor-imposed.

---

## Auto-flag escalation

Self-Checkup auto-flags route to doctor playbook actions per [`TEMPLATES/self-checkup.md`](../TEMPLATES/self-checkup.md) "Red-flag handling" table. Quick reference:

- 🔴 `high-stakes` → attending required, grand round in 48h
- 🔴 `catastrophic-axis-<n>` → open round acknowledging axis explicitly
- 🟡 `under-reporting-suspected` → broader DDx than patient's framing
- 🟡 `echo-risk` → first question to patient directly, not co-patient
- 🟢 `low-acuity` → resident-led teaching case
- 🟢 `sanity-check` → short round, confirmation not diagnosis

---

## Governance ratchet

Per [`GOVERNANCE.md`](../GOVERNANCE.md), after **10 closed cases** the founders + attendings review:

- Cure rate of cases WITH pre-intake Self-Checkup vs WITHOUT
- Median time-to-first-doctor-response with vs without
- Patient satisfaction (if surveyed)

Possible outcomes:
- Ratchet pre-intake Self-Checkup from RECOMMENDED → REQUIRED
- Keep at RECOMMENDED
- Drop entirely if data says it didn't help

Decided via the standard amendment process (PR + 2 attending approvals + 7-day Discord comment).

---

## Implementation status

| Element | Status |
|---|---|
| Template | ✅ V0.2 — `TEMPLATES/self-checkup.md` |
| Periodic CHECKUPS/ dir | ✅ V0.2 — README + INDEX.md scaffolded |
| Integration with PROTOCOL.md | ✅ V0.2 — Step 0 added |
| Integration with case-intake.md | ✅ V0.2 — link field added |
| Integration with .github/ISSUE_TEMPLATE | ✅ V0.2 — optional field added |
| Automation: arra-index on merge | ⏳ Manual at V0.2; V1+ via CI |
| Automation: trend dashboard | ⏳ V2+ |
| Doctor periodic-trend-scan obligation | ❌ Never (patient agency) |
