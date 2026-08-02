---
title: Workflow package (.spk)
description: Export, distribute, and import read-only workflow packages
sidebar_label: Workflow.spk
keywords:
 - spk
 - export
 - import
 - workflow package
---

# Workflow package (`.spk`)

`.spk` lets you **package** a finished workflow for others: they can **run / trial run**, but **cannot view or edit** flow details or save as plaintext workflow.

## Who does what

| Role | Action |
|------|--------|
| **Implementer** (you) | In the editor, **export plaintext workflow as.spk**, set password, send to customer |
| **Executor** (customer) | In the editor, **import.spk package**, enter password, get read-only flow marked **"S"** |

## Export (implementer)

1. Open **Workflow editor**, find the **root workflow** in the left library (not a subflow)
2. Hover and click the **download icon**, or follow the dialog
3. Dialog shows **export closure preview**: which subflows and template images are included
4. Set **export password** (min 8 characters); optional **password hint** (e.g. “Contact Zhang San for password” — **not the password itself**)
5. Optional **export zip package**: contains `.spk` + `导入说明.txt` (instructions **without password**)
6. Choose save location

After export, follow **distribution guidance** in the dialog:

- Send **file** (`.spk` or zip) and **password** via **different channels** (e.g. cloud drive + WeChat/phone)
- **Do not** put password and attachment in the same forwardable email

This machine keeps **recent export history** (includes local password backup, readable only in current Agent data directory) for resending if the customer forgets the password.

## Import (executor)

1. Top bar **"Import.spk"**, or in AI chat sidebar workflow library click **upload icon**
2. Select **`.spk` file** or **export zip** (zip auto-reads `.spk` inside)
3. Enter **import password** from implementer
4. If the same series is already installed (same package or root flow upgrade), choose **side-by-side install** or **overwrite existing** (overwrite keeps original `spk_xxx`; good for upgrades)
5. After import, read-only workflow with **"S"** mark appears on the left

After importing a read-only flow, you must also import commercial license per **[Workflow.spl license](./workflow-spk-license)** before trial run and task execution.

## After import — allowed vs. not

| Allowed | Not allowed |
|------|--------|
| Trial run, task run | Edit, save, change JSON |
| Read-only flowchart view | Export.spk, save as plaintext |
| Select this flow in AI chat with params | View full step params and implementer prompt |

## Uninstall.spk package

In workflow library, on **"S"** flow click **Delete**, confirm **Uninstall**. Re-import `.spk` to use again.

## FAQ

**Forgot password?**
Product **cannot recover**. Contact implementer for new `.spk` + password.

**Import same package twice?**
Yes. Each import can create new `spk_xxx` (side-by-side) or overwrite (upgrade).

**Do subflows appear separately in library?**
No. Subflows are inside the package; only root flow appears as `spk_xxx`.

**Conflict with plaintext workflows?**
No. `spk_xxx` and plaintext IDs use separate namespaces.

**Can implementer limit execution by device and time?**
See **[Workflow.spl license](./workflow-spk-license)**: implementer **issues license**, executor **imports license**; binds workflow id + phone deviceId + validity period.

## Related docs

- [Workflow.spl license](./workflow-spk-license) — issue and use license
- [Workflow editor](./workflow-editor)
- [AI chat](./ai-chat) — run.spk flows
- [Troubleshooting](./troubleshooting)
