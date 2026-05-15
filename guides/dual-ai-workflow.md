# Dual-AI Workflow Guide

ContextPartner supports a dual-AI workflow where two AI interfaces collaborate
on the same project with distinct roles, using the repo as the shared handoff layer.

This guide documents the pattern as proven in Project G Live across 45+ sessions.

---

## The Two Roles

**claude.ai** -- The Architect
- Architecture, design decisions, writing specs and docs
- EXPLORE and PLAN postures live here
- Uses the GitHub connector to read and write the repo directly
- Owns AGENT.md major updates
- Writes handoff briefs for Claude Code

**Claude Code** -- The Executor
- Writing and committing code, running build scripts and tests
- BUILD and FIX postures live here
- Has local filesystem and terminal access
- Owns acceptance testing and smoke tests
- Writes session snapshots back to /sessions/ for claude.ai to pick up

---

## Division of Labor

| Task | Owner |
|------|-------|
| Architecture decisions | claude.ai |
| Writing specs and design docs | claude.ai |
| AGENT.md updates (major) | claude.ai |
| AGENT.md updates (minor field values) | Claude Code |
| Writing and committing code | Claude Code |
| Running npm/build/test scripts | Claude Code |
| Acceptance tests and smoke tests | Claude Code |
| Session snapshots | Either (identified by "Captured by" field) |
| GitHub connector pushes | claude.ai |
| Git commits via CLI | Claude Code |

---

## Handoff Language

Handoffs happen through session snapshots. The NEXT IMMEDIATE ACTION field
always names which AI should act next.

**claude.ai to Claude Code:**
- "Claude Code: [specific task]"
- "Handed [task] to Claude Code"
- For complex handoffs: write a dedicated handoff brief document

**Claude Code to claude.ai:**
- "claude.ai: [specific task]"
- "claude.ai BUILD session"
- Session snapshot with STATE AT CAPTURE showing completed work + commit SHA

---

## The SHA Sync Issue

**Problem:** When Claude Code updates AGENT.md and commits via CLI, the GitHub
connector (used by claude.ai) sometimes reads a cached version rather than the
freshly committed SHA. This can cause claude.ai to see a stale AGENT.md.

**Symptom:** Claude Code reports "AGENT.md updated to v2.9.2, committed SHA abc1234"
but claude.ai reads the file and sees v2.8.0.

**Protocol:**
1. After any Claude Code session that updates AGENT.md, claude.ai must check
   the SHA via GitHub connector before trusting the version number
2. If SHA matches the commit Claude Code reported, proceed
3. If SHA does not match, claude.ai rewrites the relevant section from the snapshot record

**Root cause:** The GitHub connector caches reads. Back-to-back sessions
(Claude Code commits, claude.ai reads immediately) can hit stale cache.
Build in a short pause or explicit SHA verification when the handoff is
immediately after a commit.

---

## Implicit Conventions (observed in practice, not formally documented)

These patterns emerged from real usage and are worth preserving:

**Version bump ownership:** claude.ai bumps MINOR version on design decisions.
Claude Code bumps PATCH on execution. Major version for architectural changes.

**Smoke test as verification gate:** The pattern "design -> build -> Claude Code
smoke test" appears in every build cycle. The smoke test result always goes
into STATE AT CAPTURE before session close.

**"Repo clean" as safety signal:** This phrase in STATE AT CAPTURE means all
work is committed and nothing is in flight. It functions as a handoff safety check.

**Neither AI re-briefs the other from scratch.** Both read AGENT.md and the
most recent session snapshot. That is sufficient orientation. No re-explaining needed.

---

## What This Is Not

This is not a master/slave relationship. Neither AI is subordinate to the other.
They have different capabilities and different lanes. The repo is the authority.
Both AIs serve the user and the project -- not each other.
