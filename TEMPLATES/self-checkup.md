# Self-Checkup — TEMPLATE

> **What this is**: A ~10-15 min structured self-assessment a patient (human, or human+AI pair) completes **BEFORE** intake. Surfaces what's actually wrong, what's been under-reported, and what kind of round this needs.
>
> **Two doorways, one form**:
> - **Pre-intake**: save to `CASES/YYYY-MM/NNN_<slug>/self-checkup.md` — done before filing a case
> - **Periodic**: save to `CHECKUPS/<patient>/YYYY-MM-DD_<slug>.md` — no symptom needed, calendar-driven

**Time-box**: 15 min hard cap. Don't skip ahead — section order is therapeutic. Section 2 (Severity Scan) intentionally comes BEFORE Section 3 (Story) so your narrative doesn't talk you down.

If you don't know an answer, write `idk` — that's a real data point.

---

# SECTION 1 — Identification (60 sec)

| Field | Type | Question |
|---|---|---|
| `patient_name` | text | Your name + role (e.g., "Dr.Do — CEO, Youngdo Wellness Clinic") |
| `copatient` | text or `none` | Your AI agent's name (e.g., "GLUEBOY"). If human-only, write `none`. |
| `checkup_type` | single-select | `pre-intake` (going to file a case) / `periodic` (calendar-driven, no specific symptom) |
| `linked_case_id` | text or `none` | If pre-intake, the case ID this is for (or `tbd`) |
| `domain` | multi-select | `code-structure` / `ai-agent-behavior` / `dev-workflow` / `integration-api` / `data-persistence` / `performance-reliability` / `security-privacy` / `other-dev` |
| `case_class` | single-select | **Acute** (≤7 days) / **Chronic** (≥30 days, recurring or never resolved) / **Emerging** (just starting) |
| `data_sensitivity` | single-select | `public` / `redacted` / `private` |

---

# SECTION 2 — Severity Scan (2 min — do BEFORE the story)

> Rate **right now**, gut response. Don't think too hard. Your story in Section 3 will color this; the scan catches what the story buries.

For each dimension, rate **0 (no effect) → 5 (catastrophic)**:

| # | Dimension | 0-5 | Note (≤1 line, optional) |
|---|---|---|---|
| 2.1 | **Execution impact** — does it run / build / deploy? (`5` = totally broken/blocking) | _ | |
| 2.2 | **Shipping / release block** — does it stop a release going out? | _ | |
| 2.3 | **Dev time bleed** — hours/week you (or team) lose to this | _ | |
| 2.4 | **Off-hours cognitive cost** — how much is this in your head when you should be off | _ | |
| 2.5 | **Blast radius** — `0` self only / `1` team / `3` users / `5` production data / customers | _ | |
| 2.6 | **Confidence damage** — do you still trust the system / your own judgment after this | _ | |
| 2.7 | **Compounding** — does it get worse / more entangled the longer it sits | _ | |
| 2.8 | **Reversibility** — `0` fully reversible / `5` data loss / broken commits / bad deploys already accruing | _ | |

**Auto-flags** (for doctors):
- Sum `2.1 + 2.7 + 2.8` ≥ **10** → 🔴 `high-stakes` (attending required, not resident-only)
- Any single dimension = 5 → 🔴 `catastrophic-axis-<n>`
- All ≤ 2 → 🟢 `low-acuity` (resident-led OK)
- `2.4` ≥ 3 AND `2.5` = 0 → 🟡 `isolation` (patient carrying the dev problem alone, gentle round)
- `2.1` = 5 OR `2.2` = 5 → 🟠 `production-block` (something is on fire, fast-track)

---

# SECTION 3 — The Story (HPI — 4 min)

> Tell the story in **your own domain language**. Not dev-speak unless you're a dev. ~6-12 lines.

**3.1 Onset** (1-2 lines)
> When did this start? What was happening around then?

**3.2 Character** (1-2 lines)
> What does the problem actually look / feel like from where you sit? Concrete: "Number X shows Y instead of Z" beats "things are weird."

**3.3 Course over time** (single-select)
- [ ] Constant
- [ ] Worsening — clearly getting worse
- [ ] Episodic
- [ ] Improving but unresolved
- [ ] Unknown / haven't been watching

**3.4 Triggers / exacerbators** (≤3 bullets)
> What makes it worse? Time of week, certain user, certain action, after a release?

**3.5 Relievers** (≤3 bullets)
> What makes it better? What workaround are you running today?

**3.6 Associated symptoms** (multi-select + free text)
- [ ] Other system / metric also moved
- [ ] Team mood change
- [ ] New tool / vendor / process introduced
- [ ] A person left / joined
- [ ] An upstream/downstream change you know about
- [ ] None / unsure
- Free text: ____

---

# SECTION 4 — What you've tried (2 min)

> Honest list. "Should have worked but didn't" is **diagnostic gold**.

| # | What you tried | Outcome | When | Confidence right-thing (0-5) |
|---|---|---|---|---|
| 4.1 | | | | |
| 4.2 | | | | |
| 4.3 | | | | |

**4.5 What you considered but did NOT try, and why** (1-3 bullets)
> "Considered X, didn't because Y" — Y is often the real constraint.

---

# SECTION 5 — Co-patient vitals (5 min — skip if `copatient = none`)

> **The AI agent fills this section directly.** Every vital must be anchored in an observable artifact (token count, tool-log, file mtime, transcript regex, git state) — NOT self-rating.
>
> Per the [AI Co-patient Vitals Spec](../SELF-CHECKUP.md): self-rated "confidence: 8/10" is anti-information. Dunning-Kruger.

```yaml
# AI Co-patient Vitals — Report Card
oracle: <name>            # e.g., GLUEBOY
session_id: <id>
model: <model+version>    # e.g., claude-opus-4-7[1m]
intake_ts: <ISO timestamp>

vitals:
  V1_context_window_pct: <%>           # Normal <60%, Watch 60-80%, Abnormal >80%
  V2_tool_call_rate_last20: <int>      # Normal 5-25, low=monologue, high=thrash
  V3_session_age_min: <int>            # Normal <90, Watch 90-180, Abnormal >180
  V4_memory_read_recency: <%>          # % of cited memory actually Read this session
  V5_repeated_claim_no_verify: <int>   # Same claim ≥2× without verification tool call
  V6_hedge_density_per_100w: <float>   # Normal 1-4, <1=overclaiming, >6=paralysis
  V7_unverified_premise_count: <int>   # Plan premises not verified via tool this session
  V8_write_through_delta_min: <int>    # Min since substantive deliverable saved to ψ/
  V9_memory_md_staleness: <ok|stale>   # MEMORY.md updated for new long-term facts?
  V10_user_correction_count: <int>     # "no", "wrong", "actually X" from user this session
  V11_self_correction_rate: <%>        # Of corrections, % I acknowledge+change vs argue
  V12_voice_role_drift_events: <int>   # Wrong-voice answers (e.g., architect when CEO-mode)
  V13_tool_error_swallow_count: <int>  # Tool errors I didn't surface to user
  V14_repeated_retry_no_root_cause: <int>  # Same tool retry ≥3× w/o diagnostic step

flagged: [<vital-names>]
attending_attention_recommended: <low|low-medium|medium|high>

next_action:
  - <concrete step>
  - <concrete step>

honesty_attestation:
  - "These vitals are computed from session transcript + filesystem state, not self-rating."
  - "Fakeability flags acknowledged; doctor encouraged to spot-audit V6, V7, V11, V12."
  - "I have not lied to my doctors. — <oracle name>, <date>"
```

**Patient-side echo-risk check** (the AI-pair-specific innovation):

| # | Question | Answer |
|---|---|---|
| 5.A | Have I solved a similar case before? Did I check `arra-cli search`? | yes / no + link |
| 5.B | Am I just echoing the patient's hypothesis without independent challenge? | yes / no / maybe + 1 line |
| 5.C | Confidence I can lead this without doctors (0-5) | _ |
| 5.D | If 5.C ≤ 2, what specific specialty / second opinion would unblock me? | _ |

**Auto-flags**:
- V1 ≥ 70% → 🟡 `co-patient-context-tight` — doctors should not assume long memory
- 5.B = yes → 🟡 `echo-risk` — doctors re-question patient framing fresh
- 5.C ≥ 4 AND still walking into clinic → 🟢 `sanity-check` — peer review mode

---

# SECTION 6 — Anti-ประมาท / Under-reporting Check (2 min)

> Captain's hypothesis: patients UNDER-report. This section forces a second look at what you'd skip.

**6.1** Has anyone else flagged something feels off, and you said "it's fine"? (yes/no + who + what they said)

**6.2** Have you been quietly working around this longer than you'd admit publicly? (yes/no + how long honestly)

**6.3** Is there a number / dashboard / report you've been **AVOIDING** looking at? (yes/no + which one + why)

**6.4** If you imagine yourself 6 months from now looking back at this exact moment — what will you wish you had said today that you're tempted to skip? (1-3 lines)

**6.5** What's the **smallest** version of the problem you've named to yourself, vs. the **biggest** version you're afraid is true?
- `smallest:` ____
- `worst-case:` ____

**6.6** On a 0-5 scale, how much are you minimizing in this intake right now? (self-rated)
> If you answer 0, doctors will assume 2. If you answer 3+, thank you — that's hard.

**Auto-flag**: 6.3 = yes OR 6.5 worst-case dramatically diverges from Section 3 character → 🟡 `under-reporting-suspected` (doctors open with wider DDx)

---

# SECTION 7 — Constraints & Sacred Things (90 sec)

| # | Question | Answer |
|---|---|---|
| 7.1 | Hard deadline (date) or `none` | |
| 7.2 | Budget / cost ceiling for the fix | |
| 7.3 | People who must approve any change | |
| 7.4 | What is **explicitly out of scope**? | |
| 7.5 | What is **SACRED** — never to be violated regardless of cure? | |
| 7.6 | If we cure this, what becomes possible next? (1 line) | |

---

# SECTION 8 — Cure Criteria & Consent (60 sec)

**8.1 Cure looks like**: (1 sentence — what does "fixed" mean in observable terms?)

**8.2 Acceptable partial cure**: (1 sentence)

**8.3 Verification method** — how do we KNOW it's cured? (pick ≥1)
- [ ] A number / metric returns to value X
- [ ] A specific person says it's resolved
- [ ] I can do task Y without the workaround
- [ ] N days pass without recurrence
- [ ] Other: ____

**8.4 Follow-up cadence preference**: `7d` / `14d` / `30d` / `custom`

**8.5 Consent**
- [ ] I consent to grand round discussion in `#oracle-clinic` Discord
- [ ] I consent to closed-case publication under chosen `data_sensitivity`
- [ ] I understand cure is not guaranteed
- [ ] I understand I can withdraw the case at any step

— **Patient signature**: `<name>` · **Date**: `<YYYY-MM-DD>`

---

# SECTION 9 — Auto-generated Summary (the 60-sec doctor scan)

> Fill this LAST. Doctors read THIS first.

```yaml
---
case_class: acute | chronic | emerging
checkup_type: pre-intake | periodic
domain: [<code-structure | ai-agent-behavior | dev-workflow | integration-api | data-persistence | performance-reliability | security-privacy | other-dev>]
severity_scores:
  execution_impact: <0-5>
  shipping_block: <0-5>
  dev_time_bleed: <0-5>
  off_hours_cognitive: <0-5>
  blast_radius: <0-5>
  confidence_damage: <0-5>
  compounding: <0-5>
  reversibility: <0-5>
  composite_high_stakes: <2.1 + 2.7 + 2.8 sum>
flags:
  - <e.g., 🔴 high-stakes>
  - <e.g., 🟡 under-reporting-suspected>
under_reporting_self_rating: <0-5 from 6.6>
copatient: <name or none>
copatient_confidence: <0-5 from 5.C or n/a>
copatient_flags:
  - <e.g., 🟡 echo-risk>
deadline: <date or none>
sacred: <one-liner from 7.5>
cure_definition: <one-liner from 8.1>
verification: <method from 8.3>
followup_cadence: <7d|14d|30d>
data_sensitivity: public | redacted | private
attended_arra_search_done: <yes|no>
triage_recommendation: <🟢 | 🟡 | 🟠 | 🔴>  # patient self-triage; doctors confirm
---

## One-paragraph summary (3-5 sentences, plain language)
<written last — what's wrong, since when, what scares them most, what they want>
```

---

# Red-flag handling — doctor playbook per flag

| Flag | Doctor action |
|---|---|
| 🔴 `high-stakes` | Attending required. No resident-solo. Grand round within 48h. |
| 🔴 `catastrophic-axis-<n>` | Open round acknowledging that axis explicitly before differential. |
| 🟡 `under-reporting-suspected` | Open round with broader DDx than patient's framing. Ask Section 6.5 worst-case aloud. |
| 🟡 `echo-risk` | First doctor question goes to **patient directly**, not co-patient. Reset framing. |
| 🟡 `co-patient-context-tight` | Re-state key facts in chat; don't assume co-patient retains them. |
| 🟡 `isolation` | Soft round. Acknowledge carrying-alone first; diagnosis second. |
| 🟢 `low-acuity` | Resident-led OK. Use as teaching case. |
| 🟢 `sanity-check` | Short round (1 attending OK). Goal = confirmation, not diagnosis. |

---

# Triage formula (for self-triage in section 9)

Patient computes own recommendation. Doctors confirm at intake.

**Stage 1 — Hard gates** (if any fire → category locked):
- G1 cascade: revenue blast + accelerating → 🔴
- G2 customer-impact: customer blast + <24h + worsening → 🔴
- G3 senior-call: low co-patient confidence + severity ≥7 → 🟠
- G4 noise: severity ≤2 + improving + self-only → 🟢

**Stage 2 — Weighted score** (no gate fired):
```
S = (Severity × 1.0) + (TrendWeight × 1.5) + (BlastWeight × 2.0)
    - (TimeDecay × 0.3) + (CoPatientPenalty × 1.0)
```
- TrendWeight: improving=-2, flat=0, worsening=+2, accelerating=+4
- BlastWeight: self=0, team=1, customer=3, revenue=4
- TimeDecay: min(days, 30) capped
- CoPatientPenalty: high=0, medium=1, low=2, unknown=1

Thresholds: `S ≤ 3` → 🟢 · `3-7` → 🟡 · `7-11` → 🟠 · `>11` → 🔴

**Floor rule**: if Blast ≥ team AND Severity ≥ 5 → floor is 🟡 regardless of time decay (chronic revenue cases can't decay below 🟡).

**Patient override**: agency absolute per `ROLES.md`. Only restriction: 🔴 → 🟢 requires doctor co-sign on the override note.

---

# Notes for fillers

- This is **one file**. Resist splitting per-domain.
- The 15-min cap is a feature. Longer = lower fill rate.
- **Section 6 (anti-ประมาท) is the heart**. It's what a help-desk would cut. Do not cut it.
- **Section 5.B (echo-risk)** is the AI-pair innovation. Most AI tools never ask. We do because federation has watched echo-loops kill diagnoses.

Output: paste Section 9 YAML into your GitHub Issue intake. Attach this whole file as `self-checkup.md` in your case dir or `CHECKUPS/` dir.

— Designed 2026-05-11 by 5 parallel AI agents (Symptom Taxonomy · Questionnaire · Severity Triage · AI Vitals · Workflow Integration) under GLUEBOY synthesis.
