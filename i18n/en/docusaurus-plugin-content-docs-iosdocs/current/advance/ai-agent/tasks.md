---
title: Tasks & monitoring
description: View automation progress, pause, and resume
sidebar_label: Tasks & monitoring
keywords:
 - tasks
 - trial run
 - pause
 - progress
---

# Tasks & monitoring

Every workflow run — whether scheduled by AI or your **Run** / **Trial run** — creates one **task**. One or many phones running together count as **the same task**.

## Where tasks come from

| Source | Description |
|------|------|
| **AI chat** | Tell AI “run … for me”; AI schedules and starts automatically |
| **Workflow library → Run** | Click **Run** on a card in chat left panel |
| **Editor trial run** | Pick device in workflow editor, click **Trial run** / **Run** |
| **Scheduled tasks** | [Scheduled tasks](./workflow-schedule) trigger on schedule |

## Open task panel

In **AI chat** or **Workflow editor** toolbar, click **"Task panel"**.

View the list in the dialog; closing returns to the previous page.

## Task list contents

- About the **latest 100** tasks; running ones **auto-refresh**
- Each row: **status**, **source** (AI session / workflow editor / scheduled task, etc.), **device count**, success/failure counts, time
- List **collapsed by default**; click a row to expand device details
- Top **concurrency summary**: max concurrent devices, current running count (related to **Settings → Max devices per task**)

### Task statuses

| Display | Meaning |
|------|------|
| Pending | Created, not started yet |
| Running | At least one device still executing |
| Completed | All succeeded |
| Failed | All or critical steps failed |
| Partial success | Some succeeded, some failed |
| Paused | Waiting for manual action before continue |

## Open one task

When expanded, see **per-device** status:

- Which **workflow** is running
- Current **step**
- On failure, **error message**

View **per-step details** (input, output, screenshots, etc.) for troubleshooting.

## Common actions

| Button | When to use |
|------|------------|
| **Continue** | Flow has a “wait for human” step — after you finish on the phone, click to resume |
| **Force stop** | Task stuck or you want to cancel |
| **Workflow editor** | Jump to editor with **playback** of this run |
| **Copy task ID** | Provide to support |

## “Wait for human” steps

Some flows **pause** at a step for captcha, manual confirm, etc., then continue. For example:

1. Task status becomes **Paused**
2. Complete the action on the device
3. Return to task panel, click **Continue** for that device

## Playback in editor

From task panel **"Workflow editor"**, or during editor **trial run**:

- Flowchart **highlights** completed steps
- Failed steps show **reason**
- Open **Run log** for full detail

Editor also supports **retry from a step**, **run until a step**, **run selected consecutive steps** for debugging.

On production tasks with heavy OCR, the device's OCR engine is released after the task; the first OCR step on the next similar task may take a few extra seconds — normal behavior.

## Next steps

- Scheduled execution → [Scheduled tasks](./workflow-schedule)
- Run failed → [Troubleshooting](./troubleshooting)
- What a step means → [Workflow step reference](./workflow-steps)
