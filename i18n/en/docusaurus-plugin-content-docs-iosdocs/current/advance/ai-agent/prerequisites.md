---
title: Prerequisites & licensing
description: Required USB licensing and automation/BLE setup before using AI Agent
sidebar_label: Prerequisites & licensing
keywords:
 - USB license
 - automation environment
 - signed IPA
 - BLE
 - OTG
 - AI workflow
---

# Prerequisites & licensing (required)

:::info TL;DR
Before AI Agent works, complete three things: buy and bind a **USB device license** (not a screen license) → set up the **automation IPA** or **BLE/OTG** runtime → then go to [Getting started](./getting-started). Missing any step causes license failures, dead taps, or no screenshots.
:::

:::warning Complete this section before running any AI workflow

**Chat-driven device control**, **workflow editor trial runs**, **batch runs from the task panel**, and **imported `.spk` flows** all call control center capabilities under the hood.
If **USB device licensing** or the **runtime environment** is not ready, you may see: license check failures, automation won’t start, taps/screenshots no response, BLE step errors, etc.

Finish the checklist below, then use [Getting started](./getting-started).

:::

## 1. Purchase and bind USB device license

AI Agent automation steps (start environment, tap/swipe, run workflows, IME input, etc.) require a **USB device license**, same as EC scripts.

:::caution Don’t buy the wrong license type

| License type | Purpose |
|----------|--------------------------|
| **USB device license** | Run scripts, **AI workflows**, automation services, etc. |
| **USB screen license** | Screen mirroring, mouse UI control, video injection, etc. |

AI Agent needs a **USB device license**.

:::

**Steps:**

1. **Purchase a license** from official channels or resellers
2. With the phone connected over USB, open **License Center → Batch license** (device ID on the left, card number on the right)
3. Or on [https://uc.ieasyclick.com](https://uc.ieasyclick.com) go to **My iOS devices → Batch bind license**, then in the control center **License Center → Refresh license**

## 2. Configure runtime environment

Different workflow steps depend on different environments. **You can draw flows slowly before saving**, but **before trial or production runs** confirm the matching environment is ready.

### Option A: USB automation environment (proxy IPA / standalone main app)

**Use for** most UI automation, for example:

- Start automation environment (`ensure_auto_env`)
- Tap, swipe, type text
- Automation screenshot / OCR, UI node capture (`fetch_nodes`)
- IME input steps

**You need to:**

1. **Install and sign** the automation service IPA on the phone (iOS 15+ can also use the **standalone main app v7.2.0+** per the screen tutorial as the automation service — more complete)
2. Unpack the control center and configure the **developer disk image**
3. Enter the **proxy bundleID** in the control center and **enable automation** successfully (service status **Yes**)
4. Complete **USB device licensing** from the previous section

Full walkthrough: [iOS USB screen tutorial · Sign IPA](/iosdocs/advance/ios-usb-screen#sign-ipa)
(Same page covers unpacking, flashing images, starting automation, screen control, etc. — read the **proxy mode screen** section)

:::info Some steps don’t need automation environment

For example **non-automation screenshot/OCR**, some **open App**, **reset USB**, etc. can run without `ensure_auto_env`.
If the whole flow only does these, you don’t need to force automation on; mixed flows with automation steps must start the environment first. See [Workflow step reference](./workflow-steps).

:::

### Option B: BLE / OTG environment

**Use for** workflow **`ble_*` steps** (BLE peripheral tap, swipe, keys, etc.), or when you use **BLE HID screen** on the control center side.

**You need to:**

1. Prepare a BLE module/board; on the device context menu **BLE HID settings → Bind BLE**, and pass **Test BLE**
2. Follow the tutorial to enable **Bluetooth, Accessibility**, etc. on the iPhone
3. For **OTG simulated HID**, prepare an OTG module, flash firmware, and complete binding separately

Detailed guides:

- [BLE tutorial](/iosdocs/advance/ios-usb-ble) — binding, testing, phone options, WiFi provisioning, shortcuts
- [OTG tutorial](/iosdocs/advance/ios-usb-otg) — OTG HID connection, board and shortcut setup

:::tip Both environments can coexist

One phone can have **USB automation IPA** and **BLE peripherals** at the same time.
In a flow, pick `click_point` (automation env) or `ble_click` (BLE env) per step; **whichever step types you use, configure that environment first**.

:::

## 3. Pre-run checklist

| Check | OK when |
|--------|----------|
| Device online | Phone **connected** in new control center device list |
| USB device license | Bound in License Center, not expired |
| Automation env (flow has UI automation) | “Enable automation” succeeded, service status **Yes** |
| BLE / OTG (flow has `ble_*`) | Bound and **Test BLE** passed |
| LLM (AI chat / AI workflow authoring only) | [Model configured](./config); **pure run/trial does not consume Token** |

Still failing → [FAQ quick reference](./faq) · [Troubleshooting](./troubleshooting)

## Related docs

- [Getting started](./getting-started) — open AI window, pick devices
- [Workflow editor](./workflow-editor) — trial run and save
- [Tasks & monitoring](./tasks) — batch run progress
