# Case Intake — TEMPLATE

> Copy this template to `CASES/YYYY-MM/NNN_<slug>/intake.md` when opening a new case.
> Fill every section. Doctors can't help what they can't see.

---

## Metadata

- **Case ID**: `YYYY-MM-NNN_<slug>`  (e.g., `2026-05-001_jera-revenue-drift`)
- **Opened**: `<ISO timestamp>`
- **Patient**: `<human name + role>` (e.g., "Dr.Do — CEO, Youngdo Wellness Clinic")
- **Co-patient**: `<AI agent / Oracle name>` (or `none` if patient is human-only)
- **Data sensitivity**: `public` | `redacted` | `private`
- **Tags**: `<tag1>, <tag2>` (e.g., `data-pipeline`, `ai-agent`, `billing`)

---

## Self-Checkup link (optional, recommended)

- **Pre-intake self-checkup**: `<./self-checkup.md>` or `N/A`
- **Self-triage verdict**: `🟢 GREEN` / `🟡 YELLOW` / `🟠 ORANGE` / `🔴 RED` (from Section 9 of checkup) or `N/A`

If you did a [`Self-Checkup`](../../../TEMPLATES/self-checkup.md) before filing, link it here. Doctors will read it during History (step 2) to skip basic questions. If you skipped the checkup, that's OK — but consider it next time; richer signal = faster diagnosis.

---

## Symptom

> Describe the symptom in **your own domain language**. Don't pretend to be a developer if you aren't one. The doctors will translate.
> 1-3 paragraphs. What's wrong? Since when? What does it look like from your side?

[your symptom here]

---

## What you've tried

> Honest list of what you tried before walking into the clinic. Including things that "should have worked but didn't".

- Tried X → got Y
- Tried Z → got W
- Asked AI tool A → answer didn't fit

---

## What you want from this clinic

> One sentence. What's the cure look like to you?

[e.g., "I want my revenue numbers to match between JERA and Supabase, and I want to understand why they drifted."]

---

## Co-patient vitals (if applicable)

> The AI agent's own self-report. What's its context state? Where did it drift? What is it uncertain about?

- **Context state**: [e.g., "Token usage ~40%, 80 tool calls into this session"]
- **What I tried**: [e.g., "Queried supabase 4 ways, got different totals each time"]
- **Where I'm uncertain**: [e.g., "Schema of `payments` table — I'm guessing at column names"]
- **Where I failed / drifted**: [e.g., "Suggested a fix that broke another report"]

---

## Constraints / context the doctors need to know

- Time pressure (e.g., "Need answer by next Mon")
- Stakeholders (e.g., "Pim sees these numbers weekly")
- What's NOT in scope (e.g., "Don't migrate from BigQuery — that's a separate decision")
- Anything sacred (e.g., "Personal data is SACRED LAW per CLAUDE.md — never publish raw rows")

---

## Consent

- [ ] I consent to this case being discussed in `#oracle-clinic` Discord (public-ish)
- [ ] I consent to the closed case being published in this public repo (subject to `data_sensitivity` redaction)
- [ ] I understand cure is not guaranteed; doctors propose, I dispose

— Patient signature: `<your name>`  ·  Date: `<YYYY-MM-DD>`
