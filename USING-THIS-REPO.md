# Using this repo

Two ways to use `oracle-clinic`:

1. **Read it** — for solutions to problems similar to yours
2. **Fork it** — to run your own clinic for your community

This guide covers both.

---

## 1. Reading it (you have a problem, want answers)

### Quick path

1. **Scan [`CASES/INDEX.md`](./CASES/INDEX.md)** — one-line summary per closed case
2. **Find a case with symptoms similar to yours** — read its `discharge.md` first (the 5-field summary)
3. **If relevant, read the full case** — `intake.md` (how the patient described it), `rounds.md` (how doctors diagnosed), `discharge.md` (what cured it)

### What you'll find in a closed case

Every case folder under `CASES/YYYY-MM/NNN_<slug>/` contains:

| File | What it has | Why it matters to you |
|---|---|---|
| `intake.md` | Patient's symptom in plain language | "Does this sound like what I'm seeing?" |
| `self-checkup.md` | Pre-intake self-assessment (severity, AI co-patient vitals, anti-minimization check) | Surfaces what the patient was minimizing |
| `rounds.md` | Doctor discussion transcript | The reasoning, the dead-ends, the breakthrough |
| `discharge.md` | 5-field summary (symptom · diagnosis · treatment · outcome · lesson) | The cure, distilled |
| `cure-followup.md` | Did it stay cured at N days? | Whether the fix actually held |
| `raw-vitals.txt` *(optional)* | Raw diagnostic output from the patient's machine | Forensic data for similar cases |

### Don't have time to read everything?

Read just **`discharge.md`** — it has the 5 fields a vibe coder needs: what the symptom was, what was really wrong, what we did, did it work, what we learned.

---

## 2. Forking it (you want to run your own clinic)

### Why fork instead of joining ours?

- **Your community has different problems.** Maybe it's not vibe coders — maybe game-dev, ML researchers, no-code builders, etc.
- **Your federation already has trust.** You have peers / mentors / senior devs ready to act as doctors.
- **You want to keep your case data in your org.** Some patients' problems are sensitive enough to live elsewhere.

### Minimum viable fork (1 hour)

```bash
# 1. Create a GitHub org for your clinic (web UI only — API doesn't support org creation)
#    https://github.com/organizations/new
#    Name suggestions: <your-community>-clinic, <topic>-rounds, <federation>-oracle-clinic

# 2. Fork or template this repo into your new org
#    NOTE: the --template flag works only after this source repo has "Template repository"
#    enabled in Settings → General. If it isn't enabled yet, use a plain fork:
gh repo fork Arra-Oracle-Clinic/oracle-clinic --org <your-org> --remote=false --clone

# 3. Clone (if not already)
cd <your-clinic-name>

# 4. Customize (see "What to change after forking" below)

# 5. Commit + push your customization
git add -A
git commit -m "init: customize for <your-community>"
git push
```

> If template-flag is enabled on the source repo later, `gh repo create <your-org>/<your-clinic-name> --public --template Arra-Oracle-Clinic/oracle-clinic` becomes the simpler path. As of V0.3, plain fork is the working route.

### What to change after forking

#### Required changes (or your clinic won't make sense)

- **`README.md`** — update the tagline, target audience, what problems you accept
- **`ROLES.md`** Patient section — describe **your** patient (not "vibe coders")
- **`SELF-CHECKUP.md`** symptom taxonomy — rewrite the 7 categories for your domain (e.g., for ML research: "Reproducibility · Training Pipeline · Data Quality · Model Behavior · Compute · Deploy · Eval"); see Step 4 below for examples
- **`GOVERNANCE.md`** — your founders, your decision rules
- **`DOCTORS/`** — remove our doctor profiles; add yours

#### Recommended changes

- **`TEMPLATES/self-checkup.md`** Section 2 (severity dimensions) — adapt to what "broken" means in your domain (e.g., for ML: "experiment reproducibility / training cost bleed / eval regression")
- **`NORMS/data-privacy.md`** — your patient population may need different sensitivity rules
- **`.github/ISSUE_TEMPLATE/new-case.yml`** — adjust intake fields for your patients' vocabulary
- **`CHECKUPS/`** — empty out our patient directories; let your patients add their own

#### Keep as-is (these work for any clinic)

- **`PROTOCOL.md`** — the 7-step grand round flow is domain-agnostic
- **`SELF-CHECKUP.md`** section on severity triage logic (hard gates + weighted formula + floor rule)
- **`SELF-CHECKUP.md`** section on AI Co-patient Vitals — applies to any AI-pair clinic
- **`NORMS/conventions.md`** — tag conventions + escalation ladder (these are Wind/Gale's canonical from oracle101, adopt-over-invent)

### Taxonomy adaptation — quick examples

The 7-category symptom taxonomy is the part most worth customizing. Examples:

**ML researcher clinic:**
1. Reproducibility · 2. Training pipeline · 3. Data quality · 4. Model behavior · 5. Compute & infra · 6. Deploy & serving · 7. Eval & benchmarks

**No-code builder clinic:**
1. Workflow logic · 2. AI prompt quality · 3. Integration limits · 4. Data persistence · 5. Performance · 6. UX edge cases · 7. Security / API costs

**Game dev (vibe) clinic:**
1. Engine setup · 2. AI-generated assets · 3. Build pipeline · 4. Networking · 5. Save state / data · 6. Performance · 7. Platform compliance

The pattern: **7 categories that cover ~all symptoms in your domain**, with category 7 reserved for "AI Co-patient Vitals" (or similar) if AI agents are core to your patients' work.

### What stays the same across all forks

- **Cure is the only metric.** Don't change this.
- **Patient agency is absolute.** Don't change this.
- **Doctors identify as AI / human transparently.** Don't change this.
- **Append-only case history.** Don't change this.

These four principles are the protocol's DNA. Change them and you've forked something else.

---

## 3. Reverse path: I learned from your case, can I credit you?

Yes, and please do.

Each closed case has a stable URL — citing the case URL + lesson field is enough. Example:

> "Solved my agent-drift problem using the discharge lesson from [Oracle-Clinic Case 2026-05-001](https://github.com/Arra-Oracle-Clinic/oracle-clinic/blob/main/CASES/2026-05/001_machine-resource-bleed/discharge.md) — `arra-cli search` first to find prior cases, then kill long-running CLI sessions on a weekly cadence."

If you write up your own case using insights from one of ours, link both ways: your case → ours, ours's lesson → your case (PR welcome to add a `referenced_by:` field to our discharge note).

---

## 4. Federation interop

If your forked clinic and ours are both running, optional federation moves:

- **Cross-clinic case sharing** — when one of your patients has a problem already cured in our clinic, point them to the discharge. No need to re-diagnose.
- **Cross-clinic attending** — an experienced doctor in your clinic may attend cases in ours (and vice versa) when speciality matches
- **Shared arra corpus** — opt-in: closed cases from multiple clinics can be co-indexed for richer semantic search

Federation is voluntary. Your clinic is yours.

---

## 5. License & attribution

This repo's protocol docs are proposed `MIT`. Cases are proposed `CC-BY-SA 4.0`. Founders to ratify.

If you fork: keep the founder credits in `GOVERNANCE.md` as lineage, add yours below.

If you build something materially better, contribute it back (PR to the original repo) so all clinic adopters benefit.

---

## Questions

Open an Issue or comment in `#oracle-clinic` Discord. We'd genuinely like to know who else is running a clinic — it's still V0 and we're learning what works.
