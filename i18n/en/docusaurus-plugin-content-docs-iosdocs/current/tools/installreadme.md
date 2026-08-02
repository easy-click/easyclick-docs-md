---
title: Install tools
description: EasyClick iOS USB — install overview
sidebar_label: Install tools
keywords:
 - EasyClick
 - iOS USB
 - IDEA
 - IPA
 - control center
---

- **Develop on Windows / macOS; for production runs we recommend Windows 10+**
>
> Develop: IDEA writes code → send → PC control center runs logic → send → phone agent IPA → phone actions<br/>
> Deploy: compiled `.iec` → PC control center runs logic → send → phone agent IPA → phone actions

## Terms

```json showLineNumbers
Agent IPA:
	- App installed on iPhone/iPad that bridges PC ↔ phone and drives actions
	- Unsigned third-party IPAs need signing (personal developer, TrollStore recommended, or build from our source with Xcode on macOS)

Control center:
	- Windows 10+ / macOS
	- Runs scripts, manages devices, UI params; can also mirror / group-control phones

Dev plugin:
	- IDEA plugin for Windows / macOS
	- IDEA 2026.2+ does not require activating IDEA — install the plugin and use it
```

## Install steps

- [1. Sign & install agent IPA](/iosdocs/tools/signagent)
- [2. Install control center](/iosdocs/tools/installcenter)
- [3. Install IDE plugin](/iosdocs/tools/installdevtools)
