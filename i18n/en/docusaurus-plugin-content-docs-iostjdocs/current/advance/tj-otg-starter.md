---
title: EasyClick Automation Scripts_iOS Scripts_iOS No Jailbreak_iOS No Hardware_Advanced Features_OTG HID Tutorial
hide_title: false
hide_table_of_contents: false
sidebar_label: OTG HID Tutorial
description: EasyClick automation scripts — iOS no jailbreak — advanced features — OTG HID tutorial
keywords:
 - EasyClick automation scripts
 - iOS scripts
 - iOS no jailbreak
 - iOS no hardware
 - advanced features
 - OTG HID tutorial
 - OTG
 - iOS
 - HID
 - EasyClick
 - ESP32S3
 - ESP32S2
 - MINI
 - img
 - src
 - iostjimg
 - mobile automation
 - test automation
---

# OTG HID Tutorial

## Supported Firmware

- Currently supports **ESP32S3** and **ESP32S2 MINI** boards
- Can connect via OTG adapter directly (no charging), or use a **3-in-1** adapter (charging, Ethernet, OTG)
- Buy boards and adapters on Pinduoduo/1688; **3-in-1** adapter recommended
- <img src="/iostjimg/otg/esp32.png" alt="" style={{zoom:'20%'}} />

## Download Firmware {#下载固件}

- In cloud drive **iOS Resources** → **Standalone Edition** → **OTG-HID Firmware**, download firmware for your board
- Note: firmware includes **relative coordinates** and **absolute coordinates** variants
- Relative mouse has broad compatibility but requires compensation ratio calculation; if error occurs, call the zero-reset function
- Absolute mouse works well on iOS 17+ with no compensation ratio; clicks are more accurate
- For S2: hold IO button, press RST, release both together for PC to recognize
- OTG firmware does not require Bluetooth MAC — ignore MAC prompts and flash firmware

## Flash Firmware

- Flashing is the same as Android — see [Android Bluetooth Flash Firmware](/docs/advance/blehid#刷入固件)
- OTG firmware does not require Bluetooth MAC — ignore MAC prompts and flash
- Select iOS standalone OTG firmware when flashing — do not pick the wrong one
- ESP32S2: hold BOOT → tap RST to flash
- ESP32S3: after flashing, connect phone via USB-OTG port, not COM port

## Connect Phone
- Example uses **3-in-1** adapter
- Connection diagram (Ethernet optional):
 - <img src="/iostjimg/otg/otg-1.jpg" alt="" style={{zoom:'20%'}} />
- After connecting, open **Settings** — **Ethernet** should show **EasyClick NCM + HID Input**
 - <img src="/iostjimg/otg/otg-2.png" alt="" style={{zoom:'20%'}} />
- Tap **EasyClick NCM + HID Input** to see assigned IP; if none, reboot board (press RST)
 - <img src="/iostjimg/otg/otg-3.png" alt="" style={{zoom:'20%'}} />
- With Ethernet plugged in, wired network info appears
 - <img src="/iostjimg/otg/otg-4.png" alt="" style={{zoom:'20%'}} />
- Complete info above means OTG connection succeeded

## Test Features
- With OTG connected, under **App Settings → OTG HID Settings**, press **Test HOME**. Returning to home means success
- You can also test via script code
- <img src="/iostjimg/otg/otg-5.png" alt="" style={{zoom:'20%'}} />

## Coordinate Calibration
- EC 6.6.0+ adds **Coordinate Calibration** under **App Settings → OTG HID Settings → Coordinate Calibration**
- Configure OTG options and pass test, then click **Coordinate Calibration**. Semi-transparent overlay; click **Start Calibration**
- Click **Close** when done
- Calibration is saved; scripts do not need `otgEvent.setScale`
- On iOS 17+, absolute coordinates recommended — no calibration needed
- <img src="/iostjimg/otg/otg-6.png" alt="" style={{zoom:'20%'}} />

## Shortcuts

- Bind shortcuts under **App Settings → OTG HID Settings → Shortcut Binding**
- <img src="/iostjimg/otg/otg-7.png" alt="" style={{zoom:'20%'}} />

### Keyboard Shortcut Binding
- Open **Settings → Accessibility → Keyboard → Full Keyboard Access → Commands**, tap a **Command**, enter keyboard shortcut
- <img src="/iostjimg/otg/otg-10.png" alt="" style={{zoom:'20%'}} />
- Double-tap Home, go to **App Settings → OTG HID Settings → Shortcut Binding**, choose **Keyboard Shortcut**, set keys, Send
- Dialog appears; double-tap Home back to Commands screen
- <img src="/iostjimg/otg/otg-8.png" alt="" style={{zoom:'20%'}} />
- After ~10 seconds shortcut updates; tap Done
- <img src="/iostjimg/ble/ble-7.png" alt="" style={{zoom:'30%'}} />
- Return to app shortcut binding for success; retry on failure
- <img src="/iostjimg/otg/otg-9.png" alt="" style={{zoom:'20%'}} />

### Mouse Button Binding
- Open **Settings → Accessibility → Touch → AssistiveTouch → Devices → EasyClick NCM + HID Input**, tap **Customize Additional Buttons**, press mouse button
- <img src="/iostjimg/otg/otg-11.png" alt="" style={{zoom:'20%'}} />
- Double-tap Home, go to **App Settings → OTG HID Settings → Shortcut Binding**, choose **Mouse Button**, pick button (Button 4+ recommended)
- <img src="/iostjimg/otg/otg-12.png" alt="" style={{zoom:'20%'}} />
- Send, switch to AssistiveTouch device settings, wait, pick command in custom command screen

## FAQ


### Required phone settings

- Settings → Accessibility → Touch → AssistiveTouch — enable AssistiveTouch
- Settings → Accessibility → Touch → AssistiveTouch → Tracking Sensitivity — all the way left (slowest)
- Settings → Accessibility → Touch → AssistiveTouch — enable Perform Touch Gestures, Show Onscreen Keyboard; Tap Sounds optional
- Settings → Accessibility → Touch → AssistiveTouch → Mouse Keys — Initial Delay and Maximum Speed all the way left
- General → Trackpad & Mouse → Tracking Acceleration — all the way left
- Settings → Accessibility → Keyboards — enable Full Keyboard Access
- Settings → Accessibility → Keyboards → **Commands** — customize shortcuts



### Mouse drift or inaccuracy

- Phone settings not enabled — see [Required phone settings]
- Coordinates not calibrated — use `otgEvent.resetZero` for 0,0
- Scale wrong — `bleEvent.getIPhoneScale`; test with fixed coordinates if model missing
- Screen width/height not set — especially after rotation

### Using with scripts

- In IDEA color panel, ***Capture Without Automation*** and ***Live Test (No Automation)***
- In scripts: ***activeSelf.screenshot (anti-detection)*** or ***App Settings → Shortcuts Screenshot***
- Or ***App Settings → System Screen Recording***
- Except node features, **OCR, YOLO, color finding, template matching** work normally

### Absolute coordinates

- Wrong start — usually 0,0; reboot if not
- Set all scale to 1

## Shortcuts Screenshot {#快捷指令截图}
- Shortcuts screenshot uses bound Shortcuts to capture and send image to script
- **App Settings → Shortcuts Screenshot**, default port **7760**. Start service or start via code
- <img src="/iostjimg/otg/otg-13.png" alt="" style={{zoom:'30%'}} />
- In **Shortcuts app**, create a shortcut:
- <img src="/iostjimg/otg/otg-14.png" alt="" style={{zoom:'30%'}} />
- Take screenshot, POST to `http://127.0.0.1:7760/img`
 - Method POST, body File, file = screenshot
 - If screenshot unavailable, choose variable → Magic Variable → Screenshot
 - <img src="/iostjimg/otg/otg-15.png" alt="" style={{zoom:'30%'}} />
 - <img src="/iostjimg/otg/otg-16.png" alt="" style={{zoom:'30%'}} />
 - <img src="/iostjimg/otg/otg-17.png" alt="" style={{zoom:'30%'}} />
- Tap triangle at bottom to run once; allow any permission prompts
- Success looks like:
- <img src="/iostjimg/otg/otg-18.png" alt="" style={{zoom:'30%'}} />

## System Screen Recording Screenshot {#系统录屏截图}
- Requires screen recording; recording prompt may need manual tap or blind mouse click
- **App Settings → System Screen Recording Settings** — configure frame rate and quality (defaults usually fine)
- **Start Service**, then **Start Recording**
- <img src="/iostjimg/otg/otg-19.png" alt="" style={{zoom:'30%'}} />
- Recording prompt — tap **Start Broadcast**, 3-second countdown
- <img src="/iostjimg/otg/otg-20.png" alt="" style={{zoom:'30%'}} />
- Red bar at top (or red dot on newer iOS)
- <img src="/iostjimg/otg/otg-21.png" alt="" style={{zoom:'30%'}} />
- Use script screen recording functions for images
- In IDEA, for capture without automation set capture mode to **Recording Mode (Phone Recording)**
