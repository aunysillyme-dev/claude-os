---
title: Tool-to-Task Mapping
description: Master routing table mapping every task type (writing, research, visual, music, code, automation) to its primary tool with fallbacks, plus the four core rules (design, research, automation, memory) that keep tool selection consistent instead of guessed.
category: reference
status: active
tags:
  - multi-ai
  - routing
  - mcp
  - automation
  - claude
---

# Tool-to-Task Mapping
### The right tool for every job — no guessing, no overlap

---

## The Problem This Solves

Most people pick one AI tool and use it for everything.
Or they have ten tools and don't know which one to use when.

Both approaches produce mediocre output.

This is the mapping that runs the aunysillyme-dev stack.
Every tool has a defined role.
No tool does everything.
Every tool does what it does best.

---

## The Decision Rule

Before picking a tool, answer two questions:

1. **What type of output do I need?**
   Text / Image / Video / Music / Code / Data / Design

2. **What does this specific task require?**
   Research / Creation / Automation / Scheduling / Analysis

The intersection points to the right tool.

---

## How the Stack Actually Runs

This is not a list of tools that get opened manually.
It's an automated system with defined triggers.

Every domain has its own Claude instance with a scoped
reading list — vault files, protocols, and context loaded
automatically at session start before any work begins.

**Content session starts:**
Claude reads the content manual, working manual, analytics
patterns, and content pillars. Then it pulls the Typefully
queue, runs trend research across all content pillars,
and states content direction before writing a single post.
Research is automatic — not prompted.

**Music session starts:**
Claude reads the music manual, lyric analysis, Suno analysis,
and discography. Voice calibration from existing lyrics
happens before any new writing.

**Workflow session starts:**
Claude reads the AI operating system doc, workflow system,
and active workflow log. Linear issues get pulled
and any in-progress work gets flagged immediately.

**Identity / psychology sessions:**
Vault is not accessed unless explicitly requested.
This domain runs on direct conversation first.

---

## The Research Layer

Research runs in three modes depending on depth needed:

| MODE | TOOL | WHEN |
|------|------|------|
| Automated pillar scan | Claude | Every content session, before writing |
| X-specific real-time trends | Grok | When platform context matters |
| Deep research / competitor analysis | Claude Research | On demand, specific topics |
| Long-form trend reports | Gemini Deep Research | On demand, complex research tasks |
| Synthesis of all findings | Claude | Always — Claude synthesizes, never just reports |

**The rule:** Claude runs baseline research automatically.
Deeper tools get called when the task demands it.
Claude always synthesizes before output begins.

---

## Master Mapping Table

| TASK | PRIMARY TOOL | SECONDARY | NEVER USE |
|------|-------------|-----------|-----------|
| Long-form writing | Claude | ChatGPT (alt angle) | Grok |
| Social posts | Claude | — | — |
| Newsletter issue | Claude + Beehiiv | — | — |
| Trend research — content pillars | Claude (automated) | Grok (X-specific) | — |
| Deep research | Claude Research | Gemini Deep Research | — |
| Second opinion on strategy | ChatGPT | — | — |
| Voice calibration | Claude + vault | — | — |
| Character art / cinematic visuals | Grok Imagine | — | — |
| Complex scene / thumbnail with text | GPT Image 2.0 | Grok Imagine | — |
| 3D scenes / animations | Three.js MCP | — | — |
| Digital product / PDF / brand kit | Claude Design | Canva | — |
| Quick social graphic | Claude Design | Canva (fallback) | — |
| Music generation | Suno | — | — |
| Audio mastering | Soundboost AI | — | — |
| Music distribution | DistroKid | — | — |
| Royalty registration | ASCAP | — | — |
| Website build | Claude Code | — | — |
| Desktop automation / admin | Claude Cowork | — | — |
| Web extraction / reading sites | Claude Cowork | — | — |
| Form filling / registrations | Claude Cowork | — | — |
| Content scheduling | Typefully | — | — |
| Analytics review | Typefully + Claude | — | — |
| Newsletter management | Beehiiv | — | — |
| Newsletter → video | NotebookLM | — | — |
| Video production / editing | CapCut | — | — |
| Project tracking | Linear | — | — |
| Source of truth / memory | Obsidian | — | — |
| Client booking | Calendly | — | — |
| Digital product delivery | Buy Me a Coffee | — | — |
| Paid content discovery | X Ads | Google Ads | — |

---

## By Output Type

### Text Output

| OUTPUT | TOOL |
|--------|------|
| Social posts | Claude |
| Newsletter issue | Claude |
| Long-form article | Claude |
| Strategy document | Claude |
| Second opinion / alt draft | ChatGPT |
| X-native content with trending context | Grok |

---

### Visual Output

| OUTPUT | TOOL |
|--------|------|
| Character art — dark cinematic | Grok Imagine |
| Abstract / surreal art | Grok Imagine |
| Thumbnail with text overlay | GPT Image 2.0 |
| Multi-element scene composition | GPT Image 2.0 |
| PDF / digital product | Claude Design |
| Brand kit / formatted asset | Claude Design |
| Quick social graphic | Claude Design |
| 3D scene / interactive visual | Three.js MCP |

---

### Audio / Music Output

| OUTPUT | TOOL |
|--------|------|
| Original track / prototype | Suno |
| Mastered audio for distribution | Soundboost AI |
| Distributed to streaming platforms | DistroKid |
| Royalties registered | ASCAP |

---

### Video Output

| OUTPUT | TOOL |
|--------|------|
| Newsletter → cinematic overview | NotebookLM |
| Short-form edited video | CapCut |

---

### Code / Build Output

| OUTPUT | TOOL |
|--------|------|
| Website build | Claude Code |
| Links page | Claude Code |
| Software / tool build | Claude Code |

---

### Automation Output

| OUTPUT | TOOL |
|--------|------|
| Form filling / registrations | Claude Cowork |
| Web data extraction | Claude Cowork |
| File management | Claude Cowork |
| Admin tasks | Claude Cowork |

---

## Core Rules

### The Design Rule
Claude Design is the primary tool for ALL product design,
PDF creation, brand kits, digital downloads, and layout work.
Canva is a fallback for quick social graphics only.
Never use Canva for anything Claude Design can do.
Make Claude ideate and write Claude Design prompts based on branding context.

### The Research Rule
Claude runs pillar research automatically at content session start.
No prompting required — it's built into the protocol.
Deeper research uses Claude Research or Gemini Deep Research on demand.
Grok handles X-specific real-time trends.
Claude always synthesizes before output begins.

### The Automation Rule

Two types of automation run in this stack. They are not the same.

**Type 1 — Task Automation (Claude Cowork)**
If a task is repetitive, form-based, or requires reading
and extracting from websites — Claude Cowork handles it autonomously.
Do not do manually what Cowork can do in one session.

Examples:
- Registering a 150-song catalog on ASCAP
- Building hyperfollow pages across every release
- Extracting data from a website
- Filling out forms at scale

**Type 2 — Session Automation (MCPs)**
MCPs run automatically before and during sessions.
No prompting required. Claude reads and writes to connected
tools mid-session without switching tabs or copy-pasting.

How MCPs preface tasks:
- Before content sessions → Obsidian vault is read automatically
- Before writing anything → Typefully queue and analytics are pulled
- During workflow sessions → Linear issues are checked and updated
- During newsletter work → Beehiiv draft data is accessible
- During any session → Google Drive, Gmail, Calendar available on demand

The rule: if a tool is MCP-connected, Claude accesses it directly.
Never ask Claude to "go check" something it can already read.

### The Memory Rule
Obsidian is the single source of truth.
Every Claude tool reads from the same vault.
Nothing gets re-explained between sessions.
If it's not in the vault, it doesn't exist to the system.

---

## Connected MCPs

These tools connect directly to Claude via MCP.
No copy-paste. No tab switching.
Claude reads and writes to them mid-session.

| MCP | WHAT CLAUDE CAN DO |
|-----|-------------------|
| Obsidian | Read vault files before every session |
| Linear | Pull tasks, update status, create issues |
| Typefully | Pull analytics, read queue, schedule posts |
| Beehiiv | Read newsletter drafts and performance data |
| Google Drive | Access documents, research files, novel drafts |
| Gmail | Read and draft communications |
| Google Calendar | Scheduling and time context |
| Canva | Design asset management |
| Calendly | Client booking management |
| Three.js | 3D scene generation in-session |

---

*Part of [claude-os](https://github.com/aunysillyme-dev/claude-os) —
a multi-AI operating system built in public.*
