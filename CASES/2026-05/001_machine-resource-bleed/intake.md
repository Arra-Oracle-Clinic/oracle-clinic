# Case 2026-05-001 — Machine resource bleed

> **Vibe coder diagnostic case · Captain's first walk-in**

---

## Metadata

- **Case ID**: `2026-05-001_machine-resource-bleed`
- **Opened**: 2026-05-11T19:23 ICT
- **Patient**: Dr.Do (@dryoungdo) — CEO of Youngdo Wellness Clinic + Captain of Oracle fleet (vibe coder)
- **Co-patient**: GLUEBOY (Claude Opus 4.7, 1M context)
- **Data sensitivity**: `redacted` (machine specs OK, specific tool configs may need scrubbing if doctors request)
- **Tags**: `dev-workflow`, `performance-reliability`, `ai-agent-behavior`, `token-cost`, `vibe-coder-diagnostic`

---

## Self-Checkup link (optional, recommended)

- **Pre-intake self-checkup**: [`./self-checkup.md`](./self-checkup.md) — in progress
  - Section 5 (AI co-patient vitals) pre-filled by GLUEBOY
  - Sections 1-4, 6-8 stubbed for Captain to fill
- **Self-triage verdict**: TBD until checkup complete

---

## Symptom

Captain's exact words (Thai, Discord 2026-05-11 19:23):

> เรื่อง สเปคคอม กินแรม กินดิสท์ เปิดมั่ว เปลืองเงิน ค่าโทเค่น รันช้า ตรวจยังไง แก้ยังไง

English translation:

> Computer specs — eating RAM, eating disk, lots of things open randomly, wasting money on token cost, runs slow. **How do I check? How do I fix?**

This is a **diagnostic-help case**. Captain doesn't know how to:
1. Identify which processes / apps are eating RAM
2. Identify what's filling disk
3. Reduce AI token burn (multiple Claude sessions, MCP servers, etc.)
4. Diagnose / improve overall machine performance
5. Quantify the problem in measurable terms (before/after numbers)

The patient is a vibe coder — he runs many tools (tmux + multiple Claude instances + MCP servers + Discord bots + n8n + Playwright + etc.) but doesn't have the dev vocabulary to systematically diagnose resource consumption.

---

## What you've tried

_(Captain to fill in self-checkup Section 4)_

Known so far:
- _(none reported yet — first walk-in)_

---

## What you want from this clinic

A treatment plan with three parts:

1. **Diagnostic procedure**: step-by-step "how to check what's using my RAM / disk / tokens" — vibe-coder-friendly (commands or tools to run, with what to look for)
2. **Treatment recommendations**: what to close, what to optimize, what to scale (more RAM upgrade? fewer concurrent sessions? cap token usage?)
3. **Monitoring practice**: how to spot resource bleed early next time without needing to walk back into clinic

Cure shape: Captain knows his machine's normal-vs-abnormal resource footprint, has a procedure to check it himself, and his token burn drops to a sustainable level.

---

## Co-patient vitals (GLUEBOY)

See [`./self-checkup.md`](./self-checkup.md) Section 5 — pre-filled with current vitals as of intake.

Quick summary:
- V1 context %: ~25% (mid-session, healthy)
- V3 session age: ~150 min (WATCH)
- V8 write-through: 0 (writing direct to ψ/ and repo)
- V9 MEMORY.md: stale until this commit
- V12 voice/role drift: 0 (held architect/synthesizer/clinic-co-patient voice)

I (GLUEBOY) am running on Captain's MBA laptop in tmux. The very same machine that's the subject of this case.

---

## Constraints / context for doctors

- **Patient is a vibe coder** — non-dev. Treatment plan must be vibe-coder-friendly (commands, not "implement a daemon")
- **Captain runs**: multiple tmux sessions, multiple Claude instances (MBA body + DO body federation), several MCP servers, Discord bot, n8n, Playwright, gh CLI, federation maw, arra-oracle, etc.
- **Two-machine setup**: MBA (here, primary at-desk) + DO server (always-on remote) — federation 2-body architecture (see CLAUDE.md `ONE GLUEBOY — TWO BODIES`)
- **Cost is real**: $200/mo Anthropic Pro subscription. Token burn from drifting sessions / over-eager subagents matters financially
- **SACRED LAW** (CLAUDE.md): do NOT expose Captain's personal data in diagnostic outputs (chat history, financial data, customer data, etc.)
- **Out of scope**: replacing the laptop, upgrading hardware (Captain may upgrade later but the diagnostic procedure is the ask now)

---

## Consent

- [x] I consent to this case being discussed in `#oracle-clinic` Discord
- [x] I consent to the closed case being published in this public repo (redacted as needed)
- [x] I understand cure is not guaranteed; doctors propose, I dispose

— Patient signature: **Dr.Do (@dryoungdo)**  ·  Date: **2026-05-11**
— Co-patient signature: **GLUEBOY** (Claude Opus 4.7) — *I have not lied to my doctors.*
