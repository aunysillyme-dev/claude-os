# AUDIT_BRIEF.md — claude-os frontmatter scaffold pass

## Task bundle

**SCOPE, READ THIS FIRST. The checkout is NOT under `~/Claude Code`.**
The repository under audit is at this absolute path, and nowhere else:

    /private/tmp/claude-501/-Users-auny-Claude-Code/f7bf2d62-0490-4815-9a5c-f943b738de47/scratchpad/aunysillyme-dev/claude-os

The commit under audit is `9a99386f5d05f147e75f47cd6d0e8ed91d073a53`, one commit ahead of `origin/main`. Diff it
against its parent with:

    git -C /private/tmp/claude-501/-Users-auny-Claude-Code/f7bf2d62-0490-4815-9a5c-f943b738de47/scratchpad/aunysillyme-dev/claude-os diff origin/main..HEAD

A previous run of this audit returned BLOCKED because it looked for
`/Users/auny/Claude Code/claude-os`, which does not exist. That run verified
nothing. This is therefore the FIRST real audit of this change, not a second
pass.

**Purpose.** Adversarially audit one commit that adds YAML frontmatter
(`title`/`description`/`category`/`status`/`tags`) to 12 markdown files in
the `claude-os` repo, ahead of pushing to `origin/main`. AUN-1241 follow-on:
this repo's own convention already exists on 5 of 20 markdown files; this
commit brings the remaining 12 up to the same standard so the repo is
consistently machine-navigable.
**Denied actions.** Read-only audit. Do NOT edit any file, do NOT run git
commands that mutate state (commit, push, reset), do NOT touch any repo
other than this one, do NOT propose or apply fixes yourself — findings go
back to Claude, who owns any fix.
**Report contract.** For each of the 12 changed files: state whether the
frontmatter block is well-formed YAML, whether it introduces any secret/path/
account-id/email, and whether the diff touches anything outside the
frontmatter block (body prose, code blocks, the example install script in
`integrations/01_grok-mcp-setup.md`). List findings by severity; state
explicitly if nothing was found.

## Runtime
Static markdown documentation in a public GitHub repo (aunysillyme-dev/claude-os). No
application code, no server, no runtime. The change prepends a YAML
frontmatter block to 12 markdown files (articles, integration guide,
multi-AI-stack docs, session-protocol doc, and templates). Nothing executes
this frontmatter today; other files in the same repo already carry the same
convention (`title` / `description` / `category` / `status` / `tags`), and
this pass brings the remaining 12 of 20 markdown files up to that standard.

## Threat model
None in the traditional sense (no auth, no secrets, no user input, no network
surface). The real risks are the same as any docs-only frontmatter change:
(1) a leaked local path, account id, hostname, or secret in the added text,
(2) frontmatter that fails to parse as YAML, (3) an edit that accidentally
touches file body content instead of only adding frontmatter above it,
(4) one of the template files (which contain example install scripts with
placeholder API-key prompts) getting its example code altered rather than
just having frontmatter added above it.

## Callers / consumers
- The repo's own frontmatter convention (5 of 20 files already had it before
  this pass — `START-HERE.md`, `VISUAL-GUIDE.md`, `README.md`, `CHANGELOG.md`
  is unclear but `CONTRIBUTING.md` and `templates/vault-skeleton/protocols/
  README.md` were confirmed to already carry it).
- Any future static-site generator or doc indexer that parses this repo.
- Humans browsing the repo on GitHub.

## What's already verified
- `git diff --numstat` on all 12 changed files shows insertions-only (12-14
  lines each), 0 deletions in every file — confirms only frontmatter was
  prepended, no body text was touched or reordered.
- Parsed all 12 frontmatter blocks with PyYAML in a sandboxed codecalc run:
  all 12 parse as valid YAML maps with `title`, `description`, `tags` present
  and `tags` length between 3 and 7 (repo spec).
- Grepped every added line across the full diff for an em dash and the word
  "bottleneck": zero matches. (Note: the pre-existing article body at
  `articles/claude_md_is_the_starting_point.md:389` already contains the word
  "bottleneck" in prose that predates this change — untouched, out of scope
  per the task's explicit instruction not to edit file bodies.)
- Read all 12 files in full before writing their descriptions/tags, including
  the two files containing shell install scripts with an xAI API key prompt
  (`integrations/01_grok-mcp-setup.md`) — confirmed the script reads the key
  via an interactive `read -s` prompt and never writes it to a committed file;
  this is pre-existing example content, unmodified by this change.
- Grepped the added lines for `/Users/`, email addresses, and secret-shaped
  strings: none present.
- Checked category values used (`article`, `integration`, `architecture`,
  `reference`, `protocol`, `template`) against the repo's own allowed set
  (`core, guide, protocol, template, reference, architecture, integration,
  article`) — all in range.
- Ran `find . -name "*.md"` to confirm no markdown file was missed and no
  excluded file (README/CHANGELOG/CONTRIBUTING/SECURITY/CODE_OF_CONDUCT/
  LICENSE/.github/AUDIT_BRIEF) was touched; the two markdown files found
  outside the assigned list (`START-HERE.md`, `VISUAL-GUIDE.md`) already had
  frontmatter and were correctly skipped.

## What I want attacked
1. Does any of the 12 added frontmatter blocks contain a secret, internal
   path, hostname, email, or account id?
2. Did the edit alter, reorder, or remove any body content — including the
   example install script and its API-key handling in
   `integrations/01_grok-mcp-setup.md` — in any of the 12 files?
3. Is the YAML well-formed for a stricter parser than PyYAML given the block
   list syntax (`tags:\n  - foo`) used throughout?
4. Are `title`/`description`/`category` values accurate to each file's actual
   content, or do any overclaim/underclaim scope?

## Design decisions (with reasoning)
- Used the block-list frontmatter style with `category` + `status` fields,
  matching the convention already present in this repo's 5 existing
  frontmatter files (`START-HERE.md` etc.) and the exact example given in
  the task spec — this repo's convention differs from `ai-creator-os`'s
  compact inline style, confirmed by reading both before writing either.
- Reused existing tag vocabulary from this repo (`mcp`, `routing`,
  `protocols`, `template`, `vault-structure`, `obsidian-vault`,
  `knowledge-management`, `session-management`, `multi-ai`, `ai-orchestration`,
  `workflow-automation`, `navigation`) confirmed via
  `grep -rh -A8 '^tags:' --include='*.md' .` before choosing tags, so the tag
  namespace stays consistent instead of drifting per file.
- Left `START-HERE.md`, `VISUAL-GUIDE.md`, and the nested
  `templates/vault-skeleton/protocols/README.md` untouched: all three already
  had frontmatter (confirmed by reading their first lines), and the README
  is additionally excluded by name regardless.

## Full diff under audit
Note: this checkout lives at a session-scoped scratchpad path, not under
`~/Claude Code`, so it is not reachable from a fresh shell. The complete
`git diff HEAD~1 HEAD` for the frontmatter commit (all 12 files) is pasted
below verbatim -- audit from this text; do not assume any other content
exists in these files beyond what the diff context lines show.

```diff
diff --git a/articles/claude_md_is_the_starting_point.md b/articles/claude_md_is_the_starting_point.md
index 37e85f9..63c63a4 100644
--- a/articles/claude_md_is_the_starting_point.md
+++ b/articles/claude_md_is_the_starting_point.md
@@ -1,3 +1,17 @@
+---
+title: CLAUDE.md is the Starting Point
+description: Explains the five-layer context system (CLAUDE.md, vault, MCP, protocols, routing) that extends a basic CLAUDE.md file into a full persistent-memory system for Claude, with templates for each layer and a four-week rollout plan.
+category: article
+status: active
+tags:
+  - claude-md
+  - vault
+  - mcp
+  - protocols
+  - routing
+  - context-engineering
+---
+
 # CLAUDE.md is the Starting Point
 
 A markdown file at the root of your project tells Claude how to behave inside that project. It is useful. It is also the starting point — not the whole system.
diff --git a/integrations/01_grok-mcp-setup.md b/integrations/01_grok-mcp-setup.md
index 0f80783..0a78bc6 100644
--- a/integrations/01_grok-mcp-setup.md
+++ b/integrations/01_grok-mcp-setup.md
@@ -1,3 +1,16 @@
+---
+title: Connecting Grok to Claude via MCP
+description: Step-by-step setup guide for installing the Grok MCP server in Claude Desktop with an xAI API key, covering the install script, the connection test, and the session-protocol addition that routes live X search and DeepSearch into content research.
+category: integration
+status: active
+tags:
+  - grok
+  - mcp
+  - x-search
+  - integration-guide
+  - content-research
+---
+
 # Connecting Grok to Claude via MCP
 ### How to give Claude live X search and Grok's DeepSearch — 
 ### and why it changes content sessions completely
diff --git a/multi-ai-stack/01_stack_overview.md b/multi-ai-stack/01_stack_overview.md
index f039f70..5737c22 100644
--- a/multi-ai-stack/01_stack_overview.md
+++ b/multi-ai-stack/01_stack_overview.md
@@ -1,3 +1,16 @@
+---
+title: Multi-AI Stack Overview
+description: Maps each AI tool in the aunysillyme-dev stack to a single role (chief of staff, research, second opinion, visual production, music, content operations, build, system, distribution) and shows how the tools hand off to each other across four end-to-end workflows.
+category: architecture
+status: active
+tags:
+  - multi-ai
+  - ai-orchestration
+  - claude
+  - mcp
+  - workflow-automation
+---
+
 # Multi-AI Stack Overview
 ### Why one tool isn't enough — and how to run many as one system
 
diff --git a/multi-ai-stack/02_tool_to_task_mapping.md b/multi-ai-stack/02_tool_to_task_mapping.md
index b978d75..e065205 100644
--- a/multi-ai-stack/02_tool_to_task_mapping.md
+++ b/multi-ai-stack/02_tool_to_task_mapping.md
@@ -1,3 +1,16 @@
+---
+title: Tool-to-Task Mapping
+description: Master routing table mapping every task type (writing, research, visual, music, code, automation) to its primary tool with fallbacks, plus the four core rules (design, research, automation, memory) that keep tool selection consistent instead of guessed.
+category: reference
+status: active
+tags:
+  - multi-ai
+  - routing
+  - mcp
+  - automation
+  - claude
+---
+
 # Tool-to-Task Mapping
 ### The right tool for every job — no guessing, no overlap
 
diff --git a/session-protocol/01_session_start_protocol.md b/session-protocol/01_session_start_protocol.md
index d9c0e23..aa869fe 100644
--- a/session-protocol/01_session_start_protocol.md
+++ b/session-protocol/01_session_start_protocol.md
@@ -1,3 +1,16 @@
+---
+title: Session Start Protocol
+description: Defines the three-mode session-start system (lightweight, standard, deep) and the domain-specific reading lists Claude loads at the start of a session, so context loads automatically instead of being re-explained every chat.
+category: protocol
+status: active
+tags:
+  - session-management
+  - routing
+  - protocols
+  - claude
+  - context-loading
+---
+
 # Session Start Protocol
 ### How to give Claude full context before every session — automatically
 
diff --git a/templates/CLAUDE.md b/templates/CLAUDE.md
index 3b6f81d..2502051 100644
--- a/templates/CLAUDE.md
+++ b/templates/CLAUDE.md
@@ -1,3 +1,16 @@
+---
+title: CLAUDE.md Template
+description: Fill-in-the-blank CLAUDE.md skeleton (what this is, tools available, behavior rules, read-before-every-session list, routing, hard don'ts) for scaffolding a new project's root context file.
+category: template
+status: active
+tags:
+  - template
+  - claude-md
+  - scaffolding
+  - routing
+  - behavior-rules
+---
+
 # [Project Name]
 
 ## What This Is
diff --git a/templates/vault-skeleton/HQ.md b/templates/vault-skeleton/HQ.md
index 7abaab1..3054ce9 100644
--- a/templates/vault-skeleton/HQ.md
+++ b/templates/vault-skeleton/HQ.md
@@ -1,3 +1,15 @@
+---
+title: Vault HQ Template
+description: Fill-in-the-blank vault map template listing protocols, workflows, domains, the session log, and tool inventory, meant to sit at the vault root as the first file Claude reads every session.
+category: template
+status: active
+tags:
+  - template
+  - vault-structure
+  - navigation
+  - obsidian
+---
+
 # Vault HQ
 
 *The master map. Read first every session. Every folder, every file, one-line descriptions.*
diff --git a/templates/vault-skeleton/Session_Log.md b/templates/vault-skeleton/Session_Log.md
index 063b1ea..5507b1a 100644
--- a/templates/vault-skeleton/Session_Log.md
+++ b/templates/vault-skeleton/Session_Log.md
@@ -1,3 +1,15 @@
+---
+title: Session Log Template
+description: Fill-in-the-blank running log template for recording vault changes across chats (date, what changed, why), meant to be read at the start of every session immediately after HQ.md.
+category: template
+status: active
+tags:
+  - template
+  - session-log
+  - vault-structure
+  - context
+---
+
 # Session Log
 
 *Running log of vault changes across all chats. Every session that creates, edits, or deletes vault files appends an entry here. Every new session starts by reading this file.*
diff --git a/templates/vault-skeleton/protocols/01_session_start.md b/templates/vault-skeleton/protocols/01_session_start.md
index cc2be55..4d81251 100644
--- a/templates/vault-skeleton/protocols/01_session_start.md
+++ b/templates/vault-skeleton/protocols/01_session_start.md
@@ -1,3 +1,15 @@
+---
+title: Session Start Protocol Template
+description: Fill-in-the-blank protocol template specifying the read order (Session_Log, HQ, task-specific protocol) that Claude follows at the start of every session, with guidance on customizing it per vault.
+category: template
+status: active
+tags:
+  - template
+  - session-management
+  - protocols
+  - routing
+---
+
 # 01 — Session Start Protocol
 ### What Claude reads + checks at the start of every session
 *Triggers: automatic at start of every chat / "session start" / "load context"*
diff --git a/templates/vault-skeleton/protocols/02_routing_rules.md b/templates/vault-skeleton/protocols/02_routing_rules.md
index 6b1e79d..c89dde4 100644
--- a/templates/vault-skeleton/protocols/02_routing_rules.md
+++ b/templates/vault-skeleton/protocols/02_routing_rules.md
@@ -1,3 +1,15 @@
+---
+title: Routing Rules Template
+description: Fill-in-the-blank routing table template for vault operations, web research, code execution, and image generation, specifying primary and fallback tools so Claude's tool choice is not guessed.
+category: template
+status: active
+tags:
+  - template
+  - routing
+  - protocols
+  - tool-selection
+---
+
 # 02 — Routing Rules
 ### Which tool Claude calls for which job
 *Triggers: automatic — Claude reads this whenever tool selection is ambiguous*
diff --git a/vault-architecture/01_obsidian_structure.md b/vault-architecture/01_obsidian_structure.md
index 7979a4b..1b0ccba 100644
--- a/vault-architecture/01_obsidian_structure.md
+++ b/vault-architecture/01_obsidian_structure.md
@@ -1,3 +1,16 @@
+---
+title: Obsidian Vault Structure
+description: Explains how to structure an Obsidian vault as an AI memory layer, shows an example emoji-prefixed folder tree, lists which files Claude reads per session domain, and gives a six-step guide to building the vault and connecting it to Claude over MCP.
+category: architecture
+status: active
+tags:
+  - obsidian-vault
+  - vault-structure
+  - mcp
+  - knowledge-management
+  - routing
+---
+
 # Obsidian Vault Structure
 ### How to build a second brain that every AI tool reads from automatically
 
diff --git a/vault-architecture/02_mcp_connection_guide.md b/vault-architecture/02_mcp_connection_guide.md
index a30df79..6533682 100644
--- a/vault-architecture/02_mcp_connection_guide.md
+++ b/vault-architecture/02_mcp_connection_guide.md
@@ -1,3 +1,16 @@
+---
+title: MCP Connection Guide
+description: Step-by-step guide to installing the Obsidian Local REST API plugin, connecting Obsidian to Claude over MCP, testing the connection, building a starter vault structure, and troubleshooting common connection issues.
+category: integration
+status: active
+tags:
+  - mcp
+  - obsidian-vault
+  - integration-guide
+  - vault-structure
+  - troubleshooting
+---
+
 # MCP Connection Guide
 ### How to connect Obsidian to Claude and make your vault the memory layer for every session
 
```

## ROUND 1 — 2026-09-20

Auditor: Codex, `gpt-6-astra`, effort `high`, 87.3s. Commit `9a99386f` against its parent, which matches `origin/main`.

**Note on rounds.** An earlier invocation returned BLOCKED: it looked for the repo at `/Users/auny/Claude Code/claude-os`, which does not exist, because this clone lives in a scratchpad. That run verified nothing, so it was not a round. This is the first real audit of this change, and the only one.

**Verdict: PASS with one low-severity metadata finding.**

| Check | Result |
|---|---|
| YAML validity, all 12 files | Passed independent Psych 3.1.0 / libyaml parsing, duplicate-key, field-type, category/status and tag-list checks |
| Secrets, internal paths, hostnames, emails, private account ids | None. The only identifier added is `auny-ai`, already public |
| Body integrity, all 12 files | Byte-for-byte identical to the parent, including the Grok install script and its API-key handling. Outside each YAML block the only addition is a blank separator |
| Metadata accuracy | 10 of 12 pass. 2 flagged, see below |

### Finding: conflicting read order (low) — FIXED

`templates/vault-skeleton/HQ.md:3` described HQ as "the first file Claude reads every session", and `Session_Log.md:3` said it is read "immediately after HQ.md". Both are backwards.

I verified it against the repo before acting, and Codex slightly understated the case. It called this "a pre-existing body inconsistency" copied into metadata. The body is not inconsistent: `protocols/01_session_start.md` states the order in a numbered table (1 `Session_Log.md`, 2 `HQ.md`), and HQ's own body repeats it at line 53 ("1. Session_Log.md, 2. This HQ.md"). The repo agrees with itself. Only the new frontmatter disagreed with it.

That makes it worth fixing rather than noting: the descriptions are machine-readable, so an agent reading metadata instead of prose would have loaded the vault in the wrong order and been confidently wrong about why.

Both descriptions rewritten to match the documented order.

### Dispositions

| # | Finding | Disposition |
|---|---|---|
| 1 | Read-order conflict in HQ.md and Session_Log.md descriptions | **Fixed.** Both now state Session_Log first, HQ second, matching the protocol table and HQ's own body |

No other findings. One codex round, per Auny's ruling 2026-09-10.

### Verified independently before the push

`git diff origin/main..HEAD --numstat`: zero deleted lines across all 12 files, which is what proves no body was touched. The only deletions anywhere are the two description lines I replaced to fix finding 1.
