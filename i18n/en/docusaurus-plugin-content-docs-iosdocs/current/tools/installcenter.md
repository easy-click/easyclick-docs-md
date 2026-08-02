---
title: Install control center
description: EasyClick iOS USB — install and start the control center
sidebar_label: Install control center
keywords:
 - EasyClick
 - iOS USB
 - control center
 - automation
---

:::tip
- The control center runs scripts and maintains the automation environment
- Install **iTools / 爱思助手** first (driver for Windows)
- Examples below use version `9.19.0`
:::

:::tip Notes for 10.2.0+
10.2.0 uses a new UI shell (faster start, clearer layout) with built-in group screen control and standalone activator entry points. Flow matches older builds; replace the version folder with `v10.2.0`. Details → [**USB screen guide**](/iosdocs/advance/ios-usb-screen)
:::

## Download

- From [cloud downloads](/iosdocs/tools/download_resources)
- Windows: **EC package → iOS resources → USB edition → v9.19.0 → control center → ioscenter_windows-x64-9.19.0.zip**
- Extract to a path **without spaces / special characters / parentheses**
- macOS: **ioscenter_macos-amd64-9.19.0.dmg** — open and follow the installer<br/>
 <img src="/iosimg/dl-ios-center.png" alt="download center"/>

### Developer disk images (optional)

- Needed below iOS 17; skip on iOS 17+
- Download **EC package → iOS resources → DeveloperImage12.4-26.0.zip**
- Windows: extract into `bridgebin\config\DeveloperDiskImage`
- macOS: Finder → Applications → ioscenter → Show Package Contents → replace `Contents/java/app/bridgebin/config/DeveloperDiskImage`

## Start

- Windows: run **ioscenter.exe**
 <img src="/iosimg/dl-ios-center2.png" alt="start" style={{zoom:'80%'}} />

### Sign in

- Main UI → License center
- After register, complete phone verification → [**verify**](https://uc.ieasyclick.com/validateSelf?sign=BHgPnRRoTf/#/validateSelf)
- Account type: `Android center image / iOS account (USB, standalone) / license-cloud account`<br/>
 <img src="/iosimg/dl-ios-center4.png" alt="login" />
- If login fails, check Run status — bridge may be blocked by path characters or antivirus

### Center config (optional)

- If you changed the agent source `bundleId`, set the `bundleID` prefix here; otherwise skip
 <img src="/iosimg/dl-ios-center3.png" alt="bundle id" style={{zoom:'50%'}} />

## Start automation

- Required before scripts / group control
- Needs a signed agent IPA on the phone, e.g. `ios xx - easyclick-USB-agent-9.19.0.ipa`
- Device list → right-click → Start automation<br/>
 <img src="/iosimg/dl-ios-center8.png" alt="start automation" />
- Service status should change from red **No** to blue **Yes**<br/>
 <img src="/iosimg/dl-ios-center9.png" alt="service yes" />
- If it fails after ~1 minute → [**Test automation**](https://www.laoleng.vip/docs/tools/easyclick/ios-qk/qa/testenv)

### Test automation (optional)

- Device → right-click → Test automation<br/>
 <img src="/iosimg/dl-ios-center5.png" alt="test" style={{zoom:'50%'}} />
- Usually returns within 20s (up to ~1 min on iOS 17+)<br/>
 <img src="/iosimg/dl-ios-center6.png" alt="result" style={{zoom:'50%'}} />
- Failures → [**Test automation fixes**](https://www.laoleng.vip/docs/tools/easyclick/ios-qk/qa/testenv#%E5%A4%B1%E8%B4%A5)

## Device licenses

- Billed per phone (authors do not add extra fees)
- Two separate licenses — buy before using the matching feature
 - **USB device license**: debug / run scripts / record in group control
 - **USB screen license**: group mirroring
- Bind / transfer → [**License guide**](https://www.laoleng.vip/docs/tools/easyclick/ios-qk/qa/authorize)

## Run scripts

- Automation service **Yes** + **USB device license**

### Single device

- Right-click device → Run script → pick `.iec` (compile from IDEA)
 <img src="/iosimg/dl-ios-center7.png" alt="run script" style={{zoom:'50%'}} />

### Batch

- Bottom-left Scripts → right-click → Open script folder → drop `.iec` → Refresh → Run (all devices)
 <img src="/iosimg/dl-ios-center10.png" alt="batch" />
- Or group devices first, then run on the group

## Group screen control

- Home → Group screen control
- Automation on + **USB screen license**
- Demo → [**Bilibili intro**](https://www.bilibili.com/video/BV1UsBKYeEPa/?share_source=copy_web&vd_source=e808b0cf1c36665f5f5c240a5dd8bc60)
 <img src="/iosimg/dl-ios-center11.png" alt="group screen" />
