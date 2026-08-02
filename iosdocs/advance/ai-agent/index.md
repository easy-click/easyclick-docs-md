---
title: AI 智能体使用手册
description: EasyClick iOS USB 中控 AI 智能体完整使用说明
sidebar_label: 概述与目录
keywords:
  - EasyClick
  - iOS自动化
  - AI智能体
  - 工作流
  - 自然语言自动化
---

# AI 智能体使用手册

:::tip 适用版本

- 中控 **10.1.0 及以上**（**新中控**）
- 安装包中已包含 AI 智能体组件（一般无需单独安装）
:::

:::tip AI智能体视频教程
- 地址: https://space.bilibili.com/477518868/lists/8630468?type=season
- 目标是讲解AI智能体配置使用、以及工作流的使用等
:::

## 它能做什么

AI 智能体是 **新中控** 里的智能助手，帮您用 **说话的方式** 或 **拖拽流程图** 来操作已连接的 iPhone / iPad，例如：

- **AI 对话**：用中文描述需求，例如「帮我在第一台手机打开 Safari」，由 AI 理解并安排执行
- **工作流**：把常用操作做成可重复使用的自动化流程，支持一键运行、多台设备同时跑；**运行时不消耗 Token，无需连接大模型**
- **AI 写流程**：在编辑器里用自然语言描述，让 AI 帮您生成或修改流程
- **图色找图**：模板匹配、偏色找图，配合预览裁剪模板、一键取中心点再点击
- **OCR / VLM**：识文字、视觉大模型定位元素（界面树不够用时）
- **外部工具**（可选）：把常用的命令行程序、HTTP 接口、MCP 服务等接入，供流程或对话调用

:::tip 怎么打开

1. 启动 **新中控**
2. 点击 **「AI 智能体」** 按钮

后台服务由新中控自动管理，**无需** 单独启动其他程序。

:::

:::warning 使用前请先读 [环境与授权](./prerequisites)

所有 AI 工作流（含对话操作设备、试跑、批量任务）都需要 **USB 设备授权**，并按步骤类型配好 **自动化 IPA 环境** 或 **蓝牙/OTG 环境**。未完成时常见授权失败、无法点击截图。

:::

## 常见问题速查

<details class="ec-faq-item">
<summary><strong>要买哪种授权？和投屏授权一样吗？</strong></summary>

<div class="ec-faq-body">

AI 智能体需要 **USB 设备授权**（不是投屏授权）。购买后在 **授权中心 → 批量授权** 绑定。详见 [常见问题速查 · 授权](./faq) 与 [环境与授权](./prerequisites)。

</div>
</details>

<details class="ec-faq-item">
<summary><strong>一定要配大模型吗？</strong></summary>

<div class="ec-faq-body">

**AI 对话**、**AI 写流程** 需要配置大模型；**运行 / 试跑已保存工作流** 不需要。见 [常见问题速查 · 大模型](./faq) 与 [配置与数据目录](./config)。

</div>
</details>

<details class="ec-faq-item">
<summary><strong>跑工作流会消耗 Token 吗？</strong></summary>

<div class="ec-faq-body">

**不会。** 试跑、批量运行、跑 .spk 流程均 **不消耗对话大模型 Token**；仅 AI 对话与 AI 写流程会按 LLM 服务商计费。见 [常见问题速查 · Token](./faq)。

</div>
</details>

→ 完整说明：[常见问题速查](./faq)

## 界面一览

<img src="/index/ai-agent/chat.png" alt="AI 对话" width="720" />

→ 更多截图与上手路径：[界面预览](./screenshots)

## 推荐阅读顺序

| 章节 | 文档 | 适合什么时候看 |
|------|------|----------------|
| **0** | **[环境与授权](./prerequisites)** | **必读**：USB 授权、自动化 IPA、蓝牙/OTG 环境 |
| **0b** | **[常见问题速查](./faq)** | **折叠 FAQ**：授权类型、大模型、Token |
| 1 | [快速开始](./getting-started) | 第一次使用，配置大模型、选设备 |
| 1b | [界面预览](./screenshots) | 看主要界面截图与 30 秒上手路径 |
| 2 | [AI 对话](./ai-chat) | 用聊天方式驱动设备 |
| 3 | [工作流编辑器](./workflow-editor) | 编排、试跑、变量池、保存流程 |
| 4 | [工作流 .spk](./workflow-spk) | 导出 .spk 包给客户、导入只读流程 |
| 4b | [工作流 .spl 授权](./workflow-spk-license) | 签发授权、导入授权、有效期与 deviceId |
| 5 | [图色找图](./image-match) | 模板/偏色找图、预览裁剪、点击图标 |
| 6 | [工作流步骤参考](./workflow-steps) | 查某一步怎么配（查阅用） |
| 7 | [配置与数据目录](./config) | 改存储位置、模型、OCR/VLM、并发数 |
| 8 | [外部集成](./external-integration) | 对接命令行 / HTTP / MCP（进阶） |
| 8b | [定时任务](./workflow-schedule) | 按规则自动跑工作流 |
| 9 | [任务与监控](./tasks) | 看执行进度、暂停与继续 |
| 10 | [常见问题](./troubleshooting) | 打不开、连不上、执行失败 |

## 和「与 AI 结合辅助编程」有什么不同

| | [与 AI 结合辅助编程](../ios-usb-ai) | **AI 智能体（本手册）** |
|---|-----------------------------------|------------------------|
| 做什么 | 在 Cursor / IDEA 里 **写脚本** | 在专用界面里 **直接操作已连设备** |
| 适合谁 | 会写 EC 脚本、做复杂业务逻辑 | 想快速试自动化、批量跑流程 |
| 界面 | 编程软件 | AI 智能体窗口 |

两者可以一起用：复杂逻辑用脚本，日常批量任务用 AI 智能体。


