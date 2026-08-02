---
title: EasyClick Android — HID host usage
hide_title: false
hide_table_of_contents: false
sidebar_label: HID host usage
description: 'EasyClick hot code updates — automation without accessibility or USB debugging'
keywords:
 - EasyClick
 - mobile automation scripts
 - automation software
 - script hot update
 - code hot update
 - automation without accessibility or USB debugging
 - HID
 - winusb
 - tip
 - v3
 - zadig
 - Linux
 - Windows
 - '2.0'
 - Human
 - mobile automation
 - automation testing
 - script development
---
# HID host usage
:::tip
HID stands for Human Interface Device. This process requires connecting the phone to a PC and starting the EasyClick HID host program on the PC.
- v3.0.0 and later: we recommend the [HID mini host](/docs/centerscreen/hidlinux) — no driver install, lower cost, easier setup
- v3.5.0+: persistent networking lets the host run on a cloud server and slaves on a local LAN (or host and slaves on the same network)
:::

:::tip
- HID host 4.0.0+ supports the winusb driver on Windows — you can skip libusb, but if Zadig shows a driver other than winusb, switch it to winusb before running
- 3.6.0 and 4.2.0 ship keyboard and non-keyboard builds to avoid apps that detect keyboard simulation; the non-keyboard build cannot use Home and similar input keys
:::
## Download software
- Download the EasyClick HID host package **EasyClick-HID主控程序.zip** (includes Linux, Windows, and Zadig). Extract and run. Cloud drive: 123 Pan — https://www.123pan.com/s/6Hlmjv-QayIA.html
- For driver and environment setup, see Lao Leng's tutorial: https://www.bilibili.com/video/BV1Dp4y1K7JM/


:::tip
 - This guide is based on EasyClick HID host platform v1.5.0; other versions are similar
:::
## Linux platform
- Linux needs no driver install
- After extracting the zip, run **`sudo./hid-linux-v1.5.0`** in a terminal
- If you are unfamiliar with the terminal, search for how to run a binary as root
- Linux advantage: **no drivers — run the program, open the web UI, same workflow as Windows**

## Windows platform

### Run the software
- Double-click **hid-win-v1.5.0.exe** — a console window appears
- Open http://127.0.0.1:8988 in a browser to see the UI below
- If no device appears, the driver may be missing — go to **Install driver**

### Install driver
- Download **zadig-2.8.exe** from the cloud drive
- Right-click the file → **Run as administrator**
- In Zadig: **Options** → **List All Devices**
- In Zadig: **Options** → **Advanced Mode** for verbose logging
 <img src='/hid/1.png' width="500" />
- Example with a Pixel phone:
 - Box 1 is your phone; check **Edit** on the right to rename it
 - Box 2 shows the initial state — **Driver** may be **NONE** or **WinUSB**
 - Select **libusbK** on the right of the green arrow (box 3)
 - The button under box 3 may say **Install Driver** or **Replace Driver** — click it
 <br/>
 <img src='/hid/2.png' width="500" />
 - After install:
 - Red box 1 means success
 - If box 1 never appears and install hangs, check logs in box 2 — a **reboot** message means restart the PC
<br/>
<img src='/hid/3.png' width="500" />
- When done, find your phone under **libusbK** in Control Panel → Device Manager, then go to **Activate HID**
<br/>
<img src='/hid/4.png' width="500" />

### Activate HID

- After driver install, the phone should be recognized
<br/>
 <img src='/hid/5.png' width="800" />
- Click **Activate HID**

:::tip
 - On HID host 2.0.0 you can ignore this dialog and still use the host normally
:::

- The phone shows a USB accessory prompt — check the box and confirm
<img src='/hid/6.png' width="400" />
- When finished, **System settings → HID settings** in EC should show the device serial number — that means success
 <br/>
 <img src='/androidimg/hid-3.png' width="300"/>

:::tip
- This step lets the app read the serial number and match the control center device; if the center already has the serial, **Network scan** at the top also works
- After activation, the phone's PID and VID change from the previous values to **(0x18d1, 0x2d00)** (USB debugging off) or **(0x18d1, 0x2d01)** (USB debugging on)
- A VID/PID change is expected
:::

:::tip
- If VID/PID changed but the **host still does not see the device**, run Zadig again (see **Install driver**) and reinstall **libusbK**
- If Zadig lists **Adb** or **Android Accessory Interface** instead of the phone name, replace that device's driver with **libusbK** too
- Follow the same install steps as above
:::

### Initialize coordinates (deprecated in 2.0)
:::tip
- HID host 2.0.0 removed this step — activate HID mode and use coordinates directly
:::
- Click **Initialize coordinates** at the top — the system scans devices and calibrates
- The phone may show a USB accessory dialog — tap OK
- When done, the coordinates column shows current points
 <br/>
 <img src='/hid/8.png' width="800" />
### Network scan
- On the same subnet, click **Network scan** to request a link to the host
- The phone may show a USB accessory dialog — tap OK
- Resolution is submitted automatically when done
 <br/>
 <img src='/hid/8.png' width="800" />

## Activate HID device
- Click **Activate HID** in the HID host — it shows activating device
- On first activation the phone may show:
 <br/>
 <img src='/hid/6.png' width="400" />
- Check **Use by default for this USB accessory**
- Tap OK
- When finished, **System settings → HID settings** in EC should show the device serial number — success
 <br/>
 <img src='/androidimg/hid-3.png' width="300"/>
## Reset HID device
- **Reset HID device** clears the phone's default USB accessory setting — activate again afterward

:::tip
 - After activation you can develop scripts — set the HID host URL first (usually the LAN address). Example: host on PC at 192.168.1.3 → `http://192.168.1.3:8988`. You can also map the port with frp for remote access
 - Unplugging and replugging the cable may trigger a second authorization on the phone — confirm manually
:::

## Rename device
- Hover over the **Device serial number** column<br/>
 <img src='/hid/9.png' width="400" />
- Click rename and enter a label

## Block device
- If an entry is not a phone, click **Block**
- Blocked devices are stored in **blockHid.txt** next to the program
- To unblock, remove the line from **blockHid.txt** and restart the host

## Advanced networking (v2.2.0+) {#高级组网v220}
- Connect multiple HID host instances so one host controls the others
- Useful when several HID hosts work together — run the main host on Windows and slaves on Ubuntu **industrial PCs** to save cost
### Scan HID slaves
- Open **Advanced networking**, choose **I want to be the host**, click scan — slaves on the same LAN submit data automatically<br/>
 <img src='/hid/hid-4.png' width="400" />
### Connect slave to host
- Open the **slave** HID machine's admin page (not the host's), **Advanced networking** → **I want to connect to a host**, enter the host IP, save<br/>
 <img src='/hid/hid-5.png' width="400" />


## USB communication mode (v2.2.0+) {#usb通信模式v220}
- USB communication when the device network and HID host are on different networks (e.g. phone on mobile data, HID on Wi‑Fi)
- Click **Activate HID** — confirm the dialog on the phone or communication will fail<br/>
 <img src='/hid/6.png' width="400" />
- After activation, **USB read/write / communication time** shows **Enabled** and updates every ~3 seconds — communication is OK and scripts can run
 <br/>
 <img src='/hid/hid-6.png' width="400" />
- If the time column does not update, click **Reset HID**, then **Activate HID** again
## FAQ
### Host does not detect device
 - See **Install driver** and replace the driver
### Device not recognized after activating HID
 - See the green tips under **Activate HID** — reinstall the driver
### Network scan has no effect
 - Device may be offline or on another subnet — pick the matching subnet at the top and scan again
### Coordinates are inaccurate
 - Click **Initialize coordinates** to recalibrate
### Same-brand devices
 - Same brand shares PID/VID — do not reinstall the driver repeatedly
### adb and HID together
 - HID works regardless of adb; both can run at once
### libusbK driver cannot find device
 - Try **libusb-win32** in Zadig (select it on the right)
### Yellow exclamation mark
 - Driver may be wrong — reboot; if it persists, try **libusb-win32** in Zadig
### Multiple identical devices
 - One phone may appear as several entries — set one device's driver to **winusb**

### 4.0.0 error reference
- `device_info claim interface Error { kind: Unsupported, code: None, message: \"incompatible driver is installed for this device\" }`
 - Wrong driver on Windows — use **winusb**; other drivers can cause this

