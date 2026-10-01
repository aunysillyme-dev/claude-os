---
title: Obsidian Vault Structure
description: Explains how to structure an Obsidian vault as an AI memory layer, shows an example emoji-prefixed folder tree, lists which files Claude reads per session domain, and gives a six-step guide to building the vault and connecting it to Claude over MCP.
category: architecture
status: active
tags:
  - obsidian-vault
  - vault-structure
  - mcp
  - knowledge-management
  - routing
---

# Obsidian Vault Structure
### How to build a second brain that every AI tool reads from automatically

---

## What This Is

Most people store notes in Obsidian.
This is not just a note-taking app. 
It's a whole system you should utilize as your second brain.

Use Obsidian as a cognitive operating system; 
a structured vault that connects to Claude via MCP 
and becomes the memory layer for every AI tool in your stack.

Every session starts by reading it.
Every output is shaped by what's in it.
Nothing gets re-explained between sessions.

---

## The Core Concept

Claude has no memory between sessions by default.
- Projects help. 
- Custom instructions help.
But neither gives Claude the full picture of who you are,
how you work, what you've built, and what matters.

Your vault does.

When Claude reads the vault before a session,
it knows:
- Your psychological and cognitive profile
- Your voice, tone, and content rules
- Your full AI stack and how it runs
- Your active projects and open tasks
- Your analytics patterns and what's working
- Every workflow you've ever built

Not as a dump of information.
As a structured, routable knowledge base
that loads only what the current task needs.

---

## The Folder Structure
```
Vault/
├── _index.md                        ← master map — read this first
├── 📐 Claude Protocols/             ← how Claude operates
├── ✍️ Content & Brand/              ← voice, content manual, visual brand
├── 🤖 AI Workflows/                 ← system docs, stack, workflow log
├── 🎵 Music/                        ← music manual, lyrics, releases
├── 💼 Consulting Business/          ← services, clients, ad strategy
├── 🌹 DNA of a Rose/                ← novel project
├── 💰 Monetizable Ideas/            ← idea cards, product pipeline
├── 📬 Newsletter (EWF)/             ← Explained Without Fluff
├── 📲 Social Media Playbook/        ← platform strategies
├── 📖 X Growth Playbook/            ← X-specific growth system
├── 🧠 Identity/                     ← voice, values, how you think
├── 🐙 Git Repo/                     ← GitHub build log
└── 📝 Tasks/                        ← active task tracking
```
---

## The Key Files Claude Reads

### Every Session
- `_index.md` — master map, orientation file
- Domain-specific protocol file (routes the session)

### Content Sessions
- `📐 Claude Protocols/02_content_session_protocol.md`
- `✍️ Content & Brand/auny_content_manual.md`
- `✍️ Content & Brand/auny_working_manual.md`
- Analytics pulled via Typefully MCP

### Music Sessions
- `📐 Claude Protocols/03_music_session_protocol.md`
- `🎵 Music/auny_music_manual.md`
- `🎵 Music/auny_lyric_analysis.md`
- `🎵 Music/auny_suno_analysis.md`

### AI Workflow Sessions
- `🤖 AI Workflows/auny_ai_operating_system.md`
- `🤖 AI Workflows/00_workflow_system.md`
- `🤖 AI Workflows/02_trending_agents_log.md`

### Identity / Voice Calibration
- `🧠 Identity/auny_voice_calibration.md`
- `🧠 Identity/auny_who_am_i.md`
- `🧠 Identity/auny_core_identity.md`

---

## The Session Routing System

The vault doesn't get read in full every session.
That would be slow and inefficient.

Instead, the session protocol routes Claude
to read only what the current task needs.
```
Content task → read content files only
Music task → read music files only
Business task → read consulting files only
Identity work → read identity files only
```

---

## Why Emoji Folder Names

Every top-level folder has an emoji prefix.
This is not aesthetic.

Emojis force alphabetical sorting to break —
folders sort by emoji unicode order instead.
This puts the most-accessed folders at the top
and creates a visual hierarchy without nesting everything.

It also makes the vault scannable at a glance.
Claude reads the folder names when navigating.
The emoji signals what domain the folder belongs to
faster than words alone.

---

## The MCP Connection

This is what makes the vault an AI memory layer
instead of just a notes app.

MCP (Model Context Protocol) connects Obsidian
directly to Claude. Claude can read and write
vault files mid-session without any copy-paste.

**What Claude can do via MCP:**
- Read any vault file before executing a task
- Write new files (session logs, idea cards, workflow docs)
- Append updates to existing files
- Search across the entire vault
- List folder contents to orient before reading

**What this means in practice:**
```
Claude opens a content session → reads the content manual automatically → pulls Typefully analytics via MCP
→ runs research → writes posts → logs anything new back to the vault
```

Zero manual context loading.
Zero re-explaining between sessions.

---

## How To Build This For Yourself

### Step 1 — Install Obsidian
Free. obsidian.md. Works on Mac, Windows, iOS, Android.

### Step 2 — Create Your Folder Structure
Start with 3-5 folders. Don't over-architect.
Suggested minimum:
```
Vault/
├── _index.md
├── AI Protocols/
├── Content/
├── Projects/
└── Identity/
```
### Step 3 — Build Your Core Files
Before connecting to Claude, you need content worth reading.

**Start with these four:**
1. Who you are (background, role, what you're building)
2. Your voice and tone guide (how you write, what to avoid)
3. Your current focus (active projects, immediate goals)
4. Your AI stack (what tools you use and why)

These four files give Claude enough to produce
calibrated, on-brand output from session one.

### Step 4 — Connect Obsidian to Claude via MCP
This requires installing the Obsidian MCP plugin
and connecting it in Claude's settings.

Full setup guide: `vault-architecture/02_mcp_connection_guide.md`

### Step 5 — Write a Session Protocol
Tell Claude explicitly what to read before every session.
Throw it in your project instructions. 
Even a basic version works. Here's an example of mine:
```
You are Claude, chief of staff and Black Widow 🕷️ to Auny (@AunySillyMe).

This project is for building, documenting, and monetizing AI workflows and agent systems. Every workflow we build is simultaneously a tool, a content piece, and a product.

VAULT FIRST: At the start of every session, access the Obsidian vault and read:
1. _index.md (master map)
2. 📐 Claude Protocols/01_session_start_protocol.md
3. 🤖 AI Workflows/00_workflow_system.md
4. 🤖 AI Workflows/01_ai_stack_inventory.md

When Auny drops a trending agent post from X: run the full teardown framework from 📐 Claude Protocols/05_workflow_build_protocol.md and log it in 🤖 AI Workflows/02_trending_agents_log.md.

Always check 🤖 AI Workflows/03_monetization_tracker.md for revenue angles.

Auny's AI stack includes: Claude, Claude Cowork, ChatGPT, Gemini + Google Pro, Grok + Grok Imagine, Suno, Soundboost AI, Linear AI, Obsidian, Typefully, Beehiiv, Canva.

Build balanced: speed + depth + visibility. Every workflow is content before it's a product.
```
### Step 6 — Use It. Update It.
The vault is only valuable if it reflects current reality.

Update it when:
- Preferences change
- New projects start
- Workflows get built
- Decisions get made

Claude can write updates back to the vault mid-session.
You never have to manually maintain it.

---

## What This Is Not

- Not a Notion database
- Not a journaling app
- Not a second brain for personal knowledge management

It is infrastructure. 
The same way a database serves an application, the vault serves your AI stack. 
Any LLM you choose to use can read your vault and gather context on you right away. 
So you never lose your data, never have to feel like 'oh but GPT knows so much about me, 
i would have to start from scratch'  when a new, more powerful and capable model comes out!

Your system gets smarter over time automatically.

---

## The Result

Now I run a one-person AI stack with a shared memory layer. 
No tool starts from zero. No context gets lost between sessions.
The system compounds - every session builds on the last - across all LLMs and AI tools.

---

*Part of [claude-os](https://github.com/aunysillyme-dev/claude-os) —
a multi-AI operating system being built in public.
Learning as I go. Sharing it all for you*
