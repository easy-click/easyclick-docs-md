---
title: EasyClick Android Docs — Mobile Automation Scripts — Project Creation and Running
hide_title: false
hide_table_of_contents: false
sidebar_label: Project Creation and Packaging
description: 'EasyClick mobile automation scripts — create EasyClick projects in IDEA, preview, run, debug, package APK, view logs, and more'
keywords:
 - EasyClick
 - mobile automation scripts
 - automation software
 - game automation
 - IDEA development tools download
 - IDEA download
 - Android no root
 - create project
 - preview project
 - run project
 - package project
 - apk
 - IEC
 - APK
 - tip
 - docs
 - zh
 - cn
---


# Project Creation

## Create Project

:::tip
 - To create, run, and preview a project, see the [First Project](/docs/getting-start) chapter
:::

## IEC File Compilation
- Hot updates typically use IEC files
- Select the project, then use the right-click menu or the top IDEA menu
- `IDEA menu - EasyClick Android - Compile IEC File`
- After compilation, the file is saved as `build/release.iec` in the current project


## APK Packaging

- In the top IDEA menu, select `EasyClick Android - Package Project`
 - Standard packaging — sufficient for most users
 - Enterprise packaging — for cloud control; use only together with EC cloud control
 - Debug packaging — used during development to avoid detection of the EC debug package as a base debug build
 - After packaging, configure the package name via `IDEA menu - EasyClick Android - EasyClick Settings - Basic Settings - Debug version package name`. Then connect the device over USB so the APK is recognized correctly

- Packaging compiles source and outputs an APK; all logs appear in `EasyClick Android Run Log`
- After packaging, the APK is output to `current project folder\build\release.apk`. Transfer it to the phone and install

:::tip
 - If you use proxy mode, you need to activate the device. See the activation section in [Glossary](/docs/funcs/devtools/dev-tools-word)
:::



## Packaging Parameter Reference
<img src='/androidimg/devintro/pkg-1.png' alt="pkg-1.png" style={{zoom:'30%'}} />
<img src='/androidimg/devintro/pkg-2.png' alt="pkg-2.png" style={{zoom:'30%'}} />
<img src='/androidimg/devintro/pkg-3.png' alt="pkg-3.png" style={{zoom:'30%'}} />
<img src='/androidimg/devintro/pkg-4.png' alt="pkg-4.png" style={{zoom:'30%'}} />
<img src='/androidimg/devintro/pkg-5.png' alt="pkg-5.png" style={{zoom:'30%'}} />
<img src='/androidimg/devintro/pkg-6.png' alt="pkg-6.png" style={{zoom:'30%'}} />
