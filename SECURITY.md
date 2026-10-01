# Security policy

This repository is documentation only: markdown files describing session protocols, vault architecture, and multi-AI workflow frameworks. It has no runtime, no server, no package to install, and no dependencies of its own.

If a doc here recommends a practice that turns out unsafe (a credential-handling pattern, an insecure MCP setup step), report it the same way as a security issue.

Code-related reports (the grok-mcp-server Worker, its auth gate, its deploy) belong on [aunysillyme-dev/grok-mcp-server](https://github.com/aunysillyme-dev/grok-mcp-server), not here.

## Reporting a vulnerability

Use GitHub's private vulnerability reporting on this repository (Security tab, "Report a vulnerability"). That opens a private security advisory, not a public issue.

You will get an acknowledgement within 7 days.

## Scope

In scope: the accuracy and safety of the documentation itself. Out of scope: any tool, MCP server, or third-party service this documentation describes; report those to their own repositories.
