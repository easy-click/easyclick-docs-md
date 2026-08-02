---
title: EasyClick Android — OTG HID hardware
hide_title: false
hide_table_of_contents: false
sidebar_label: OTG HID hardware guide
description: 'EasyClick hot code updates — automation without accessibility or USB debugging'
keywords:
 - EasyClick
 - mobile automation scripts
 - automation software
 - script hot update
 - code hot update
 - automation without accessibility or USB debugging
 - ESP32
 - OTG
 - HID
 - S2
 - S3
 - download
 - tip
 - img
 - src
 - mobile automation
 - automation testing
 - script development
---
# Overview
:::tip
- Supported hardware: **ESP32-S2**, **ESP32-S3**
- Firmware is free; buy dev boards on Taobao, Pinduoduo, 1688, etc.
:::
## Hardware photo
- Example shows a basic adapter; for charge + OTG use a 3-in-1 adapter
- <img src="/androidimg/otg/s1.png" alt="" style={{zoom:'20%'}} />

## Download firmware
- Cloud drive: [Downloads](/community/download_area)
- Path: **Developer tools → Android resources → OTG HID firmware → ESP32-S3 or ESP32-S2** — download the `.bin` for your board
- **Keyboard** and **non-keyboard** builds — some apps detect keyboard HID; the non-keyboard build cannot use Home and similar input APIs
- Download **flash_download_tool.zip** for ESP32 to flash firmware


## Flash firmware
- Same steps as Bluetooth: [Flash Bluetooth firmware](/docs/advance/blehid#刷入固件)
- OTG firmware does not need the Bluetooth MAC — skip that step and flash only
- **S2**: hold **IO**, press **RST**, release both so the PC detects the board
- **ESP32-S2**: hold **BOOT**, tap **RST**
- **ESP32-S3**: after flashing, plug into the phone's **USB OTG** port, not the COM port
## Authorize and test
- First plug-in may show an authorization dialog
- <img src="/androidimg/otg/s2.png" alt="" style={{zoom:'20%'}} />
- Check **Always use** (wording may vary) and confirm
- **System settings → OTG HID settings** → **Connect OTG** — allow any permission prompt
- Click **Test HOME** — if you return to the home screen, it works; next step is scripting
- <img src="/androidimg/otg/s3.png" alt="" style={{zoom:'20%'}} />
## Use in scripts
- After the steps above and a successful test, write scripts and call API functions

