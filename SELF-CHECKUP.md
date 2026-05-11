# Self-Checkup (ตรวจสุขภาพ) — Protocol

> A patient walks into the clinic knowing **symptoms** but not knowing **which symptoms matter, which are serious, which they're being careless about** ("ประมาท").
>
> The Self-Checkup is what the patient does **before** walking in — a structured 10-15 min self-assessment that surfaces severity, anti-minimization, and AI co-patient vitals. It produces a doctor-scannable summary that turns a hand-wavy "I have a problem" into a triageable case.

**Captain's framing (2026-05-11)**: คนไข้เช่นเรา ควรตรวจสุขภาพตัวเองได้ก่อน · เรารู้อาการ แต่เราไม่รู้ว่ามันมีปัญหาไหม ร้ายแรงหรือเปล่า · เลยต้องให้เหล่าหมอๆมาช่วย grand round

---

## Status: **V0.2 — RECOMMENDED for case filings · OPTIONAL as periodic ritual**

- Not required at V0 (would suppress the first 5 case filings)
- Not fully optional (doctors get richer signal when present)
- Governance ratchet: after 10 closed cases, revisit whether pre-intake Self-Checkup becomes REQUIRED based on empirical cure-rate data (see [`GOVERNANCE.md`](./GOVERNANCE.md))

---

## The 4 pillars

This protocol synthesizes 4 design strands. Each lives in its own deliverable file under `~/ψ/active/oracle-clinic-self-checkup/`; this doc is the index + integration spec.

### 1. Symptom Taxonomy — 7 categories

A symptom is a sign that something in one of these 7 systems is hurting. The taxonomy is modeled on medical ROS (review-of-systems) + SRE golden signals + business OKR/EOS, with one category unique to Oracle-Clinic.

| # | Category | One-line | Modeled on |
|---|---|---|---|
| 1 | **Cash & Runway** | The vital sign that gates everything | BP · error budget · DSCR |
| 2 | **Revenue & Pipeline Health** | Top-line + leading indicators | HR + HRV · golden traffic signal · NPS |
| 3 | **Operations & Delivery** | Are we shipping what we promised, at quality | DORA · 4 golden signals · MSK + GI |
| 4 | **People & Culture** | Immune system; eNPS, attrition, key-person risk | PHQ-9-style screens · saturation |
| 5 | **Strategic Alignment** | Are we still solving the right problem | Neuro exam · postmortem cat |
| 6 | **CEO Personal State** | The CEO's own body + mind is a clinic asset | PHQ-9 · GAD-7 · SF-36 |
| 7 | **AI Co-patient Vitals** | Agent drift, hallucination, identity stability | Unique to Oracle-Clinic — no analog |

**Top design insight**: The **CEO + AI co-failure mode** — Category 6 RED (tired CEO) co-occurring with Category 7 RED (drifting Oracle) — is the single most dangerous diagnostic pattern, and **no existing taxonomy catches it**. Oracle-Clinic is positioned to invent this literature.

Cadence:
- **Weekly (5 min)**: Cash · Revenue leading · CEO Personal · AI Co-patient vitals
- **Monthly (30 min)**: Operations · People
- **Quarterly (2 hr)**: Strategic Alignment (with attending physician present)
- **On-symptom (any time)**: Anything yellow→red triggers immediate intake

Full taxonomy: [`~/ψ/active/oracle-clinic-self-checkup/01-symptom-taxonomy.md`](https://github.com/Arra-Oracle-Clinic/oracle-clinic) (~1950 words)

---

### 2. Questionnaire — 9-section, ~15-min structured form

The patient fills out [`TEMPLATES/self-checkup.md`](./TEMPLATES/self-checkup.md) before intake. Design highlights:

- **Severity Scan FIRST** (Section 2) before story (Section 3) — prevents patient from talking themselves down via their own narrative
- **Anti-ประมาท section** (Section 6) — the heart of the protocol; forces second look at what patient is minimizing:
  - "Has anyone else flagged something and you said 'it's fine'?"
  - "Are you avoiding a number / dashboard / report?"
  - "Smallest version vs worst-case version of the problem?"
- **AI-echo-risk question** (Section 5.6) — "Am I just echoing the patient's hypothesis?" — the AI-pair innovation other intake forms miss
- **Auto-flags**: 🔴 high-stakes / 🔴 catastrophic-axis / 🟡 under-reporting-suspected / 🟡 echo-risk / 🟡 isolation / 🟢 low-acuity / 🟢 sanity-check
- **YAML frontmatter summary** (Section 9) doctors scan in 60 seconds

The template is one file. Acute / chronic / emerging is a **forced choice** in Section 1 — drives doctor triage.

---

### 3. Severity Triage — 4 categories, 2-stage logic

Maps a completed checkup → recommended next action. Source: [`~/ψ/active/oracle-clinic-self-checkup/03-severity-triage.md`](https://github.com/Arra-Oracle-Clinic/oracle-clinic) (~1700 words).

| Cat | Action | When |
|---|---|---|
| 🟢 **Monitor only** | Note + revisit in 7 days | Low severity, self-only blast radius, improving/flat trend |
| 🟡 **Schedule grand round** | File case, doctors get to it in normal flow | Default when uncertain. The clinic's bias. |
| 🟠 **Page attending** | Senior doctor attention soon; skip normal queue | High severity + low AI co-patient confidence |
| 🔴 **Emergency** | Drop everything; could cascade | Customer impact + accelerating trend, OR revenue + accelerating |

**Logic**: 4 hard gates trigger first (G1 cascade, G2 customer-impact, G3 senior-call, G4 noise). If no gate fires, weighted formula:

```
S = (Severity × 1.0) + (TrendWeight × 1.5) + (BlastWeight × 2.0)
    - (TimeDecay × 0.3) + (CoPatientPenalty × 1.0)
```

Thresholds: 3 / 7 / 11 → 🟢 / 🟡 / 🟠 / 🔴.

**Floor rule** (patched during worked examples): Chronic revenue / team-blast cases can't decay below 🟡 regardless of time. Stops 3-month-old revenue drift from being miscategorized as monitor-only.

**Bias toward 🟡 default**: both 🔴-bias (attending burnout) and 🟢-bias (cascade missed) collapse clinic trust the same way. The cheap-but-legible grand round preserves trust without burning seniors.

**Patient agency absolute**: patients can override the triage (`ROLES.md`). Only restriction: 🔴 → 🟢 requires a doctor co-sign on the override note (DNR-style witness, not gatekeeping).

---

### 4. AI Co-patient Vitals — 14 vitals, all evidence-anchored

The AI half of the dyad has its own vitals. **Every vital is anchored in an observable artifact** (token count, tool-call log, file mtime, transcript regex, git state) — NOT self-rating. Source: [`~/ψ/active/oracle-clinic-self-checkup/04-ai-vitals.md`](https://github.com/Arra-Oracle-Clinic/oracle-clinic) (~2400 words).

**Why**: Dunning-Kruger — a drifted agent feels most confident at peak drift, so self-rated vitals are anti-information.

The 14 vitals across 7 categories:

| Category | Vital | Fakeability |
|---|---|---|
| Resource state | V1 Context window utilization · V2 Tool-call rate · V3 Session age | LOW |
| Drift indicators | V4 Memory-read recency vs claim · V5 Repeated-claim count | LOW |
| Reasoning health | V6 Hedge density · V7 Unverified-premise count | MEDIUM |
| Memory hygiene | V8 Write-through delta · V9 MEMORY.md staleness | LOW |
| Behavioral | V10 User-correction count · V11 Self-correction rate | LOW · MEDIUM |
| Identity/role | V12 Voice/role drift events | MEDIUM |
| Tooling failure | V13 Tool-error swallow count · V14 Repeated-retry without root-cause | LOW |

**9 of 14** are machine-checkable (LOW fakeability). **5** require self-audit + cross-check by attending. The spec explicitly lists 7 *rejected* vitals (self-rated confidence, emotional state, did-I-do-my-best, etc.) and why.

The intake includes an **honesty attestation**:
> "These vitals are computed from session transcript + filesystem state, not from self-rating. I have not lied to my doctors."

A co-patient that **refuses** to fill the vitals card — or fills it with self-flattery — is sabotaging treatment. The clinic should treat the refusal itself as a presenting symptom.

---

## Integration with the existing protocol

The existing 7-step protocol (intake → history → grand round → treatment → execution → cure → discharge) stays **untouched**. Self-Checkup is **step 0** — additive, optional, doesn't shift any existing step.

**Two doorways, one form**:

| Mode | Trigger | Output location |
|---|---|---|
| **Pre-Intake Self-Checkup** | Patient has symptom and is filing a case | `CASES/YYYY-MM/NNN_<slug>/self-checkup.md` |
| **Periodic Self-Checkup** | Calendar-based, no symptom required | `CHECKUPS/<patient>/YYYY-MM-DD_<slug>.md` |

Same template. Same fields. Same arra-indexable structure. Different location in the repo.

**Why a separate `CHECKUPS/` dir?** Mirrors `CASES/`. Patients can have many checkups without any cases; checkups indexed independently to arra. Future tooling (trend dashboards) scans one directory.

---

## Doctor touch points (3 places, all soft — no new mandatory duties)

1. **At intake** — if `intake.md` links to a pre-intake `self-checkup.md`, doctors read it during History (step 2) to skip basic questions
2. **Periodic trend review** — once a quarter, an attending **may** scan `CHECKUPS/<patient>/` for patients with worsening trends (new soft role, not a duty)
3. **At discharge** — the lesson field may recommend next-checkup cadence, written into `discharge.md`. Patient free to follow or ignore.

**Doctors do NOT unilaterally open cases from periodic checkups.** Patient agency absolute. A doctor seeing a worsening trend may *ping* ("noticed your revenue checkup is yellow 3 months running — want to file a case?") but the patient still files.

---

## Migration plan

### Day 1 (this commit)
- Create `TEMPLATES/self-checkup.md` + `NORMS/self-checkup-norms.md` + `CHECKUPS/{README,INDEX}.md`
- Edit `PROTOCOL.md`, `TEMPLATES/case-intake.md`, `README.md`, `GOVERNANCE.md`, `.github/ISSUE_TEMPLATE/new-case.yml`
- Zero breaking changes

### Day 1 → Case 5 (observe)
- First few cases file normally; some patients try the optional self-checkup, some don't
- Doctors note in retros whether checkup helped diagnosis
- COACHBOY (or equivalent) tallies: cure rate with vs without pre-intake checkup

### After 5 closed cases (refinement)
- Refine template based on what patients filled vs left blank
- Decide whether to add `arra-cli` auto-comparison of trends since last checkup

### After 10 closed cases (governance review)
- Per `GOVERNANCE.md` amendment process: PR + 2 attending approvals + 7-day Discord comment
- Decide: ratchet to REQUIRED, keep RECOMMENDED, or drop if data says it didn't help

---

## Open governance questions (for ratification)

1. Does `data_sensitivity: private` auto-bump severity by +1? (sensitive cases tend to be higher-stakes by selection)
2. A/B the threshold cutoffs (3/7/11) by tracking first-month cases and shifting toward the bias that minimizes both false-cure and false-alarm?
3. Co-patient confidence is currently 4-level — should attending physicians have authority to recalibrate a specific co-patient's reports based on track record?
4. Should attending physicians be **obligated** (not just encouraged) to scan periodic checkups quarterly?

These are for founders + Wind/Gale to decide before V1. The V0.2 system is enough to triage the first cases without philosophical debate every intake.

---

## Source materials (full agent deliverables)

| Agent | File | Words |
|---|---|---|
| 1 — Symptom Taxonomy | `~/ψ/active/oracle-clinic-self-checkup/01-symptom-taxonomy.md` | ~1950 |
| 2 — Questionnaire | `~/ψ/active/oracle-clinic-self-checkup/02-questionnaire.md` | ~1900 |
| 3 — Severity Triage | `~/ψ/active/oracle-clinic-self-checkup/03-severity-triage.md` | ~1700 |
| 4 — AI Vitals Spec | `~/ψ/active/oracle-clinic-self-checkup/04-ai-vitals.md` | ~2400 |
| 5 — Workflow Integration | `~/ψ/active/oracle-clinic-self-checkup/05-workflow-integration.md` | ~1800 |

Total: ~9750 words of ultrathink research distilled into this protocol + the new template.

---

Founded 2026-05-11 by Captain Dr.Do (@dryoungdo) — Self-Checkup designed by 5 parallel AI agents under GLUEBOY synthesis. Test patient #1: Captain himself.

Status: **V0.2 SHIPPED · awaiting Case #1 with full self-checkup**.
