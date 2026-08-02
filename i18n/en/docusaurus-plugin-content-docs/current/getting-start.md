---
title: EasyClick Android — First project
hide_title: false
hide_table_of_contents: false
sidebar_label: First project
description: 'Create your first EasyClick Android script project in IDEA, activate a device, and run'
keywords:
 - EasyClick
 - Android automation
 - IDEA
 - first project
---

:::info TL;DR
Create your first EasyClick **Android script project** in IDEA: install the plugin → create a module → USB-connect a device → activate and run. **No root required.** Prefer IDEA 2026.2+ (no IDEA activation after installing the plugin).
:::

:::tip
- This chapter uses IDEA macOS 2024.1.4 and EasyClick Android plugin 10.9.0; other versions may differ slightly
- Windows steps are the same as macOS
- IDEA 2019.1.0 through the latest builds are supported
- **IDEA 2026.2+ does not require activating IDEA** — install the plugin and use it
:::

# First script project

## Download the tools

- Downloads: [Tutorials & resources](./tools/download_resources)
- Install the latest EasyClick IDEA plugin — walkthrough: https://blog.csdn.net/qq_35246620/article/details/78289074
- Also see [Downloads](/community/download_area)
- IDEA: [JetBrains IDEA download](https://www.jetbrains.com/idea/download/)
- **Prefer IDEA 2026.2+ — no IDEA activation required after installing the plugin**
- On Windows, launch `bin/idea64.exe`

## Create a workspace and project

- Open an empty folder (Open)
<img src='/androidimg/first/openworkspace.png' alt="openworkspace.png" style={{zoom:'30%'}} />
- Workspace looks like this
<img src='/androidimg/first/workspace2.png' alt="workspace.png" style={{zoom:'30%'}} />
<br/>
- Right-click → New → Module
<img src='/androidimg/first/makeproject.png' alt="makeproject.png" style={{zoom:'30%'}} />
- Choose **EasyClick Android — script project**, then Next

<img src='/androidimg/first/selecttype.png' alt="selecttype.png" style={{zoom:'30%'}} />

- Enter a module name (Chinese or English OK; avoid spaces/special characters)

<img src='/androidimg/first/modulename.png' alt="modulename.png" style={{zoom:'30%'}} />
- Created structure (see the IDE tools chapter for details)
<img src='/androidimg/first/projectstruct.png' alt="projectstruct.png" style={{zoom:'30%'}} />

## Connect a device

- Open the bottom panel **EasyClick Android run log** for detailed output
- Menu: `EasyClick Android → Device connect → USB connect` (USB debugging must be on)
<img src='/androidimg/first/connectdevice.png' alt="connectdevice.png" style={{zoom:'30%'}} />
- Connecting installs and launches the APK automatically
- If install fails, download the APK from `EasyClick Android → Device connect → Download APK` and install manually
- Success looks like this
<img src='/androidimg/first/conok.png' alt="conok.png" style={{zoom:'30%'}} />

## Preview and run

- Right-click the project → Android → Preview / Run
<img src='/androidimg/first/previewpro.png' alt="previewpro.png" style={{zoom:'30%'}} />
- Or right-click inside a JS file for the same actions
- After the first successful run, use the top toolbar shortcut
<img src='/androidimg/first/runshort.png' alt="runshort.png" style={{zoom:'30%'}} />

:::tip
That's your first project — welcome to EasyClick development!
:::
