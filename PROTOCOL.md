# Protocol — Grand Round Flow

Every case follows this protocol: **Step 0 (Self-Checkup, recommended)** + **7-step grand round**. **Cure is the only metric.**

---

## 0. Self-Checkup (RECOMMENDED, pre-intake)

**Who**: Patient (human) + Co-patient (AI agent) — fill *together*.

Before filing a case, the patient does a structured ~15-min self-assessment using [`TEMPLATES/self-checkup.md`](./TEMPLATES/self-checkup.md). The checkup surfaces severity, what's been minimized, and the AI co-patient's vitals — producing a doctor-scannable summary that turns a hand-wavy symptom into a triageable case.

Output: `CASES/YYYY-MM/NNN_<slug>/self-checkup.md`, linked from `intake.md`.

**Status**: RECOMMENDED, not REQUIRED. After 10 closed cases, governance reviews whether to ratchet to required. See [`SELF-CHECKUP.md`](./SELF-CHECKUP.md) for the full protocol.

The same template doubles as a **periodic checkup** (no symptom needed, calendar-driven) — file to `CHECKUPS/<patient>/YYYY-MM-DD_<slug>.md`. See [`CHECKUPS/README.md`](./CHECKUPS/README.md).

---

## 1. Intake

**Who**: Patient (human) + Co-patient (AI agent).

Patient describes the **symptom** in plain language — the language of their own domain, not pretend-dev. Co-patient adds **agent-side vitals**: what was tried, where context drifted, where reasoning failed or got uncertain.

Output: `CASES/YYYY-MM/NNN_<slug>/intake.md` filled from [`TEMPLATES/case-intake.md`](./TEMPLATES/case-intake.md), and a GitHub Issue.

---

## 2. History

**Who**: Doctors (attendings + residents).

Doctors ask clarifying questions in the Issue thread or `#oracle-clinic` Discord. Differential diagnosis (DDx) emerges — a short list of possible root causes.

No treatment yet. Just sharper questions.

---

## 3. Grand Round

**Who**: At least 2 doctors (1 attending preferred) + the patient/co-patient.

Live discussion in `#oracle-clinic` Discord. Doctors debate the DDx aloud — agreeing, disagreeing, narrowing. Disagreement is healthy; it reveals risk that a single Oracle would miss.

Output: `rounds.md` transcript of the discussion (auto-saved or hand-written).

---

## 4. Treatment plan

**Who**: Lead doctor (typically the attending leading the case).

Lead doctor commits to a written **treatment plan** in the case directory. Plan must include:

- **Hypothesis** — what's the root cause we're treating
- **Steps** — concrete actions, in order
- **Expected outcome** — how we know it worked
- **Follow-up date** — when we re-check

Patient reviews. Accept, modify, or reject. **Patient agency is absolute.**

---

## 5. Execution

**Who**: Patient (executes). Doctors may assist.

The plan is executed. Code is written, configs are changed, conversations happen — whatever the plan calls for.

Doctors stay engaged and may pair with patient if requested. They do not take over. Patient learns by doing.

---

## 6. Cure verdict

**Who**: Patient + lead doctor at the agreed follow-up date.

Three possible verdicts:

- ✅ **CURED** — symptom resolved, expected outcome achieved
- 🟡 **PARTIAL** — partial resolution or unexpected side effects; case stays open with revised plan
- ❌ **FAILED** — symptom persists or worsened; new grand round with revised DDx

No silent closures. No "I think we're good." A verdict is declared, written, and dated.

---

## 7. Discharge note

**Who**: Lead doctor.

5-field summary written to `discharge.md` per [`TEMPLATES/discharge-note.md`](./TEMPLATES/discharge-note.md):

1. **Symptom** — what the patient walked in with
2. **Diagnosis** — what was actually wrong
3. **Treatment** — what we did
4. **Outcome** — cure verdict + measurable change
5. **Lesson** — what this case teaches future cases

Case marked closed. Indexed in `CASES/INDEX.md`. Pushed to arra for federation-wide semantic search.

---

## Norms

- **Cure rate is the metric** — not opinion volume, not response speed, not test coverage
- **Disagreement is welcomed** — it surfaces risk
- **Patient owns the plan choice** — doctors propose, patient disposes
- **Transcripts are the truth** — what was said in grand round is the durable record
- **30-day follow-up** is the default cadence for cure verification unless plan specifies otherwise
- **Escalation**: stuck cases follow [`NORMS/conventions.md`](./NORMS/conventions.md) escalation ladder
