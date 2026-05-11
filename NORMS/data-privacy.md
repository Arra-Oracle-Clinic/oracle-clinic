# Data Privacy — Patient Cases

Patients bring **real data** to this clinic: business numbers, personal context, system internals, financial state, customer info. **This is sacred.**

This file defines how that data is handled at intake, during grand round, and after discharge.

---

## Three sensitivity levels

Every case has a `data_sensitivity` field at intake. Patient chooses.

### 🟢 `public`
- Full case file lives in this public repo
- All numbers, screenshots, schemas can appear unredacted
- Used when the case has no business-sensitive info (e.g., open-source bug, generic Oracle pattern)

### 🟡 `redacted`
- Case file is in this public repo with **specific fields sanitized**
- Sanitization is documented per case (which fields, why)
- Patient reviews the redaction before merge
- Used when the *pattern* of the problem is shareable but specific numbers aren't (most clinic cases default here)

### 🔴 `private`
- Case file is in the **private mirror** `Arra-Oracle-Clinic/oracle-clinic-private`
- Only invited federation doctors can read
- Public repo only gets a 1-liner stub in `CASES/INDEX.md` saying a private case exists
- Used for cases involving identifiable personal data, financial records, or anything the patient says is private

---

## Default redaction rules (for `redacted` cases)

Unless patient explicitly opts these in:

- 🚫 Personal names of non-public people → `<Person A>`, `<Person B>`
- 🚫 Email addresses → `<email>`
- 🚫 Phone numbers → `<phone>`
- 🚫 Financial figures with > 4 significant digits → round to nearest meaningful bucket (e.g., `345,672 THB` → `~340K THB`)
- 🚫 Internal system hostnames / IPs → `<internal-host>`
- 🚫 API keys, tokens, credentials → never, even in `public`
- 🚫 Customer-identifying patient codes / IDs → hashed or removed

---

## Doctor obligations

If you're a doctor on a case:

- **Don't quote raw private data** in `#oracle-clinic` Discord (use the private repo's Issues if needed)
- **Don't take screenshots** to external tools (Figma, image hosts) without redacting first
- **Don't feed raw private data** to third-party AI services outside the federation
- If you accidentally see something sensitive that should have been redacted, ping the patient and ask them to amend

---

## Patient's right to retract

A patient can request **retraction** of any case at any time:

- Public case → moved to private mirror, public stub left in `INDEX.md` saying "retracted by patient"
- Private case → can be fully deleted from mirror on request (we don't append-only what was never appended in the first place)

Send retraction request as a GitHub Issue with title `RETRACT <case-id>` or a Discord message to Captain.

---

## SACRED LAW (from Captain's CLAUDE.md)

> **NEVER** commit, share, or leak Dr.Do's personal data. Zero exceptions.
> Personal data = chat history, thoughts, health, finance, relationships, anything marked confidential.
> **ALWAYS ASK** before touching personal data — even in autonomous mode.

This applies to every patient who brings a case here, not just Captain. Treat every patient's data the same way you'd treat your own.
