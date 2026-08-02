---
title: First project
description: EasyClick iOS USB — create your first project
sidebar_label: First project
keywords:
 - EasyClick
 - iOS USB
 - IDEA
 - first project
---

- Install the IDE plugin first → [Install IDE plugin](/iosdocs/tools/installdevtools)
- Video → [**Create a project & overview**](https://www.laoleng.vip/docs/free-courses/ec-ios-usb#2%E5%BC%80%E5%8F%91%E6%8F%92%E4%BB%B6-%E5%AE%89%E8%A3%85%E4%B8%8E%E7%AE%80%E4%BB%8B)

## 1. Create a project

- EasyClick uses a multi-module layout
- Create and open an empty folder, e.g. `ProjectIOS`
 <img src="/iosimg/ios-create-module-3.png" alt="open folder" />
- Right-click → New Module<br/>
 <img src="/iosimg/ios-create-module-2.png" alt="new module" />
- Choose **EasyClick iOS USB — script project**, then Next
 <img src="/iosimg/ios-create-module-1.png" alt="select type" />
- Enter a module name → Create
 <img src="/iosimg/ios-create-module-4.png" alt="module name" />

## 2. Connect to the control center

- Prerequisites
 - Agent installed
 - Control-center automation started
 - Phone has a **USB device license**
- Connect for debugging<br/>
 <img src="/iosimg/ios-create-module-5.png" alt="connect"/><br/><br/>
- Default `http://127.0.0.1:8019` — change IP:port to debug against another PC<br/><br/>
 <img src="/iosimg/ios-create-module-6.png" alt="address"/>
- Confirm — connection logs appear at the bottom of IDEA
- With multiple devices, select the one you want to debug
