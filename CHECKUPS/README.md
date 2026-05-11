# CHECKUPS — Periodic Self-Checkups

Patient-owned periodic self-assessments. **No symptom required.** Calendar-driven. Patient-owned cadence.

For symptom-driven pre-intake checkups, see `CASES/YYYY-MM/NNN_<slug>/self-checkup.md` instead.

---

## What lives here

Standalone periodic checkups that aren't tied to any specific case. Patients (vibe coders) do these to:

- Surface emerging technical symptoms BEFORE they become acute cases
- Track trends across the 7-category dev symptom taxonomy (Code Structure · AI Agent Behavior · Dev Workflow · Integration & APIs · Data & Persistence · Performance & Reliability · Security & Privacy)
- Build a longitudinal record for arra-indexed retrieval
- Practice the discipline of looking at vitals on a cadence (especially AI co-patient drift, which compounds silently)

---

## Layout

```
CHECKUPS/
├── README.md              ← you are here
├── INDEX.md               Auto-updated 1-liner index
└── <patient-handle>/
    ├── 2026-05-11_baseline.md
    ├── 2026-06-11_monthly.md
    └── 2026-09-11_quarterly-deep.md
```

`<patient-handle>` is the patient's preferred handle (e.g., `dryoungdo`, `nazt`, `wind6686`). One directory per patient.

---

## How to do a periodic checkup

1. Copy [`TEMPLATES/self-checkup.md`](../TEMPLATES/self-checkup.md) to `CHECKUPS/<your-handle>/YYYY-MM-DD_<slug>.md` (slug examples: `monthly`, `quarterly-deep`, `post-trip`, `pre-board-meeting`).
2. Set `checkup_type: periodic` in Section 1.
3. Fill out as normal. Time-box 15 min.
4. Open a PR adding the file.
5. Merge — your checkup is indexed to arra automatically (V0.2: manual; V1+ via CI workflow).

---

## Cadence guidance (patient-owned, not enforced)

Suggested cadences for vibe coders:
- **Weekly (5 min)** — AI Co-patient vitals (Section 5) only. Quick check on agent drift.
- **Monthly (15 min)** — Full 7-category sweep. Surface emerging issues before they go acute.
- **Per-project (one-time)** — When starting a new repo, codebase, or major integration. Establish baseline.
- **On-symptom (any time)** — Skip to filing a case. Self-Checkup not required when something is on fire.

Patients pick their own pace. The clinic provides the template + indexing; the patient provides the cadence.

---

## Doctor touch points (soft, all encouraged not required)

- **Quarterly trend review** — an attending **may** scan `CHECKUPS/<patient>/` for patients with worsening trends. New soft role; not a duty.
- **Discharge follow-up** — when a case closes, the discharge note may recommend a next-checkup cadence. Patient is free to follow or ignore.

**Doctors do NOT unilaterally open cases from periodic checkups.** Patient agency is absolute (per `ROLES.md`). A doctor noticing a worsening trend may ping the patient ("noticed your revenue checkup is yellow 3 months running — want to file a case?"). The patient still files.

---

## Privacy

Periodic checkups inherit the patient's default `data_sensitivity`:
- `public` → file lives here in the public repo
- `redacted` → sanitized version here, sensitive fields documented per checkup
- `private` → file in `Arra-Oracle-Clinic/oracle-clinic-private` (collaborators only), public stub here in `INDEX.md`

**Patient decides per checkup. No auto-publish.** See [`NORMS/data-privacy.md`](../NORMS/data-privacy.md).

---

## Patient ownership

Each patient owns their own `<patient-handle>/` directory:
- Patient can amend their own checkups (PR)
- Patient can retract any checkup at any time (delete via PR)
- Other patients / doctors do NOT edit another patient's checkup files

The repo's permission model assists this — collaborators have write access to the whole repo, but social convention enforces patient-folder boundaries.

---

## Why a separate `CHECKUPS/` dir?

- Mirrors `CASES/`
- Patients can have many checkups without any cases
- Independent arra indexing for trend analysis
- Future tooling (trend dashboards, anomaly detection) scans one directory

See also: [`SELF-CHECKUP.md`](../SELF-CHECKUP.md) — the full protocol spec.
