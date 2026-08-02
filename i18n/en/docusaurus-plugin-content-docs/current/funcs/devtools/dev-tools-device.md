---
title: EasyClick Android Docs — Mobile Automation Scripts — Device Connection
hide_title: false
hide_table_of_contents: false
sidebar_label: Device Connection
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
 - USB
 - png
 - WIFI
 - img
 - src
 - androidimg
 - APK
 - condevice
 - all
 - mobile automation
---


# Device Connection

- Location: `IDEA menu bar - EasyClick Android - Device Connection`

 <img src='/androidimg/condevice/all.png' alt="all.png" style={{zoom:'30%'}} />

## Device Connection
### USB Connection
- Connect the phone to the computer and enable USB debugging in phone settings (search online for how to enable USB debugging on your device)
- Open the `EasyClick Android Run Log` window at the bottom of IDEA
- Select `IDEA menu bar - EasyClick Android - Device Connection - USB Connection`
- After selection, the EasyClick APK is installed automatically and the proxy environment is activated automatically

### Wi-Fi Direct Connection
- Wi-Fi connection means the phone and computer are on the same LAN; IDEA can connect directly to the debug app
- Open the EasyClick app on the phone. If you do not know how to get the EasyClick app, see **APK Download and Installation**
 - After opening the app, tap the `≡` icon in the top-left corner to see `Current Phone IP`
 - <br/><img src='/androidimg/condevice/settip.png' alt="settip.png" style={{zoom:'30%'}} />
- Select `IDEA menu bar - EasyClick Android - Device Connection - Wi-Fi Direct Connection`
- Click Wi-Fi connection, then enter the phone IP address
 <br/><img src='/androidimg/condevice/con2.png' alt="con2.png" style={{zoom:'30%'}} />

### APK Download and Installation
- Some phones cannot install the APK directly via ADB; use download instead
- Select `IDEA menu bar - EasyClick Android - Device Connection - Download APK File`
- Copy the downloaded APK to the phone and install it manually
- Or select `IDEA menu bar - EasyClick Android - Device Connection - Scan QR Code to Install APK`

### Remote Debugging
- See [Remote Debugging](/docs/funcs/devtools/dev-tools-remote)


