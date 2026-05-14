# Session Snapshot Template

File naming: `SESSION-YYYY-MM-DD-HHMM-UTC.md`
Location: `/sessions/` in the project repo

Use minute-level timestamps. Never date-only.
"Captured by" is required -- it is the handoff ownership marker.
"NEXT IMMEDIATE ACTION" must specify which AI should execute it.
"DO NOT DO THIS" is never omitted -- write "None identified this session" if nothing applies.

---

--- SESSION SNAPSHOT ---
TIMESTAMP: YYYY-MM-DD HH:MM UTC
Captured by: [Claude (claude.ai) | Claude Code]
Posture at capture: [BUILD | FIX | EXPLORE]
Trigger: [context checkpoint / exit signal / distress signal / session close]

WHAT THIS SESSION WAS DOING
[2-4 sentences. What this specific session worked on from the moment it opened.]

STATE AT CAPTURE -- TIMESTAMP: YYYY-MM-DD HH:MM UTC
[What is committed. What exists only in context. What is half-built.
Reference wip/ branch commit if applicable.
Include specific commit SHAs when work was committed this session.
Use "Repo clean" when all work is committed and nothing is in flight.]

DECISIONS MADE THIS SESSION
TIMESTAMP: YYYY-MM-DD HH:MM UTC -- [decision and reason]
TIMESTAMP: YYYY-MM-DD HH:MM UTC -- [rejected option and why]

OPEN THREADS -- TIMESTAMP: YYYY-MM-DD HH:MM UTC
[Questions mid-air. Unresolved tensions. Things about to be decided. Be specific.]

PARTIALLY BUILT WORK
[If anything exists only in context and cannot be committed to wip/:
reconstruct it fully and completely here. Not a summary -- the actual work.
If it can be committed to wip/, do that and reference the commit here.
If nothing is partially built, write: "None. All work is committed."]

DO NOT DO THIS
[Wrong turns already taken. Things that look tempting but were ruled out.
Questions already closed that must not be reopened.
If nothing applies, write: "None identified this session."]

NEXT IMMEDIATE ACTION
TIMESTAMP: YYYY-MM-DD HH:MM UTC
[One thing. Specific. Actionable. Name which AI should execute it:
"Claude Code: [task]" or "claude.ai: [task]"]
--- END SNAPSHOT ---

---

## Notes on Format

**Volume:** The system has handled 45+ sessions across 6 days without format degradation.
Keep snapshots facts-first. Philosophy and reasoning belong in AGENT.md, not snapshots.

**Timestamps on decisions:** Every item in DECISIONS MADE carries its own timestamp,
not just the session header. This is how you reconstruct the decision trail later.

**Commit SHAs:** Write the actual 7-character short SHA, not just "I committed this."

**SESSIONS-INDEX.md:** Update this file at every session close.
Format: `TIMESTAMP | Posture | AI | one-sentence summary`
