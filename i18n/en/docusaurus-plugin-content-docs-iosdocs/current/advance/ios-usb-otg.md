---
title: OTG HID tutorial
description: EasyClick automation scripts — iOS no-jailbreak OTG HID tutorial
keywords:
 - EasyClick automation scripts iOS no-jailbreak OTG HID tutorial
 - IPA
 - iOS
 - ipa
 - ble
 - BLE
 - ESP32C3
 - img
 - src
 - iostjimg
 - c3
 - EasyClick
 - mobile automation
 - automation testing
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview
- iOS currently supports **ESP32S3** boards
- OTG simulated actions do not conflict with proxy IPA — combine or use separately
- OTG can skip proxy IPA and use `image.captureFullScreenNoAuto` — no mirroring, less detection risk, no Bluetooth interference
- Firmware is free; buy boards on Taobao, Pinduoduo, 1688, etc.
- OTG uses the data cable — a 3-in-1 adapter (Ethernet + OTG + charging) is recommended
- <img src="/iosimg/otg/otg-0.png" alt="" style={{zoom:'20%'}} />

## Download firmware
- Cloud drive: **iOS resources → USB edition → OTG firmware**
- Currently **ESP32S3** only; **iOS 17+** required

## Flash firmware
- Same as Android — see [Android Bluetooth flash firmware](/docs/advance/blehid#刷入固件)
- Select **iOS USB OTG** firmware
- MAC address — same as Android docs

## Bind OTG device
- Label board and phone with MAC for mapping
- Control center → right-click device → **OTG HID settings → Bind OTG device**
 <br/><img src="/iosimg/otg/otg-1.png" alt="" style={{zoom:'30%'}} />
- Pick serial port; or clear **Show bound only**, **Force refresh**, or enter **last 8 MAC chars**
- **Bluetooth/OTG MAC** column shows bound hardware

## OTG networking
- Right-click → **OTG HID settings → Set WiFi (provisioning)**
 <br/><img src="/iosimg/otg/otg-2.png" alt="" style={{zoom:'30%'}} />
- Enter SSID/password, **Set**, restart or replug board

## Scan OTG IP
- Control center needs board IP — provision Wi‑Fi first, then scan
- Right-click → **OTG HID settings → Scan board IP**
- IP appears under **Bluetooth/OTG hardware IP**
- After bind + network, PC is only needed for setup

## Enable wireless debugging on phone
- OTG occupies the cable — use wireless debugging to reach the control center
- **Settings → General → Transfer or Reset iPhone → Reset → Reset Location & Privacy** — clears trust with PC
- Install **iTools / 爱思助手** on PC, connect USB, tap **Trust**
- **iTools → Toolbox → iTools screen mirroring** installs **Bonjour** (required for wireless debugging) — follow prompts until **Process manager → Bonjour service running**
- <img src="/iosimg/otg/otg-a-1.png" alt="" style={{zoom:'10%'}} /><img src="/iosimg/otg/otg-a-2.png" alt="" style={{zoom:'10%'}} /><img src="/iosimg/otg/otg-a-3.png" alt="" style={{zoom:'10%'}} />
- With **Bonjour** running, control center right-click → **Wireless debugging → Enable**, restart phone — connection shows **Network**
 <br/><img src="/iosimg/otg/otg-4.png" alt="" style={{zoom:'20%'}} /><img src="/iosimg/otg/otg-5.png" alt="" style={{zoom:'20%'}} />




## Test OTG
- With wireless debugging, firmware, and phone settings ready:
- Connect OTG — **mouse dot** on phone, or **Settings → Ethernet → EasyClick NCM+HID input**
- Right-click → **OTG HID settings → Test OTG**
 <br/><img src="/iosimg/otg/otg-3.png" alt="" style={{zoom:'30%'}} />
- Click **Mouse move** or **HOME** — phone should react


## Keyboard shortcuts
- Same as BLE — **Full Keyboard Access → Commands**; scripts use `otgEvent.keyPressChar`
- Right-click → **Bluetooth BLE settings → Add keyboard shortcut**
 <br/><img src="/iosimg/otg/otg-6.png" alt="" style={{zoom:'30%'}} />
- Example:
 - **Commands → Notification Center** → shortcut dialog
 - Control center: **gui** + **b**, **Send**, **Done** on phone
 - Send **gui+b** again → Notification Center opens

## Input
### With proxy IPA
- If proxy IPA starts automation, use **inputText**, etc.

### Custom IME
- **EC standalone main app** as IME: **imeApi** with `imeApi.forwardImeServer` first

### Without proxy IPA or IME
- Cloud: **iOS resources → iOS Shortcuts helper.zip**
- Shortcuts fetch URL → clipboard → paste (same as BLE doc)
- Example screenshots and steps match [Bluetooth BLE tutorial](./ios-usb-ble#without-proxy-ipa-or-ime)
- Script: [Type text via Shortcuts](/iosdocs/funcs/ble-event-api#利用快捷指令进行输入文字)

## Video and images
### With proxy IPA
- Proxy IPA inserts to Photos

### Without proxy IPA
- **iOS Shortcuts helper.zip** — same as BLE
- Shortcuts download to Photos — see BLE **Without proxy IPA** section
- Script: [Insert to Photos via Shortcuts](/iosdocs/funcs/ble-event-api#利用快捷指令进行插入相册)

## FAQ

### Can't enable wireless debugging
- **Reset Location & Privacy**, reconnect and trust
- **Bonjour** must run; phone and PC on same LAN and pingable — [reference](https://m.i4.cn/article/55710.html)
- If control center can't enable remote debug: **iTools → Feature toggles → WiFi debugging**

### Which settings to enable on iPhone for OTG
- Same as BLE — see [Which settings to enable on iPhone for BLE](./ios-usb-ble#which-settings-to-enable-on-iphone-for-ble)

### Using with scripts
- IDEA: **Non-automation capture**, **Live test (non-automation)**
- Scripts: `image.captureFullScreenNoAuto`, `image.startPreCapScreen`
- No node APIs; **OCR, YOLO, color, template match** work

### Absolute coordinates
- Start (0,0); restart phone if wrong

### Control center shows wireless serial name
- Restart control center; close flash tool before opening control center
