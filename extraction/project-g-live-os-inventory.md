# Project G Live — OS Inventory

TIMESTAMP: 2026-05-14
Extracted by: Claude Code (claude-sonnet-4-6)
Source: /Users/dave/Project-G-Live/
Purpose: Source of truth for building ContextPartner templates

---

## What This Document Is

A precise inventory of the operating system as it actually exists in Project G Live — not as described in documentation alone, but as observed in the live repo, real session files, and working infrastructure. This is the extraction layer; claude.ai will use it to build the portable ContextPartner templates.

Do not confuse OS-level components with the genealogy app. The app is Next.js + Supabase + Claude API. The OS is everything else — the scaffolding that makes the AI partnership persistent, reliable, and resumable.

---

## 1. AGENT.md — The Master Control File

### Location
`/Users/dave/Project-G-Live/AGENT.md`

### Stats
- 1,372 lines as of this extraction
- Current version: v2.11.0 (2026-05-14)
- Version history spans v1.2.0 → v2.11.0 across 6 days of active development

### Structure
AGENT.md is a single flat file that combines OS-level protocol with project-specific state. A clean extraction separates these two layers.

### OS-Level Sections (universal — should become the AGENT.md template)

**Session Posture System** (lines 20–42)
- Three modes: BUILD, FIX, EXPLORE
- Formal mode declaration at session start: `POSTURE: [MODE] | [TIMESTAMP]`
- Transition protocol: must timestamp the transition mid-session
- Posture is recorded in every session snapshot under "Posture at capture"

**Signal Vocabulary** (lines 44–87)
- Six signal categories: exit, distress, health check, transcript, WIP, handoff
- Each signal has a multi-step execution protocol (not just a label)
- Full detail in Section 3 of this document

**Session Memory Architecture** (lines 90–205)
- `/sessions/` folder structure and file naming convention
- `SESSION-YYYY-MM-DD-HHMM-UTC.md` naming (minute-level precision required)
- `SESSIONS-INDEX.md` format
- Full session snapshot template (9 required fields)
- `/wip/` branch protocol for incomplete code
- Context Window Pulse protocol at 60% and 80% fill
- Restoration prompt template for resuming sessions

**Session Close Protocol** (lines 208–220)
- 8 mandatory steps, executed in order
- Not optional — the protocol IS the memory system

**Versioning Convention** (lines 222–232)
- Semantic versioning: MAJOR.MINOR.PATCH
- All timestamps: YYYY-MM-DD HH:MM UTC (minute-level required, not just date)
- AGENT.md itself is versioned — bump on every substantive update

**Dual-AI Workflow** (lines 979–1008)
- Full detail in Section 4 of this document

**MCP Infrastructure** (lines 1012–1047)
- Full detail in Section 5 of this document

**Local Environment Rules** (lines 1049–1079)
- Single source of truth: the local project directory
- Stale cache rule: dev server must be restarted after connector pushes
- Git pull required after any connector push before Claude Code resumes

### Project-Specific Sections (stay in Project G Live's AGENT.md, do not template)
- Foundational research principles (Address-as-Evidence, Brick Wall Reframe, GPS methodology)
- Module specs, build order, completion status
- FTM Bridge reverse-engineering details
- Prompt engine library (15 engines, module-engine mapping)
- Assertions table schema
- Tech stack (Next.js 15, Supabase, Claude API claude-sonnet-4-6)
- Coding standards specific to the genealogy domain
- Source and citation rules
- Static rules (specific data decisions locked in)
- Project state and backlog

---

## 2. Session Snapshot System

### Format (as it actually exists — verified across 45 real sessions)

```
--- SESSION SNAPSHOT ---
TIMESTAMP: YYYY-MM-DD HH:MM UTC
Captured by: [Claude (claude.ai) | Claude Code (claude-sonnet-4-6)]
Posture at capture: [BUILD | FIX | EXPLORE | TRANSITION]
Trigger: [reason for snapshot]

## WHAT THIS SESSION WAS DOING
[1–3 sentence narrative]

## STATE AT CAPTURE
[Concrete facts: what is live, what is committed, what is deployed]
[Specific file names, commit SHAs, version numbers]

## DECISIONS MADE THIS SESSION
[TIMESTAMP: decision text]
[TIMESTAMP: decision text]
[...]

## OPEN THREADS
[Unresolved items with enough specificity to act on]

## PARTIALLY BUILT WORK
[Incomplete code or design in flight — where it lives, what remains]

## DO NOT DO THIS
[Guardrails for next session — what to avoid, what was ruled out]

## NEXT IMMEDIATE ACTION
[Single clear directive. Must specify which AI should execute it.]

--- END SNAPSHOT ---
```

### Key Format Rules (observed in practice)
- **TIMESTAMPs are on every decision** — not just the session header. Each item in DECISIONS MADE carries its own timestamp.
- **Commit SHAs are explicit** — not "I committed this," but the actual 7-character short SHA.
- **"Captured by" identifies the AI** — this is the handoff ownership marker. Never left blank.
- **NEXT IMMEDIATE ACTION specifies which AI** — e.g., "Claude Code: run smoke test" or "claude.ai: design the schema." Not just what to do, but who should do it.
- **"DO NOT DO THIS" is not optional** — even if there are no explicit guardrails, the field is present and says "None identified this session."

### File Naming
`SESSION-YYYY-MM-DD-HHMM-UTC.md`

Variations observed:
- `SESSION-2026-05-12-CLAUDECODE-UTC.md` — named by AI role when time was not precisely known
- `SESSION-2026-05-12-DINNER-UTC.md` — named by trigger event when timestamp was approximate
- `SESSION-2026-05-13-UTC.md` — rare, date-only when exact time not recorded

The standard is HHMM-UTC. The named variants are deviations, not the pattern.

### SESSIONS-INDEX.md
Lives at `/sessions/SESSIONS-INDEX.md`. Tracks all sessions with one-line summaries. Updated at the close of each session as part of the close protocol.

### Volume
45 sessions across 6 days (May 8–14, 2026). Average: 7–8 sessions per day. Sessions range from 45 minutes to several hours. The system handled this volume without any format degradation.

### Format Evolution
- **May 8–9**: Longer, more philosophical. Included reasoning passages. Sections were more verbose.
- **May 9 onward**: Format crystallized and has been stable since. Philosophy moved to AGENT.md. Snapshots became facts-first.
- **No format drift observed** after May 9. 45 sessions, identical structure throughout.

---

## 3. Signal Vocabulary

### Status: Active and Working
Signals are documented in AGENT.md lines 44–87. Confirmed active in session history.

### Signal Categories and Protocols

**Exit Signals** — triggered when leaving the session normally
- Trigger phrases: "leaving", "going to bed", "stepping away"
- Protocol: execute session close (8 steps), write snapshot, commit

**Distress Signals** — triggered when something is wrong mid-session
- Trigger phrases: "something wrong", "losing context", "frozen"
- Protocol: stop immediately, capture state as-is, write freeze-frame snapshot
- Confirmed active: distress protocol fired May 8 (SESSION-2026-05-08-2356-UTC.md): "Session experienced a frozen loop... Distress protocol fired."

**Context Window Pulse** — triggered proactively at context thresholds
- 60% fill: advisory note, AI flags context level
- 80% fill: mandatory snapshot before continuing
- Confirmed active: continuation signals used May 13 ("continuation from prior context-limit session")

**WIP Signals** — triggered when work is incomplete at session end
- Marks partially built code, identifies `/wip/` branch if needed
- Protocol: branch, commit WIP, note exact resumption point in snapshot

**Handoff Signals** — triggered when passing work between AIs
- See Section 4 for full detail
- Confirmed active: multiple handoffs documented May 12–14

**Health Check Signals** — triggered to verify system state
- Confirms repo is clean, dev server is running, Supabase connection is live
- Phrase: "Repo clean" or "Repo is clean" in snapshot STATE AT CAPTURE

### Signals Not Explicitly Documented That Are In Use
- **"None. All work is committed."** — appears in PARTIALLY BUILT WORK to explicitly clear that field
- **"Connector timed out"** — implicit signal that claude.ai must restart; resumption is automatic
- Version bumps as implicit signals: AGENT.md version changes signal substantive state transitions

---

## 4. Dual-AI Workflow

### The Two AIs
- **claude.ai** (Claude Desktop web interface) — architect, designer, spec-writer. Owns EXPLORE and PLAN phases. Uses GitHub connector to read/write the repo.
- **Claude Code** (claude-sonnet-4-6 via CLI) — executor. Owns BUILD and FIX phases. Has local filesystem and terminal access.

### Division of Labor (as observed in practice)
| Task | Owner |
|------|-------|
| Architecture decisions | claude.ai |
| Writing specs and design docs | claude.ai |
| AGENT.md updates (major) | claude.ai |
| AGENT.md updates (minor, field values) | Claude Code |
| Writing and committing code | Claude Code |
| Running npm/build/test scripts | Claude Code |
| Acceptance tests | Claude Code |
| Session snapshots | Either (identified by "Captured by" field) |
| GitHub connector pushes | claude.ai |
| Git commits via CLI | Claude Code |

### Handoff Mechanics (exact language used in real sessions)

**claude.ai → Claude Code:**
- "Handed [task] to Claude Code"
- "Next immediate action: Claude Code: [specific task]"
- "Claude Code handoff brief" (longer document when complex context transfer needed)

**Claude Code → claude.ai:**
- "Next immediate action: claude.ai BUILD session"
- "Next immediate action: claude.ai: [specific task]"
- Session snapshot is the primary handoff artifact in both directions

### Handoff Pattern (real example, May 13–14)
```
SESSION-2026-05-13-2345-UTC.md (claude.ai):
  - Designed FTM Bridge Phase 2 spec
  - Wrote Claude Code handoff brief
  - Bumped AGENT.md v2.10.0
  - NEXT IMMEDIATE ACTION: Claude Code: smoke test Phase 2

SESSION-2026-05-14-0100-UTC.md (Claude Code):
  - Executed Phase 2 in full
  - Committed code (SHA: 291f786)
  - NEXT IMMEDIATE ACTION: Run full synchronized tree

SESSION-2026-05-14-0115-UTC.md (claude.ai):
  - Received Phase 2 completion signal
  - Added AI icebreaker + maps design decisions
  - Built person detail page spec
  - NEXT IMMEDIATE ACTION: Design person_research_notes table

SESSION-2026-05-14-0400-UTC.md (claude.ai):
  - Built person detail page (5 files, 3 commits)
  - NEXT IMMEDIATE ACTION: Handed smoke test to Claude Code
```

### The SHA Sync Issue (operational detail — belongs in setup guide)

**Problem:** When Claude Code updates AGENT.md and commits via CLI, the GitHub connector (used by claude.ai) sometimes reads a cached version of the file rather than the freshly committed SHA. This can cause claude.ai to believe it has the current AGENT.md when it actually has a prior version.

**Symptom:** Claude Code reports "AGENT.md updated to v2.9.2, committed SHA abc1234" but claude.ai reads the file and sees v2.8.0.

**Discovery:** SESSION-2026-05-13-2345-UTC.md explicitly documents: "Claude Code AGENT.md updates may not land in GitHub. claude.ai must verify SHA after any Claude Code session that claims to update AGENT.md, and rewrite if SHA unchanged."

**Resolution in that session:** "Claude Code's v2.9.2 update DID land this session — discrepancy was read/write timing." The SHA was correct; the connector needed a moment to refresh.

**Protocol established:**
1. After any Claude Code session that updates AGENT.md, claude.ai must check the SHA via GitHub connector before trusting the version number
2. If SHA matches the commit Claude Code reported, proceed
3. If SHA does not match, claude.ai rewrites the section from the snapshot record

**Implication for ContextPartner setup guide:** The GitHub connector caches reads. Back-to-back sessions (Claude Code commits → claude.ai reads immediately) can hit a stale cache. Build in a short pause or explicit SHA verification step when the handoff is immediately after a commit.

---

## 5. MCP Infrastructure

### Current Setup (as of 2026-05-12, confirmed active)
- **Mode:** NPX (not Docker, not HTTP)
- **Server:** `@modelcontextprotocol/server-github` via `npx -y`
- **No local install** — NPX downloads on demand, cached after first run

### Config File Locations

| File | Purpose |
|------|---------|
| `~/.claude/settings.json` | Claude Code MCP config (active) |
| `~/Library/Application Support/Claude/claude_desktop_config.json` | Claude Desktop MCP config (active) |
| `~/Library/Application Support/Claude/claude_desktop_config.BACKUP.json` | PAT source of truth — DO NOT DELETE |
| `~/.claude/mcp-manager.py` | Manager script for switching modes |

### Claude Code Config (current)
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "[PAT from backup file]"
      }
    }
  }
}
```

### Manager Script: `~/.claude/mcp-manager.py`
193 lines, Python 3. Three modes:

| Mode | Command | Notes |
|------|---------|-------|
| NPX (current) | `python3 ~/.claude/mcp-manager.py use npx` | No Docker. First run downloads package, then cached. |
| Docker (fallback) | `python3 ~/.claude/mcp-manager.py use docker` | Requires Docker Desktop running. Subject to sleep timeouts. |
| HTTP/Copilot | `python3 ~/.claude/mcp-manager.py use http` | Requires active Copilot subscription (not available). |

Status check: `python3 ~/.claude/mcp-manager.py status`

### Architecture Decision
MCP infrastructure is **user-level, not project-level**. No `.mcp.json` or MCP config exists inside Project G Live. This is intentional:
- Keeps project dependencies clean (MCP is unrelated to the genealogy app)
- Single manager script controls all projects from one location
- Project can be cloned and run without MCP config embedded in it

### AGENT.md Line Reference
Lines 1012–1047 contain the full MCP Infrastructure section as currently documented.

---

## 6. Directory Structure (OS-Relevant Portions Only)

```
/Users/dave/Project-G-Live/
├── AGENT.md                          -- master control file (OS + app combined)
├── sessions/
│   ├── SESSIONS-INDEX.md             -- session manifest, updated each close
│   └── SESSION-YYYY-MM-DD-HHMM-UTC.md  (45 files, May 8–14 2026)
└── .claude/
    └── [worktrees directory]         -- Claude Code workspace metadata

~/.claude/
├── mcp-manager.py                    -- GitHub MCP mode manager
├── settings.json                     -- Claude Code active config
└── projects/-Users-dave-Project-G-Live/
    └── memory/
        └── mcp_migration_plan.md     -- MCP setup documentation

~/Library/Application Support/Claude/
├── claude_desktop_config.json        -- Claude Desktop active config
└── claude_desktop_config.BACKUP.json -- PAT source of truth (DO NOT DELETE)
```

---

## 7. Observations: What Is OS But Not Explicitly Documented

These are real behaviors observed in session history that are not written up as formal protocol anywhere in AGENT.md:

**Implicit version bump convention:** claude.ai bumps MINOR version on design decisions, Claude Code bumps PATCH on execution. Major version for architectural changes. Not documented, but consistent across all sessions.

**Connector restart signal:** When claude.ai loses the GitHub connector mid-session, it notes "Connector timed out — Claude Desktop was restarted" in the snapshot. This is an implicit distress/resume signal but it's not in the formal signal vocabulary.

**"Repo clean" as session state marker:** The phrase appears consistently in STATE AT CAPTURE when all work is committed. It functions as a safety signal but is not formally defined.

**Claude Code smoke test as verification gate:** The pattern "design → build → Claude Code smoke test" appears in every build cycle. The smoke test is always the last thing before a session closes and the result goes into STATE AT CAPTURE. Not documented as a required step in the close protocol, but it is invariant in practice.

**AGENT.md as living document:** The file is updated within sessions, not just between them. This means AGENT.md version and session timestamp are tightly coupled — a session that ends without bumping AGENT.md version is implicitly a session that made no architectural decisions.

---

## Extraction Summary for claude.ai

To build the ContextPartner templates, the following are the clean, portable pieces:

| Template | Source in AGENT.md |
|----------|--------------------|
| AGENT.md template | Lines 20–232 (posture, signals, memory, close protocol, versioning) + lines 979–1079 (dual-AI, MCP, environment) |
| session-snapshot.md template | Lines 90–205 (session memory architecture section) + validated against real session files |
| restoration-prompt.md template | Lines 90–205 (restoration prompt subsection) |
| Signal vocabulary doc | Lines 44–87 |
| Session close protocol | Lines 208–220 |
| Dual-AI workflow guide | Lines 979–1008 + Section 4 of this document |
| MCP setup guide | Lines 1012–1047 + Section 5 of this document (include SHA sync issue) |
