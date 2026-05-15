# ContextPartner Setup Guide

TIMESTAMP: 2026-05-14
Status: First draft — based on Project G Live reference implementation

---

## What You Are Setting Up

A complete ContextPartner deployment has three components. All three are required. Missing any one of them means the OS does not function.

| Component | What it does |
|-----------|-------------|
| **GitHub repo** | The memory store. Persistent, versioned, AI-agnostic. |
| **AGENT.md** | The state document. Everything the AI needs to know at session start. |
| **claude.ai Project folder + system prompt** | The boot sequence. How the AI knows to read the repo instead of relying on conversation history. |

The third component — the Project folder — was invisible for a long time because it worked silently in the background. It is the most important thing to get right, because without it, the first two components don't reliably activate.

---

## Component 1: The GitHub Repo

Create a new GitHub repository for the project. It should be:

- **Private or public** — your choice; the OS works either way
- **Initialized with a README** — you need at least one file to clone
- **Accessible via GitHub connector** — the claude.ai GitHub connector must have read/write access

The repo needs two directories at minimum:

```
/sessions/     -- session snapshots go here
/              -- AGENT.md lives at the root
```

Everything else (extraction docs, templates, guides, app code) is project-specific.

---

## Component 2: AGENT.md

AGENT.md is the state document. It lives at the root of the repo. It is the single source of truth for everything the AI needs to know: the session memory system, the posture system, the signal vocabulary, the project state, the decisions made, what not to do.

**Create AGENT.md before your first real working session.** The system prompt tells the AI to read AGENT.md at boot. If the file doesn't exist, the boot fails. Session zero is setup-only: create the repo, write AGENT.md, then begin working sessions.

Use the template at `/templates/AGENT.md` in this repo (once built). For now, use Project G Live's AGENT.md as the reference — strip out all genealogy-specific content and replace with your project's state.

AGENT.md should be versioned: `vMAJOR.MINOR.PATCH` in the header. Bump the version whenever you make a substantive update. The system prompt asks the AI to "confirm the version" — this is how you know the right file was read.

---

## Component 3: The claude.ai Project Folder and System Prompt

### What a claude.ai Project Folder Is

In claude.ai, you can create Project folders that contain a persistent system prompt. Every new conversation opened inside that Project automatically receives the system prompt before the conversation begins. This is the boot sequence.

### Why This Matters

Without a Project folder, every session starts with a blank AI. You would have to manually paste a restoration prompt at the start of every conversation. The Project folder automates this — the OS boots automatically every time you open a new conversation.

### How to Create the Project Folder

1. In claude.ai, click **Projects** in the left sidebar
2. Click **New Project**
3. Name it to match your project (e.g., "Project G Live", "ContextPartner")
4. Open **Project Instructions** and paste the system prompt (see below)
5. Connect the GitHub connector to the project if not already connected
6. All future working sessions on this project should be opened from inside this Project folder

### The System Prompt Template

Copy this exactly, substituting only the bracketed values:

```
This is [PROJECT-NAME] (github: [USERNAME]/[REPO-NAME]). At session start, use the GitHub connector to read AGENT.md, confirm the version, fetch the most recent session snapshot from /sessions/, then ask for posture: BUILD, FIX, or EXPLORE. Do not begin work until posture is confirmed. If the GitHub connector is unavailable or you cannot read AGENT.md, stop and tell me before proceeding. The repo is the source of truth -- never rely on memory from previous conversations.
```

**What to substitute:**
- `[PROJECT-NAME]` — the human name of the project (e.g., "Project G Live")
- `[USERNAME]` — your GitHub username (e.g., "davewilbur78")
- `[REPO-NAME]` — the repository name (e.g., "Project-G-Live")

**That is the only customization required.** Do not change anything else on the first deployment.

---

### What Must Never Be Changed or Weakened

Two parts of this system prompt are load-bearing. They look like they could be simplified or removed. They cannot.

---

**"The repo is the source of truth -- never rely on memory from previous conversations."**

This line enforces statelessness. Without it, the AI uses conversation history from the Project folder — previous sessions accumulate and bleed into new ones. The AI "remembers" things from past conversations that it was never meant to carry forward. The result is state corruption: the AI is partially running from the repo and partially running from cached conversation context, and you cannot tell which is which.

This line must stay exactly as written. Do not soften it ("try to use the repo"), do not remove it, do not rephrase it as a preference. It is a hard instruction, not a guideline.

---

**"If the GitHub connector is unavailable or you cannot read AGENT.md, stop and tell me before proceeding."**

Without this, a connector failure produces a corrupted session silently. The AI cannot read AGENT.md, so it falls back to conversation history or makes something up, and you do not find out until the session has produced work based on wrong state.

This line creates a hard stop at the one moment when the whole boot sequence depends on external infrastructure working correctly. If the connector is down, you want to know immediately — not after 45 minutes of work.

---

### What Can Be Extended

Once the core system prompt is working, you can extend it for project-specific boot steps. Add them before the posture question, after the AGENT.md read:

```
This is [PROJECT-NAME] (github: [USERNAME]/[REPO-NAME]). At session start, use the GitHub connector to read AGENT.md, confirm the version, fetch the most recent session snapshot from /sessions/. [Add any project-specific checks here -- e.g., "Check the Supabase connection status in AGENT.md."] Then ask for posture: BUILD, FIX, or EXPLORE. Do not begin work until posture is confirmed. If the GitHub connector is unavailable or you cannot read AGENT.md, stop and tell me before proceeding. The repo is the source of truth -- never rely on memory from previous conversations.
```

Do not add complexity until the base system is running cleanly.

---

### A Note on System Prompt and AGENT.md Coupling

The system prompt hardcodes the posture vocabulary: BUILD, FIX, EXPLORE. These terms come from AGENT.md. If you later change the posture system in AGENT.md (add modes, rename them), you must also update the system prompt to match. They will drift silently if you don't. Make it a habit: any time you change the posture vocabulary in AGENT.md, check the system prompt.

---

## The Claude Code Boot Path (Separate from This)

The three-part setup above is the **claude.ai boot path**. Claude Code is a separate AI interface and boots differently.

Claude Code does not use the claude.ai Project folder or system prompt. It uses:
- A `CLAUDE.md` file at the repo root (if present) — Claude Code reads this automatically at session start
- Or a project brief (like `CLAUDE-CODE-BRIEF.md`) that you point Claude Code to explicitly

For dual-AI projects (claude.ai handles architecture/design, Claude Code handles execution), both boot paths must be set up. See `/guides/dual-ai-workflow.md` for the full dual-AI setup (to be written).

---

## Session Zero: Before Your First Working Session

When setting up a new project, do these steps before opening your first real working session inside the Project folder:

1. Create the GitHub repo
2. Clone it locally (if Claude Code is in the workflow)
3. Write AGENT.md — at minimum: posture system, session memory system, session close protocol, project description, and current state
4. Commit AGENT.md to the repo
5. Create the claude.ai Project folder
6. Paste the system prompt with your repo substituted in
7. Open a test conversation inside the Project folder and verify the boot sequence fires correctly: AI reads AGENT.md, confirms version, checks /sessions/ (empty is fine), asks for posture

If the test session boots correctly, the OS is live. Begin working sessions.

---

## Maintenance Notes

- **SESSIONS-INDEX.md**: Once you have more than a handful of sessions, create a `SESSIONS-INDEX.md` in `/sessions/` and update it at each session close. The AI fetches the most recent snapshot — an index makes that lookup reliable when the folder is large.
- **Connector health**: The GitHub connector occasionally times out or loses authorization. If a session starts and the AI cannot read AGENT.md, check the connector status in claude.ai settings before assuming the repo has a problem.
- **AGENT.md version**: The system prompt asks the AI to "confirm the version." If you see the wrong version number at session start, the connector served a cached read. Restart the connector or open a new conversation and try again.
