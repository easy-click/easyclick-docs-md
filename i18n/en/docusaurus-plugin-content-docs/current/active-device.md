---
title: EasyClick Android — Activate device
hide_title: false
hide_table_of_contents: false
sidebar_label: Activate Android device
description: 'EasyClick mobile automation — activate Android devices via OTG, PC, USB, or IDEA'
keywords:
 - EasyClick
 - mobile automation scripts
 - automation software
 - EasyClick script activation
 - OTG device activation
 - USB agent activation
 - br
 - img
 - src
 - androidimg
 - active
 - png
 - width
 - IDEA
 - APP
 - mobile automation
 - automation testing
 - script development
---

# Activate Android device

## Activate via IDEA
- In IDEA: EasyClick dev tools → Activate device → Activation mode 1 or 2, then click
- Open EasyClick run logs — if activation succeeds, you are done
<br/>
<img src='/androidimg/idea-active.png' width='400' />
<br/>
<img src='/androidimg/idea-active-ok.png' width='400' />

## In-app self-activation
- Enable USB debugging on the device
- Enable ADB Wi‑Fi debugging on the device
- In the EC APK system settings, tap **Activate self**
- Activation runs automatically; if a USB authorization prompt appears, check **Always allow** and confirm<br/>
<img src='/androidimg/active-usb-debug.png' width='200' />
<br/>
- Activation succeeded<br/>
<img src='/androidimg/active-self-ok.png' width='200' />
<br/>
- When conditions 1 and 2 are met, you can call the script `activeSelf` function
- Manually enable ADB Wi‑Fi: [https://www.jianshu.com/p/a9543f2e89de](https://www.jianshu.com/p/a9543f2e89de)
- To permanently enable ADB via ROM changes, search online for guides
- You can use a standalone batch ADB Wi‑Fi opener


## Batch activation from PC

- From the EasyClick dev plugin download area, get the batch activation tool, extract, and run the matching `.exe`
<br/>
<img src='/androidimg/pc-active.png' width='400' />

## Device activates another device

- Demo video: https://www.bilibili.com/video/BV1zA411L7hy
- Terms: phone with EC installed = **A**; phone being activated = **B**
- Use an OTG cable; EC is installed on phone A
- Plug OTG male end into phone A, female end into phone B; in EC system settings → **Activate other device** — devices are scanned and activated automatically
- If phone B shows a USB authorization prompt, approve it
## Walkthrough

- Enable OTG
<br/>
<img src='/androidimg/otg-open.jpg' width='400' />

- OTG cable overview
<br/>
<img src='/androidimg/otg-usb.png' width='400' />


- Connect devices
<br/>
<img src='/androidimg/otg-con.png' width='400' />



- If a USB debugging dialog appears, allow it
<br/>
<img src='/androidimg/otg-usb-dev.png' width='400' />

- Enable file transfer for activation
<br/>
<img src='/androidimg/otg-file.png' width='400' />

- Activation succeeded
<br/>
<img src='/androidimg/ot-acive-ok.png' width='400' />

- If the connection fails, unplug and replug the cable and retry



## Accessibility keep-alive
- In IDEA: EasyClick dev tools → Activate / keep-alive device → Accessibility keep-alive
- Enter the packaged APK package name

## Batch enable ADB Wi‑Fi
- In IDEA: EasyClick dev tools → Activate / keep-alive device → Batch enable ADB Wi‑Fi debugging





## ROM built-in activator

- 1. Download `V1.12.0-EC-activate-device-accessibility-keepalive.zip (Chinese release filename)`, extract, find the activator binary for the phone CPU under the `agent` folder (search whether your CPU is ARM or x86)
- 2. Copy the file to a path on the phone, e.g. `/data/local/tmp/agent`
- 3. Run via adb: `adb shell /data/local/tmp/agent -mode=runagent -dport=19901,19902,19903 --password=123 &` to run in background
 - `password`: password
 - `dport`: ports, comma-separated

- 4. Run `adb shell netstat -ant` — if port `19901` is listening, it succeeded
- 5. For ROM auto-start, place the binary under `/system/bin/` and on boot run `/system/bin/agent -mode=runagent -dport=19901,19902,19903 --password=123 &` in background
