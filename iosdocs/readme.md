---
title: EasyClick_iOS免越狱自动化脚本_产品介绍_EasyClick自动化脚本编写
hide_title: false
hide_table_of_contents: false
sidebar_label: iOS USB版本产品介绍
description: >-
  EasyClick iOS免越狱自动化脚本；新中控 10.2.0+ 内置 AI 智能体（中文对话 + 可视化工作流）；
  亦支持 EC 脚本开发与 Cursor 辅助编程。支持 iOS 12 - 27+。
keywords:
  - EasyClick苹果手机自动化脚本
  - iOS免越狱
  - AI智能体
  - 工作流
  - 自然语言自动化
  - 苹果手机脚本
  - ios手机投屏
  - iOS
  - EasyClick
  - USB
  - OCR
  - OpenCV
  - 手机自动化
  - 自动化测试
---

# iOS USB版本产品介绍

:::info 一句话摘要（TL;DR）
EasyClick iOS USB 版是**免越狱**真机自动化：可写 EC 脚本，或在新中控用 **AI 智能体**（中文对话 / 可视化工作流）批量操作 iPhone；支持 iOS 12–27+。跑已保存工作流**不消耗**大模型 Token。先读 [环境与授权](./advance/ai-agent/prerequisites)，再 [快速开始](./advance/ai-agent/getting-started)。
:::

:::tip 新中控 10.2.0+：AI 智能体

在新中控点击 **「AI 智能体」**，即可 **用中文对话驱动已连 iPhone**，或用 **可视化工作流** 批量执行自动化。  
**跑已保存的工作流不消耗大模型 Token。** 使用前请先阅读 **[环境与授权](./advance/ai-agent/prerequisites)**（USB 设备授权 + 自动化 IPA 或 蓝牙环境）。

→ [AI 智能体使用手册](./advance/ai-agent) · [快速开始](./advance/ai-agent/getting-started) · [10.2.0 更新说明](./changelog)

:::

:::tip USB新中控使用教程
- 地址： https://space.bilibili.com/477518868/lists/8621841?type=season
:::

苹果手机免越狱自动化软件，适合游戏自动化、办公自动化、自动化测试等场景。相对于市面上其他产品，EasyClick iOS **无需越狱**；支持纯软件自动化，也支持蓝牙 HID 硬件方案，降低风控风险；并提供开放 API，便于二次开发中控产品。

## 两种使用方式

| 方式 | 适合谁 | 从哪里开始 |
|------|--------|------------|
| **AI 智能体** | 不写代码；运维、测试、批量操作 | [环境与授权](./advance/ai-agent/prerequisites) → [快速开始](./advance/ai-agent/getting-started) |
| **EC 脚本开发** | 开发者；复杂业务逻辑 | [第一个工程](./firstproject) → [脚本函数](./funcs) |
| **AI 辅助写脚本** | 使用 Cursor / IDEA 的开发者 | [与 AI 结合辅助编程](./advance/ios-usb-ai) |

:::info AI 智能体 ≠ Cursor 写脚本

- **AI 智能体**：在新中控专用窗口里 **直接操作已连设备**（对话 + 工作流编辑器）。  
- **与 AI 结合辅助编程**：在 IDE 里用 Cursor 等 **编写、编译 EC 脚本（.iec）**。

:::

## 快速链接

| 主题 | 文档 |
|------|------|
| AI 智能体手册 | [概述与目录](./advance/ai-agent) |
| **常见问题速查** | [授权 / 大模型 / Token（折叠）](./advance/ai-agent/faq) |
| USB 授权与环境 | [环境与授权](./advance/ai-agent/prerequisites) |
| 签名 IPA / 开自动化 | [USB 投屏教程 · 签名 ipa](./advance/ios-usb-screen#签名ipa) |
| 蓝牙 BLE / OTG | [蓝牙 BLE 教程](./advance/ios-usb-ble) · [OTG 教程](./advance/ios-usb-otg) |
| 界面预览 | [AI 智能体界面预览](./advance/ai-agent/screenshots) |

## 常见问题（AI 智能体）

<details class="ec-faq-item">
<summary><strong>要买哪种授权？和投屏授权一样吗？</strong></summary>

<div class="ec-faq-body">

需要 **USB 设备授权**（跑脚本、AI 工作流），不是 **USB 投屏授权**。绑定步骤见 [环境与授权](./advance/ai-agent/prerequisites) · [购买授权](./advance/ios-usb-screen#购买授权)。

</div>
</details>

<details class="ec-faq-item">
<summary><strong>一定要配大模型吗？</strong></summary>

<div class="ec-faq-body">

**AI 对话**、**AI 写流程** 需要；**运行 / 试跑已保存工作流** 不需要。见 [配置与数据目录](./advance/ai-agent/config)。

</div>
</details>

<details class="ec-faq-item">
<summary><strong>跑工作流会消耗 Token 吗？</strong></summary>

<div class="ec-faq-body">

**不会。** 试跑与批量运行不消耗对话 LLM Token；仅 AI 对话与 AI 写流程会计费。→ [完整说明](./advance/ai-agent/faq)

</div>
</details>

## 界面预览

<img src="/index/ai-agent/cover.png" alt="AI 智能体" width="640" />

对话 · 工作流编辑器 · 任务面板等截图见 **[界面预览](./advance/ai-agent/screenshots)**。

## iOS USB版产品特性

- 简单易上手：AI 智能体 **无需写代码**；亦支持 JavaScript 脚本与智能 IDE
- 丰富的 API，图像识别，免费 OCR
- 可打包独立 `.iec` 发布；工作流可导出 `.spk` 分发给客户只读运行
- 免越狱，支持 iOS 12.0 ~ 27.0+
- 点击坐标区域内随机；色块、颜色、控件查找
- OpenCV 模板匹配，识别率 95% 以上
- 纯软件 + 纯硬件（蓝牙）双路径，降低风控

## 工具特性

- 使用 JavaScript 开发，Java 类库可直接复用
- 智能 IDE，屏幕实时同步
- 自带日志查看，实时看运行结果

## EasyClick iOS USB版能做什么

- 中文对话 / 工作流批量操作多台 iPhone（AI 智能体）
- App 自动化测试、数据提取、营销工具
- App 爬虫等脚本场景

## EasyClick iOS USB版适合人群

- 希望 **不写代码** 做批量自动化的运维与测试人员 → **AI 智能体**
- 有意学习自动化脚本、做复杂逻辑的研发人员 → **EC 脚本 + IDE**
- 企业 App 测试团队

## 技术交流群

- Q群1: 777164022 Q群2: 922739785 Q群3：647082990
- Q群4: 772810035 Q群5: 484379843 Q群6：435253761
- Q群7: 397570651 Q群8: 12076933 Q群9: 778278905
- Q群10: 205262444
