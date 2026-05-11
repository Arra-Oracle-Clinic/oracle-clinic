# Self-Checkup — Step 0 Protocol

> **The thing patients do BEFORE filing a case.**
>
> A vibe coder walks in knowing **symptoms** but not knowing **which symptoms matter, which are dangerous, which they're being careless about** ("ประมาท"). The Self-Checkup is a 15-min structured self-assessment that turns hand-wavy "I have a problem" into a triageable case.

**Scope**: This is a clinic **for vibe coders** — non-devs building with AI agents. Symptoms are technical: code structure, AI agent behavior, dev workflow, integrations, data, performance, security. **Not** business problems.

**Status**: V0.3 · **RECOMMENDED** before case filing · **OPTIONAL** as periodic ritual · Governance ratchet to REQUIRED after 10 closed cases (see [`GOVERNANCE.md`](./GOVERNANCE.md))

---

## The 4 design pillars

The protocol synthesizes 4 ideas. Each is described concisely below; the full research drafts live in `~/ψ/active/oracle-clinic-self-checkup/0[1-5]-*.md` (~9,750 words — ultrathink-mode 5-agent synthesis).

### Pillar 1 — Symptom Taxonomy (7 categories)

| # | Category | Example symptom |
|---|---|---|
| 1 | **Code Structure & Architecture** | "Where do things go? Should I split this file?" |
| 2 | **AI Agent Behavior** | Drift, hallucination, context loss, tool errors |
| 3 | **Dev Workflow & Tooling** | Git, CI/CD, env, packages, builds |
| 4 | **Integration & APIs** | Webhooks, OAuth, MCP, schemas |
| 5 | **Data & Persistence** | Schema, integrity, race conditions, migrations |
| 6 | **Performance & Reliability** | Slow code, memory leaks, deploy failures |
| 7 | **Security & Privacy** | Credentials, permissions, PII, SACRED LAW |

**Top design insight**: The **code + AI co-failure mode** — broken setup (any category 1-7) **plus** a drifting AI co-patient — is the most dangerous diagnostic pattern. The human doesn't understand the dev problem; the AI is also lost. Standard help-desk advice assumes at least one party can navigate. Oracle-Clinic must catch both.

### Pillar 2 — Questionnaire (9 sections, ~15 min)

The template at [`TEMPLATES/self-checkup.md`](./TEMPLATES/self-checkup.md). Key design choices:

- **Severity Scan first** (Section 2) before Story (Section 3) — prevents the patient from talking themselves down via narrative
- **Anti-ประมาท section** (Section 6) — the heart of the protocol; surfaces what the patient is minimizing ("Are you avoiding a number? Have others flagged this and you said 'it's fine'?")
- **AI-echo-risk question** (Section 5.B) — "Am I (the AI co-patient) just echoing the patient's hypothesis?" — the AI-pair innovation other intake forms miss
- **YAML frontmatter summary** (Section 9) — doctors scan it in 60 seconds

### Pillar 3 — Severity Triage (4 categories)

| Cat | Action |
|---|---|
| 🟢 **Monitor only** | Note + revisit in 7 days |
| 🟡 **Schedule grand round** | File case, normal flow. *Default when uncertain.* |
| 🟠 **Page attending** | Senior attention soon; skip normal queue |
| 🔴 **Emergency** | Drop everything; could cascade |

**Logic — Stage 1 hard gates** (any fire → category locked):
- G1 Cascade: revenue blast + accelerating → 🔴
- G2 Customer-impact: customer blast + <24h + worsening → 🔴
- G3 Senior-call: AI co-patient confidence low + severity ≥ 7 → 🟠
- G4 Noise: severity ≤ 2 + improving + self-only → 🟢

**Stage 2 weighted score** (no gate fired):
```
S = (Severity × 1.0) + (TrendWeight × 1.5) + (BlastWeight × 2.0)
  − (TimeDecay × 0.3) + (CoPatientPenalty × 1.0)
```
- TrendWeight: improving = −2, flat = 0, worsening = +2, accelerating = +4
- BlastWeight: self = 0, team = 1, customer = 3, revenue = 4
- TimeDecay: `min(days_since_onset, 30)`
- CoPatientPenalty: high = 0, medium = 1, low = 2, unknown = 1

Thresholds: `S ≤ 3` → 🟢 · `3-7` → 🟡 · `7-11` → 🟠 · `>11` → 🔴

**Floor rule**: If Blast ≥ team AND Severity ≥ 5, floor is 🟡 (chronic team-impacting cases can't decay below schedule-grand-round regardless of age).

**Bias toward 🟡**: Both 🔴-bias (attending burnout) and 🟢-bias (cascade missed) collapse clinic trust the same way. The cheap-but-legible grand round preserves trust without burning seniors.

**Patient override**: Agency absolute (per [`ROLES.md`](./ROLES.md)). Only restriction: 🔴 → 🟢 requires doctor co-sign on the override note (DNR-style witness, not gatekeeping).

### Pillar 4 — AI Co-patient Vitals (14 evidence-anchored signals)

The AI agent half of the dyad has its own vitals. **Every vital is anchored in an observable artifact** (token count, tool-call log, file mtime, transcript regex, git state) — NOT self-rating. Why: Dunning-Kruger. A drifted agent feels most confident at peak drift; self-rated vitals are anti-information.

| Category | Vitals | Fakeability |
|---|---|---|
| Resource state | V1 context % · V2 tool-call rate · V3 session age | LOW |
| Drift indicators | V4 memory-read recency · V5 repeated-claim count | LOW |
| Reasoning health | V6 hedge density · V7 unverified-premise count | MEDIUM |
| Memory hygiene | V8 write-through delta · V9 MEMORY.md staleness | LOW |
| Behavioral | V10 user-correction count · V11 self-correction rate | LOW · MEDIUM |
| Identity/role | V12 voice/role drift events | MEDIUM |
| Tooling failure | V13 tool-error swallow count · V14 repeated-retry without root-cause | LOW |

**9 of 14 are machine-checkable.** 5 need self-audit + attending cross-check.

Mandatory **honesty attestation** at the end of every vitals card:
> "These vitals are computed from session transcript + filesystem state, not self-rating. I have not lied to my doctors."

A co-patient that refuses the vitals card, or fills it with self-flattery, is **sabotaging treatment**. Treat the refusal itself as a presenting symptom.

---

## Integration with the 7-step protocol

Self-Checkup is **Step 0 — pre-intake, recommended, additive**. The existing 7-step grand round stays untouched.

**Two doorways, one template**:

| Mode | Trigger | Output location |
|---|---|---|
| **Pre-intake** | Patient has symptom and is filing a case | `CASES/YYYY-MM/NNN_<slug>/self-checkup.md` |
| **Periodic** | Calendar-driven, no symptom required | `CHECKUPS/<patient>/YYYY-MM-DD_<slug>.md` |

Same template. Same fields. Same arra-indexable structure. Different folder.

---

## Doctor touch points (3 places, all soft)

1. **At intake** — if `intake.md` links to a pre-intake `self-checkup.md`, doctors read it during History (step 2) to skip basic questions
2. **Quarterly trend review** — an attending **may** scan `CHECKUPS/<patient>/` for worsening trends (new soft role, not duty)
3. **At discharge** — the lesson field may recommend next-checkup cadence; patient free to follow or ignore

**Doctors do NOT unilaterally open cases from periodic checkups.** Patient files. Always.

---

## Cadence (patient-owned, not enforced)

- **On-symptom (any time)** — skip checkup, file case, doctors structure intake. Don't slow yourself when something is on fire.
- **Weekly (5 min)** — AI co-patient vitals only. Quick drift check.
- **Monthly (15 min)** — full 7-category sweep
- **Per-project (one-time)** — when starting a new repo / codebase / major integration. Establishes baseline.

---

## Migration plan

| When | What |
|---|---|
| Day 1 (this commit) | Create template + CHECKUPS dir + integration edits. Zero breaking changes. First case can still ship without it. |
| Day 1 → Case 5 | Observe: which patients use it? Did it help diagnosis? |
| After 5 closed cases | Refine template based on real fill patterns |
| After 10 closed cases | Governance review: ratchet RECOMMENDED → REQUIRED if data shows cure-rate lift |

---

## Open questions for ratification

1. Does `data_sensitivity: private` auto-bump severity by +1? (Sensitive cases tend to be higher-stakes by selection.)
2. Should we A/B the 3/7/11 cutoffs? Track first-month cases.
3. Should attending physicians be **obligated** (not just encouraged) to scan periodic checkups quarterly?

Founders + Wind/Gale to decide before V1. The V0.3 system is enough to triage the first cases without philosophical debate every intake.

---

## Source research

The synthesis above distills ~9,750 words from 5 parallel AI agents (Symptom Taxonomy · Questionnaire · Severity Triage · AI Vitals · Workflow Integration). The full research drafts are kept in the founders' working directory; adopters who fork this repo don't need them to use the protocol.

The V0.3 refactor (2026-05-11) narrowed Agent 1's original business-broad taxonomy (Cash · Revenue · Ops · People · Strategic · CEO Personal · AI) to AI/dev-only scope per Captain's correction. **Adopters in different domains should re-do this narrowing for their own audience** — see [`USING-THIS-REPO.md`](./USING-THIS-REPO.md) for taxonomy-adaptation examples.

---

Founded **2026-05-11** by 5 parallel AI agents under GLUEBOY synthesis. Refactored same day for vibe-coder scope. Captain = test patient #1.
