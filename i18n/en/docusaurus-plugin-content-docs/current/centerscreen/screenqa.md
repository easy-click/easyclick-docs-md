---
title: EasyClick Android — Control center screen mirroring FAQ
hide_title: false
hide_table_of_contents: false
sidebar_label: Mirroring FAQ
description: EasyClick control center screen mirroring — LAN mirroring, script management, synchronized control
keywords:
 - EasyClick
 - mobile automation scripts
 - automation software
 - control center screen mirroring
 - Douyin group control
 - Kuaishou group control
 - game group control
 - vpn
 - https
 - com
 - html
 - adb
 - HID
 - exe
 - bbs
 - ieasyclick
 - thread
 - mobile automation
---

# Mirroring FAQ


### Antivirus removes files
- Antivirus may quarantine `android-bridge.exe` and `seqb.exe`. Add both to the trust/allow list, then restart the software
### ADB conflicts

- Fixes:
 - [Connect device / ADB issues](https://bbs.ieasyclick.com/thread-147-1-1.html)
 - [ADB occupied](https://bbs.ieasyclick.com/thread-22-1-1.html)
### VPN blocks LAN
- Set the VPN to non-global mode, or exclude the EC app from the VPN so the PC and phone stay on the same LAN

### Mirroring is not smooth
- Wi‑Fi / network mirroring depends on the network. Try:
 - Left toolbar → mirroring settings → lowest frame rate and compression
 - Set the NIC to full-duplex — guide: [https://www.yisu.com/jc/72184.html](https://www.yisu.com/jc/72184.html)

### HID does not recognize the phone
- Turn off Developer options on the phone, then restart the HID host or PC

### Taps do nothing
- Network mirroring: enable accessibility automation
- HID mode: activate HID first
- USB mirroring: enable ADB debugging; on Xiaomi and similar phones, allow simulated taps in Developer options
- Example (Xiaomi): enable **USB debugging (Security settings) — Allow simulating input**
- Wrong click mode also breaks taps — Toolbar → Mirroring settings → Click mode: choose **HID** for HID mode, otherwise system default
### Keep the app alive
- Some phones (OPPO, vivo, etc.) kill background apps — keep EC alive:
 - Open the EC floating window
 - Set EC as the default IME
 - In EC system settings, disable battery optimization
- OPPO
 - Settings → Battery → turn off smart power saving
 - Settings → Battery → App freeze → disable for EC
 - Settings → Apps → EC → Battery → allow background; also allow autostart, allow other apps to start, floating window, notifications
- Huawei
 - Settings → Battery → Performance mode; more battery settings → keep network while asleep
 - Settings → Apps → EC → Battery details → Launch → allow autostart, associated launch, background activity

- Other brands are similar — check system settings or search for your model

### Bluetooth mouse does nothing
- First connect may still be pairing after a tap
- Or test the link under App → System settings → Bluetooth BLE

### OTG mouse does nothing
- Test under App → System settings → OTG HID
