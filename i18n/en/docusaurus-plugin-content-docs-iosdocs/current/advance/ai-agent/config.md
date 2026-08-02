---
title: Config & data directories
description: LLM, storage locations, and common settings
sidebar_label: Config & data directories
keywords:
 - LLM
 - data directory
 - OCR
 - VLM
 - settings
---

# Config & data directories

- Most settings are done in the AI Agent window — **no** manual file editing required. This page covers common options; file paths are for backup and troubleshooting.
- On first use, **AI Settings** requires a **data storage directory** to avoid data loss.

## Settings dialog

Path: **AI chat** or **Workflow editor** → toolbar **"Settings"**

| Setting | Description |
|--------|------|
| **Data storage directory** | Where workflows, chat history, model config, etc. are stored |
| **Max devices per task** | Max phones **running at once** per task (default 100); **restart AI** after changing |
| **Auto-save workflow on tab switch** | Whether the editor auto-saves when switching tabs; takes effect **immediately** |

If prompted that the data directory is not ready on first open, follow the prompt into **Settings** to confirm.

:::tip After changing data directory

**Restart AI** (click **"Start AI"** again or close and reopen the window). **Logs** stay in `aibin/log/` and do not move with the data directory.

:::

## LLM configuration

Path: toolbar **"Model config"**

### Multiple profiles supported

For example, one DeepSeek profile for daily use and one local Ollama for testing. Each profile needs:

| Field | Description |
|----|------|
| **Name** | Display name in the UI |
| **API URL** | API endpoint from your provider |
| **Model name** | e.g. `deepseek-chat` |
| **API key** | API Key |

The UI provides templates for **DeepSeek, OpenAI, Ollama**, etc. — pick one and set the key.

### Where to select a profile

- **AI chat**: below the input box, choose which profile **this session** uses
- **Workflow AI panel**: choose which profile when generating flows

After saving, you generally **do not need to restart**; key changes take effect immediately.

### Without LLM configured

Chat accepts only simple commands like `list workflows`, `run …` — see [AI chat](./ai-chat).

## Vision model VLM (advanced)

Workflow **"VLM · visual locate"** steps and semantic element lookup use VLM as a fallback. Config is stored in **`vlm_config.json`** under the data directory.

:::note

There is **no dedicated settings UI yet** — edit that file manually; a graphical UI may come in a later version. Most users can skip this and use **node capture** or **image/color matching** instead.

:::

## OCR engine notes

Workflow **OCR** steps support **ocrType** (default `paddleOcrNcnnV5`):

| ocrType | Description |
|---------|------|
| `paddleOcrNcnnV5` | Default, local NCNN |
| `v5` | ONNX PaddleOCR v5 |
| `v4` | ONNX PaddleOCR v4 |
| `ocrLite` | OcrLite |

**Cached per device + engine type**: if one phone uses multiple types, each occupies memory separately; released after a formal **task ends**; next recognition may reload (first run slightly slower). **OCR preview** in the editor keeps the engine loaded for continuous debugging.

## Template directory for image matching

If you set a **working directory** in **Flow config**, create under it:

```text
{working directory}/assets/templates/ ← store PNG/JPG templates
```

In steps, set `templatePath` to a relative path, e.g. `assets/templates/btn_login.png`. See [Image/color matching](./image-match).

## What's in the data directory (for backup)

Default root: `aibin/data/aiagent/` (or the path you set in Settings)

| Folder / file | Contents |
|---------------|--------|
| `workflows/` | All workflows |
| `runtime_data/` | Files produced at runtime (default save `runtime_data/{deviceId}/`, read `runtime_data/readdata/`) |
| `llm_profiles.json` | LLM configuration |
| `vlm_config.json` | VLM configuration |
| `chat/sessions/` | Chat history |
| `tasks.json` | Recent task records |
| `external_registry.json` | External CLI / HTTP / MCP registration |
| `audit/` | Output files from some tasks |

**Backup / migration**: copy the entire data directory; logs in `aibin/log/` are optional.

In the workflow library, **Open folder** quickly locates a flow's files.

## Advanced: environment variables (optional)

Only if you are comfortable with command-line deployment. Can override default LLM URL, keys, etc., e.g. `IOS_AI_LLM_API_KEY`. For daily use, **Model config** in the UI is enough.

## Next steps

- Image/color matching and templates → [Image/color matching](./image-match)
- Connect external programs → [External integration](./external-integration)
- Path or startup issues → [Troubleshooting](./troubleshooting)
