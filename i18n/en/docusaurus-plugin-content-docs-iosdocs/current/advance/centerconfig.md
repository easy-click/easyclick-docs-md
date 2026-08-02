---
title: System Configuration
description: EasyClick automation scripts — iOS no jailbreak — system configuration
keywords:
 - EasyClick automation scripts iOS no jailbreak system configuration
 - config
 - bridgebin
 - toml
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
 - remote mirroring
 - OCR
 - PPOCR
 - YOLO
 - Cursor
 - AI coding
---



## Bridge Configuration

The bridge configuration path is usually `bridgebin/config/config.toml` under the control center installation directory. You can edit it with Notepad.

Find the following configuration options in the folder:

```
[site]
# Control center address — used for distributed deployment
# ws:// is fixed; after it comes the IP of the PC running the control center + control center port
remote = "ws://127.0.0.1:8019"

[agent]
# Whether to enable device discovery: 1 = enabled, 2 = disabled
# Enabled by default; if using web tools mode deployed to a server, you may disable it
startup = 1
# Agent runtime port
# Sets the runtime port for the agent IPA
agentPort = 18500
# Screen mirroring runtime port
# Sets the runtime port for agent screen mirroring
screenPort = 18600
# Device discovery mode: 1 = listen, 2 = scan
scanMode = 1
# Agent startup timeout in seconds
# Time the control center waits to detect agent IPA startup result
detectTimeout = 15
# For iOS 15+ devices, whether to run the agent IPA as a normal app: 1 = yes, 2 = no
# For iOS 15+, this mode keeps automation running without restarting the phone
run15iOSIpaAgentAsApp = 2
# Whether to automatically reset USB connection when starting the agent service: 1 = yes, 2 = no
resetUsbConAuto = 2
# Screen mirroring quality: 1–100
screenSteamQuality = 50
```
