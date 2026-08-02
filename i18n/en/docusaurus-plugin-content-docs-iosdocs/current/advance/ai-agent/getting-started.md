---
title: Getting started
description: AI Agent install, launch, and first-time setup
sidebar_label: Getting started
keywords:
 - AI Agent
 - launch
 - first-time setup
---

# Getting started

:::info TL;DR
In the new control center, open **AI Agent** to drive a connected iPhone via chat, or run visual workflows in batch. You must finish a **USB device license** (not screen license) and the automation/BLE environment first → [Prerequisites & licensing](./prerequisites). **Trial runs / saved workflows do not consume** chat tokens.
:::

:::warning Prerequisites & licensing (required)

Before running workflows, trial runs, or chat-driven device control, complete **[Prerequisites & licensing](./prerequisites)**:

- Purchase and bind **USB device license** (not screen license)
- For UI automation steps: install/sign IPA, enable automation → [USB screen tutorial · Sign IPA](/iosdocs/advance/ios-usb-screen#sign-ipa)
- For `ble_*` steps: set up BLE/OTG → [BLE tutorial](/iosdocs/advance/ios-usb-ble), [OTG tutorial](/iosdocs/advance/ios-usb-otg)

:::

:::tip AI Agent video tutorials
- URL: https://space.bilibili.com/477518868/lists/8630468?type=season
- Covers AI Agent setup and workflow usage
:::


## FAQ quick reference

<details class="ec-faq-item">
<summary><strong>License, LLM, Token — how do they relate?</strong></summary>

<div class="ec-faq-body">

- **License**: Buy **USB device license** (not screen), plus IPA / BLE setup → [Prerequisites & licensing](./prerequisites)
- **LLM**: Only **AI chat** and **AI workflow authoring** need it; running saved flows does not → [Model config](./config#llm-configuration)
- **Token**: **Trial / run workflows do not consume** chat Token → [FAQ details](./faq)

</div>
</details>

→ [FAQ quick reference (full)](./faq)

## Before you start

1. Open the **new control center** (`新中控.exe` — **New Control Center** — in the install folder)
2. Confirm devices are **connected normally** (same as screen mirroring and scripts)
3. Completed **USB licensing** and **runtime environment** in [Prerequisites & licensing](./prerequisites)

:::tip

Daily use: **open new control center** → click **"AI Agent"**. Background services start automatically — no manual steps.

:::

## What AI Agent helps you do

**No coding** — connect phones, **speak in Chinese** or **click a few buttons**, and run many iPhones / iPads in parallel. Good for ops, batch testing, repetitive tasks.

### Speak and go (AI chat)

Tell AI in plain language; it runs on selected phones, for example:

- “**Open WeChat** on every phone”
- “**Non-automation screenshot** on the first one” or “**Non-automation OCR** — is there a login button?”
- “What automation can I run?”

If info is missing, AI **asks follow-ups** (e.g. which device) instead of guessing.

### Build once, reuse (workflows)

Turn fixed routines like “open App → tap here → wait 3s → type → save screenshot” into a flowchart, then:

- **One-click run** from the workflow library
- **Many phones at once**; progress in the task panel
- Need **manual captcha** on a step? Flow pauses; finish and click **Continue**

:::tip Workflows don’t consume Token

**Running or trial-running saved workflows** uses the local step engine — **no chat LLM**, **no Token**, **no LLM API** required.

Only **AI chat** (Chinese task planning) and **AI workflow authoring** in the editor use the chat LLM. After the flow is built, daily batch runs are **zero Token cost**.

If the flow includes **"VLM · visual locate"** steps, only those steps call the vision model per `vlm_config.json`, separate from chat LLM; without such steps, the whole flow stays offline from chat LLM.

:::
### Don’t know how to draw a flow? Let AI write it

In the workflow editor, describe in Chinese, e.g. “Open Safari, visit a URL, then screenshot” — AI can **generate the flowchart** for you to trial-run, tweak, and save.

### Typical scenarios

| Scenario | What you can do |
|------|----------|
| **Multi-device batch ops** | Select 10, 50 devices, run one flow together |
| **App walkthrough** | Non-automation/automation screenshot, OCR, **template/color-offset match**, VLM locate, auto tap/swipe |
| **Login / register / checkout** | Save as workflow; run on schedule or on demand |
| **Album asset delivery** | **Import images/videos** from PC folders to device albums |
| **Input method scenarios** | Dedicated IME steps: type, paste, read clipboard |
| **Connection recovery** | Add reset USB, wait reconnect in flow to reduce manual unplug |

:::tip vs. writing scripts

**AI Agent**: **Operate connected devices** in a dedicated window — fast to learn, good for quick flows and batch tasks.
Complex business logic still fits EC scripts; **use both**.

:::

## Three-step onboarding

:::tip What does the UI look like?

Browse [UI preview](./screenshots) first (chat / workflow / task panel screenshots and 30-second path).

:::

### Step 1: Open AI Agent

1. Start the **new control center**
2. Click **"AI Agent"**
3. Default tab is **AI chat**; if the top bar says not connected, click **"Start AI"**

### Step 2: Configure LLM (recommended)

For **Chinese chat** to drive devices, configure an LLM first:

1. Toolbar **"Model config"**
2. Click **Add**, fill in your provider — DeepSeek, OpenAI, local Ollama, etc.:
 - **Name**: memorable label, e.g. “DeepSeek main”
 - **API URL**, **model name**, **API key**: per provider docs
 - Quick templates for **DeepSeek / OpenAI / Ollama** — pick one and set the key
3. **Save**, then select the config below the chat input

:::note Works without LLM

Without config, chat still accepts simple commands like `list workflows`, `run xxx workflow`. Configuring an LLM improves the experience.

:::

### Step 3: Select devices

On the **left**:

- **Devices**: check phones to control (multi-select)
- **Groups**: pick many at once by screen mirroring group

After selection, tell AI “on these phones…”

## How to know you’re ready

| Where to look | OK when |
|--------|----------------|
| Top connection status | **Connected** or green indicator |
| Model name | Current model visible in toolbar |
| Left devices | Online devices checked |
| Workflow library | Can **New** or see existing flows |

## Where data is stored (usually no change)

By default, workflows, chat history, model config live under **`aibin/data/aiagent/`** in the install directory.

If you change path in **"Settings"→ Data storage directory**, content moves there; **restart AI** after (“Start AI” or reopen window).

**Run logs** always stay in **`aibin/log/`**, independent of data directory.

:::tip

Backup: copy the whole `aibin/data/aiagent/` folder to keep workflows and config.

:::

## Next steps

- [Prerequisites & licensing](./prerequisites) — USB license, automation IPA, BLE/OTG (required)
- [AI chat](./ai-chat) — operate devices via chat
- [Image/color matching](./image-match) — template/color-offset match
- [Workflow editor](./workflow-editor) — draw flows, trial run, save
- [Troubleshooting](./troubleshooting) — won’t open, won’t connect
