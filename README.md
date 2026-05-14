# ContextPartner

An OS for working with AI as a genuine long-term partner.

---

## What This Is

ContextPartner is a protocol, a set of templates, and a working system that solves one of the most frustrating problems in working with AI: the session ends and everything is gone.

Not just the conversation. The decisions. The reasoning. The things that were tried and ruled out. The shape of the work. All of it.

Most people try to work around this with summary handoffs, built-in memory features, or just hoping the AI remembers. None of it works at the fidelity real work requires. The AI is holding the state, and the AI can't hold it reliably.

ContextPartner solves it differently. It moves the memory out of the AI entirely and puts it somewhere the AI doesn't control -- a version-controlled repository. GitHub doesn't hallucinate. GitHub doesn't forget. GitHub doesn't have a context window.

The AI becomes stateless by design. Every session starts fresh, reads the record, and picks up exactly where the work was. The continuity lives in the repo, not in the model.

That's the core mechanism. But the result is something more than persistent memory.

The result is a partner.

---

## What It Actually Creates

A partner who knows the whole story. Who was there for every decision. Who remembers what was tried and why it didn't work. Who you never have to re-explain everything to. Who picks up where you left off without being briefed.

AI by nature forgets. ContextPartner defeats that. It makes something persistent that was never meant to be persistent -- a working relationship between a human and an AI that compounds over time.

You bring the vision, the judgment, the taste.
The OS brings the continuity, the structure, the memory.
The AI brings the execution capacity.

None of those three work at full power without the other two.

---

## The Technical Description

A protocol for giving any AI persistent, reliable project memory by externalizing state to a version-controlled repository.

---

## The Vision Statement

A framework for treating AI as a reliable long-term collaborator rather than a single-session tool.

---

## Where This Came From

This system was not designed in the abstract. It was built and proven inside a real, complex, long-horizon project: a personal genealogy research platform (Project G Live) that required hundreds of sessions across multiple AI interfaces over months.

The author tried other approaches first. Google Drive connectors (read-only, not enough). Built-in AI memory features (unpredictable, sometimes more harmful than helpful). Summary handoffs between sessions (a fantasy -- the AI is summarizing its own hallucinations). GitHub with only web access (cached reads, not good enough for back-to-back sessions). 

Each failure pointed toward the same solution: the memory has to live outside the AI entirely, in a system with real write access and real version control, readable by any AI at the start of any session.

The genealogy app was chosen as the proving ground deliberately. It was the most complex, most personally meaningful piece of software the author had ever wanted to build. If the OS could hold up under those conditions -- months of work, dozens of modules, a dual-AI workflow, real data -- it could hold up anywhere.

It held up.

---

## The Layers

**The App** -- whatever project is being built. ContextPartner is project-agnostic.

**The OS** -- this system. AGENT.md, session snapshots, posture declarations, signal vocabulary, GitHub as memory, dual-AI workflow, MCP connector. Everything that makes continuity possible.

**The Meta** -- thinking about and designing the OS itself. This repo.

---

## What Is Here

This repo is the home of the OS itself -- extracted from Project G Live and made portable.

/templates/     -- AGENT.md template, session snapshot format, restoration prompt format
/protocol/      -- Posture system, signal vocabulary, session close protocol
/guides/        -- Setup guide, Claude Code integration, dual-AI workflow
/instances/     -- Project G Live as the reference implementation

Status: founding document committed. Extraction from Project G Live in progress.

---

## Draft Name Note

The name ContextPartner is a working draft. It captures two of the three core ideas:
context (what it holds) and partner (what it creates). The third idea -- that it extends
what AI is capable of beyond its natural limits -- may yet find its way into the name.
The name is not final. The thing it names is real.

---

## The First Instance

Project G Live -- github.com/davewilbur78/Project-G-Live

A personal genealogy research platform. 12+ modules complete. Real data. Hundreds of sessions.
The proving ground that made ContextPartner real.
