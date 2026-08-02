---
title: 界面预览与上手路径
description: AI 智能体主要界面截图与 30 秒上手路径
sidebar_label: 界面预览
keywords:
  - AI智能体
  - 截图
  - 工作流编辑器
---

# 界面预览与上手路径

:::note 关于截图

若与您本地 UI 略有差异，以 **新中控 10.1.0+** 实际界面为准。

:::

## 主要界面

### AI 对话

用中文描述任务，选择左侧设备与工作流后执行。

<img src="/index/ai-agent/chat.png" alt="AI 智能体对话界面" width="920" />
<br/>
<img src="/index/ai-agent/tasks2.png" alt="AI 智能体对话界面" width="920" />

### 工作流编辑器

可视化流程图、试跑、变量池与 AI 辅助写流程。

<img src="/index/ai-agent/workflow.png" alt="工作流编辑器界面" width="920" />

### 任务与监控

批量运行时查看每台设备进度，支持暂停与继续。

<img src="/index/ai-agent/tasks.png" alt="任务执行监控界面" width="920" />

## 30 秒上手路径

```mermaid
flowchart LR
  A[启动新中控] --> B[USB 连接 iPhone]
  B --> C[授权中心绑定 USB 设备授权]
  C --> D[配置自动化 IPA 或 蓝牙环境]
  D --> E[点击 AI 智能体]
  E --> F{要做什么?}
  F -->|临时操作| G[AI 对话]
  F -->|固定流程| H[工作流库 / 编辑器]
  G --> I[选设备 → 中文描述 → 执行]
  H --> J[试跑 → 保存 → 任务面板批量跑]
```

详细步骤：

1. [环境与授权](./prerequisites) — USB 授权 + 运行环境  
2. [快速开始](./getting-started) — 打开窗口、配模型、选设备  
3. [AI 对话](./ai-chat) 或 [工作流编辑器](./workflow-editor)

## 相关文档

- [概述与目录](/iosdocs/advance/ai-agent/)
- [常见问题](./troubleshooting)
