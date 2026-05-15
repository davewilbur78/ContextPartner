# ContextPartner Setup Guide

TIMESTAMP: 2026-05-14
Status: First draft — merged from Claude Code + claude.ai parallel work

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

## What You Need

- A GitHub account
- Claude Desktop (for the claude.ai interface)
- Claude Code (for local execution, if using the dual-AI workflow)
- A GitHub Personal Access Token (PAT) with repo read/write permissions
- Node.js installed (for NPX mode MCP)

---

## Step 1 — Create the Project Repo

Create a new GitHub repository for your project. Public or private, your choice. Initialize with a README so the repo has a default branch.

The repo needs two things at minimum:

```
AGENT.md           -- at the root
/sessions/         -- for session snapshots
```

Everything else (app code, docs, templates) is project-specific.

---

## Step 2 — Set Up the GitHub MCP Connector

The GitHub MCP connector gives claude.ai read/write access to your repo. This is the mechanism by which the OS reads and writes state. Without it, claude.ai can only access cached web versions of your files — not reliable enough for back-to-back sessions.

### NPX Mode (recommended)

NPX mode runs the GitHub MCP server via Node.js without Docker. First run downloads a small package; subsequent runs use the cache. No container sleep issues.

**Claude Desktop config** (`~/Library/Application Support/Claude/claude_desktop_config.json`):
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "your-pat-here"
      }
    }
  }
}
```

**Claude Code config** (`~/.claude/settings.json`):
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "your-pat-here"
      }
    }
  }
}
```

After editing config files: quit and reopen Claude Desktop for changes to take effect.

### PAT Storage

Store a backup of your config with the PAT somewhere safe. Project G Live uses `claude_desktop_config.BACKUP.json` in the same folder. Do not delete it — it is the PAT source of truth if you need to reconfigure.

### MCP is User-Level, Not Project-Level

Do not put MCP config inside your project repo. One MCP setup serves all your projects from the same machine. This keeps project repos clean and portable.

---

## Step 3 — Create AGENT.md

Copy `/templates/AGENT.md` from this repo into your project repo root. Fill in the project name and initial state. Leave the OS sections (posture system, signal vocabulary, session memory architecture, session close protocol) unchanged. Add project-specific sections below.

Commit it. AGENT.md is now the single source of truth for your project.

**Create AGENT.md before your first working session.** The boot sequence tells the AI to read AGENT.md. If the file doesn't exist, the boot fails. Session zero is setup-only; the OS is live from session one onward.

AGENT.md should be versioned: `vMAJOR.MINOR.PATCH` in the header. Bump on every substantive update. The boot sequence confirms the version at session start — this is how you know the right file was read.

---

## Step 4 — Create the /sessions/ Folder

Create a `/sessions/` folder in your project repo. Add a `SESSIONS-INDEX.md` file with a header line:

```markdown
# Sessions Index

Format: TIMESTAMP | Posture | AI | one-sentence summary
```

Commit it. The session archive starts here.

---

## Step 5 — Create the claude.ai Project Folder

This is the boot sequence. Every session you open inside the Project folder automatically receives the system prompt before the conversation begins. The OS boots without any manual input required.

### How to Create It

1. In claude.ai, click **Projects** in the left sidebar
2. Click **New Project**
3. Name it to match your project (e.g., "Project G Live", "ContextPartner")
4. Open **Project Instructions** and paste the system prompt below
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

That is the only customization required. Do not change anything else on the first deployment.

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

Once the core system prompt is working, you can add project-specific boot steps. Add them before the posture question, after the session snapshot fetch:

```
This is [PROJECT-NAME] (github: [USERNAME]/[REPO-NAME]). At session start, use the GitHub connector to read AGENT.md, confirm the version, fetch the most recent session snapshot from /sessions/. [Add project-specific checks here — e.g., "Check the Supabase connection status in AGENT.md."] Then ask for posture: BUILD, FIX, or EXPLORE. Do not begin work until posture is confirmed. If the GitHub connector is unavailable or you cannot read AGENT.md, stop and tell me before proceeding. The repo is the source of truth -- never rely on memory from previous conversations.
```

Do not add complexity until the base system is running cleanly.

### A Note on System Prompt and AGENT.md Coupling

The system prompt hardcodes the posture vocabulary: BUILD, FIX, EXPLORE. These terms come from AGENT.md. If you later change the posture system in AGENT.md (add modes, rename them), you must also update the system prompt to match. They will drift silently if you don't.

---

## Step 6 — Clone Locally for Claude Code

If your project uses Claude Code (for local execution, git commits, running scripts), clone the repo to a fixed local path:

```
git clone https://github.com/[USERNAME]/[REPO-NAME] /Users/[you]/[project-name]/
```

Use this exact path every Claude Code session. Never clone again — always work from this directory. Always `git pull` before starting local work after any connector push from claude.ai.

---

## The Claude Code Boot Path (Separate from This)

The steps above describe the **claude.ai boot path**. Claude Code is a separate interface and boots differently.

Claude Code does not use the claude.ai Project folder or system prompt. It uses:
- A `CLAUDE.md` file at the repo root (if present) — Claude Code reads this automatically at session start
- Or a project brief (like `CLAUDE-CODE-BRIEF.md`) that you point Claude Code to explicitly at session start

For dual-AI projects, both boot paths must be set up. See `/guides/dual-ai-workflow.md` for the full dual-AI setup (to be written).

---

## Known Issues

**Stale cache on connector reads:** The GitHub connector caches reads. If claude.ai reads a file immediately after Claude Code commits it, claude.ai may see the prior version. Build in a short pause or verify the SHA. See the SHA sync issue section in `/extraction/project-g-live-os-inventory.md` and the future `/guides/dual-ai-workflow.md` for the full protocol.

**Connector goes to sleep:** If using Docker mode, the container may sleep between sessions and break the connector. NPX mode avoids this entirely. If the connector is unresponsive at session start, the hard stop in the system prompt will catch it.

**Wrong AGENT.md version at session start:** If the version reported at boot doesn't match what you last committed, the connector served a cached read. Restart the connector or open a new conversation and try again.

**Wrong directory in Claude Code:** Always confirm you are in the right project directory before running anything. A wrong working directory is the cause of most "code isn't there" issues.
