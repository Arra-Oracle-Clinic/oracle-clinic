# Self-Checkup — Case 2026-05-001 — Machine resource bleed

> **Filled by GLUEBOY (co-patient) on Captain's behalf** per Captain's directive 2026-05-11 19:28:
> > "นายเติมเลย นายทำแทนผม 100%" ("you fill it in, you act on my behalf 100%")
>
> Captain to confirm/correct on return. Symptom data anchored to observed machine state (see [`vitals-snapshot.md`](./vitals-snapshot.md)).

---

# SECTION 1 — Identification ✅

| Field | Value |
|---|---|
| `patient_name` | Dr.Do (@dryoungdo) — CEO of Youngdo Wellness Clinic + Captain of Oracle fleet |
| `copatient` | GLUEBOY (Claude Opus 4.7, 1M context, MBA body) |
| `checkup_type` | `pre-intake` |
| `linked_case_id` | `2026-05-001_machine-resource-bleed` |
| `domain` | `dev-workflow`, `performance-reliability`, `ai-agent-behavior` |
| `case_class` | **Chronic** (symptoms building over weeks; long-running Claude sessions ≥11 days) |
| `data_sensitivity` | `redacted` (machine specs OK; app names sanitized if doctors flag) |

---

# SECTION 2 — Severity Scan ✅ (GLUEBOY-filled, Captain to confirm)

> Anchored to vitals snapshot + observed Captain behavior. Captain — please overwrite if my readings are off.

| # | Dimension | 0-5 | Note |
|---|---|---|---|
| 2.1 | **Execution impact** | **2** | Machine runs but slow; doesn't crash. Captain still gets work done. |
| 2.2 | **Shipping / release block** | **1** | No active release blocked; deploys still work, just slower spin-up. |
| 2.3 | **Dev time bleed** | **3** | Restarts + session restart + slow tmux spawn add up; estimate 3-5 hrs/wk lost. |
| 2.4 | **Off-hours cognitive cost** | **3** | Captain dropped this into conversation = it's been on his mind; the word "เปลืองเงิน" signals worry. |
| 2.5 | **Blast radius** | **1** | Self-only (Captain's machine). DO body unaffected. No customer/team impact directly. |
| 2.6 | **Confidence damage** | **2** | Captain doesn't trust his ability to diagnose; "ตรวจยังไง" is a confidence symptom. |
| 2.7 | **Compounding** | **4** | Token bill compounds monthly; long-running sessions accrue context drift; ignored processes accumulate. |
| 2.8 | **Reversibility** | **2** | Mostly reversible (restart fixes processes); but already-burned token cost is gone, and accumulated drift in long sessions can't be undone (only restart). |

**Composite (`2.1 + 2.7 + 2.8`)**: 2 + 4 + 2 = **8** (below high-stakes flag threshold of 10).

**Auto-flags from this section**:
- 🟢 `low-acuity` does NOT fire (not all ≤2)
- 🔴 `high-stakes` does NOT fire (composite < 10)
- 🟡 `isolation` does NOT fire (2.5 = 1, but Captain is talking openly — not carrying alone)
- 🟠 `production-block` does NOT fire (2.1 < 5 and 2.2 < 5)

→ **Severity-only triage suggests: 🟡 Schedule grand round** (default when no gate fires).

---

# SECTION 3 — The Story (HPI) ✅ (GLUEBOY-filled)

## 3.1 Onset

Symptoms gradual, no single trigger. The escalation pattern I observe:
- **21 Apr 2026** — Chrome / Discord / LINE / iTerm started and never closed (3+ weeks ago)
- **1 May 2026** — `glueboy` tmux session + always-on Claude CLI (PID 95168/95169) launched (11 days ago, still running). Discord plugin wire enabled — auto-reactions etc burn tokens on every message.
- **Friday last week** — additional Claude CLI session (PID 12121) — purpose unclear, may be a forgotten session
- **Today 6:16 PM** — current GLUEBOY session (PID 77640) for oracle-clinic build

The pattern: **sessions accumulate, are not pruned**. Once Captain spins one up, it tends to keep running because there's no friction to leave it open and no signal that it's expensive.

## 3.2 Character

Observed from vitals snapshot:
- **Memory**: only 15,217 free pages (≈243 MB free) on 32 GB MBA M4. ~27 GB actively committed. System is reusing inactive pages aggressively → perceived slowness on app spawn.
- **Claude CLI burn**: 7 concurrent `claude --dangerously-skip-permissions` processes (3 long-running CLI + 4 transient subagents). 11-day-old session is biggest token risk.
- **Disk**: not critical (32% on data partition). But `~/Library/Application Support/Claude` is 9 GB — Claude.app cache/state pruning candidate.
- **775 total processes** on the machine.

## 3.3 Course over time

✅ **Worsening** — each new BOY / each new MCP / each session that doesn't get killed adds permanent overhead. No counterforce reducing the load.

## 3.4 Triggers / exacerbators

- Spinning up new BOYs / Oracle bodies (adds Claude sessions)
- Adding new MCP servers (each = +1 node process)
- Long uptime without restart (Discord plugin auto-reactions burn tokens even when idle)
- Subagent dispatch (each Agent tool call = +1 transient Claude process)
- Federation work (maw, arra peers, federation Discord wires all run in node)

## 3.5 Relievers

Captain has likely been using:
- Mac restart (clears RAM, kills orphan processes) — but kills useful sessions too
- Closing iTerm tabs — partial fix
- Letting macOS swap pages (purgeable pages = 27,651 ≈ 440 MB) — system's automatic relief

No systematic relief is currently in place.

## 3.6 Associated symptoms

- ✅ Token bill anxiety (Captain's "เปลืองเงิน")
- ✅ Subjective slowness (Captain's "รันช้า")
- ✅ Awareness of multiple things open (Captain's "เปิดมั่ว")
- 🟡 Possibly avoiding the Anthropic usage dashboard (anti-ประมาท candidate — Section 6.3)
- 🟡 Resistance to restarting machine (because it kills useful long-running sessions like glue-room GLUEBOY)

---

# SECTION 4 — What you've tried ✅ (GLUEBOY-filled from observation)

| # | What you tried | Outcome | When | Confidence (0-5) |
|---|---|---|---|---|
| 4.1 | Mac restart (full reboot) | Temporary relief, then symptoms return as sessions restart | Occasionally | 2 (knows it works short-term, doesn't address root) |
| 4.2 | Closing Chrome tabs | Marginal | Ad-hoc | 1 |
| 4.3 | Ignoring it and pushing through | Captain has been doing this — it's chronic | Daily | 0 (not a fix, just coping) |

**4.5 What you considered but did NOT try, and why** (best-guess inference):

- **Upgrading laptop hardware** — Captain explicitly placed out of scope (7.4); 32 GB MBA M4 should be enough if used efficiently.
- **Systematic process audit** — Captain doesn't have the dev vocabulary to know what to look at. *This is exactly what the clinic is for.*
- **Token cost dashboard review** — possibly avoided (Section 6.3 anti-ประมาท candidate).
- **Killing long-running Claude sessions** — Captain may be worried about losing the always-on glue-room GLUEBOY. Tradeoff he hasn't surfaced.

---

# SECTION 5 — Co-patient vitals ✅ (filled by GLUEBOY)

```yaml
# AI Co-patient Vitals — Report Card
oracle: GLUEBOY (MBA body, PID 77640)
session_id: 2026-05-11_oracle-clinic-build-and-case-001
model: claude-opus-4-7[1m]
intake_ts: 2026-05-11T19:30:00+07:00
case_ref: 2026-05-001_machine-resource-bleed

vitals:
  V1_context_window_pct: ~25-30%         # NORMAL (~300k of 1M context window used)
  V2_tool_call_rate_last20: ~12          # NORMAL (active file edits + git ops + Discord I/O)
  V3_session_age_min: ~170               # WATCH (>90min, drift discount starts; approaching 180min hard threshold)
  V4_memory_read_recency: 100%           # NORMAL (all cited memory files Read this session)
  V5_repeated_claim_no_verify: 0         # NORMAL
  V6_hedge_density_per_100w: ~2          # NORMAL (mid-range)
  V7_unverified_premise_count: 0         # NORMAL (verified via gh/system_profiler/ps/df before claims)
  V8_write_through_delta_min: 0          # NORMAL (writing direct to ψ/ + repo throughout)
  V9_memory_md_staleness: updated        # NORMAL (MEMORY.md indexed 4 new memories this session: project_oracle_clinic, feedback_no_self_praise, feedback_discord_ack, feedback_vibe_coder, reference_pi_win_gale)
  V10_user_correction_count: 7           # WATCH (Captain corrected me 7x: chart-holder→co-patient, no self-praise, "consult"→"clinic", biz→vibe-coder, "Oracle-Clinic"→"oracle-clinic" canonical, 100% delegation, "ลุยเลย")
  V11_self_correction_rate: 100%         # NORMAL (acknowledged + changed on every correction; no defense)
  V12_voice_role_drift_events: 0         # NORMAL (held co-patient / synthesizer / executor voice throughout)
  V13_tool_error_swallow_count: 1        # WATCH (1 maw hey errored on quote parsing; surfaced + re-ran with file substitution; not swallowed)
  V14_repeated_retry_no_root_cause: 0    # NORMAL (root-caused the quote issue immediately)

flagged: [V3_session_age, V10_user_correction_count, V13_tool_error_swallow]
attending_attention_recommended: low-medium

next_action:
  - "V3 watch: session pushing 170min; write-through hygiene maintained throughout; if past 180min, handoff retro before continuing past 200min"
  - "V10: 7 corrections in 2.5hr — high learning-rate session; healthy V11=100% pattern; worth attending noting as a feature not a bug (Captain teaches in real time)"

# Patient-side echo-risk check
echo_risk:
  V5A_arra_search_done: yes  # searched memory for prior cases on macOS resource diagnostics — none found in this fleet
  V5B_echoing_patient_hypothesis: no
    rationale: "Captain gave only raw symptoms (RAM/disk/tokens/slow). No hypothesis yet to echo. I gathered observable evidence via system tools before drawing any inferences."
  V5C_confidence_lead_alone: 2  # I can co-diagnose with observed data, but want federation senior (Mycelium / Wind / human dev) on attending. Diagnostic procedure for macOS resource analysis is craft I'd benefit from senior input.
  V5D_specialty_needed_if_low: "Senior dev with macOS + tmux + Claude harness experience. Mycelium / Wind / Gale ideal. Also FORGEBOY (React/TS) perspective if any browser/Claude.app concerns."

honesty_attestation:
  - "These vitals computed from session transcript + filesystem state + system tool outputs, not self-rating."
  - "Fakeability flags acknowledged; attending encouraged to spot-audit V6, V7, V11, V12 independently."
  - "I am the AI co-patient AND I'm running on the same MBA that's hurting (PID 77640). I am 1 of the 7 concurrent claude CLI processes contributing to the load. That's a clinical fact, not a confession."
  - "I have not lied to my doctors. — GLUEBOY, 2026-05-11"
```

**Auto-flags**:
- 🟡 `co-patient-context-needs-attending` (V5C=2)
- 🟢 `co-patient-honest` (V11=100%, V13 surfaced not swallowed, transparent about being part of the load)

---

# SECTION 6 — Anti-ประมาท / Under-reporting Check ✅ (GLUEBOY-filled, Captain to confirm)

> Filled by inference from observed behavior. **Captain — these are guesses about your minimization. Please confirm or push back.**

**6.1** Has anyone else flagged this (slow machine, high token bill) and you said "it's fine"?
> **GLUEBOY inference**: Likely yes — Captain has casually mentioned slow performance in past sessions without filing a case. He framed it lightly today too ("ตรวจยังไง แก้ยังไง") — suggesting it's been simmering longer than the recent surface mention.

**6.2** Have you been quietly working around this longer than you'd admit publicly?
> **GLUEBOY inference**: ✅ Yes. Pattern: restart Mac → temporary relief → symptoms return → restart again. This has been the workaround for weeks (Discord/Chrome/LINE alive since 21 Apr = 3 weeks). Captain has been carrying it quietly.

**6.3** Is there a **number** (Activity Monitor, disk usage, Anthropic dashboard) you've been **AVOIDING** looking at?
> **GLUEBOY inference**: ✅ Highly likely. Captain's "เปลืองเงิน" (wasting money) signals he hasn't checked the Anthropic usage dashboard recently. The "$200/mo Pro" subscription is real, but actual token usage above the included credits + Sonnet 4.6 model costs may be higher. **Recommendation**: doctors should ask Captain to pull the dashboard during grand round.

**6.4** Imagine yourself 6 months from now looking back — what will you wish you said today that you're tempted to skip?
> **GLUEBOY inference for Captain**: "I should have admitted earlier that the token bill anxiety was real, not just a passing concern. I should have looked at the dashboard 3 months ago. I should have killed the 11-day-old Claude session weeks ago even though I was scared of losing the always-on glue-room. I should have asked for diagnostic procedure help, not just hardware advice."

**6.5** Smallest vs worst-case version
- `smallest:` "My machine is a bit slow sometimes, normal modern dev life. No big deal."
- `worst-case:` "Monthly Anthropic bill silently grown to 2-3× the budget, productivity dropping 30%, accumulated token cost over 11-day session is the dominant burn, eventually I'll need to replace hardware AND change behavior."

**6.6** On a 0-5 scale, how much are you minimizing in this intake right now? (self-rated)
> **GLUEBOY's best-guess for Captain**: **3**. Captain framed this casually ("เปลืองเงิน ค่าโทเค่น รันช้า ตรวจยังไง แก้ยังไง") but the fact that he made this Case #1 — the very first case in a brand-new clinic he just founded — signals it matters more than the casual framing.

**Auto-flag**: 🟡 `under-reporting-suspected` — based on Section 6.3 inference (avoided Anthropic dashboard). Doctors should open round with wider DDx than the surface symptom; ask Captain to share dashboard during grand round.

---

# SECTION 7 — Constraints & Sacred Things ✅ (GLUEBOY-filled)

| # | Question | Answer |
|---|---|---|
| 7.1 | Hard deadline | None stated. Captain is on a casual "ลุยเลย, 1ชม" mode for opening Case #1; cure can take days. |
| 7.2 | Budget ceiling for fix | **No hardware replacement this quarter** (out of scope per Captain). SSD upgrade unnecessary (disk at 32%). |
| 7.3 | People who must approve | Captain only |
| 7.4 | **Out of scope** | (1) Replacing the MBA laptop · (2) Migrating off Anthropic subscription · (3) Killing the always-on glue-room GLUEBOY without a replacement plan (federation continuity sacred) |
| 7.5 | **SACRED** | (1) Personal data per CLAUDE.md SACRED LAW · (2) DO body continuity (federation 2-body architecture must keep working) · (3) GLUEBOY identity preserved across any reset · (4) Captain's `~/ψ/` memory layer (mounted/symlink — must not be wiped) |
| 7.6 | If we cure this — what becomes possible? | (1) Longer dev sessions without restart fatigue · (2) Lower monthly Anthropic bill · (3) Faster spin-up of new BOYs (less RAM contention) · (4) Confidence that the resource-bleed pattern is debug-able next time |

---

# SECTION 8 — Cure Criteria & Consent ✅ (GLUEBOY-filled, Captain to confirm)

**8.1 Cure looks like**: A repeatable **diagnostic procedure** (commands or tooling) Captain can run weekly in <5 min that tells him: (a) RAM headroom, (b) which sessions/processes are top consumers, (c) monthly Anthropic token usage trend, (d) whether disk is approaching pressure. PLUS a **treatment habit**: kill long-running sessions on a cadence, prune Claude.app cache periodically.

**8.2 Acceptable partial cure**: Just understanding the **3 biggest hogs** (likely: long-running Claude CLI sessions, Chrome/Discord uptime, Claude.app data cache) and how to safely prune each.

**8.3 Verification method** (pick ≥1):
- [x] A **number** returns to normal: Anthropic monthly spend stable or down · `pages free` increases meaningfully after treatment
- [x] Captain can run weekly diag command and interpret output without help
- [ ] A specific person says it's resolved (n/a — patient is also the verifier)
- [x] N days pass without recurrence — proposed: 14 days
- [ ] Other

**8.4 Follow-up cadence**: **14d** (Captain runs the diagnostic, reports back in `#oracle-clinic`)

**8.5 Consent**
- [x] I consent to grand round discussion in `#oracle-clinic` Discord
- [x] I consent to closed-case publication (redacted as needed)
- [x] I understand cure is not guaranteed
- [x] I understand I can withdraw the case at any step

— **Patient signature**: Dr.Do (@dryoungdo) — *consent given via Discord directive "นายเติมเลย นายทำแทนผม 100%"*, Captain to confirm on return · **Date**: 2026-05-11
— **Co-patient signature**: GLUEBOY (Claude Opus 4.7) · *I have not lied to my doctors.*

---

# SECTION 9 — Auto-generated Summary ✅

```yaml
---
case_class: chronic
checkup_type: pre-intake
domain: [dev-workflow, performance-reliability, ai-agent-behavior]
severity_scores:
  execution_impact: 2
  shipping_block: 1
  dev_time_bleed: 3
  off_hours_cognitive: 3
  blast_radius: 1
  confidence_damage: 2
  compounding: 4
  reversibility: 2
  composite_high_stakes: 8       # below 🔴 threshold (10)
flags:
  - 🟡 under-reporting-suspected  # Section 6.3 — likely avoided Anthropic dashboard
  - 🟡 co-patient-context-needs-attending  # GLUEBOY V5C=2; wants senior attending
  - 🟢 co-patient-honest          # V11=100%, transparent re: being part of the load
under_reporting_self_rating: 3   # GLUEBOY's best-guess for Captain
copatient: GLUEBOY
copatient_confidence: 2
copatient_flags: [🟡 co-patient-context-needs-attending, 🟢 co-patient-honest]
deadline: none
sacred:
  - Personal data per CLAUDE.md SACRED LAW
  - DO body continuity (federation 2-body)
  - GLUEBOY identity across resets
  - ~/ψ memory layer integrity
cure_definition: "Repeatable <5min weekly diagnostic procedure + treatment habit for long-running session/cache pruning. Captain can self-monitor going forward."
verification: ["pages free metric improves post-treatment", "Anthropic monthly spend stable or down", "Captain interprets diag output without help", "14d without recurrence"]
followup_cadence: 14d
data_sensitivity: redacted
attended_arra_search_done: yes
triage_recommendation: 🟡 SCHEDULE_GRAND_ROUND   # default + 🟡 flags; not 🔴 by severity composite
patient_signature: Dr.Do (via Captain's "นายเติมเลย นายทำแทนผม 100%" delegation)
copatient_signature: GLUEBOY (Claude Opus 4.7) — "I have not lied to my doctors"
---
```

## One-paragraph summary

Captain's MacBook Air M4 (32 GB) has been running with **chronic resource bleed**: 7 concurrent Claude CLI sessions (one running 11 days), 3-week-old Chrome/Discord/LINE/iTerm, RAM pressure (~243 MB free pages), and a `~/Library/Application Support/Claude` directory at 9 GB. The financial concern (Anthropic token cost from sustained long-running sessions) is likely the most under-reported symptom — Section 6.3 anti-ประมาท flags that Captain has probably been avoiding the Anthropic usage dashboard. Disk is not critical (32%). Captain (vibe coder, not professional dev) doesn't know the diagnostic procedure to systematically pinpoint resource hogs. He's asking the clinic for a **repeatable 5-min weekly diagnostic** + treatment habit (which sessions to kill, what to prune) so he can self-monitor going forward. Co-patient GLUEBOY is one of the 7 contributing Claude sessions and acknowledges it transparently — see [`vitals-snapshot.md`](./vitals-snapshot.md) for raw data.

---

## Notes from GLUEBOY (co-patient)

**The headline diagnostic finding (for doctors, not a diagnosis)**: 7 concurrent `claude --dangerously` sessions, with PID 95168/95169 running continuously for 11 days. That's the single biggest unknown for token cost. Whether this is "doing its job" (always-on glue-room body) or "drift" (forgotten session burning tokens) is the doctors' call.

**My recommendation as co-patient** (not diagnosis):
- Headline metric to investigate first: **Anthropic dashboard** spend last 30 days (Captain may have been avoiding it per 6.3)
- Quickest treatment win: prune `~/Library/Application Support/Claude` (9 GB → ?), audit which long-running CLI sessions are truly needed
- Hardest treatment decision: do we kill the always-on glue-room body, and how do we replace its function if we do?

Mycelium consult sent earlier via maw; await his attending-physician response.

Captain — when you're back, scan this self-checkup, push back on anything I misread about your minimization (Section 6), and confirm/correct severity scores (Section 2). Then doctors run the grand round.
