---
title: FAQ (license / LLM / Token)
description: Top three AI Agent questions — which license, when LLM is needed, whether runs consume Token
sidebar_label: FAQ quick reference
keywords:
 - USB license
 - LLM
 - Token
 - AI Agent
---

# FAQ quick reference

The three most common questions. Expand titles for details; full environment checklist in **[Prerequisites & licensing](./prerequisites)**; troubleshooting in **[Troubleshooting](./troubleshooting)**.

<details class="ec-faq-item">
<summary><strong>Which license do I need? Same as screen license?</strong></summary>

<div class="ec-faq-body">

**No.** AI Agent workflows, trial runs, and chat-driven device control use the same stack as EC scripts and require a **USB device license**.

| License type | Purpose |
|----------|------|
| **USB device license** | Scripts, **AI workflows**, automation tap/screenshot, etc. |
| **USB screen license** | Screen mirroring, mouse UI control, video injection, etc. |

**Steps:**

1. **Purchase USB device license** (not screen license) from official channels or resellers
2. Phone on USB → **License Center → Batch license** (device ID left, card number right)
3. Or on [User center](https://uc.ieasyclick.com) **My iOS devices → Batch bind license**, then **License Center → Refresh license** in the control center

Also configure **automation IPA** or **BLE/OTG** per flow steps — see [Prerequisites & licensing](./prerequisites) · [Buy license](/iosdocs/advance/ios-usb-screen#purchase-license) · [Sign IPA](/iosdocs/advance/ios-usb-screen#sign-ipa).

</div>
</details>

<details class="ec-faq-item">
<summary><strong>Must I configure an LLM? Which features need it?</strong></summary>

<div class="ec-faq-body">

**Not every feature.** LLM is only for “AI understands natural language and generates content”:

| Feature | LLM required? |
|------|----------------|
| **AI chat** (Chinese descriptions to operate devices) | ✅ Yes |
| **AI workflow authoring** (natural language generate/edit in editor) | ✅ Yes |
| **Run / trial-run saved workflow** | ❌ No |
| **Batch run from task panel** | ❌ No |
| **Import and run read-only `.spk` flow** | ❌ No |

Without LLM, AI chat still accepts simple commands like `list workflows`, `run …` — see [AI chat](./ai-chat).

In toolbar **"Model config"** set API URL, model name, and key; pick separately for chat vs. AI authoring. See [Config & data dirs · LLM config](./config#llm-configuration).

**VLM (vision LLM)** is separate from chat LLM: only **VLM · visual locate** and similar steps need it, in `vlm_config.json` — see [Config · VLM](./config#vision-model-vlm-advanced).

</div>
</details>

<details class="ec-faq-item">
<summary><strong>Do workflows consume Token?</strong></summary>

<div class="ec-faq-body">

**Running or trial-running saved workflows does not consume chat LLM Token**, and **does not** require an active LLM API connection.

Scenarios that **do** consume Token (billed by your LLM provider):

- **AI chat** — model understands request, plans, calls device tools
- **Workflow editor** **AI workflow authoring** — generate or edit steps

Scenarios that **don’t** use chat LLM and **don’t** consume chat Token:

- Workflow **trial run**
- **Task panel** batch run
- **Saved** plaintext flows or **imported `.spk`** flows

:::note Other compute

- **OCR** uses local engine (e.g. PaddleOCR NCNN), not chat Token.
- **VLM steps** bill separately per vision provider if configured — unrelated to chat Token.
- **Image/color matching** is local OpenCV — no LLM Token.

:::

</div>
</details>

## Related docs

- [Prerequisites & licensing](./prerequisites) — full checklist and self-check table
- [Getting started](./getting-started) — first open and device selection
- [Config & data dirs](./config) — LLM, VLM, data directory
- [Troubleshooting](./troubleshooting) — won’t open, won’t connect, execution failures
