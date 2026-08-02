---
title: Install the IDE plugin
description: EasyClick HarmonyOS Next — install the IDEA plugin
sidebar_label: Install IDE plugin
keywords:
 - EasyClick
 - HarmonyOS Next
 - IDEA
 - plugin
---

:::tip
- Download the latest plugin from the cloud drive folder **HarmonyOS Next resources → Dev plugin**
:::

## 1. Install the plugin

- IDEA 2019.1.1 and newer are supported
- **IDEA 2026.2+ does not require activating IDEA** — install the plugin and use it
- Plugin install walkthrough: https://blog.csdn.net/qq_39597203/article/details/88683118

## 2. Create a project

- HarmonyOS Next and Android both use a multi-module layout
- Open an empty folder first
 <img src="/iosimg/image-20220105095538754.png" alt="" style={{zoom:'30%'}} />
- Right-click the project → New Module
 <img src="/hmimg/center/5.png" alt="" style={{zoom:'30%'}} />

- Choose **HarmonyOS Next USB script project**, enter a name, select `next`
 <img src="/hmimg/center/6.png" alt="" style={{zoom:'30%'}} /><br/>
 <img src="/hmimg/center/7.png" alt="" style={{zoom:'30%'}} /><br/>
 <img src="/hmimg/center/8.png" alt="" style={{zoom:'30%'}} />

## 3. Connect to the control center

- 1. Plugin installed
- 2. Agent / runtime ready
- 3. Control center running
- Menu: `EasyClick HarmonyOS Next → Device connect → USB connect to center`<br/>
 <img src="/hmimg/center/9.png" alt="" style={{zoom:'50%'}} />
- Keep the default address unless you changed the center port<br/>
 <img src="/hmimg/center/10.png" alt="" style={{zoom:'50%'}} />
- Confirm — connection status and logs appear in the bottom panel **EasyClick HarmonyOS Next run log**
