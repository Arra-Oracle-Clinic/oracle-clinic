# Periodic Checkups — Index

Auto-updated 1-line summary per checkup. Newest at top.

| Patient | Date | Slug | Cadence | Self-triage | Flags |
|---|---|---|---|---|---|
| _(awaiting first periodic checkup)_ | | | | | |

---

## Self-triage legend

- 🟢 `GREEN` — no case needed, monitor only, revisit per cadence
- 🟡 `YELLOW` — consider filing a case soon if trend continues
- 🟠 `ORANGE` — file case now; senior attention recommended
- 🔴 `RED` — emergency; file case immediately

See [`SELF-CHECKUP.md`](../SELF-CHECKUP.md) for the triage formula.

---

## How to add an entry

When you submit a periodic checkup PR, add a row above with:
- **Patient**: your handle (e.g., `dryoungdo`)
- **Date**: `YYYY-MM-DD`
- **Slug**: short description (e.g., `monthly`, `pre-board`, `post-trip`)
- **Cadence**: `weekly` / `monthly` / `quarterly` / `ad-hoc`
- **Self-triage**: the patient's own verdict
- **Flags**: comma-separated flag emojis from Section 9 of the checkup

Example:
```
| dryoungdo | 2026-05-15 | monthly | monthly | 🟡 | 🟡 echo-risk, 🟡 under-reporting-suspected |
```
