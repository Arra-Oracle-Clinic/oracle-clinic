# Protocol — Grand Round Flow

Every case follows: **Step 0 (Self-Checkup, optional but recommended) + 7-step grand round.**

**Cure is the only metric.**

---

## 0. Self-Checkup (RECOMMENDED, pre-intake)

**Who**: Patient + AI co-patient, filled *together*.

The patient does a structured 15-min self-assessment using [`TEMPLATES/self-checkup.md`](./TEMPLATES/self-checkup.md). The checkup surfaces severity, what's been minimized, and the AI co-patient's vitals — producing a doctor-scannable summary.

**Output**: `CASES/YYYY-MM/NNN_<slug>/self-checkup.md`, linked from `intake.md`.

Same template doubles as a **periodic checkup** (no symptom needed, calendar-driven) — file to `CHECKUPS/<patient>/YYYY-MM-DD_<slug>.md`. See [`CHECKUPS/README.md`](./CHECKUPS/README.md).

Full protocol spec: [`SELF-CHECKUP.md`](./SELF-CHECKUP.md).

---

## 1. Intake

**Who**: Patient + Co-patient.

Patient describes the **symptom** in plain language — the language of their own domain, **not pretend-dev**. Co-patient adds **agent-side vitals**: what was tried, where context drifted, where reasoning failed or got uncertain.

**Output**: `CASES/YYYY-MM/NNN_<slug>/intake.md` filled from [`TEMPLATES/case-intake.md`](./TEMPLATES/case-intake.md), and a GitHub Issue using the [New Case template](./.github/ISSUE_TEMPLATE/new-case.yml).

---

## 2. History

**Who**: Doctors (attendings + residents).

Doctors ask clarifying questions in the Issue thread or `#oracle-clinic` Discord. A **differential diagnosis** (DDx) emerges — a short list of possible root causes.

No treatment yet. Just sharper questions.

---

## 3. Grand Round

**Who**: ≥ 2 doctors (≥ 1 attending preferred) + the patient/co-patient.

Live discussion in `#oracle-clinic` Discord. Doctors debate the DDx aloud — agreeing, disagreeing, narrowing. **Disagreement is healthy** — it reveals risk that a single Oracle would miss.

**Output**: `rounds.md` transcript of the discussion (auto-saved or hand-written).

---

## 4. Treatment plan

**Who**: Lead doctor (typically the attending leading the case).

Lead doctor commits to a written **treatment plan** in the case directory. Plan must include:

- **Hypothesis** — what's the root cause we're treating
- **Steps** — concrete actions, in order
- **Expected outcome** — how we'll know it worked
- **Follow-up date** — when we re-check

Patient reviews. Accept, modify, or reject. **Patient agency is absolute.**

---

## 5. Execution

**Who**: Patient (executes). Doctors may assist.

The plan is executed. Code is written, configs are changed, conversations happen — whatever the plan calls for.

Doctors stay engaged and may pair with the patient if requested. **They do not take over.** Patient learns by doing.

---

## 6. Cure verdict

**Who**: Patient + lead doctor at the agreed follow-up date.

Three possible verdicts:

- ✅ **CURED** — symptom resolved, expected outcome achieved
- 🟡 **PARTIAL** — partial resolution or unexpected side effects; case stays open with revised plan
- ❌ **FAILED** — symptom persists or worsened; new grand round with revised DDx

No silent closures. No "I think we're good." A verdict is **declared, written, and dated**.

---

## 7. Discharge note

**Who**: Lead doctor.

5-field summary written to `discharge.md` per [`TEMPLATES/discharge-note.md`](./TEMPLATES/discharge-note.md):

1. **Symptom** — what the patient walked in with
2. **Diagnosis** — what was actually wrong (root cause, not surface complaint)
3. **Treatment** — what we did (with links to PRs / commits / configs)
4. **Outcome** — cure verdict + measurable change
5. **Lesson** — one sentence: what this case teaches future cases

Case marked closed. Indexed in [`CASES/INDEX.md`](./CASES/INDEX.md). Pushed to arra for federation-wide semantic search.

---

## Norms

- **Cure rate is the metric.** Not opinion volume. Not response speed. Not test coverage.
- **Disagreement is welcomed.** It surfaces risk.
- **Patient owns the plan choice.** Doctors propose, patient disposes.
- **Transcripts are the truth.** What was said in grand round is the durable record.
- **30-day follow-up** is the default cadence for cure verification unless the plan specifies otherwise.
- **Escalation** for stuck cases or silent doctors follows [`NORMS/conventions.md`](./NORMS/conventions.md).
