---
title: New UI operation guide
description: EasyClick iOS USB screen mirroring — new UI operation guide, including start automation, screen mirroring, text input, script recording, and more
sidebar_label: New UI operation guide
keywords:
 - EasyClick automation scripts iOS no-jailbreak USB screen mirroring new UI operation guide
 - iOS
 - USB
 - screen mirroring
 - automation
 - EasyClick
 - mobile automation
 - automation testing
---

# New UI operation guide

This chapter covers how to use the new screen mirroring UI. First complete software download, IPA signing, and disk image setup in the [main tutorial](./ios-usb-screen).

:::tip USB screen mirroring video tutorial
- URL: https://space.bilibili.com/477518868/lists/8622038?type=season
- Covers EasyClick iOS USB screen features: synchronized mirroring, text input, taps, video/image upload, clipboard, script recording, and more
:::

****
## How to enter

1. Start and sign in to the control center
2. In the top toolbar, click **Group screen control**
3. In the dialog, select **New UI** (recommended), then click OK
4. Wait for the screen mirroring client window to open

## UI overview

After entering, you will see these areas:

| Area | Description |
|------|------|
| Sidebar | Left: **Workspace** and **More tools** tabs |
| Device list | Shows all connected devices; supports multi-select |
| Main control window | Center area showing the main control device screen |
| Mini screens | Draggable device thumbnails; right-click for the action menu |
| System settings | Open the settings panel from the sidebar or toolbar |

## Start automation

:::tip
Starting automation is done in the control center, not inside the screen mirroring client. Before starting automation, confirm in the control center:
- Proxy IPA is signed and installed on the phone
- Developer disk image is placed correctly
- BundleId prefix for the proxy IPA is filled in under control center system settings
- The same value is entered in the screen mirroring client **IPA bundle ID** setting (find **IPA bundle ID** in the sidebar or toolbar)
:::

1. Return to the control center main UI; confirm the phone is connected via USB and iTools/爱思助手 recognizes the device
2. In the control center device list, right-click the target device and choose **Start automation**
3. Wait briefly until **Service status** shows blue **Yes** — that means success
4. If it does not succeed within 1 minute, right-click and choose **Test automation** to see the error
5. After automation starts successfully, return to the screen mirroring client for further steps

:::tip
After the first successful start, you can click **Start screen mirroring** directly in the screen mirroring client without returning to the control center each time. The automation service keeps running in the background.
:::

## Screen mirroring

1. Ensure the automation service is running
2. In system settings, set screen mirroring to **Proxy IPA** mode
3. In the sidebar or toolbar, click **Start screen mirroring**
4. Wait a few seconds; the main control window shows the phone screen
5. You can operate the phone with the mouse:
 - Adjust swipe mode in system settings

## Common operations

Where to find these actions in the new UI:

### Text input

- In system settings under **Content input → Input settings**, choose the system default or a custom input method
- Custom input requires the File Transfer Helper IPA; see **Keyboard setup guide** in the settings panel

### Script recording

- Click **Record script** in the sidebar
- After creating a script, click **Start recording** and operate the phone in the main control window
- Save when done, then choose to run
- Note: you must operate in the main control window to record; running scripts requires USB device authorization

### Upload images / videos

- **iOS 15+**: Use **Upload video** or **Upload image** in the toolbar; or right-click a mini screen → Images & videos
 - Supports **Select folder** for batch upload, or **Select image/video** for single files
- **Below iOS 15**: Use **File Transfer Helper** — install the File Transfer Helper IPA and follow the dialog instructions

### Clipboard

#### Copy text to phone
- Click **Copy text** in the toolbar
- On iOS 15+, bring the proxy IPA to the foreground first (click **Open IPA**), then send text
- Below iOS 15, use File Transfer Helper

#### Read phone clipboard
- Right-click a mini screen → Clipboard → **Read clipboard to PC**
- On iOS 15+, bring the proxy IPA to the foreground first

### Open URL

- iOS 15+: Use **Open app** in the toolbar — open the proxy IPA first, then enter BundleId or URL
- Below iOS 15: Use File Transfer Helper

### Other

- USB flash disconnect, restart device, restart communication tunnel, etc. are in the device right-click menu or toolbar
- If automation fails to start, try USB flash disconnect; on iOS 17+, try restart communication tunnel

## Related docs

- [Main tutorial: screen mirroring prep & Bluetooth HID](./ios-usb-screen)
- [Install control center](/iosdocs/tools/installcenter)
- [Bluetooth BLE tutorial](/iosdocs/advance/ios-usb-ble)
