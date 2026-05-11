# Self-Checkup (ตรวจสุขภาพ) — Protocol

> A patient walks into the clinic knowing **symptoms** but not knowing **which symptoms matter, which are serious, which they're being careless about** ("ประมาท").
>
> The Self-Checkup is what the patient does **before** walking in — a structured 10-15 min self-assessment that surfaces severity, anti-minimization, and AI co-patient vitals. It produces a doctor-scannable summary that turns a hand-wavy "I have a problem" into a triageable case.

**Scope**: This is a **clinic for vibe coders** — people (CEOs, founders, designers, students, hobbyists) who build with AI agents (Claude, Cursor, ChatGPT, etc.) but aren't professional devs. Symptoms are technical: code structure, AI agent behavior, dev workflow, integrations, data, performance, security.

**NOT business problems** (revenue, team, strategy belong elsewhere). **NOT a generic AI helpdesk** (must go through full clinic flow + cure verdict).

**Captain's framing (2026-05-11)**:
- "เราเป็นคนไข้ในเรื่อง coding · การ set structure นั่นนี่"
- "อันนี้ AI clinic ไม่ใช่ business clinic"
- **"CLINIC FOR VIBE CODER"** ← the audience locked here

---

## Status: **V0.3 — RECOMMENDED for case filings · OPTIONAL as periodic ritual** (AI/dev scope locked)

- Not required at V0 (would suppress the first 5 case filings)
- Not fully optional (doctors get richer signal when present)
- Governance ratchet: after 10 closed cases, revisit whether pre-intake Self-Checkup becomes REQUIRED based on empirical cure-rate data (see [`GOVERNANCE.md`](./GOVERNANCE.md))

---

## The 4 pillars

This protocol synthesizes 4 design strands. Each lives in its own deliverable file under `~/ψ/active/oracle-clinic-self-checkup/`; this doc is the index + integration spec.

### 1. Symptom Taxonomy — 7 categories (AI / dev focus)

A symptom is a sign that something in one of these 7 technical systems is hurting. The taxonomy is dev-clinic-specific, drawing from SRE golden signals + AI agent failure modes + software architecture patterns.

| # | Category | One-line | Example symptoms |
|---|---|---|---|
| 1 | **Code Structure & Architecture** | "Where do things go?" repo layout, modules, abstractions, design patterns | "I don't know where to put this feature" · "this file is becoming a monster" · "should I split this?" · "is my abstraction wrong?" |
| 2 | **AI Agent Behavior** | Drift, hallucination, context loss, voice/role drift, tool errors, model issues | "agent keeps looping" · "context window saturated" · "drifted into wrong voice" · "hallucinated a file that doesn't exist" · "tool error swallowed" |
| 3 | **Dev Workflow & Tooling** | Git, CI/CD, env setup, packages, builds, IDE, terminal/tmux | "branch strategy is a mess" · "build flakes" · "env vars between machines diverged" · "package upgrade broke X" · "tmux session lost" |
| 4 | **Integration & APIs** | 3rd-party APIs (Claude/OpenAI/etc), webhooks, OAuth, schemas, federation interop (MCP, MAW) | "webhook isn't firing" · "OAuth scope too narrow" · "MCP server disconnects" · "schema mismatch between systems" |
| 5 | **Data & Persistence** | Schema design, integrity, race conditions, sync, migrations, query perf | "schema unclear, designed wrong" · "data drift between mirrors" · "race condition" · "slow query" · "migration scary, no rollback plan" |
| 6 | **Performance & Reliability** | Latency, memory, timeouts, scaling, deploy reliability, error handling | "code slow but I don't know where" · "OOM on cron" · "deploy fails 30% of the time" · "can't scale beyond N concurrent" |
| 7 | **Security & Privacy** | Credentials, permissions, secrets, PII, SACRED LAW compliance | "secret might be exposed" · "OAuth scope too broad" · "PII leaking into logs" · "violating CLAUDE.md SACRED LAW?" |

**Top design insight**: The **code + AI co-failure mode** — a broken/unclear technical setup (any category 1-7) **plus** a drifting AI co-patient (Section 5 vitals abnormal) — is the most dangerous diagnostic pattern. The human doesn't understand the dev problem; the AI is also lost. Standard help-desk advice assumes at least one party can navigate. Oracle-Clinic must catch both.

Cadence (post-V0):
- **On-symptom (any time)**: anything painful enough to walk into clinic → file case immediately
- **Weekly (5 min)**: AI Co-patient vitals — quick check on agent drift even when no acute symptom
- **Monthly (15 min)**: full 7-category sweep — surface emerging issues before they become acute
- **Per-project (one-time)**: when starting a new repo, codebase, or major integration — establish baseline

Source research: [`~/ψ/active/oracle-clinic-self-checkup/01-symptom-taxonomy.md`](https://github.com/Arra-Oracle-Clinic/oracle-clinic) is the original Agent 1 output, which leaned business-broad. This refactor (2026-05-11 v0.3) narrows to AI/dev scope per Captain's correction.

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
