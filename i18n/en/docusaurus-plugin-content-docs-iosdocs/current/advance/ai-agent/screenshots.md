---
title: UI preview & onboarding path
description: AI Agent main UI screenshots and 30-second onboarding path
sidebar_label: UI preview
keywords:
 - AI Agent
 - screenshots
 - workflow editor
---

# UI preview & onboarding path

:::note About screenshots

If your local UI differs slightly, trust the **new control center 10.1.0+** actual interface.

:::

## Main screens

### AI chat

Describe tasks in Chinese; pick devices and workflows on the left, then run.

<img src="/index/ai-agent/chat.png" alt="AI Agent chat UI" width="920" />
<br/>
<img src="/index/ai-agent/tasks2.png" alt="AI Agent chat UI" width="920" />

### Workflow editor

Visual flowchart, trial run, variable pool, and AI-assisted authoring.

<img src="/index/ai-agent/workflow.png" alt="Workflow editor UI" width="920" />

### Tasks & monitoring

View per-device progress during batch runs; pause and resume supported.

<img src="/index/ai-agent/tasks.png" alt="Task monitoring UI" width="920" />

## 30-second onboarding path

```mermaid
flowchart LR
  A[Start new control center] --> B[USB connect iPhone]
  B --> C[License Center: bind USB device license]
  C --> D[Set up automation IPA or BLE env]
  D --> E[Click AI Agent]
  E --> F{What to do?}
  F -->|Ad hoc| G[AI chat]
  F -->|Fixed flow| H[Workflow library / editor]
  G --> I[Pick devices → describe in Chinese → run]
  H --> J[Trial run → save → batch from task panel]
```

Detailed steps:

1. [Prerequisites & licensing](./prerequisites) — USB license + runtime environment
2. [Getting started](./getting-started) — open window, configure model, pick devices
3. [AI chat](./ai-chat) or [Workflow editor](./workflow-editor)

## Related docs

- [Overview & index](/iosdocs/advance/ai-agent/)
- [Troubleshooting](./troubleshooting)
