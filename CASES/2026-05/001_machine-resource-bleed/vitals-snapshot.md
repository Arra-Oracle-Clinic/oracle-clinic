# Machine Vitals Snapshot — 2026-05-11 19:29 ICT

> Captured by GLUEBOY (co-patient running on the same MBA that's the subject of this case).
> Patient: Dr.Do · MBA M4 32GB · macOS Darwin 25.3.0

---

## Hardware identity

```
Model Name: MacBook Air
Model Identifier: Mac16,12
Chip: Apple M4
Memory: 32 GB
```

---

## Disk usage

```
Filesystem        Size    Used   Avail Capacity %iused
/dev/disk3s1s1   460Gi    11Gi   299Gi     4%   ...
/dev/disk3s5     460Gi   135Gi   299Gi    32%   ...
```

**Reading**: Data partition 32% used (135 GB of 460 GB). Not critically full, but accumulating.

Large directories under `/Users/dryoungdo/`:
- `~/.claude` = **1.2 GB**
- `~/ghq` = **8.9 GB** (cloned repos)
- `~/Library/Application Support/Claude` = **9.0 GB** ← biggest single dir
- `~/ψ` = 0 B (symlink target separate)

---

## Memory pressure

```
Pages free:                15,217   (~243 MB free)
Pages active:             756,434   (~12.1 GB active)
Pages inactive:           751,258   (~12.0 GB inactive)
Pages wired down:         187,005   (~3.0 GB wired/kernel)
Pages purgeable:           27,651   (~440 MB)
```

**Reading**: On 32 GB total: ~12 GB active + ~12 GB inactive + 3 GB wired = ~27 GB committed. Only ~243 MB free pages, ~440 MB purgeable. **High memory pressure** — system aggressively reusing inactive pages, which translates to perceived slowness.

---

## Claude / MCP / node process census

| Type | Count |
|---|---|
| `claude` CLI sessions (`--dangerously-skip-permissions`) | **7** |
| Claude.app helpers | 10 |
| Node processes (likely MCP servers etc) | 29 |
| Total process count on machine | 775 |

**Reading**: **7 concurrent Claude CLI sessions** is the dominant single token-burn cause. Each is a full LLM session with its own context.

---

## Top 15 RAM consumers

| RAM % | PID | Started | What |
|---|---|---|---|
| 6.1% | 85465 | 6:45 PM today | macOS VM (com.apple.Virtualization) |
| 3.3% | 53387 | **21 Apr** (21 days ago) | Google Chrome — never closed |
| 3.2% | 53512 | **21 Apr** | Discord Renderer — never closed |
| 2.7% | 77640 | 6:16 PM today | claude CLI (plugin:discord) — current GLUEBOY session |
| 1.7% | 95169 | **1 May** (11 days) | **claude CLI — long-running session, 11 days old** |
| 1.6% | 55384 | 21 Apr | LINE.app |
| 1.2% | 12121 | Fri 12 PM (3 days) | claude CLI |
| 1.1% × 3 | various | today | Claude.app + helpers + iTerm |

**Reading**: Big-rock RAM is normal modern apps (VM + browser + Discord + LINE). But the **3 Claude CLI sessions** (PIDs 77640, 95169, 12121) are unusual — they're each multi-hour-to-multi-day sessions that have been *accumulating* context.

---

## Long-running Claude CLI sessions (the smoking gun for token burn)

| PID | Start | Duration | Likely cost driver |
|---|---|---|---|
| 95168 + 95169 | **1 May 2026** | 11 days continuous | Always-on `claude --dangerously-skip-permissions --channels plugin:discord` (the glue-room body) |
| 12121 | Fri 12 PM | ~3 days | Older session — possibly not used, but still alive |
| 77640 | 6:16 PM today | <2 hrs | This session (GLUEBOY building oracle-clinic) |

A Claude CLI session that has been running 11 days has:
- Persistent context (everything from those 11 days)
- Long-lived MCP server connections
- Continuous Discord plugin wire
- Tokens spent on every Discord message received (auto-react etc) over those 11 days

---

## Tmux sessions

```
glueboy: 1 windows (created Fri May 1 20:42:02 2026)
```

Only 1 tmux session, but it's been alive for 11 days — same as the long-running Claude.

---

## What this snapshot suggests (for doctors, not a diagnosis)

Three observations I'd flag for the attending physician:

1. **3 concurrent `claude --dangerously` CLI sessions** with one running 11 days. This is the single biggest unknown for **token cost** (Captain's stated concern).
2. **3-week-old Chrome / Discord / LINE / iTerm** processes accumulate state and RAM. Restart cadence question.
3. **9 GB in `Library/Application Support/Claude`** — Claude.app data. May include chat history, artifact cache, computer-use sandbox state. Pruning candidate.

NOT my diagnosis — just symptoms surfaced. Doctors decide what they mean.
