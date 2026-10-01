---
title: Connecting Grok to Claude via MCP
description: Step-by-step setup guide for installing the Grok MCP server in Claude Desktop with an xAI API key, covering the install script, the connection test, and the session-protocol addition that routes live X search and DeepSearch into content research.
category: integration
status: active
tags:
  - grok
  - mcp
  - x-search
  - integration-guide
  - content-research
---

# Connecting Grok to Claude via MCP
### How to give Claude live X search and Grok's DeepSearch — 
### and why it changes content sessions completely

---

## What This Unlocks

Claude's built-in web search is good.
It doesn't search X natively.

Grok does two things nobody else does:

**1. Real-time X post search**
Search actual posts on X by keyword, handle, or date range.
Right now. Live. With citations.
Not web search that happens to include some tweets.
Actual X post search.

**2. DeepSearch**
Web research that's significantly more current
than standard search tools.

When Grok is connected to Claude via MCP,
Claude can call both of those tools
directly inside a session —
without switching apps, without copying anything.

**What this looks like in a content session:**
```
"Suggest me 5 posts on AI agents"
→ Claude reads your content pillars from the vault
→ Claude calls Grok's x_search for what's trending
on X about AI agents right now
→ Claude writes posts in your voice,
timed to live conversation
```
That combination — vault context + live X data + 
your voice — is what takes your automation to the next level.

---

## What You Need

- Claude Desktop (desktop app, not browser)
- An xAI API key (free credits available)
- 15 minutes

---

## Step 1 — Get Your xAI API Key

The Grok MCP requires an xAI API key.

**The good news: **xAPI free credits are super generous.**


And if anything, even $5 will last you a very long time!

**The setup:**

## 1. Get your xAI API key

To get access:
• Go to console.x.ai
• Sign up / log in
• Go to API Keys → create a key

---

## 2. Tell Claude: 

```
"write me a bash install script to connect the Grok MCP server to Claude Desktop using my xAI API key and give me step by step instructions on how to set it up"
```

It'll write the whole script for you

And tell you how to run it on your terminal

---

Or if you just wanna copy-paste; use this:

```
bash
#!/bin/bash
set -e

echo "Installing Grok MCP server..."

# Install the Grok MCP package
npm install -g @xai/grok-mcp 2>/dev/null || \
npx -y @modelcontextprotocol/create-server grok-server

# Get Claude Desktop config location
if [[ "$OSTYPE" == "darwin"* ]]; then
    CONFIG_DIR="$HOME/Library/Application Support/Claude"
else
    CONFIG_DIR="$APPDATA/Claude"
fi

CONFIG_FILE="$CONFIG_DIR/claude_desktop_config.json"

echo ""
echo "Paste your xAI API key when prompted."
echo "It will not be saved to any file or chat."
read -s -p "xAI API Key: " XAI_KEY
echo ""

# Add Grok MCP to Claude Desktop config
python3 - <<EOF
import json, os

config_path = "$CONFIG_FILE"
xai_key = "$XAI_KEY"

if os.path.exists(config_path):
    with open(config_path, 'r') as f:
        config = json.load(f)
else:
    config = {}

if 'mcpServers' not in config:
    config['mcpServers'] = {}

config['mcpServers']['grok'] = {
    "command": "npx",
    "args": ["-y", "@xai/grok-mcp"],
    "env": {
        "XAI_API_KEY": xai_key
    }
}

with open(config_path, 'w') as f:
    json.dump(config, f, indent=2)

print("Grok MCP added to Claude Desktop config.")
EOF

echo ""
echo "Done. Now:"
echo "1. Press Cmd+Q to fully quit Claude Desktop"
echo "2. Reopen Claude Desktop"
echo "3. Go to Settings → Developer"
echo "4. You should see 'grok' in your MCP servers list"
```

Save the file as `install-grok-mcp.sh` on your Desktop.

---

## Step 3 — Run The Script

Open Terminal (Mac: search "Terminal" in Spotlight).

Run:
```bash
bash ~/Desktop/install-grok-mcp.sh
```

When prompted, paste your xAI API key.
Hit Enter.

The key is entered directly in Terminal —
it never touches a file or a chat.

---

## Step 4 — Restart Claude Desktop

Fully quit Claude Desktop.

Reopen Claude.

Go to **Settings** → **Developer** →
you should see `grok` in your MCP servers list
alongside any other servers you have running.

---

## Step 5 — Test The Connection

Open a new Claude conversation and type:
```
Use Grok's x_search to find what's trending
on X about AI workflows right now.
```
Claude should call the Grok MCP,
run an X search, and return live results with citations.

If it works — you're live. 🕷️

---

## Step 6 — Update Your Session Protocol

Connection confirmed. Now tell Claude when and how to use it.

Add this to your Claude Project instructions:
```
Example
-------

RESEARCH PROTOCOL:
When running research, use Grok MCP x_search
for live X trend data — not general web search.
Search within my content pillars only:

AI workflows and tools
Creator systems and monetization
Music and independent artist content

A topic trending on X doesn't automatically mean
it's relevant. Cross-reference against 'xyz' before including in results.
State what you searched and what you found
before writing anything.
```
**The search filter is critical.**
Without it, Claude will surface whatever is trending —
not what's relevant to your specific needs.

---

## What The Automated Session Looks Like

With Grok MCP connected and the protocol in place,
one of my content session runs like this automatically:
```
1. Pull published posts (last 30)       ← Typefully MCP
2. Pull scheduled queue                 ← Typefully MCP
3. Pull analytics (last 7 days)         ← Typefully MCP
4. Run research (Grok MCP - x_search)   ← Grok MCP
5. Apply recency weighting
6. Read signals
7. State content direction
8. Cross-reference queue
9. Verify format rotation
10. Write
```
Steps 1-4 happen automatically via MCP
before Claude starts drafting posts.
No prompting required.
Built into the session protocol.

---

Read more about my protocols and workflows on my repo here: 
[aunysillyme-dev claudeOS](https://github.com/aunysillyme-dev/claude-os)

Check out how to build a full vault structure:
[vault architecture](https://github.com/aunysillyme-dev/claude-os/tree/main/vault-architecture)

---

## The Three Tools Available

When you install the Grok MCP you get access to:

| TOOL | WHAT IT DOES | WHEN TO USE |
|------|-------------|-------------|
| `x_search` | Live X post search by keyword, handle, date | Content research, trend awareness |
| `web_search` | Grok's DeepSearch — more current than standard | Deep research, current events |
| `chat_completion` | Call Grok directly as a model | X-native reasoning, 2M token context |

For content sessions, `x_search` is the primary tool.
`web_search` adds depth when a topic needs it.
`chat_completion` is for tasks that benefit from
Grok's X-native knowledge or larger context window.

---

## The Honest Limitation

Grok's MCP support is still early.

The full bidirectional read/write vault experience
(like the Obsidian MCP) isn't there yet with Grok.
The X search and DeepSearch tools work reliably.
`chat_completion` via MCP is functional but evolving.

The X search angle alone makes this worth setting up.
There is no other way to get live X post data
directly inside a Claude session.

---

## What This Adds To The Stack

Before Grok MCP:
Claude researches using general web search.
It finds articles about what was trending.
Not what's trending right now on X.

After Grok MCP:
Claude searches X directly.
It finds posts from the last hour.
It writes content timed to live conversation —
not yesterday's news.

For anyone creating content for X specifically,
this is the highest-leverage MCP addition
to the stack after Obsidian.

---

## Related Files

- [Session Start Protocol](../session-protocol/01_session_start_protocol.md)
  — how Grok MCP fits into the full session loop
- [Tool-to-Task Mapping](../multi-ai-stack/02_tool_to_task_mapping.md)
  — where Grok sits in the full stack
- [Vault Architecture](../vault-architecture/01_obsidian_structure.md)
  — the Obsidian MCP setup this builds on

---

*aunysillyme-dev/claude-os — a multi-AI operating system being built in public.*
*Learning as I go. Sharing it all for you.* 🕷️
