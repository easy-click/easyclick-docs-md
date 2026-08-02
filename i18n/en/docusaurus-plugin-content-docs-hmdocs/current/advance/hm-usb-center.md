---
title: New control center guide
description: EasyClick HarmonyOS Next USB — new control center guide
sidebar_label: New control center guide
keywords:
 - EasyClick HarmonyOS Next USB new control center guide
 - control center
 - groups
 - automation
 - BLE
 - HID
 - EasyClick
 - mobile automation
 - automation testing
 - script development
 - HarmonyOS Next
---

# New control center guide

This chapter covers daily use of the **HarmonyOS Next USB control center** (**3.0.0+**). Install and connect steps: [Install the control center](/hmdocs/tools/installcenter).

:::tip Downloads
- Control center and screen mirroring: [Downloads](/community/download_area) or [Tutorials & resources](/hmdocs/tools/download_resources)
- Cloud path: **HarmonyOS Next resources → USB edition** — download the latest full package
:::

:::tip Version notes
- **3.0.0+**: new control center UI, group/script sidebar, batch rename
- **3.1.0+**: new screen mirroring UI, OCR PPOCR-v6
- **3.2.0+**: Bluetooth HID settings, script `bleEvent`, BLE gestures in mirroring
:::

## Start and sign in

1. Extract to an **English path** (no Chinese or spaces)
2. On Windows, double-click **`新中控.exe`** (**New Control Center**; use this entry — do not launch the screen app alone)
3. Wait for startup → **智控管理** (**Smart Control**); sign in or register if prompted
4. Status bar shows runtime state, online device count, etc.

If startup fails: check path and free ports; use **Restart control center** / **Refresh status** on the launch page.

## UI overview

Top bar — four sections:

| Entry | Description |
|------|------|
| **智控管理** (Smart Control) | Device list, scripts, groups, automation (main workspace) |
| **定时任务** (Scheduled tasks) | Run scripts on daily or interval schedules |
| **中控设置** (Control center settings) | Basic and cloud control settings |
| **授权中心** (License center) | Sign-in, USB device license, screen mirroring license |

**智控管理** (Smart Control) layout: left **groups + scripts**, center **search + device table**, top **Common / Device** toolbar.

## Connect devices

### USB

1. Enable developer mode and **USB debugging**
2. Connect via USB cable; trust this computer for debugging
3. Device appears in the list when ready

See [Install the control center — Connect a phone](/hmdocs/tools/installcenter#connect-a-phone).

### Wireless debugging

1. Prefer USB first, then toolbar **Enable wireless debugging** (avoids random phone ports)
2. Unplug USB, click **Scan wireless devices**, enter IP and port
3. Or read the wireless debug address in developer options and scan manually

## Common actions in 智控管理 (Smart Control)

### Toolbar · Common

| Button | Description |
|------|------|
| **执行脚本** (Run script) | Run the current script (`.iec` / `.js`) on checked devices |
| **停止脚本** (Stop script) | Stop scripts on selected devices |
| **开启自动化** (Start automation) | Start automation service (service status → Yes) |
| **停止自动化** (Stop automation) | Stop automation service |

You can also drag `.iec` files into the window.

### Toolbar · Device

| Button | Description |
|------|------|
| **新增分组** (New group) | Create a device group |
| **集控投屏** (Group mirroring) | Open mirroring — **全新界面** (**New UI**, recommended) or classic UI |
| **扫描无线设备** (Scan wireless devices) | Scan wireless debug devices |
| **回收内存** (Reclaim memory) | Trigger control center memory reclaim |
| **查看实时日志** (View live logs) | Requires a selected device |

### Search and shortcuts

- Search: device ID / serial / alias
- **Select all** / **Deselect all**, **Column display** to customize the table
- **UI参数设置(新版)** (UI parameters, new version): separate UI parameters window
- With focus on the device list: `Ctrl/Cmd+A` select all; `Ctrl/Cmd+S` select all and start automation; `Ctrl/Cmd+T` stop automation

### Left sidebar — groups and scripts

- **Groups**: filter devices; right-click to add / delete / rename / sort / refresh
- **Scripts**: single-click to select, **double-click** to run; right-click to run, refresh, open script folder

## Device context menu

Right-click one or more devices:

### Scripts and automation

- Run script / Stop script
- **Pause/Resume script** → pause or continue execution
- **Automation management** → start or stop automation

### USB HID control

- One-click USB HID, activate / reset USB HID
- USB-to-network connection, disable / enable USB debugging

Script APIs: [USB HID events](/hmdocs/funcs/hid-event-api).

### Bluetooth HID settings (3.2.0+)

- Bind / unbind Bluetooth BLE, test BLE, test keyboard keys
- Set WiFi (provisioning), scan board IP, find Bluetooth / find phone

Full steps: [Bluetooth BLE guide](/hmdocs/advance/hm-usb-ble).

### Connection and device

- Flash-disconnect USB, enable / disconnect wireless debugging
- Reboot device, clear offline devices (use with care)
- Set group / remove from group, set alias
- Copy device ID / serial / all info
- View license info (opens authorization center)

## Control center settings

### Basic

- Auto-start automation when a phone connects
- OpenCV on/off, random script start delay, memory reclaim interval
- Select all devices when switching groups
- Data storage path

### Cloud control

- Cloud URL, device ID field (serial / alias), heartbeat interval
Click **Save** after changes.

## Authorization center

1. Sign in (or register)
2. Filter connected / disconnected devices
3. **Batch license / rebind**: **USB device license** (scripts) vs **USB screen mirroring license** (mirroring)
4. Per-device right-click: add / rebind / transfer license

You can also bind on the website, then **Refresh** in the authorization center.

## Scheduled tasks

1. Open **定时任务** (Scheduled tasks)
2. **Add task**: daily / interval minutes / interval seconds; pick group or devices and script
3. **Start task** / **Pause task** to control scheduling

## Open screen mirroring

New mirroring launches from **新中控** (New Control Center) — no separate screen app:

1. Double-click **`新中控.exe`** (New Control Center), sign in, connect devices; **Start automation** if needed
2. Toolbar **集控投屏** (Group mirroring)
3. Choose **全新界面** (**New UI**, recommended — new mirroring client)
4. Continue with the [New screen mirroring guide](/hmdocs/advance/hm-usb-screen)

## Quick start checklist

1. Double-click **`新中控.exe`** (New Control Center) → wait → sign in
2. USB connect phone → device in list
3. (Optional) groups, alias, licensing
4. **Start automation** → service status **Yes**
5. Run scripts, or **集控投屏 → 全新界面** (Group mirroring → New UI)
6. (Optional) 3.2.0+ bind Bluetooth BLE for HID taps

## Related docs

- [Downloads](/community/download_area)
- [Tutorials & resources](/hmdocs/tools/download_resources)
- [Install the control center](/hmdocs/tools/installcenter)
- [New screen mirroring guide](/hmdocs/advance/hm-usb-screen)
- [Bluetooth BLE guide](/hmdocs/advance/hm-usb-ble)
- [First project](/hmdocs/firstproject)
