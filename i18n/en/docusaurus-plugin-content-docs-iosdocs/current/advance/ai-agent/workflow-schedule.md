---
title: Workflow scheduled tasks
description: Automatically run saved workflows on a schedule — daily time and interval modes
sidebar_label: Scheduled tasks
keywords:
 - scheduled tasks
 - schedule
 - workflow
 - auto run
 - cron
---

# Workflow scheduled tasks

Scheduled tasks let **AI Agent** **automatically run saved workflows** on rules you set, without clicking **Run** each time. Each trigger creates a normal **task** — view progress and results in [Tasks & monitoring](./tasks).


## Open scheduled tasks page

1. Start **new control center**, open **AI Agent**
2. Top navigation **"Scheduled tasks"**

Page includes: **New**, **Refresh**, **Search**, and list of all rules.

## Create a scheduled task

Click **"New scheduled task"**, fill in:

| Field | Description |
|------|------|
| **Task name** | Recognizable name, e.g. “Daily check-in” |
| **Workflow** | Workflow to run (search supported) |
| **Schedule type** | See table below |
| **Max executions** | `0` = unlimited; rule **auto-pauses** when limit reached |
| **When device busy** | See [Device busy policy](#device-busy-policy) |
| **Startup params** | Optional JSON passed to workflow (same as manual run params) |
| **Target devices** | **By device** or **By group** multi-select; resolved to device list on submit |

### Schedule types

Same meaning as control center scheduled task **Time**:

| Type | Description | Example |
|------|------|------|
| **Daily** | Once per day at **hour:minute:second** | Daily 9:00:00 |
| **Interval minutes** | Trigger again N minutes after last **successful** trigger | Every 5 minutes |
| **Interval seconds** | Trigger again N seconds after last **successful** trigger | Every 30 seconds |

:::info No catch-up

If AI Agent process **restarts or stops**, missed triggers during downtime are **not** run in bulk after recovery. Interval rules continue from **last run time** (or when created/re-enabled); daily rules trigger at **next** matching time only.

:::

### Device busy policy

When a device **is running another workflow task**:

| Policy | Behavior |
|------|------|
| **Skip** (default) | Device **skipped** for this trigger; others run normally |
| **Stop current task then run** | **Cancel** running task on that device, then start this workflow |

If all target devices are skipped, trigger fails; reason recorded in **Recent error** column.

### After save

- New rule defaults to **Running** (`active`)
- List shows: name, workflow, schedule summary, device count, status, execution count, last run time, etc.

## Common list actions

| Action | Description |
|------|------|
| **Edit** | Change name, workflow, schedule, devices, etc. |
| **Run now** | Trigger immediately without waiting for schedule (debugging) |
| **Pause** | No auto trigger while paused; can **Resume** |
| **Delete** | Permanently delete rule |
| **Search** | Filter by task name, workflow, status, source, etc. |

## Identify source in task panel

Tasks from scheduled triggers show source **"Scheduled task"** in [Tasks & monitoring](./tasks); if the rule has a name, it appears too (e.g. `Scheduled task · Daily check-in`).

Other sources:

| Source label | Meaning |
|----------|------|
| **AI session** | Scheduled from chat, or **Run** on workflow card in chat |
| **Workflow editor** | Editor **Trial run** / **Run** |
| **Scheduled task** | Auto schedule described here |
| **Manual execution** | Other API trigger (uncommon) |

Task list **collapsed by default**; click a row to expand per-device execution.

## Usage notes

1. **AI Agent must be running**
 Schedule runs inside AI Agent process; no trigger if process is down. Usually kept alive by new control center — do not leave AI Agent closed long term.

2. **Workflow must exist and be runnable**
 Sealed workflows (.spk +.spl) must be within validity; otherwise trigger fails, error in **Recent error** column.

3. **Devices & licensing**
 Same as manual run — complete [Prerequisites & licensing](./prerequisites) (USB license, IPA, etc.).

4. **Concurrency**
 One device runs **one** workflow task at a time; multiple devices can run in parallel, limited by **Settings → Max devices per task**.

5. **Same as manual run**
 Scheduled trigger uses same chain as **Run / Trial run** — steps, variables, license checks identical; **does not consume** chat LLM Tokens.

## FAQ

<details class="ec-faq-item">
<summary><strong>After pause and resume, does it run immediately?</strong></summary>

<div class="ec-faq-body">

**Interval** rules recalculate interval from current time on resume — **no** burst catch-up from pause period.
**Daily** rules wait for next matching time.

</div>
</details>

<details class="ec-faq-item">
<summary><strong>If group membership changes, does the scheduled task follow?</strong></summary>

<div class="ec-faq-body">

If you chose **By group** at creation, the saved **device ID list** is fixed at that moment. Later group changes **do not** sync to existing rules — **edit** the scheduled task and re-select, or use **By device**.

</div>
</details>

<details class="ec-faq-item">
<summary><strong>What happens when execution limit is reached?</strong></summary>

<div class="ec-faq-body">

Rule auto-changes to **Paused**. Click **Resume** to continue (if still at limit, **edit** to raise limit — current logic uses cumulative `execCount`; edit does not reset count; create new rule or wait for future reset support).

</div>
</details>

## Next steps

- View one trigger’s details → [Tasks & monitoring](./tasks)
- Author workflows → [Workflow editor](./workflow-editor)
- Execution failures → [Troubleshooting](./troubleshooting)
