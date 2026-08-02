---
title: New screen mirroring guide
description: EasyClick HarmonyOS Next USB — new screen mirroring guide
sidebar_label: New screen mirroring guide
keywords:
 - EasyClick HarmonyOS Next USB new screen mirroring guide
 - screen mirroring
 - group control
 - synchronized control
 - BLE
 - HID
 - OCR
 - EasyClick
 - mobile automation
 - automation testing
 - script development
 - HarmonyOS Next
---

# New screen mirroring guide

This chapter covers **HarmonyOS Next USB screen mirroring** (**3.1.0+**). Install the control center and connect devices first: [New control center guide](/hmdocs/advance/hm-usb-center).

:::tip Downloads
- Mirroring ships with the control center package — [Downloads](/community/download_area) or [Tutorials & resources](/hmdocs/tools/download_resources)
- Cloud path: **HarmonyOS Next resources → USB edition** — latest version
- Launch via **`新中控.exe`** (New Control Center) → **集控投屏 → 全新界面** (Group mirroring → New UI)
:::

:::tip Version notes
- **3.1.0+**: new mirroring UI, OCR PPOCR-v6
- **3.2.0+**: **USB + Bluetooth HID** mirroring preset, BLE gestures and board connection modes
:::

## How to open

Open mirroring from **新中控** (New Control Center) — do not double-click the screen app alone:

1. Double-click **`新中控.exe`** (New Control Center), wait, sign in ([New control center guide](/hmdocs/advance/hm-usb-center))
2. Confirm the control center is running (status bar)
3. Toolbar **集控投屏** (Group mirroring)
4. Choose **全新界面** (**New UI**, recommended) and confirm

First launch may show onboarding: welcome → light/dark theme → get started.

## UI overview

| Area | Description |
|------|------|
| **Left sidebar** | Workbench / More; select all, sync control, start/stop mirroring |
| **Thumbnail grid** | Device previews; check, box-select, right-click menu |
| **Main control area** | Large view for the main device; embed in sidebar or float / separate window |
| **System settings** | Input, mouse, OCR, mirroring preset, theme, etc. |

## Before mirroring

1. Phone connected via USB (or wireless debugging) to the control center
2. **Start automation** on target devices — service status **Yes**
3. Complete **USB device license** / **USB screen mirroring license** as needed
4. For BLE gestures: [bind Bluetooth BLE](/hmdocs/advance/hm-usb-ble) on the control center; phone paired with the board

## Mirroring presets (System settings → Mirroring settings)

Use preset cards for one-click setup:

| Preset | Gesture mode | Input suggestion | Use case |
|------|----------|----------|------|
| **USB + automation service** | **Proxy** | System default | Normal tap/swipe mirroring |
| **USB + Bluetooth HID** (3.2.0+) | **Bluetooth HID** | Follow HID | Gestures via ESP32 board |
| **Custom** | Manual | Manual | Advanced tuning |

Advanced (expand):

- **Gesture execution**: Proxy / Bluetooth HID (taps and swipes only; keyboard in **Content input**)
- **BLE board connection**: Serial / Network (board must be provisioned)

## Start mirroring

1. Confirm preset in system settings (default **USB + automation service** works)
2. Check devices on the left (or **Select all**)
3. Click **Start mirroring**; or thumbnail right-click → **Mirroring → Start mirroring**
4. Thumbnails appear in a few seconds; **Stop mirroring** to end

Thumbnail right-click also: **Mirroring settings**, **Landscape / Portrait**.

## Main device and sync control

1. Thumbnail right-click → **Set as main** (or **Clear main**)
2. Operate the phone from the main view
3. Enable **Sync control** in the sidebar to mirror actions to checked devices
4. Main title bar **控 / 鼠 / 键** (Control / Mouse / Keyboard): sync master switch, mouse, keyboard

### Main view layout (sidebar → More)

- **Floating window**: in-page float / **Separate window**
- **Page embed**: embed left / embed right

## Common operations

### Text input

1. **System settings → Content input**
2. **Input method**: system default (proxy) or **Follow HID** (BLE keyboard)
3. Typing mode, Enter to send, newline in input box
4. Type in the main area input box (Chinese: system default + automation recommended)

### Upload image / video

- Sidebar workbench tools, or thumbnail right-click → **Images & video → Upload image / video**
- Batch upload per device folder

### Open app / URL

- Thumbnail right-click → **More → Open app / Open URL**
- Or workbench shortcuts

### OCR (3.1.0+)

1. **System settings → OCR** — pick engine (**PPOCR-v6** available)
2. Optional **Release resources after recognition**
3. Thumbnail right-click **Recognize screen text**, or OCR on the main toolbar

### Scripts / multi-device reply

- Sidebar **More → Reply**: script reply, multi-device reply
- Multi-select thumbnails for batch reply

### System control

Sidebar **More → System control** or thumbnail **More**:

- Kill processes (all / selective), clear recents
- Lock / unlock, volume ±, flash-disconnect USB, reboot device

Most advanced actions require sign-in.

## Thumbnail context menu summary

| Category | Actions |
|------|------|
| Mirroring | Start/stop mirroring, mirroring settings, landscape/portrait |
| Main device | Set as main / clear main |
| Groups & naming | Set group, rename |
| Images & video | Upload image, upload video |
| Recognition | Recognize screen text |
| Bluetooth | **Bluetooth HID settings** (bind/test, 3.2.0+) |
| More | Open app/URL, cleanup, lock/unlock, reboot, flash-disconnect USB |

Box-select multiple thumbnails for batch menu (includes multi-device reply); enable **Drag to select thumbnails** in system settings.

## Other sidebar features

- **Device groups**: filter thumbnails; right-click to manage / batch group / sort / refresh
- **Common tools**: custom sort; upload media, size, orientation, swipe, HDC devices, license query, system settings, etc.
- **Resolution / orientation**: mirroring settings, thumbnail/main size, landscape/portrait

**Save bandwidth on group switch** reduces streaming for hidden groups.

## Other system settings tabs

| Tab | Highlights |
|-----|------|
| **Mouse** | Scroll wheel, swipe mode (normal/multi-touch), drag-select thumbnails, sync random delay |
| **Other** | Theme, default main on launch, auto-mirror, toolbar position, thumbnail toolbar visibility, USB bandwidth saving |

## Bluetooth HID mirroring flow (3.2.0+)

1. [Bind and test Bluetooth](/hmdocs/advance/hm-usb-ble) on the control center
2. Mirroring **System settings → Mirroring settings** → **USB + Bluetooth HID**
3. **Content input**: **Follow HID**; board connection: serial or network
4. After **Start mirroring**, taps/swipes on main/thumbnails use BLE
5. If issues persist, thumbnail right-click **Bluetooth HID settings** to recheck bind and serial

## Quick start checklist

1. Double-click **`新中控.exe`** (New Control Center) → **集控投屏 → 全新界面** (Group mirroring → New UI)
2. Preset **USB + automation service** (or BLE as needed)
3. Check devices → **Start mirroring**
4. Set main device → enable **Sync control** for group control
5. Use upload, OCR, scripts, system control as needed

## FAQ

- **Launch page asks to start service first**: double-click **`新中控.exe`** (New Control Center), wait for startup, then open mirroring from **集控投屏** (Group mirroring)
- **Picture but no touch**: check automation is on; for BLE, check bind and phone pairing
- **Sync not working**: enable **Sync control** in the sidebar, check target thumbnails, open **控/鼠/键** (Control/Mouse/Keyboard) on the main bar

## Related docs

- [Downloads](/community/download_area)
- [Tutorials & resources](/hmdocs/tools/download_resources)
- [New control center guide](/hmdocs/advance/hm-usb-center)
- [Bluetooth BLE guide](/hmdocs/advance/hm-usb-ble)
- [Install the control center](/hmdocs/tools/installcenter)
- [USB HID events](/hmdocs/funcs/hid-event-api)
- [Bluetooth BLE events](/hmdocs/funcs/ble-event-api)
