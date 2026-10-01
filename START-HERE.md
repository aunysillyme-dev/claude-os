---
title: Start Here
description: Quick navigation guide for new users to the claude-os repository. Choose your path based on time available and depth needed.
category: guide
status: active
difficulty: beginner
reading_time: 5-15 minutes
tags:
  - getting-started
  - navigation
  - documentation
---

# Start Here 🕷️
### New to this repo? Read these files in this order.

---

## What This Repo Is

A documented, production-tested framework for running Claude
and a coordinated stack of AI tools as an actual operating system.

Not a prompt file. Not a CLAUDE.md hack.
A complete architecture with session routing, vault memory,
persona framework, and real workflow outputs.

Everything here has been run in production.
Nothing is theoretical.

---

## If You Have 5 Minutes

Read this one file first:

**[README.md](./README.md)**

It tells you what the system is, what's in the repo,
and what it has actually produced.

---

## If You Want To Understand The System

Read in this order:

**1. How sessions start**
[session-protocol/01_session_start_protocol.md](./session-protocol/01_session_start_protocol.md)

The routing system that loads context before every session.
The most important file for understanding how it works.

**2. What tools do what**
[multi-ai-stack/02_tool_to_task_mapping.md](./multi-ai-stack/02_tool_to_task_mapping.md)

Every tool in the stack with a defined role.
No overlap. No guessing.

**3. The vault architecture**
[vault-architecture/01_obsidian_structure.md](./vault-architecture/01_obsidian_structure.md)

How Obsidian becomes the memory layer for every AI tool.
The foundation everything else runs on.

---

## If You Want To Set It Up

**Connect Obsidian to Claude:**
[vault-architecture/02_mcp_connection_guide.md](./vault-architecture/02_mcp_connection_guide.md)

Step-by-step MCP setup. No code required.

**Connect Grok for live X search:**
[integrations/01_grok-mcp-setup.md](./integrations/01_grok-mcp-setup.md)

How to give Claude real-time X post search inside every session.

**Connect Remotion for video generation:**
[integrations/03_remotion-mcp-setup.md](./integrations/03_remotion-mcp-setup.md)

How to generate TikTok slideshows directly from Claude sessions.

---

## If You Want To Go Deep

**The full stack overview:**
[multi-ai-stack/01_stack_overview.md](./multi-ai-stack/01_stack_overview.md)

Every tool, every role, every workflow chain.

**Visual system guide:**
[VISUAL-GUIDE.md](./VISUAL-GUIDE.md)

The system laid out visually.

---

## What's Being Built Next

- `persona-framework/` — how to build a named AI persona with a real job description
- `workflow-builds/` — end-to-end documented workflows with real outputs
- `cognitive-ai-interface` — a separate repo on giving Claude your decision architecture

Follow the build:
- X: [@AunySillyMe](https://x.com/AunySillyMe)
- Newsletter: [Explained Without Fluff](https://explained-without-fluff.beehiiv.com)

---

*aunysillyme-dev/claude-os — a multi-AI operating system being built in public.*
*Learning as I go. Sharing it all for you.* 🕷️
