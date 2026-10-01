---
title: MCP Connection Guide
description: Step-by-step guide to installing the Obsidian Local REST API plugin, connecting Obsidian to Claude over MCP, testing the connection, building a starter vault structure, and troubleshooting common connection issues.
category: integration
status: active
tags:
  - mcp
  - obsidian-vault
  - integration-guide
  - vault-structure
  - troubleshooting
---

# MCP Connection Guide
### How to connect Obsidian to Claude and make your vault the memory layer for every session

---

## What MCP Is

MCP stands for Model Context Protocol.
It's an open standard that lets Claude connect directly
to external tools — reading and writing to them mid-session
without copy-paste, without tab switching, without re-explaining.

When Obsidian is connected via MCP:
- Claude reads your vault files before every session
- Claude writes new files and updates existing ones
- Your vault becomes a shared memory layer across every AI tool

This is what turns a notes app into an operating system.

---

## What You Need

- Obsidian installed (free — obsidian.md)
- Claude Pro or higher (MCP requires a paid plan)
- The Obsidian Local REST API plugin
- 15 minutes

---

## Step 1 — Install the Obsidian Local REST API Plugin

This plugin creates a local server that Claude connects to.

1. Open Obsidian
2. Go to **Settings** → **Community Plugins**
3. Turn off Safe Mode if prompted
4. Click **Browse** and search for **"Local REST API"**
5. Install and enable it
6. Go to the plugin settings and note the **API Key** it generates
   — you'll need this in Step 3

The plugin runs a local server on port 27124 by default.
Keep Obsidian open whenever you want Claude to read the vault.

---

## Step 2 — Find Your Vault Path

Claude needs to know where your vault lives on your machine.

**Mac:**
Open Obsidian → click the vault name bottom left →
"Manage Vaults" → hover over your vault name →
the path appears (something like `/Users/yourname/Documents/MyVault`)

**Windows:**
Same steps — path will look like `C:\Users\yourname\Documents\MyVault`

Copy this path. You'll need it in the next step.

---

## Step 3 — Connect Obsidian to Claude

1. Go to **claude.ai**
2. Click your profile → **Settings** → **Integrations**
3. Find **Obsidian** in the connectors list
4. Click **Connect**
5. Enter your **API Key** from Step 1
6. Enter your **vault path** from Step 2
7. Click **Save**

Claude will confirm the connection.
From this point forward, Claude can read and write
to your vault directly from any conversation.

---

## Step 4 — Test the Connection

Open a new Claude conversation and type:

```
List the files in my Obsidian vault root directory.
```

Claude should return your vault's top-level folder structure.

If it does — you're connected. 🕷️

If it doesn't:
- Make sure Obsidian is open
- Make sure the Local REST API plugin is enabled
- Double-check the API key and vault path
- Try restarting Obsidian and reconnecting

---

## Step 5 — Build Your Vault Structure

Connection is live. Now give Claude something worth reading.

An empty vault or an unstructured one gives Claude nothing
useful to work with. The quality of your vault directly
determines the quality of every session output.

**Recommended minimum structure:**
```
Vault/
├── _index.md              ← master map — what lives where
├── AI Protocols/          ← how Claude operates in each domain
├── Content/               ← voice guide, content manual
├── Projects/              ← active work, context per project
└── Identity/              ← who you are, how you think
```
**The four files to build first:**
1. `_index.md` — master map of your vault
2. Voice and tone guide — how you write, what to avoid
3. Current focus — active projects and immediate goals
4. Your AI stack — what tools you use and why

Full vault structure guide and reference:
[vault-architecture/01_obsidian_structure.md](https://github.com/aunysillyme-dev/claude-os/blob/main/vault-architecture/01_obsidian_structure.md)

Once these folder system exists — move to Step 6.

---

## Step 6 — Tell Claude How to Use It

Connection is live. Vault has structure. 
Now tell Claude what to read and when.

Without explicit instructions, Claude won't know
which files matter or in what order to read them.
This is where the session protocol comes in.

Add this to your Claude Project instructions
under **Settings → Project Instructions:**
```
VAULT FIRST: At the start of every session,
access the Obsidian vault and read:

_index.md — the master map
The relevant protocol file for this domain
Any active task or context files

Do not begin work until these files are read.
If vault access fails, say so before proceeding.
```
This makes vault reading automatic —
Claude does it before every session without being asked.

**Want to go deeper?**
Scope the instructions by domain so Claude loads
only what's relevant to the current task —
not everything in the vault at once.

Example:
```
Content work → read content manual + voice guide
Music work   → read music manual + lyric analysis
Business     → read services doc + active client context
```
Full session routing guide:
[session-protocol/01_session_start_protocol.md](https://github.com/aunysillyme-dev/claude-os/blob/main/session-protocol/01_session_start_protocol.md)

Once your instructions are set — your system is live.
Every session starts with full context from this point forward.

---

| ACTION | COMMAND |
|--------|---------|
| Read a specific file | "Read [filename] from my vault" |
| List folder contents | "List files in [folder name]" |
| Search across vault | "Search my vault for [topic]" |
| Create a new file | "Create a new file called [name] in [folder]" |
| Append to existing file | "Add this to [filename]" |
| Update a file | "Update [filename] with [content]" |

You rarely need to use these commands directly.
If the session protocol is set up correctly,
Claude reads what it needs automatically.

---

## How the Reading Works in Practice

**Content session:**
```
Claude reads:
→ _index.md (orientation)
→ content_manual.md (voice + rules)
→ working_manual.md (current focus)
→ Pulls Typefully queue via MCP
→ Runs research
→ Writes posts
→ Logs session notes back to vault
```

**Music session:**
```
Claude reads:
→ _index.md
→ music_manual.md
→ lyric_analysis.md (if writing lyrics)
→ suno_analysis.md (if building prompts)
→ discography.md (to avoid thematic repeats)
```

**Workflow build session:**
```
Claude reads:
→ _index.md
→ ai_operating_system.md
→ workflow_system.md
→ Pulls open Linear issues via MCP
→ Builds workflow
→ Documents output back to vault
```

Zero manual loading. Zero re-explaining.

---

## Keeping Multiple MCPs Connected

Obsidian doesn't have to be the only MCP.

The full stack runs with:

| MCP | WHAT IT ADDS |
|-----|-------------|
| Obsidian | Vault — source of truth |
| Linear | Task management in real time |
| Typefully | Content queue + analytics |
| Beehiiv | Newsletter data |
| Google Drive | Documents + research files |
| Gmail | Communications |
| Google Calendar | Scheduling context |

Each MCP connects independently.
Claude uses whichever ones are relevant to the current task.

---

## Common Issues

**Claude says it can't access the vault:**
- Is Obsidian open? The plugin only runs while Obsidian is active.
- Did the API key change? Regenerate in plugin settings and reconnect.

**Claude reads stale information:**
- Update the vault file. Claude reads what's there — if a file is outdated, the output reflects that.

**Claude creates files in the wrong folder:**
- Be explicit in your session protocol. "All new files go in [folder name]" removes ambiguity.

**Connection works on desktop but not mobile:**
- The Local REST API plugin runs locally. Mobile Claude can't reach a local server on your desktop.
- Solution: Keep a desktop Claude session open for vault-connected work.

---

## What This Unlocks

Once the vault is connected and the session protocol is set:

Every session starts with full context.
Every output reflects your actual voice, goals, and current work.
Every workflow update gets written back automatically.

The system builds on itself.
The longer you run it, the more calibrated it gets.
You never start from zero again.

---

*aunysillyme-dev/claude-os — a multi-AI operating system being built in public.*
*Learning as I go. Sharing it all for you.* 🕷️
