# Setup Guide

How to deploy ContextPartner on a new project.

---

## What You Need

- A GitHub account
- Claude Desktop (for claude.ai interface)
- Claude Code (for local execution)
- A GitHub Personal Access Token (PAT) with repo read/write permissions
- Node.js installed (for NPX mode MCP)

---

## Step 1 -- Create the Project Repo

Create a new GitHub repository for your project. Public or private, your choice.
Public repos are easier to manage with the connector.

Initialize with a README so the repo has a default branch.

---

## Step 2 -- Set Up the GitHub MCP Connector

The GitHub MCP connector is what gives claude.ai read/write access to your repo.
This is the critical piece. Without it, claude.ai can only read cached web versions
of your files -- not good enough for back-to-back sessions.

### NPX Mode (recommended)

NPX mode runs the GitHub MCP server via Node.js without Docker.
First run downloads a small package. Subsequent runs use the cache.

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
Store a backup of your config with the PAT somewhere safe.
Project G Live uses `claude_desktop_config.BACKUP.json` in the same folder.
Do not delete it -- it is the PAT source of truth if you need to reconfigure.

### MCP is User-Level, Not Project-Level
Do not put MCP config inside your project repo.
One MCP setup serves all your projects from the same machine.
This keeps project repos clean and portable.

---

## Step 3 -- Create AGENT.md

Copy `/templates/AGENT.md` from this repo into your project repo root.
Fill in the project name and initial state.
Leave the OS sections (posture, signals, memory architecture, close protocol) unchanged.
Add project-specific sections below the Static Rules section.

Commit it. This is now the single source of truth for your project.

---

## Step 4 -- Create the /sessions/ Folder

Create a `/sessions/` folder in your project repo.
Add a `SESSIONS-INDEX.md` file with a header line.

```markdown
# Sessions Index

Format: TIMESTAMP | Posture | AI | one-sentence summary
```

Commit it. The session archive starts now.

---

## Step 5 -- First Session

Open Claude Desktop. Start a new conversation.

Paste this as your opening message:

```
This is [PROJECT NAME] (github: [username]/[repo-name]).
At session start, use the GitHub connector to read AGENT.md, confirm the version,
fetch the most recent session snapshot from /sessions/, then ask for posture.
Do not begin work until posture is confirmed.
The repo is the source of truth -- never rely on memory from previous conversations.
```

That prompt is your session starter for every session going forward.
Save it somewhere easy to paste.

---

## Step 6 -- Clone Locally for Claude Code

Clone your project repo to a fixed local path:
```
git clone https://github.com/[username]/[repo-name] /Users/[you]/[project-name]/
```

Use this exact path every Claude Code session.
Never clone again -- always work from this directory.
Always `git pull` before starting local work after any connector push from claude.ai.

---

## Known Issues

**Stale cache on connector reads:** The GitHub connector caches reads.
If claude.ai reads a file immediately after Claude Code commits it,
claude.ai may see the prior version. Build in a short pause or verify the SHA.
See `/guides/dual-ai-workflow.md` for the full SHA sync protocol.

**Connector goes to sleep:** If using Docker mode, the container may sleep
between sessions and break the connector. NPX mode avoids this entirely.

**Wrong directory in Claude Code:** Always confirm `pwd` is your project directory
before running anything. A wrong directory is the cause of most "code isn't there" issues.
