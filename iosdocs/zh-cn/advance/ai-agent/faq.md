---
title: 常见问题（授权 / 大模型 / Token）
description: AI 智能体使用前最常问的三件事：买什么授权、何时需要大模型、跑流程是否消耗 Token
sidebar_label: 常见问题速查
keywords:
  - USB授权
  - 大模型
  - Token
  - AI智能体
---

# 常见问题速查

下面三条是咨询里出现最多的问题。点击标题展开详情；完整环境清单见 **[环境与授权](./prerequisites)**，排障见 **[常见问题](./troubleshooting)**。

<details class="ec-faq-item">
<summary><strong>要买哪种授权？和投屏授权一样吗？</strong></summary>

<div class="ec-faq-body">

**不一样。** AI 智能体跑工作流、试跑、对话里操作设备，底层与 EC 脚本相同，必须使用 **USB 设备授权**。

| 授权类型 | 用途 |
|----------|------|
| **USB 设备授权** | 跑脚本、**AI 工作流**、自动化点击/截图等 |
| **USB 投屏授权** | 投屏、鼠标操作界面、插入视频等 |

**操作步骤：**

1. 通过官方或代理商 **购买 USB 设备授权**（不要买成投屏授权）
2. 手机 USB 连接中控后，打开 **授权中心 → 批量授权**（左侧设备 ID，右侧卡号）
3. 或在 [用户中心](https://uc.ieasyclick.com) **我的 iOS 设备 → 批量绑定授权**，再回中控 **授权中心 → 刷新授权**

另需按流程步骤配好 **自动化 IPA 环境** 或 **蓝牙/OTG 环境**，见 [环境与授权](./prerequisites) · [购买授权](/iosdocs/zh-cn/advance/ios-usb-screen#购买授权) · [签名 ipa](/iosdocs/zh-cn/advance/ios-usb-screen#签名ipa)。

</div>
</details>

<details class="ec-faq-item">
<summary><strong>一定要配大模型吗？哪些功能需要？</strong></summary>

<div class="ec-faq-body">

**不是全部功能都需要。** 大模型（LLM）只在「让 AI 理解自然语言并生成内容」时使用：

| 功能 | 是否需要大模型 |
|------|----------------|
| **AI 对话**（用中文描述让 AI 操作设备） | ✅ 需要 |
| **AI 写流程**（编辑器里用自然语言生成/改流程） | ✅ 需要 |
| **运行 / 试跑已保存工作流** | ❌ 不需要 |
| **任务面板批量跑流程** | ❌ 不需要 |
| **导入并运行 .spk 只读流程** | ❌ 不需要 |

未配置大模型时，AI 对话仍可使用 `list workflows`、`run …` 等简单指令，见 [AI 对话](./ai-chat)。

在工具栏 **「模型配置」** 填写 API 地址、模型名与密钥；对话与 AI 写流程处分别选用。详见 [配置与数据目录 · 大模型配置](./config#大模型配置)。

**VLM（视觉大模型）** 与对话 LLM 分开：仅 **VLM·视觉定位** 等步骤需要，配置在 `vlm_config.json`，见 [配置 · VLM](./config#视觉模型-vlm进阶)。

</div>
</details>

<details class="ec-faq-item">
<summary><strong>跑工作流会消耗 Token 吗？</strong></summary>

<div class="ec-faq-body">

**运行、试跑已保存的工作流不会消耗对话大模型的 Token**，也 **不需要** 当时连着大模型 API。

会消耗 Token（按您配置的 LLM 服务商计费）的场景：

- **AI 对话**里让模型理解需求、规划并调用设备工具
- **工作流编辑器**里用 **AI 写流程** 生成或修改步骤

不会走对话 LLM、因此 **不消耗对话 Token** 的场景：

- 工作流 **试跑**
- **任务面板** 批量运行
- 运行 **已保存** 的明文流程或 **导入的 .spk** 流程

:::note 其它算力说明

- **OCR** 步骤使用本地引擎（如 PaddleOCR NCNN），不走对话 Token。
- **VLM 步骤** 若配置了视觉模型，会按 VLM 服务商单独计费，与对话 Token 无关。
- **图色找图** 为本地 OpenCV 匹配，不消耗 LLM Token。

:::

</div>
</details>

## 相关文档

- [环境与授权](./prerequisites) — 完整清单与自检表
- [快速开始](./getting-started) — 首次打开与选设备
- [配置与数据目录](./config) — 大模型、VLM、数据目录
- [常见问题](./troubleshooting) — 打不开、连不上、执行失败
