---
title: EasyClick iOS USB — Product intro
hide_title: false
hide_table_of_contents: false
sidebar_label: iOS USB intro
description: >-
 EasyClick iOS no-jailbreak automation; Control Center 10.2.0+ built-in AI Agent
 (chat + visual workflows); EC scripting and Cursor-assisted coding. iOS 12–27+.
keywords:
 - EasyClick
 - iOS automation
 - no jailbreak
 - AI Agent
 - workflow
 - USB
 - OCR
---

# iOS USB edition

:::info TL;DR
EasyClick iOS USB is **no-jailbreak** automation: write EC scripts, or use the control-center **AI Agent** (chat + visual workflows) to batch-drive iPhones. Supports iOS 12–27+. Running a **saved workflow does not consume** LLM tokens. Start with [Prerequisites & licensing](./advance/ai-agent/prerequisites), then [Getting started](./advance/ai-agent/getting-started).
:::

:::tip Control Center 10.2.0+: AI Agent

In the new control center, open **AI Agent** to drive a connected iPhone with **natural-language chat**, or run batch automation with **visual workflows**.
**Running a saved workflow does not consume LLM tokens.** Read **[Prerequisites & licensing](./advance/ai-agent/prerequisites)** first (USB device license + automation IPA or BLE setup).

→ [AI Agent handbook](./advance/ai-agent) · [Getting started](./advance/ai-agent/getting-started) · [10.2.0 changelog](./changelog)

:::

:::tip USB control center video series
- https://space.bilibili.com/477518868/lists/8621841?type=season
:::

No-jailbreak iPhone automation for games, office workflows, and QA. EasyClick iOS needs **no jailbreak**; software automation and BLE HID hardware paths help reduce risk; open APIs support custom control-center products.

## Two ways to work

| Approach | Best for | Start here |
|------|--------|------------|
| **AI Agent** | No coding; ops, QA, batch runs | [Prerequisites](./advance/ai-agent/prerequisites) → [Getting started](./advance/ai-agent/getting-started) |
| **EC scripting** | Developers; complex logic | [First project](./firstproject) → [Script APIs](./funcs) |
| **AI-assisted scripting** | Cursor / IDEA developers | [AI-assisted coding](./advance/ios-usb-ai) |

:::info AI Agent ≠ Cursor writing scripts

- **AI Agent**: operates connected devices inside the control center (chat + workflow editor).
- **AI-assisted coding**: use Cursor (etc.) in the IDE to **write and compile EC scripts (`.iec`)**.

:::

## Quick links

| Topic | Doc |
|------|------|
| AI Agent handbook | [Overview](./advance/ai-agent) |
| **FAQ** | [License / LLM / tokens](./advance/ai-agent/faq) |
| USB license & environment | [Prerequisites](./advance/ai-agent/prerequisites) |
| Sign IPA / enable automation | [USB screen · Sign IPA](./advance/ios-usb-screen#sign-ipa) |
| BLE / OTG | [BLE guide](./advance/ios-usb-ble) · [OTG guide](./advance/ios-usb-otg) |
| UI screenshots | [AI Agent screenshots](./advance/ai-agent/screenshots) |

## FAQ (AI Agent)

<details class="ec-faq-item">
<summary><strong>Which license do I need? Same as screen license?</strong></summary>

<div class="ec-faq-body">

You need a **USB device license** (scripts / AI workflows), not a **USB screen license**. Binding steps: [Prerequisites](./advance/ai-agent/prerequisites) · [Buy license](./advance/ios-usb-screen#purchase-license).

</div>
</details>

<details class="ec-faq-item">
<summary><strong>Do I have to configure an LLM?</strong></summary>

<div class="ec-faq-body">

Required for **AI chat** and **AI workflow authoring**. **Running / trial-running a saved workflow** does not need it. See [Config & data dirs](./advance/ai-agent/config).

</div>
</details>

<details class="ec-faq-item">
<summary><strong>Does running a workflow consume tokens?</strong></summary>

<div class="ec-faq-body">

**No.** Trial and batch runs do not consume chat LLM tokens; only AI chat and AI workflow authoring bill tokens. → [Full FAQ](./advance/ai-agent/faq)

</div>
</details>

## UI preview

<img src="/index/ai-agent/cover.png" alt="AI Agent" width="640" />

More screenshots: **[AI Agent screenshots](./advance/ai-agent/screenshots)**.

## Product highlights

- Easy to start: AI Agent **needs no code**; also supports JavaScript + smart IDE
- Rich APIs, image recognition, free OCR
- Ship standalone `.iec`; workflows export as `.spk` for read-only client runs
- No jailbreak; iOS 12.0 ~ 27.0+
- Randomize taps in a region; color / node find
- OpenCV template matching (high recognition rate)
- Software + hardware (BLE) paths for lower risk

## Tooling

- JavaScript with reusable Java libraries
- Smart IDE with live screen sync
- Built-in logs

## What you can build

- Chat / workflow batch ops on many iPhones (AI Agent)
- App QA, data extraction, marketing tools
- Crawler-style script scenarios

## Who it is for

- Ops / QA who want **no-code** batch automation → **AI Agent**
- Developers building complex logic → **EC scripts + IDE**
- Enterprise App QA teams

## Community QQ groups

- Group 1: 777164022 · Group 2: 922739785 · Group 3: 647082990
- Group 4: 772810035 · Group 5: 484379843 · Group 6: 435253761
- Group 7: 397570651 · Group 8: 12076933 · Group 9: 778278905
- Group 10: 205262444
