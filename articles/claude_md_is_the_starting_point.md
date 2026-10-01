---
title: CLAUDE.md is the Starting Point
description: Explains the five-layer context system (CLAUDE.md, vault, MCP, protocols, routing) that extends a basic CLAUDE.md file into a full persistent-memory system for Claude, with templates for each layer and a four-week rollout plan.
category: article
status: active
tags:
  - claude-md
  - vault
  - mcp
  - protocols
  - routing
  - context-engineering
---

# CLAUDE.md is the Starting Point

A markdown file at the root of your project tells Claude how to behave inside that project. It is useful. It is also the starting point — not the whole system.

Most people stop at CLAUDE.md. They write a 60–100 line file, watch their output improve, and assume the architecture is solved.

It isn't.

CLAUDE.md handles one slice of one session in one project. The real leverage — context that persists across chats, memory that survives a fresh terminal, protocols that load before every task, routing rules that decide which tool to call when — sits in the layers above it. None of it lives in CLAUDE.md.

This article is what comes after the starting point. Five layers. Templates included. Production-tested.

---

## The Five-Layer Context System

```
┌─────────────────────────────────────────┐
│  Layer 0: CLAUDE.md  (starting point)   │
│  Layer 1: Vault      (knowledge base)   │
│  Layer 2: MCP        (memory layer)     │
│  Layer 3: Protocols  (session rules)    │
│  Layer 4: Routing    (tool selection)   │
└─────────────────────────────────────────┘
```

Each layer solves a problem the layer before it can't.

| LAYER | WHAT IT DOES | WHAT BREAKS WITHOUT IT |
|---|---|---|
| CLAUDE.md | Per-project behavior rules | Claude defaults to generic output |
| Vault | Structured knowledge Claude can read | Re-explaining yourself every session |
| MCP | Persistent memory across sessions | No continuity between chats |
| Protocols | Repeatable workflows Claude loads on demand | Inconsistent execution of the same task |
| Routing | Which tool to call for which job | Claude reaches for the wrong tool |

---

## Layer 0 — CLAUDE.md (The Starting Point)

CLAUDE.md sits at the root of a project. Claude Code reads it automatically. Claude Desktop reads it when added as project context.

It's good for:
- Naming the project and its scope
- Listing tools available in this context
- Stating behavior rules that apply everywhere in this project
- Pointing to deeper docs (this is the part most people skip)

**Minimal viable CLAUDE.md:**

```markdown
# [Project Name]

## What This Is
[One sentence — what gets built or maintained here]

## Tools Available
- [Tool 1]
- [Tool 2]

## Behavior Rules
- [Rule 1]
- [Rule 2]

## Read Before Every Session
- [path/to/protocol.md]
- [path/to/context.md]
```

The last section is what makes CLAUDE.md a routing file instead of a wall-of-text file. Point Claude at the deeper layers. Don't try to cram everything into one file.

---

## Layer 1 — The Vault

A vault is a folder of markdown files structured for an AI to read.

Obsidian is the most common host because of wikilinks (`[[file_name]]`) and folder emoji prefixes (which make file paths legible to both humans and machines). But the vault doesn't require Obsidian — any folder of markdown files works. Cursor, VS Code, plain filesystems all work.

**What goes in the vault:**
- Domain knowledge (your work, your projects, your decisions)
- Reference data (links, IDs, credentials documented but not stored as plaintext)
- Frameworks and processes you re-use
- Notes from past sessions

**What does NOT go in the vault:**
- API keys, passwords, secrets (use environment variables)
- Anything you wouldn't paste into a Claude prompt

**Sample vault structure (production-tested):**

```
my-vault/
├── HQ.md                          # vault map — read first every session
├── 📐 Protocols/                   # session rules
│   ├── README.md
│   ├── 01_session_start.md
│   └── 02_content_session.md
├── 🤖 Workflows/                   # documented multi-step processes
│   ├── README.md
│   └── workflow_name.md
├── ✍️ Content/                     # domain knowledge — content
│   ├── README.md
│   └── voice_manual.md
├── 💼 Business/                    # domain knowledge — business
│   └── ...
└── 📝 Session_Log.md              # cross-chat change log
```

**The pattern:**
- Root has an HQ file — the vault map (table of contents with wikilinks)
- Top-level folders use emoji prefixes for visual scanning and unique searchability
- Each folder has a README.md explaining what's inside and how to use it
- A session log at the root records changes across chats (this is how context survives sessions)

**HQ.md template:**

```markdown
# Vault HQ

## 📐 Protocols
- [[01_session_start]] — read first every session
- [[02_content_session]] — content batch workflow

## 🤖 Workflows
- [[workflow_name]] — what it does

## ✍️ Content
- [[voice_manual]] — voice + format rules

[...etc — every folder, every file, with one-line descriptions]
```

The HQ file is what Claude reads first to understand the entire vault's structure without crawling every folder.

---

## Layer 2 — MCP (Memory Layer)

MCP (Model Context Protocol) is how Claude reads and writes the vault as a live system instead of pasted text.

Two common patterns:

### Pattern A — Obsidian Plugin

Install the `obsidian-mcp-tools` plugin. Connect Claude Desktop to the local Obsidian REST API. Claude can now read, write, search, and patch files in the vault on your machine.

**Pros:** Easy to set up. Works inside Obsidian's permission model.
**Cons:** Local only. Won't work on phone or other devices.

### Pattern B — Custom MCP Server

Build a dedicated MCP server that exposes vault operations as tools. Run it locally (Node + iCloud-synced vault folder) and/or host it on Cloudflare Workers / Vercel for remote access.

**Pros:** Works on any device. Survives Obsidian downtime. You control the tools exposed.
**Cons:** Build effort. Requires sync infrastructure if you want multi-device.

**Tools a vault MCP server should expose:**

| TOOL | PURPOSE |
|---|---|
| `read_file(path)` | Read a vault file |
| `write_file(path, content)` | Create or overwrite |
| `append_to_file(path, content)` | Append without overwriting |
| `delete_file(path)` | Remove a file |
| `list_files(directory?)` | Browse the vault |
| `search_files(query, directory?)` | Find files by content |
| `patch_file(path, find, replace)` | Targeted edits |

**Hybrid sync architecture (production-tested):**

```
Local vault (iCloud Drive)
    ↓
auto-sync script (launchd / cron) every 2 min
    ↓
GitHub private repo (vault backup)
    ↓
Cloudflare Worker (hosted MCP)
    ↓
Claude (any device)
```

This lets you write to the vault locally while reading from the vault remotely. Same context. Multiple devices.

---

## Layer 3 — Protocols

Protocols are repeatable workflows you've written down so you stop re-explaining them every session.

A protocol is just a markdown file in the vault. Claude reads it when triggered. The triggers are the magic.

**Common protocol types:**

| PROTOCOL | TRIGGER | WHAT IT DOES |
|---|---|---|
| Session start | Start of every chat | Loads HQ, checks tasks, reads task-specific rules |
| Content batch | "build me content" | Pulls analytics, runs research, drafts in queue format |
| Code review | "review this PR" | Loads style guide, checks for known anti-patterns |
| Project handoff | End of work session | Updates task tracker, logs changes |

**Protocol template:**

```markdown
# [Protocol Name]
### One-line description
*Triggers: "phrase 1" / "phrase 2"*

---

## When To Run This
[Specific conditions]

## Step-By-Step
1. [Action] — [why]
2. [Action] — [why]
3. [Action] — [why]

## Hard Rules
- [Non-negotiable]
- [Non-negotiable]

## Output Format
[How the result should be presented]

## Related Files
- [[other_protocol]]
- [[related_context]]
```

**Session start protocol example:**

```markdown
# Session Start Protocol

## Read In This Order Every Session
1. Session_Log.md       — changes since last chat
2. HQ.md                — vault map
3. [task-specific protocol based on user's first message]

## Then Check
- Task tracker (Linear / Things / whatever) for open issues
- Recently modified files relevant to the task

## Then Act
```

A 20-line session start protocol replaces 50 lines of "remember to check X, Y, Z" you'd otherwise re-type into CLAUDE.md.

---

## Layer 4 — Routing Rules

Routing tells Claude which tool to call for which job.

In a multi-tool stack — web search, MCP servers, code execution, image generation — Claude can guess wrong. Routing rules eliminate the guessing.

**Routing rule template:**

```markdown
# Tool Routing Rules

## For [task type]
- Primary: [tool name]
- Fallback: [tool name]
- Never use: [tool name and why]

## For [task type]
- Primary: [tool name]
- Fallback: [tool name]

## Domain-Specific Routing
- [Domain] → [tool]
- [Domain] → [tool]
```

**Worked example:**

```markdown
## Vault Operations
- Primary: custom MCP server (works on any device)
- Fallback: Obsidian plugin (local only)
- Never use: copy-paste from vault into chat — defeats the system

## Web Research
- Primary: Grok x_search (for X-native research)
- Secondary: standard web_search (for everything else)
- Use Context7 for: documentation lookups specifically

## Code Execution
- Primary: Claude's code interpreter
- Fallback: terminal via Claude Code

## Image Generation
- Stylized character: [tool]
- Photorealistic scene: [tool]
- Concept/abstract: [tool]
```

Without routing, Claude reaches for whatever tool feels right in the moment. With routing, the choice is already made. Less guesswork. Cleaner output.

---

## How To Start

You don't build all five layers on day one. Stack them in order.

### Week 1 — CLAUDE.md + Vault Skeleton
- Write CLAUDE.md for one project (Layer 0)
- Create a vault folder with an HQ.md + 2–3 folders + 1 protocol file (Layer 1)
- Point CLAUDE.md at the vault

### Week 2 — Connect MCP
- Install obsidian-mcp-tools OR write a minimal custom MCP server (Layer 2)
- Connect Claude to read/write the vault
- Migrate domain knowledge into vault files

### Week 3 — Write Protocols
- Identify 2–3 workflows you repeat constantly (Layer 3)
- Write each as a protocol file
- Add triggers and references in HQ.md

### Week 4 — Add Routing
- List every tool Claude has access to in your stack (Layer 4)
- Write routing rules for the top 5 task types
- Test by running tasks and watching tool choice

After 4 weeks, you have a context system. Not a markdown file pretending to be one.

---

## What This Replaces

| BEFORE | AFTER |
|---|---|
| Re-explaining your project at the start of every chat | Claude reads HQ.md and knows |
| Forgetting which conventions you set last week | Protocols persist; conventions reload |
| Generic output that ignores your style | Voice/style rules in vault files |
| Wrong tool for the job | Routing rules pick correctly |
| Hand-pasting vault files into the chat | MCP reads them live |
| Starting from zero in every Claude Code session | Session start protocol loads context |

The point isn't to write more documentation. It's to write it once.

---

## Anti-Patterns

Things that look like Layer 1+ but aren't:

- **The 500-line CLAUDE.md.** Still Layer 0. Just bigger. Doesn't survive across projects.
- **A "knowledge base" Notion page.** Claude can't read it natively. Has to be re-pasted.
- **Long custom instructions in Claude project settings.** Useful but capped at the project. No portability.
- **Custom GPTs / Projects with system prompts.** Same problem — locked to one product, no MCP.
- **One big markdown file with everything in it.** Eventually unreadable for both you and Claude.

Modular files Claude can navigate beat one giant file every time.

---

## Templates Repo

All templates from this article — CLAUDE.md, HQ.md, session start protocol, content protocol, routing rules — are in the `templates/` directory of this repo.

To use:
1. Clone the repo
2. Copy `templates/CLAUDE.md` to your project root
3. Copy `templates/vault-skeleton/` to wherever you keep your vault
4. Edit the placeholders for your domain
5. Connect MCP per Layer 2 instructions

---

## What You Should See After Implementing

- Sessions start with full context, not a cold start
- Same task gets executed the same way every time
- Output stays on-voice without per-prompt reminders
- Tool selection stops being random
- You stop typing "remember to..." at the start of every chat

These are real outcomes from running this system in production. Not theoretical.

---

## Why This Matters

The bottleneck in AI output isn't the model. It's the architecture you give the model.

A more capable Claude on top of a flat CLAUDE.md still produces flat output. The same Claude on top of a vault, MCP, protocols, and routing produces work that's calibrated to you, your business, and your standards — every single session.

CLAUDE.md is the starting point.

The full system is everything that comes after it.

---

*aunysillyme-dev/claude-os — a multi-AI operating system being built in public.*
*Learning as I go. Sharing it all for you.* 🕷️
