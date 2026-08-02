---
title: EasyClick Android Docs — Mobile Automation Scripts — Accessibility and Proxy
hide_title: false
hide_table_of_contents: false
sidebar_label: Glossary
description: 'EasyClick mobile automation scripts, game automation scripts — how to connect Android devices, install the EasyClick APK, and activate Android devices'
keywords:
 - EasyClick
 - mobile automation scripts
 - game automation
 - IDEA development tools download
 - EasyClick device activation
 - IDEA download
 - Android no root
 - Android accessibility tap
 - tip
 - docs
 - zh
 - cn
 - centerscreen
 - openscreen
 - ROOT
 - shizuku
 - hid
 - mobile automation
---


# Glossary
:::tip
- This section explains common terms for newcomers to EasyClick Android
- Automation service in EasyClick is optional at runtime unless you need automated taps and similar actions
 - IDEA screenshots, node capture, and similar features also require automation to be enabled
:::



## Automation Modes
### Accessibility Mode
- Uses Android's built-in accessibility service for automation and can operate on nodes
- In the EasyClick app's `System Settings`, enable `Accessibility mode (automation)` or configure it in code
- **Accessibility service usually must be enabled manually**, but EasyClick control center screen mirroring (grant accessibility / activate proxy) can enable it automatically. See [Activate proxy mode](/docs/centerscreen/otherfunc#激活代理模式)
- Accessibility mode has the smallest permission footprint
:::tip
- Accessibility auto-start permission
 - Download the activator authorization from the software resource cloud drive: `Android Resources-V1.15.0-EC Device Activation-Accessibility Keep-Alive.zip`, then follow the included instructions
 - Control center screen mirroring can grant accessibility auto-start. See [Grant accessibility auto-start](/docs/centerscreen/openscreen#授权无障碍)
:::

### Proxy Mode
- After activating the device with one of the EasyClick activator, IDEA development tools, or EasyClick control center screen mirroring system, you gain the required permissions
- In the EasyClick app's `System Settings`, enable `Proxy mode (automation)` or configure it in code
- Proxy mode has broader permissions than accessibility mode: nodes, taps, and more are supported, including functions in the shell module

### ROOT
- ROOT mode has the broadest permissions; grant EasyClick app root on the phone (steps vary by root tool)
- In the EasyClick app's `System Settings`, enable `ROOT permission grant`; a ROOT authorization dialog appears — grant it
- After ROOT is granted, start `proxy mode automation` or `accessibility mode automation`

### shizuku
- Software similar to EasyClick activation mode; search online for details. You can use shizuku to start the automation service
- If shizuku permission is granted, start the automation service directly
### hid
- Uses external PC hardware to simulate taps without enabling accessibility or USB debugging, but nodes are not available
- HID supports taps but not node operations; IME and screenshots work; you can use find-image, find-color, OCR, and similar approaches for scripting
- See [HID host usage](/docs/advance/hid) and [HID events](/docs/funcs/hid-event-api)

## Screenshots
### Screenshot With Permission
- With accessibility mode automation, calling `image.requestScreenCapture` during permission request shows a screenshot authorization dialog; on some phones EasyClick accepts and taps authorize automatically

### Screenshot Without Permission
- With proxy mode automation, `image.requestScreenCapture` works without extra authorization
## Input Method
### IME Input
- Some apps block input; EasyClick includes its own IME. In phone Settings → Language & input, select the EasyClick keyboard
- In EasyClick app System Settings, enable `Set as default input method`; `Show input method keyboard` is optional
- IME functions start with `ime`; search the docs for usage, e.g. [IME input text](/docs/funcs/global/global-shortcut#imeinputtext-输入法输入数据)
## Proxy Activation
- Activation is needed when you have no root, no shizuku, do not want accessibility, but still want proxy mode automation
- Check activation under `EasyClick App System Settings - No ROOT activated`; checked means activated
### IDEA Activation
- Activate from IDEA: `EasyClick Android - Activate & Keep-Alive Device - Mode 1 activation or Mode 2 activation`. Check `EasyClick Android Run Log` for results
### PC Activation
#### Standalone Activator
 - Download from the software resource cloud drive: `Android Resources-V1.15.0-EC Device Activation-Accessibility Keep-Alive.zip`, then follow the included instructions
#### Control Center Screen Mirroring Activator
 - Download control center screen mirroring from the cloud drive: `Android Resources-Control Center Screen Mirroring System-EasyClick Android Control Center Screen Mirroring System v(version).zip`, then follow the included instructions
 - See [Activate proxy mode](/docs/centerscreen/otherfunc#激活代理模式)
