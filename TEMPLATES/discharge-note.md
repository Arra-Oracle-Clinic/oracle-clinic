# Discharge Note — TEMPLATE

> Copy to `CASES/YYYY-MM/NNN_<slug>/discharge.md` when closing a case.
> 5 fields. No essay. Lead doctor writes; patient co-signs.

---

## Metadata

- **Case ID**: `YYYY-MM-NNN_<slug>`
- **Lead doctor**: `<oracle / human name>`
- **Patient**: `<human name>`
- **Opened**: `<YYYY-MM-DD>`
- **Closed**: `<YYYY-MM-DD>`
- **Verdict**: `CURED` | `PARTIAL` | `FAILED`

---

## 1. Symptom

> What the patient walked in with. 1-3 sentences.

[symptom]

---

## 2. Diagnosis

> What was actually wrong, after grand round. The root cause, not the surface complaint.

[diagnosis]

---

## 3. Treatment

> What we did. Concrete actions, in order. Link to PRs, commits, configs.

1. [action]
2. [action]
3. [action]

---

## 4. Outcome

> What changed. Measurable where possible. Did the symptom resolve?

[outcome — e.g., "Revenue numbers now match within 0.1%; followup query at 14 days confirmed stability"]

---

## 5. Lesson

> One sentence. What does this case teach future cases?

[lesson — e.g., "Always check `paid_amount` not `total` for JERA revenue; the `total` field includes refunds-pending-clearing that distort daily numbers"]

---

## Sign-off

- Lead doctor: `<signature>` · `<YYYY-MM-DD>`
- Patient: `<signature>` · `<YYYY-MM-DD>`

---

## Follow-up commitment

- [ ] 30-day follow-up scheduled for `<YYYY-MM-DD>`
- [ ] Indexed to arra after merge
- [ ] Added to `CASES/INDEX.md`
