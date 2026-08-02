---
title: Troubleshooting
description: AI Agent startup, connection, and execution failure diagnosis
sidebar_label: Troubleshooting
keywords:
 - troubleshooting
 - logs
 - USB
 - license
---

# Troubleshooting

Look up by symptom. Most issues are found in **logs** or **error text in the task panel**.

## AI Agent window won't open

**Symptom**: Clicking **"AI Agent"** in the new control center does nothing.

**Check**:

1. Install directory has **`aibin`** folder and AI programs inside (usually bundled with installer)
2. Open **`aibin/log/`**, check `javaexec-iosagent-ui.log` for errors
3. Windows first run may need **WebView2**; installer folder often has installer, or install from control center menu
4. Antivirus blocking

## Shows not connected

**Symptom**: Toolbar says AI service offline.

**Check**:

1. Click **"Start AI"**
2. Check **`aibin/log/javaexec-iosaiagent.log`** for errors
3. If still failing, close AI window and reopen from new control center

## Can only type list workflows, can't chat normally

**Cause**: LLM not configured, or wrong key / API URL.

**Fix**:

1. Click **"Model config"**, confirm API URL, model name, **API key**
2. Toolbar should show current model name
3. Corporate network blocking external APIs (proxy, firewall)

## AI says it can't find a workflow

**Fix**:

1. **Refresh** workflow library on the left
2. Ask AI to “list all workflows”, or confirm name in library
3. If newly created, **save** in editor first

## Device license failure

**Symptom**: Task error about license; `device_auth_ok` fails in chat or flow.

**Fix**:

1. Confirm **USB device license** purchased and bound (not screen license) → [Prerequisites & licensing](./prerequisites) or [USB screen tutorial · Purchase license](/iosdocs/advance/ios-usb-screen#purchase-license)
2. Confirm phone **online** in new control center device list
3. Replug USB, or check proxy App / automation service
4. **Refresh license** in control center **License Center**
5. If flow has “license check” step, confirm correct type (USB device license / screen license)

## Can't start automation / tap or screenshot fails

**Symptom**: `ensure_auto_env` fails, or automation screenshot / tap no response.

**Fix**:

1. Per [Prerequisites & licensing](./prerequisites): IPA signed and installed, bundleID configured, developer disk image ready
2. **Enable automation** in control center until service status is **Yes**; right-click device **Test automation** for logs
3. Details: [USB screen tutorial · Sign IPA](/iosdocs/advance/ios-usb-screen#sign-ipa), [Start automation](/iosdocs/advance/ios-usb-screen-new#start-automation)
4. If you only need screenshot/OCR without automation, use **Non-automation screenshot/OCR** steps or chat commands

## BLE step failure

**Symptom**: `ble_click` etc. fail in workflow.

**Fix**:

1. Complete Bluetooth/OTG setup in [Prerequisites & licensing](./prerequisites)
2. Control center **Bluetooth HID settings → Test Bluetooth BLE**
3. See [BLE tutorial](/iosdocs/advance/ios-usb-ble), [OTG tutorial](/iosdocs/advance/ios-usb-otg)

## Screenshot fails in chat

**Symptom**: “Automation screenshot” fails.

**Fix**:

1. Say “**Start automation environment**” first, then screenshot
2. If you don't want automation, use “**Non-automation screenshot**” or “**Non-automation OCR**”

Both screenshot types use the **same names** in chat and workflows: **Non-automation screenshot** / **Automation screenshot**.

## OCR slow first time or slow again after task ends

**Cause**: OCR engine loads per **device + engine type**; released after formal **task ends**; next run re-initializes.

**Fix**:

1. Consecutive OCR in one flow **reuses** engine — much faster from second time
2. Editor **OCR preview** keeps engine loaded — good for debugging
3. Don't mix many `ocrType` values casually — each type uses separate memory

## Image match / color-offset match no match

1. Confirm template path relative to **working directory**, or re-**crop template** in **Match preview**
2. **Non-automation image match** needs **no** automation; **automation image match** needs **Start automation environment** first
3. Transparent/color-offset templates: use **color-offset match** step, not plain template match
4. Tap offset wrong: add **Match data extraction** (center) then **tap**, don't use top-left coordinates directly

See [Image/color matching](./image-match).

## Can't open App / automation won't start

**Suggested order** (add steps in flow or ask AI to follow):

1. **Reset USB connection**
2. On Windows try **USB flash disconnect** (like unplug), then **wait 5–10 seconds**
3. **Start automation environment**, wait until ready
4. **Open App** again

Still failing: expand task in **Task panel**, see **which step failed** and error text.

## Workflow save / validate failure

1. Click **Validate**, read first red message
2. Common: missing path, flowchart not connected to end, subflow reference missing
3. Check params against [Workflow step reference](./workflow-steps)

## Can't pick device for trial run

- Need **online** device in new control center
- **Refresh** device list in editor trial run bar

## External CLI won't run

- In **CLI/MCP integration**, confirm **added** and name matches
- Check **`aibin/log/api-error.log`**

## Data stored in wrong place

**Expected**:

- Business data: `aibin/data/aiagent/` (or directory you set in Settings)
- Logs: `aibin/log/`

If paths wrong after upgrade, use **10.1.0+** new control center and AI components.

## Log locations

| Content | Path |
|------|------|
| AI service | `aibin/log/javaexec-iosaiagent.log` |
| AI window | `aibin/log/javaexec-iosagent-ui.log` |
| API errors | `aibin/log/api-error.log` |

Task panel or control center **Open AI log directory** opens `aibin/log/` in one click.

## Many devices run slowly

- Increase **Max devices per task** in **Settings**, then **restart AI**
- One phone **cannot** run two tasks at once

## Version info

AI Agent window **status bar** shows version and build time.

## Still stuck

Prepare:

1. Relevant logs from `aibin/log/`
2. **Task ID** (copy from task panel)
3. Problem **workflow** (workflow library → **Open folder**, copy files)
4. New control center version, reproduction steps

Provide all when contacting support.
