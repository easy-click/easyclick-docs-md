---
title: Standalone Activator Tutorial
description: EasyClick iOS scripts — no jailbreak, no hardware — how to package an IPA project
keywords:
 - EasyClick
 - iOS scripts
 - automation scripts
 - iOS no jailbreak
 - iOS script tutorial
 - iOS no hardware scripts
 - iOS IPA packaging
 - UDID
 - EC
 - udid
 - iOS
 - img
 - src
 - iostjimg
 - online
 - USB
 - WIFI
 - mobile automation
---

## Online Network Initialization & Licensing

:::info Compatibility

- Requires EC standalone main app 5.4.0+
- For UDID licensing only; use USB initialization for ECID licensing
 :::

### Activate License Code

- If you purchased a standalone license from a reseller or official channel, log in to the [Admin Console](https://uc.ieasyclick.com/), go to **iOS Card Management → My iOS Devices → Batch Bind License**
- You can also rebind or transfer licenses in the admin console
- Get UDID via i4Tools (copy device identifier) or long-press copy from EC standalone main app

### License Initialization — Get UDID

- Install the latest EC standalone main app, open it, tap **Settings** (top right)
- Expand **Logged-in User Info**, tap **Online Initialize**
- <img src="/iostjimg/online/1.png" width="200" />
- Tap **Get UDID** — browser opens; tap **Get UDID** again on the browser page
- <img src="/iostjimg/online/2.png" width="200" />
- Tap **Get UDID** — download profile prompt; tap **Allow**
- <img src="/iostjimg/online/3.png" width="200" /><img src="/iostjimg/online/4.png" width="200" />
- After download, go to **Settings → General → VPN & Device Management**, find **Query Device UDID for Authorization**
- Tap it, tap **Install** (top right) through completion; browser opens automatically
- <img src="/iostjimg/online/5.png" width="200" /><img src="/iostjimg/online/6.png" width="200" /><img src="/iostjimg/online/7.png" width="200" />
- After UDID appears in browser, tap **Jump to App**, then **Read UDID** in the app
- <img src="/iostjimg/online/8.png" width="200" /><img src="/iostjimg/online/9.png" width="200" />
- After UDID success, you can log in

### License Initialization — Login
- After UDID success, tap **User Login** and enter username and password

### License Initialization — Refresh License
- If license not updated, tap **Refresh License**


## USB Initialization & Licensing

:::tip USB activation video tutorial
- URL: https://www.bilibili.com/video/BV1Ysgr6YELZ
:::


:::tip
Activator features

- Set phone license (license must be purchased)
- Initialize phone information
- WiFi connection mode — flash developer image without USB cable
- Provides tjCenter module API functions
 :::

:::tip New version (10.2.0+)
The new standalone activator client is available. Connect device via USB, then in control center click **Standalone Activator → New Interface** — no bridge URL or browser needed. See → [**Standalone Activator Tutorial**](/iosdocs/advance/ios-usb-activator)
:::

### Download Activator

- Bundled with USB control center
 - Cloud drive → iOS Resources → USB v6.16.0 → Control Center folder
- Standalone download
 - Cloud drive → iOS Resources → USB v6.16.0 → Bridge folder — download bridge for your OS
- Standalone control center screen mirroring system
 - Standalone control center can initialize standalone info, manage licenses, scripts, and parameters
 - **Recommended**: [Standalone Control Center Screen Mirroring System](/iostjdocs/funcs/tjcenter/)
- Cloud drive: [Tutorial Resources](/iostjdocs/tools/download_resources)

### Open Activator

- Via USB control center
 - USB control center includes activator:
 - Smart Control → Standalone Activator
 <img src="/iostjimg/tjcenter-1.png"/>
- Via bridge program
 - Activator is integrated in bridge
 - Run `ios-bridge.exe` (Linux/Mac: terminal `ios-bridge`)
 - Browser: [http://127.0.0.1:8020/active](http://127.0.0.1:8020/active)
 - Note: On Windows, tray message **Program started, running in tray** — click EasyClick tray icon → **Open Standalone Activator**

### Log In

- Open activator page; log in with iOS USB control center account. Register if needed
- Login button is top right

### Initialize Device {#初始化设备}

:::tip

**You must kill the main app process — do not open it when clicking Initialize<br/>**
**You must kill the main app process — do not open it when clicking Initialize<br/>**
**You must kill the main app process — do not open it when clicking Initialize<br/>**
**If still failing, restart activator and repeat initialization**

:::

- Refresh devices on activator page to reload connected devices
- Set **main app bundleId** (standalone main app bundle ID)
- Set agent app bundleId (for starting automation service)
- Select device(s), click Initialize
 <img src="/iostjimg/tjcenter-2.png"/>
- **License expired** — purchase standalone license from reseller or official
- **Success** — check **standalone main app → Settings → User Info** for device and user data
 <br/><img src="/iostjimg/tjcenter-3.png" width="200" />

### Licensing

- Activator page licensing
 - Rightmost column **License** button opens dialog
 <br/><img src="/iostjimg/tjcenter-4.png" width="200" />
 - Enter card number from official/reseller to bind
 <br/><img src="/iostjimg/tjcenter-5.png" width="200" />
 - After binding, click **Refresh License**
- USB control center licensing
 - Batch standalone licensing supported
 - License Center → Batch License
 <br/><img src="/iostjimg/tjcenter-6.png" width="400" />
 - License type: standalone; left = device ID, right = license code; click License, then **Update License**
 <br/><img src="/iostjimg/tjcenter-7.png" width="400" />

### WiFi Connection

- After WiFi mode, phone reconnects after reboot; flash developer image without cable
- Select device, click Enable WiFi Connection
 <br/><img src="/iostjimg/tjcenter-8.png" width="400" />
- On success, unplug cable, wait, refresh devices — connection type shows Network

### Other

- Activator also flashes developer images
 - Download images from cloud drive, extract to `config/DeveloperDiskImage/` under activator
- Start Service — start automation service; configure agent bundleId first

### Name and iOS Version Not Showing

- Repair drivers with i4Tools; after repair dialog, click Reset Winsock, disable firewall

### Activation Retry

- **Main app bundleId in activator config is required**
- Steps: **Unplug/replug phone → kill main app → Initialize**. If still failing:
 - 1. Restart bridge
 - 2. Unplug/replug phone again
 - 3. Kill main app (do not open), Initialize again
 - Success = user info on app Settings page

### iOS 17+ Activation

- iOS 17+ needs EC 7.1.0+ activator or USB control center 7.1.0+
- iOS 17+ needs usbncm driver — see USB [iOS 17 Guide](/iosdocs/otherinc/ios17), or use latest i4Tools live mirroring once
- Image flash and other steps same as older iOS versions

