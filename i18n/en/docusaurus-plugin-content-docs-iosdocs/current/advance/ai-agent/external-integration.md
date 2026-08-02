---
title: External integration
description: Connect workflows and AI chat to external CLI, HTTP, and more (advanced)
sidebar_label: External integration
keywords:
 - CLI
 - MCP
 - HTTP
 - external tools
---

# External integration (advanced)

:::info Who this is for

For users who need to connect **their own programs and HTTP APIs** into automation. If you only use AI chat and workflows daily, **you can skip** this chapter.

:::

AI Agent can register **external capabilities** you trust for **workflow steps** or **AI chat** to call.

Open via toolbar **"CLI/MCP integration"**

## What you can register

| Type | Typical use |
|------|----------|
| **Command-line program** | Run local `.exe`, scripts — organize files, call in-house tools |
| **HTTP API** | Call REST APIs — query data, notify other systems |
| **MCP service** | Connect MCP-protocol tools (advanced integration) |

After register and **Save**, corresponding steps in editor and AI calls in chat can select them.

## Register command-line program

1. Click **Select executable**, pick local program
2. Fill **Name** (system generates ID; editable)
3. Optional: put **documentation** in same folder (e.g. `SKILL.md`) so AI understands usage
4. Save

**In workflow**: add **Run CLI** step, select registered program, fill args.

**In chat**: say “use xxx tool to list a directory” — AI builds command from registry (registered programs only, safer).

:::warning Security

CLI runs with **control host PC** permissions — only register programs **you trust**.

:::

## Register HTTP API

1. Fill **Name**, **Base URL** (e.g. `https://api.example.com`)
2. Optional default method, headers
3. Save

**In workflow**: add **Call HTTP** step, pick registered endpoint or full URL.

**In chat**: can probe API temporarily; fixed flows should be in workflow.

## Register MCP service

MCP connects external tools supporting the protocol (local process or remote HTTP). On save you can **sync tool list**, then pick specific tools in workflow or chat.

Details vary by provider — configure per MCP provider docs.

## vs. CLI in “AI-assisted coding”

Tools in [AI-assisted coding](../ios-usb-ai) mainly **compile EC scripts**; registration here is for **external programs during automation**. The two **do not interfere**. To call EC compiler from a flow, **register it separately** on this page.

## Next steps

- Step parameters → [Workflow step reference](./workflow-steps) (External integration steps)
- Failures after register → [Troubleshooting](./troubleshooting)
