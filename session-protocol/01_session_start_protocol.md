---
title: Session Start Protocol
description: Defines the three-mode session-start system (lightweight, standard, deep) and the domain-specific reading lists Claude loads at the start of a session, so context loads automatically instead of being re-explained every chat.
category: protocol
status: active
tags:
  - session-management
  - routing
  - protocols
  - claude
  - context-loading
---

# Session Start Protocol
### How to give Claude full context before every session — automatically

---

## The Problem This Solves

Claude has Projects. It has memory. It has custom instructions.
Most people set those up once and still get generic output.

The problem isn't the tools. It's the structure.

Without a routing system, Claude loads everything or nothing.
Either it's overwhelmed with irrelevant context,
or it's starting from zero.

This protocol solves that.
Claude reads what's relevant to the task at hand.
According to your instructions.

---

## Step 0 — Identify the Mode

Before loading any context, classify the request:

| MODE | TRIGGER | WHAT HAPPENS |
|------|---------|--------------|
| **Lightweight** | Quick question, one-off task, simple rewrite | Answer directly. Skip context loading. |
| **Standard** | Clear task in a known domain | Load domain-specific context only |
| **Deep** | Cross-domain, complex, building something new | Full session loop — all context sources |

**Lightweight examples:**
- "rewrite this sentence"
- "give me 3 subject line options"
- "quick question about X"

**Standard examples:**
- "write 10 posts for this week"
- "draft a newsletter issue about X"
- "build a campaign for this release"

**Deep examples:**
- "update the vault based on today's session"
- "build out a new content pillar from scratch"
- "design a workflow I haven't built before"

---

## Lightweight Mode

No context loading. Just answer.

Use what's already visible in the conversation.
If the answer requires context that isn't there,
say so and switch to Standard mode.

---

## Standard Mode — The Session Loop

For any task with a clear domain, run in this order:

```
IDENTIFY  → what domain is this?
READ      → load only the files for that domain
CHECK     → pull relevant open tasks
WORK      → execute
UPDATE    → mark completed tasks immediately
SAVE      → write new context if anything changed
```
---

## Domain Routing

Each domain has a defined reading list.
Load only what the task needs.

---

### ✍️ CONTENT
*(posts, scheduling, analytics, newsletter)*

Load:
- Content manual
- Voice and tone guide
- Content pillars
- Analytics patterns — last 7 days
- Scheduled queue — avoid duplicates

Then:
- Run research on current trends before writing
- Check format rotation — no same format back to back
- Cross-reference recent posts — no repeats

---

### 🎵 MUSIC
*(lyrics, releases, distribution, strategy)*

Load:
- Music manual
- Discography and release history
- Lyric analysis (if writing lyrics)

Then:
- Check distribution status
- Note any upcoming release windows

---

### 🤖 AI WORKFLOWS
*(building workflows, documenting tools, experiments)*

Load:
- AI operating system doc
- Workflow system overview
- Active workflow log

Then:
- Run the workflow build protocol
- Document output before ending session
- Every workflow built is also a content piece

---

### 💼 BUSINESS
*(services, clients, consulting, ads)*

Load:
- Services ecosystem
- Active client context
- Ad strategy (if running campaigns)

Then:
- Flag content opportunities from business work
- Every client win is potential content

---

### 🧠 IDENTITY / VOICE
*(calibration, voice matching, deep personal content)*

Load:
- Core identity document
- Cognitive and psychological profile
- Voice analysis from past top-performing content

---

## Universal Rules

These apply in every mode, every session:

- **Context first** — load relevant files before executing
- **Modular reads** — load for the domain, not everything
- **Voice always** — every content task checks voice guide first
- **Update as you go** — new preferences logged immediately
- **Mark tasks done** — no completed work stays open
- **Flag uncertainty** — never proceed on an assumption without naming it

---

## How to Implement This

### Option 1 — Claude Projects (no code required)
Create a Claude Project.
Paste your domain context into the Project instructions.
Claude loads it automatically every session.
Start with one domain. Add more as you go.

### Option 2 — Obsidian + MCP (full system)
Connect your Obsidian vault to Claude via MCP.
Claude reads your vault before every session.
Every tool in your stack reads from the same database.
One source of truth. No re-explaining.

See `vault-architecture/` in this repo for the full setup.

### Option 3 — CLAUDE.md (developers)
Using Claude Code?
Drop a `CLAUDE.md` file in your project folder.
Claude Code reads it automatically at session start.
Scope it to the project — not everything, just what's relevant.

---

## The Result

Sessions that start with full context instead of zero.
Output calibrated to your actual work, voice, and goals.
No re-explaining. No generic responses.

The protocol is the difference between
Claude as a tool and Claude as a system.

---

*Part of [claude-os](https://github.com/aunysillyme-dev/claude-os) —
a multi-AI operating system built in public.*
