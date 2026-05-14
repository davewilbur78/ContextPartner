# Claude Code Brief -- ContextPartner

TIMESTAMP: 2026-05-14 23:13 UTC
Written by: Claude (claude.ai)
For: Claude Code

---

## What This Repo Is

ContextPartner is a new project. It is the OS that has been running inside
Project G Live -- extracted, named, and given its own home.

Read the README first. It tells the full story of what this is and where it came from.

This is not a software application. It is a protocol and a set of templates
for working with AI as a genuine long-term partner across sessions, projects,
and time. The mechanism: state lives in a version-controlled repo, not in the AI.
The AI reads the repo at session start and is fully oriented every time.

---

## The Dual-AI Model for This Project

claude.ai is the architect and writer for ContextPartner.
It designed the system, holds the vision, and writes the documents and templates.
It works via GitHub connector -- reads and writes the repo directly.

Claude Code is the extractor and executor.
Your job is to read the live Project G Live implementation and produce a precise
inventory of what the OS actually consists of today -- not as documented in AGENT.md
alone, but as it genuinely exists in the running project.

Neither of you duplicates the other's work.
Both of you read this repo at session start to orient.

---

## Your First Job

Go to /Users/dave/Project-G-Live/ and do a full OS inventory.

You are looking for everything that constitutes the operating system -- not the
genealogy app, but the scaffolding around it. Specifically:

1. AGENT.md -- read it fully. Note its structure, its sections, what is
   OS-level (universal protocol) vs what is project-specific (genealogy app state).
   The goal is to understand what a clean separation would look like.

2. /sessions/ -- examine the snapshot format as it actually exists in real files,
   not just as documented. Note any variations or evolution in the format over time.
   Look at the most recent 5 sessions to understand current practice.

3. Signal vocabulary -- is it being used? Are there any signals in the session
   history that suggest the vocabulary has evolved beyond what AGENT.md documents?

4. The dual-AI workflow -- look for evidence of how claude.ai and Claude Code
   actually hand off to each other. What does a real handoff look like in practice?

5. The MCP infrastructure -- note the current working setup (NPX mode, manager
   script location, config file locations). This will go in the setup guide.

6. Anything else you observe that constitutes the OS but is not explicitly
   documented anywhere.

Produce a clean inventory document and commit it to this repo at:
/extraction/project-g-live-os-inventory.md

Format it clearly so claude.ai can use it to build the ContextPartner templates.

---

## What Happens After Your Inventory

claude.ai will read your inventory and use it to build:

- /templates/AGENT.md -- the lean, project-agnostic template
- /templates/session-snapshot.md -- the snapshot format as a clean template
- /templates/restoration-prompt.md -- the restoration prompt format
- /protocol/ -- posture system, signal vocabulary, session close protocol
- /guides/ -- setup guide, dual-AI workflow guide

Your inventory is the source of truth for that build. Get it right.

---

## How to Work on This Project

At the start of every Claude Code session on ContextPartner:
1. Read this file
2. Read README.md
3. Check /extraction/ to see what has already been done
4. Check with dave on what to do next

Write session snapshots to /sessions/ in this repo when closing,
using the same format as Project G Live sessions.

The repo is at: github.com/davewilbur78/ContextPartner
Local clone when ready: /Users/dave/ContextPartner/

---

## One Important Note

Do not modify Project G Live while working on this.
ContextPartner is being built alongside it, not instead of it.
Project G Live continues running on its existing OS unchanged.
We are extracting and generalizing, not migrating.
