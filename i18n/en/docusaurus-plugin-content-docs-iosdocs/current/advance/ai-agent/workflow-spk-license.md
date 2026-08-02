---
title: Workflow commercial license (.spl)
description: Implementer issues.spl commercial license; executor imports license and runs.spk flows
sidebar_label: Workflow.spl license
keywords:
 - spl
 - commercial license
 - issue license
 - import license
 - deviceId
 - spk
---

# Workflow commercial license (`.spl`)

:::tip Implemented

AI Agent client supports: **Issue license**, **Import / update license**, **Issue history** (implementer local), **License details** (executor view).

:::

## What problem it solves

**Commercial license (`.spl`)** controls **whether execution is allowed**:
- Implementer issues license by **workflow**, **phone deviceId**, and **validity period**
- After customer imports `.spk`, they must also **import / update `.spl`** before trial run, tasks, and AI runs are allowed
- Self-written plaintext flows are unaffected — **only** read-only flows imported from `.spk` (marked **"S"**) need commercial license.
- Workflow developers selling **spk workflows** use commercial license for executors

## Three “licenses” — don’t mix them

| Type | Who manages | Purpose |
|------|------|-------------------------------------|
| **USB device license** | EasyClick platform | Run automation taps, screenshots, etc. ([Prerequisites & licensing](./prerequisites)) |
| **`.spk` workflow password** | Implementer | Import flow package |
| **`.spl` commercial license** | Implementer | Allow **which workflow** on **which phones**, **when** |

---

## Issue license (implementer)

Implementer issues `.spl` file for a saved **workflow** in **Workflow editor**, sends to customer for import.

### Entry

1. Open **Workflow editor**
2. Top bar click **"Issue license"**

### Issue license tab

1. **Select workflow** (required)
 - Search by name, ID, description for **root workflow** (not subflow, not imported `spk_*`)
 - After selection shows **packageId**; if not assigned yet, **save** that root workflow once in editor

2. **Licensed phone device IDs** (required, one per line)
 - Copy device IDs from control center device list into text box
 - **"Copy device IDs"** copies current list to clipboard (for reissue, verification)

3. **Effective time / expiry time**
 - Default: effective now, **expires one month later** (editable)

4. **License password** (required, min 8 characters)
 - Deliver with `.spl` to customer; **prefer different channel from `.spk` import password** (different WeChat/phone/email)

5. **Password hint, customer notes** (optional)
 - Notes saved only in implementer local issue history, not written to `.spl` file

6. Click **"Export SPL license"**, choose save location, get `.spl` file

7. Send **`.spl` file** and **license password** to customer (**do not** mix with `.spk` password in one forwardable message)

:::info Renewal / add phones

Open **Issue license** again, select **same workflow**, change device IDs or expiry, re-export `.spl`. Customer **updates license** on existing `spk_*` flow — usually **no need** to re-issue because of `.spk` upgrade.

:::

### History tab

After each successful issue, this machine saves **issue history** (plaintext backup) in data directory for implementer reissue, renewal, accounting — **not** sent to customer.

| Feature | Description |
|------|------|
| **Search** | Filter by workflow name, license ID, packageId, device ID, etc. |
| **License ID** | Matches License in `.spl`; for customer verification |
| **Copy device IDs** | One-click copy all deviceIds from that issue (newline-separated) |
| **Copy license password** | Quick resend when customer forgets password |
| **Issue again** | Pre-fill workflow, phones, validity in Issue tab; edit and re-export |
| **Delete** | Delete local history only; does not affect customer’s imported license |

---

## Use license (executor)

After importing [`.spk`](./workflow-spk), executor imports commercial license for the read-only flow before trial run and batch runs.

### Prerequisites

1. Completed import with implementer’s **`.spk` + workflow import password**; read-only flow with **"S"** in library
2. Obtained **deviceId** of phones to run from control center; confirm implementer included those IDs in `.spl`

### Import / update license

**Method 1: Workflow editor**

1. Find **"S"** read-only flow in left library
2. Hover, click **unlock icon** (import / update commercial license)
3. Select `.spl` file, enter **license password** (not `.spk` import password)
4. On success, license status beside flow: **Unauthorized / Licensed / Expired / Not yet effective**

**Method 2: AI chat · workflow library**

1. Open **AI Agent** → **AI chat**
2. In left workflow library, on **"S"** flow click **unlock icon**
3. Same: select `.spl`, enter **license password**

:::warning Cannot import flow A’s.spl into flow B

`.spl` must match current `.spk` package

:::

### View license details (executor)

In **AI chat · workflow library**, on `spk_*` flow click **details icon** (document icon) to open **SPK details** dialog:

| Tab | Content |
|-----|------|
| **SPK details** | Name, version, step count, description, etc. |
| **License details** | License status, license ID, package ID, effective / expiry time, licensed device list; **copy all device IDs** |

Badge on workflow list matches above status for quick check whether run is allowed.

### Runtime validation

Before trial run, task run, or AI param run on `spk_*` flow, system checks:

1. `.spl` imported
2. **Currently selected phone** deviceId is in license list
3. Within **effective time ~ expiry time**

On failure, task fails with message (e.g. “Device not licensed — not in commercial license list”).

---

## Validity period
- **Online** required before running `.spk` flows
- **Offline or time sync failure blocks execution** — restore network and retry
---

## FAQ

**Same password for `.spk` and `.spl`?**
**Not recommended.** `.spk` decrypts flow package; `.spl` is commercial execution license. Deliver via separate channels.

**Re-issue `.spl` after `.spk` upgrade?**
Usually **not needed**. Same workflow — old `.spl` still works with new package

**Same `.spl` on multiple PCs, multiple `spk_xxx`?**
Yes. Limits are **which workflow + which phones + validity**, not Agent install count.

**Forgot license password?**
Product **cannot reverse password from `.spl`**. Contact implementer for password from **issue history**, or re-issue `.spl`.

**“Device not licensed, not in commercial license list”?**
Current phone’s device ID not in `.spl` list — contact implementer to add phone or re-issue `.spl`.

**“Cannot sync time online”?**
Check network and retry; `.spk` commercial license requires online before execution.

**USB device license failure?**
That is platform [USB device license](./prerequisites) — separate from `.spl`; handle separately.

---

## Related docs

- [Workflow.spk](./workflow-spk) — export, import read-only flows
- [Workflow editor](./workflow-editor) — trial run, issue license entry
- [AI chat](./ai-chat) — workflow library, import license, view details
- [Prerequisites & licensing](./prerequisites) — USB device license
- [Tasks & monitoring](./tasks) — batch runs
- [Troubleshooting](./troubleshooting)
