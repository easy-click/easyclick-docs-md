---
title: AI chat
description: Drive iOS devices with natural language automation
sidebar_label: AI chat
keywords:
 - AI chat
 - natural language
 - device selection
---

# AI chat

:::tip Before operating devices

Clicks, screenshots, workflow runs, etc. in chat depend on **USB device license** and the **runtime environment**. Read [Prerequisites & licensing](./prerequisites) first.

:::

After opening AI Agent, the default tab is **AI chat**. Describe what you want in **Chinese**; AI helps pick devices, find flows, and schedule execution.

## Layout

- **Top**: toolbar (connection status, model, settings, tasks, etc.)
- **Left**: device selection + **workflow library** (card list — run, edit)
- **Center**: chat history with multiple **chat tabs**
- **Bottom**: input box; when an LLM is configured, choose **which model this session uses**

Switch between **"Chat"** and **"Workflow editor"** at the top.

## Common toolbar buttons

| Button | Purpose |
|------|----------|
| **Connection status** | See if AI service is online; click **Start AI** when offline |
| **Model config** | Add or edit LLMs (DeepSeek, OpenAI, etc.) |
| **History** | Open past chats — continue or delete |
| **Task panel** | View running and completed tasks (see [Tasks & monitoring](./tasks)) |
| **Settings** | Data location, concurrent device count, etc. |
| **Workflow editor** | Arrange automation flows |
| **New session** | Start a blank chat |
| **CLI/MCP integration** | External CLI, etc. (advanced — see [External integration](./external-integration)) |

## How to use: three steps

1. **Check devices on the left** (if none selected, AI will ask which phone to use)
2. **Describe your need in Chinese** in the input box; press **Enter** to send (**Shift + Enter** for newline)
3. Wait for AI's reply; when execution is needed it creates a task — check progress in the **Task panel**

### Example prompts

- “Open Safari on the first phone for me”
- “What automation can I run now?”
- “Run the login flow”
- “**Non-automation screenshot** — show the current screen” (device connected; no automation needed)
- “**Automation screenshot** — save the current screen” (automation environment must be started first)
- “**Recognize screen text** — is there a login button?”
- “**Non-automation image match** with template btn_ok.png” (prepare template path beforehand)
- “Use **visual locate** to find the settings button in the top-right” (VLM must be configured)

AI replies in Chinese; it **asks follow-ups** when information is missing instead of inventing flow names.

## Screenshots, OCR, and image matching

AI chat and workflows use the **same names**:

### Screenshots and OCR

| Phrase / step | Meaning | Automation required? |
|-------------|------|----------------|
| **Non-automation screenshot** | Bridge full-screen capture | **No** |
| **Automation screenshot** | Agent service full-screen capture | **Yes** |
| **Non-automation OCR** | Bridge capture + text recognition | **No** |
| **Automation OCR** | Agent capture + text recognition | **Yes** |

### Image/color matching

| Phrase / step | Meaning |
|-------------|------|
| **Non-automation image match** / **Automation image match** | OpenCV template matching |
| **Non-automation color-offset match** / **Automation color-offset match** | Transparent / color-offset templates |

For template paths, region crop, center extraction, and other **complex settings**, use **Match preview** in the workflow editor — see [Image/color matching](./image-match).

### Corresponding workflow steps

Editor menu: **Screenshot**, **OCR**, **Image/color match**, **Visual locate (VLM)**, etc. — names match the table above.

:::tip

Each chat tab can use a **different model** for comparison. After switching tabs, the model selector below the input updates accordingly.

:::

## Workflow library (left)

Lists your saved **main flows** (subflows appear in the editor). Each card supports:

| Action | Description |
|------|------|
| **Run** | Execute immediately on selected devices (select devices first) |
| **Edit** | Open in workflow editor |
| **Open folder** | Open the flow file folder on your PC (for backup) |
| **Delete** | Remove this flow |
| **New** | Create a blank flow |

Search box filters by name or description.

## Without LLM configured

If no API key is set, only **fixed commands** work:

| Input | Effect |
|------|------|
| `list workflows` | List all workflows |
| `run workflowId` | Run on selected devices; optional device ID |

Still recommended to configure an LLM per [Getting started](./getting-started) for a natural experience.

## Tips

- Chat history **auto-saves** — reopen from **History**
- AI replies **stream character by character**; click cancel if you can't wait
- **Find button from image** in chat is less complete than in the workflow editor; for fixed icons use workflow **image/color matching** + **Match preview** — see [Image/color matching](./image-match)

## Next steps

- Repeatable, visual flows → [Workflow editor](./workflow-editor)
- Fixed-icon button matching → [Image/color matching](./image-match)
- Track execution → [Tasks & monitoring](./tasks)
