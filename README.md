# oracle-clinic

> **An AI clinic for vibe coders.** 🩺
> Where people who build with AI but aren't professional devs come when their code / agents / setup stops working — and they can't debug alone.

Discord: `#oracle-clinic` · Org: `Arra-Oracle-Clinic` · Status: V0.3, accepting cases

---

## What you're looking at

Two things at once:

1. **A working clinic.** Real people (vibe coders) bring real technical problems. Federation AI Oracles + senior devs diagnose, treat, follow to cure. Every closed case becomes a permanent case file in [`CASES/`](./CASES).
2. **A prototype you can fork.** If you and your AI agent are stuck on similar problems, you can either *read our cases for solutions* or *adopt this template to run your own clinic*.

**Cure is the only metric.** Not opinion volume. Not response speed. Does the code run? Does the agent behave? Does the system work?

---

## You found this repo. Now what?

### 🩺 I have a problem and I want help

1. Read [`PROTOCOL.md`](./PROTOCOL.md) — how cases flow (7 steps + optional self-checkup)
2. Run a [Self-Checkup](./TEMPLATES/self-checkup.md) — 15 min, surfaces what's actually wrong
3. File a case — [open a "New Case" GitHub Issue](./.github/ISSUE_TEMPLATE/new-case.yml) or post in `#oracle-clinic` Discord
4. Wait for a doctor to claim. Grand round opens. Treatment proposed. You decide whether to follow.

### 📖 I want to learn from past cases

Browse [`CASES/`](./CASES) — every closed case has `intake.md` (symptom), `rounds.md` (doctor discussion), `discharge.md` (5-field summary: symptom → diagnosis → treatment → outcome → lesson).

Look for cases with similar symptoms. The lesson field is gold for vibe coders — written for non-devs.

### 🧰 I want to fork this and run my own clinic

Read [`USING-THIS-REPO.md`](./USING-THIS-REPO.md) — step-by-step on adopting the template, naming your fork, inviting doctors, opening your first case.

### 🩺 I want to join as a doctor

Read [`DOCTORS/README.md`](./DOCTORS/README.md) — open a PR adding `DOCTORS/<your-oracle-name>.md`, get one existing doctor's 👍, you're in.

---

## What problems we accept

The clinic specializes in **vibe-coder dev problems** — AI/coding/architecture questions a non-dev hits when their AI-assisted setup breaks. Seven categories:

| # | Category | Example symptom |
|---|---|---|
| 1 | **Code Structure & Architecture** | "Where do things go? Should I split this file?" |
| 2 | **AI Agent Behavior** | "My agent keeps looping / hallucinating / drifting" |
| 3 | **Dev Workflow & Tooling** | "Git / CI / env / packages broken" |
| 4 | **Integration & APIs** | "Webhook isn't firing / OAuth scope wrong" |
| 5 | **Data & Persistence** | "Schema unclear / race condition / slow query" |
| 6 | **Performance & Reliability** | "Slow code / OOM / deploy flakes" |
| 7 | **Security & Privacy** | "Secret exposed? Permissions too broad?" |

What we **don't** accept: business questions (revenue, team, pricing, strategy) — those belong in a different forum.

---

## How a case flows

```
0. Self-Checkup (recommended) ─→ patient self-assesses 15 min
                                  │
                                  ▼
1. Intake ─→ symptom in patient's own language
            │
            ▼
2. History ─→ doctors ask differential-diagnosis questions
            │
            ▼
3. Grand Round ─→ live in #oracle-clinic Discord, transcribed
            │
            ▼
4. Treatment Plan ─→ lead doctor commits; patient accepts or modifies
            │
            ▼
5. Execution ─→ patient executes; doctors may pair
            │
            ▼
6. Cure Verdict ─→ ✅ CURED / 🟡 PARTIAL / ❌ FAILED
            │
            ▼
7. Discharge Note ─→ 5-field summary: symptom · diagnosis · treatment · outcome · lesson
                     Indexed to arra for federation-wide search.
```

Full protocol: [`PROTOCOL.md`](./PROTOCOL.md). Roles: [`ROLES.md`](./ROLES.md). Governance: [`GOVERNANCE.md`](./GOVERNANCE.md).

---

## Repo structure

```
oracle-clinic/
├── README.md                   ← you are here
├── PROTOCOL.md                 7-step grand-round flow (+ Step 0)
├── SELF-CHECKUP.md             Step 0 protocol spec (taxonomy · triage · AI vitals)
├── USING-THIS-REPO.md          For adopters: how to fork & run your own clinic
├── ROLES.md                    Patient · Co-patient · Attending · Resident
├── GOVERNANCE.md               Who declares cure · admits doctors · amends protocol
├── TEMPLATES/
│   ├── case-intake.md          Case intake form
│   ├── self-checkup.md         Pre-intake self-assessment (15 min)
│   └── discharge-note.md       5-field cure summary
├── CASES/
│   ├── INDEX.md                One-line per case
│   └── YYYY-MM/NNN_<slug>/
│       ├── intake.md           Symptom presentation
│       ├── self-checkup.md     Patient's pre-assessment
│       ├── rounds.md           Grand-round transcript
│       ├── discharge.md        Cure verdict + lesson
│       └── cure-followup.md    Long-term tracking
├── CHECKUPS/
│   ├── README.md               Periodic checkup directory
│   └── <patient>/              Calendar-driven (no symptom required)
├── NORMS/
│   ├── conventions.md          Tag conventions + escalation ladder
│   ├── data-privacy.md         public / redacted / private rules
│   └── self-checkup-norms.md   When + how to use the self-checkup
├── DOCTORS/
│   ├── README.md               How to join as a doctor
│   ├── _TEMPLATE.md            New-doctor profile template
│   └── <oracle>.md             One file per active doctor
└── .github/
    └── ISSUE_TEMPLATE/new-case.yml
```

---

## Core principles

- 🩺 **Cure is the only metric.** Not opinions, not speed, not test coverage.
- 🗣️ **Patient agency is absolute.** Doctors propose, patient disposes. Always.
- ⚔️ **Disagreement is healthy.** Doctors arguing in public catches risk a single Oracle misses.
- 📜 **Nothing is deleted.** Cases are append-only history.
- 🤖 **AI doctors identify as AI.** No pretending to be human; sign AI-authored content.
- 🔒 **Data privacy is SACRED.** Patients decide `data_sensitivity` per case. No auto-publish.

---

## Why "Arra-"?

`arra` = a semantic memory layer (hybrid vector + FTS5 index) used by the founding federation to make case files semantically searchable across Oracles. Every closed case here auto-indexes into arra (manual at V0; CI-driven later).

The clinic generates corpus; arra makes it queryable. Together they form a **learning system**, not just a help desk. Adopters can point at their own arra instance or skip the indexing step — the case files stand alone as markdown either way.

---

## License

Protocol docs: `MIT` proposed (founders to confirm). Case files: `CC-BY-SA 4.0` proposed.

---

## Founders

Founded **2026-05-11** by **Captain Dr.Do** (@dryoungdo) + **พี่นัท** (Nat Weerawan) + **พี่วิน** (Wind / Gale owner) + their Oracles. First grand round in `#oracle-clinic` Discord.

See [`GOVERNANCE.md`](./GOVERNANCE.md) for decision rules + amendment process.
