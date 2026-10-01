---
title: Multi-AI Stack Overview
description: Maps each AI tool in the aunysillyme-dev stack to a single role (chief of staff, research, second opinion, visual production, music, content operations, build, system, distribution) and shows how the tools hand off to each other across four end-to-end workflows.
category: architecture
status: active
tags:
  - multi-ai
  - ai-orchestration
  - claude
  - mcp
  - workflow-automation
---

# Multi-AI Stack Overview
### Why one tool isn't enough — and how to run many as one system

---

## The Problem With Single-Tool Workflows

Most people pick one AI and use it for everything.
Claude for writing. ChatGPT for brainstorming. Gemini occasionally.
No defined roles. No handoff patterns. No system.

The result: mediocre output across the board.
Not because the tools are bad.
Because no tool is best at everything.

---

## The Design Principle

Every tool in this stack has one job.
When it does that job, nothing does it better.
When the task falls outside that job, a different tool runs it.

No overlap. 

---

## The Stack — By Role

### Chief of Staff
**Claude**

Primary reasoning engine. Writes, strategizes, synthesizes,
manages the vault, runs session protocols, and coordinates
every other tool in the stack.

Claude doesn't just produce output.
It reads context before every session, routes tasks to the
right tool, and updates the system after every session ends.

Everything flows through Claude first.

---

### Research Layer

**Gemini + Google Pro** — deep research, long-context analysis,
competitor breakdowns, trend reports. Called on demand
when a topic needs real depth.

**Claude Research** — focused deep dives on specific topics.
Runs inside the same session without switching tools.

**Grok** — X-native real-time data. What's moving on the
platform right now. Trending topics, viral post mechanics,
platform-specific context.

**Claude (automated)** — baseline pillar research runs
automatically at content session start. Built into the protocol.
No prompting required.

The rule: Claude synthesizes everything.
It never outputs raw research — it translates it into
actionable direction before a single word gets written.

---

### Second Opinion Layer

**ChatGPT** — alternative angles, second-opinion drafts,
different framing on strategy decisions.

Used when Claude's output needs a challenger.
Not a replacement — a pressure test.

---

### Visual Production

**Grok Imagine** — dark cinematic character art,
consistent visual identity across the brand.

**GPT Image 2.0** — complex scene compositions,
thumbnails requiring text overlay, multi-element images.

**Three.js MCP** — 3D scene generation and interactive
animations. Runs directly inside Claude sessions via MCP.

**Claude Design** — all digital products, PDFs, brand kits,
formatted assets. Primary design tool for anything
that gets published or sold.

**Canva** — fallback for quick social graphics only.

---

### Music Production

**Suno** — AI music generation. Prototyping tracks,
building full productions from prompt to audio.

**Soundboost AI** — audio mastering for distribution-ready output.

**DistroKid** — distribution to all streaming platforms
+ streaming revenue collection.

**ASCAP** — performing rights registration
+ performance royalty collection.

Two separate revenue streams from every release:
streaming revenue (DistroKid) and performance royalties (ASCAP).

---

### Content Operations

**Typefully** — scheduling across X, LinkedIn, Threads, Bluesky.
Analytics pulled via MCP before every content session.

**Beehiiv** — newsletter. Explained Without Fluff.
Draft data accessible via MCP mid-session.

**NotebookLM** — newsletter issues converted to
cinematic video overviews.

**CapCut** — short-form video production and editing.

---

### Build Layer

**Claude Code** — website builds, software, tools.
aunysillyme.com and auny.media/links both built here.

**Claude Cowork** — autonomous desktop agent.
Form filling, web extraction, file management,
admin tasks at scale.

---

### System Layer

**Obsidian** — the vault. Single source of truth
for every tool in the stack. Connected via MCP.
Claude reads it before every session. And writes updates.

**Linear** — project and workflow tracking.
Issues pulled and updated via MCP whenever needed.

---

### Distribution & Monetization

**Buy Me a Coffee** — digital product delivery,
one-time purchases.

**X Ads** — paid discovery. Only runs on content
that's already proven organically.

**Calendly** — client booking for consulting
and strategy calls.

---

## How The Tools Talk To Each Other

Most tools in this stack don't operate in isolation.
They feed each other.

```
CONTENT WORKFLOW
Gemini/Grok (research) → Claude (synthesize + write) → Typefully (schedule) 

MUSIC WORKFLOW
Suno (generate) → Soundboost AI (master) → DistroKid (distribute) → ASCAP (register royalties) → Revenue from two streams

PRODUCT WORKFLOW
Claude (write + design brief) → Claude Design (build PDF/product) → Buy Me a Coffee / Gumroad (deliver + sell)

WEBSITE WORKFLOW
Claude (architecture + direction) → Claude Code (build) → Claude Cowork (populate + maintain)
```
---

## The MCP Layer

These tools connect directly to Claude.
No tab switching. No copy-paste.
Claude reads and writes to them whenever needed.

| MCP | SESSION ROLE |
|-----|-------------|
| Obsidian | Vault read before every session |
| Linear | Tasks pulled + updated in real time |
| Typefully | Queue + analytics before content sessions |
| Beehiiv | Newsletter data on demand |
| Google Drive | Documents, research, novel drafts |
| Gmail | Communications |
| Google Calendar | Scheduling context |
| Canva | Design asset access |
| Calendly | Client booking |
| Three.js | 3D generation in-session |

---

## What This Stack Produces

| OUTPUT | TOOLS IN THE CHAIN |
|--------|-------------------|
| Daily content (10+ posts/week) | Grok + Claude + Typefully |
| Newsletter issues | Claude + Beehiiv + NotebookLM |
| Music releases | Suno + Soundboost AI + DistroKid + ASCAP |
| Digital products | Claude + Claude Design + Buy Me a Coffee |
| Websites + tools | Claude + Claude Code |
| Visual content | Grok Imagine + GPT Image 2.0 + CapCut |
| Client work | Claude + Obsidian + Calendly + Linear |

---

## The Result

A one-person operation running at the output level
of a small team — without the overhead.

Every tool has a role.
Every session starts with context.
Every output has a documented workflow behind it.

---

*Part of [claude-os](https://github.com/aunysillyme-dev/claude-os) —
a multi-AI operating system built in public.*

