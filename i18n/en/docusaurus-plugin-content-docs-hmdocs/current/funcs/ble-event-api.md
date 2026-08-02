---
title: Bluetooth BLE Events
description: EasyClick automation scripts — HarmonyOS Next Bluetooth BLE events bleEvent
keywords:
 - EasyClick automation scripts HarmonyOS Next Bluetooth BLE events
 - bleEvent
 - BLE
 - ESP32
 - openSerial
 - setScreenSize
 - setSendCmdType
 - systemKey
 - keyPressChar
 - clickPoint
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - HarmonyOS Next
 - USB
---

## Overview {#说明}

- Inject touch and key events via **PC ↔ ESP32 Bluetooth HID board ↔ phone** (phone acts as HID Host)
- Script object prefix is `bleEvent`, e.g. `bleEvent.clickPoint(100, 200)`
- Before use, bind **UDID ↔ bleMac** in control center via right-click **"Bluetooth HID Settings"**; phone must be paired with the board
- For HarmonyOS Next USB control center 3.2.0+
- Setup and firmware flashing: [Bluetooth BLE Tutorial](/hmdocs/advance/hm-usb-ble)
- Different from [USB HID Events](/hmdocs/funcs/hid-event-api) (`hidEvent`): USB HID uses cable/AOA; this module uses a Bluetooth board

:::tip
- Touch uses **absolute pixel coordinates** (same as Android BleCommander); call `setScreenSize(width, height)` first
- Communication: `setSendCmdType(1)` serial (default) / `setSendCmdType(2)` network (board must be on WiFi)
- Return value: `null` or empty string = success; other strings = error message
- For long Chinese text, use proxy input (e.g. `inputText`) instead of HID key codes
:::

## Connection and Configuration {#连接与配置}

### forceRefreshSerialPort Force Refresh Serial Port and MAC {#forcerefreshserialport-强制刷新串口与-mac}

* Force refresh serial port and MAC mapping
* For HarmonyOS Next USB control center 3.2.0+
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let r = bleEvent.forceRefreshSerialPort();
    logd(r == null || r === "" ? "success" : r);
}
main();
```

### setScreenSize Set Screen Size {#setscreensize-设置屏幕尺寸}

* Set screen size (touch coordinate w/h context)
* If unknown, use screenshot width and height
* For HarmonyOS Next USB control center 3.2.0+
* @param w Screen width
* @param h Screen height
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let img = image.captureFullScreen();
    let w = img.getWidth();
    let h = img.getHeight();
    img.recycle();
    let r = bleEvent.setScreenSize(w, h);
    logd(r == null || r === "" ? "success" : r);
}
main();
```

### openSerial Open Serial Communication {#openserial-打开串口通信}

* Open serial communication (PC↔board, bleConnectMode=1)
* For HarmonyOS Next USB control center 3.2.0+
* @param timeout Serial timeout in milliseconds; default 15 seconds
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    bleEvent.setSendCmdType(1);
    let r = bleEvent.openSerial(15000);
    logd(r == null || r === "" ? "success" : r);
}
main();
```

### closeSerial Close Serial Communication {#closeserial-关闭串口通信}

* Close serial communication
* For HarmonyOS Next USB control center 3.2.0+
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let r = bleEvent.closeSerial();
    logd(r == null || r === "" ? "success" : r);
}
main();
```

### setSerialTimeout Set Serial Timeout {#setserialtimeout-设置串口超时}

* Set serial timeout
* For HarmonyOS Next USB control center 3.2.0+
* @param out Serial timeout in milliseconds; default 15 seconds
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let r = bleEvent.setSerialTimeout(15000);
    logd(r == null || r === "" ? "success" : r);
}
main();
```

### setSendCmdType Set Communication Method {#setsendcmdtype-设置通信方式}

* Set serial or network communication with the board; default is serial
* For HarmonyOS Next USB control center 3.2.0+
* @param tt 1 = serial, 2 = network
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    // 1 serial 2 network
    let r = bleEvent.setSendCmdType(1);
    logd(r == null || r === "" ? "success" : r);
}
main();
```

### setWifiInfo Set Board WiFi {#setwifiinfo-设置开发板-wifi}

* Set network info so the board can connect to WiFi
* For HarmonyOS Next USB control center 3.2.0+
* @param name WiFi SSID
* @param pwd WiFi password
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let r = bleEvent.setWifiInfo("your_ssid", "your_password");
    logd(r == null || r === "" ? "success" : r);
}
main();
```

### resetBle Reset Board {#resetble-重启开发板}

* Reset the board (equivalent to pressing RST)
* For HarmonyOS Next USB control center 3.2.0+
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let r = bleEvent.resetBle();
    logd(r == null || r === "" ? "success" : r);
}
main();
```

### light Blink LED {#light-点亮-led}

* Blink the board LED
* For HarmonyOS Next USB control center 3.2.0+
* @param num Number of blink cycles
* @param lightToOff Time from on to off in milliseconds
* @param offToLight Time from off to on in milliseconds
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let r = bleEvent.light(3, 200, 100);
    logd(r == null || r === "" ? "success" : r);
}
main();
```

### showBleName Show Bluetooth Name {#showblename-显示蓝牙名称}

* Show Bluetooth name for discovery and pairing
* For HarmonyOS Next USB control center 3.2.0+
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let r = bleEvent.showBleName();
    logd(r == null || r === "" ? "success" : r);
}
main();
```

### hideBleName Hide Bluetooth Name {#hideblename-隐藏蓝牙名称}

* Hide Bluetooth name
* For HarmonyOS Next USB control center 3.2.0+
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let r = bleEvent.hideBleName();
    logd(r == null || r === "" ? "success" : r);
}
main();
```

## System Keys and Keyboard {#系统键与键盘}

### systemKey System Key {#systemkey-系统按键}

* System key
* For HarmonyOS Next USB control center 3.2.0+
* @param key `home` / `recents` (recent tasks; Win+Tab on HarmonyOS) / `back`
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    bleEvent.setSendCmdType(1);
    bleEvent.openSerial(15000);
    logd(bleEvent.systemKey("home"));
    sleep(1000);
    logd(bleEvent.systemKey("recents"));
    sleep(1000);
    logd(bleEvent.systemKey("back"));
}
main();
```

### keyPress Key (ASCII Code) {#keypress-按键ascii-码}

* Key press
* For HarmonyOS Next USB control center 3.2.0+
* @param prefix Modifier key; can be empty: `alt` / `ctrl` / `gui` / `r_ctrl` / `r_shift` / `shift`
* @param code Integer ASCII; e.g. 65 = `A`, 97 = `a`
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    // Focus an input field first
    let r = bleEvent.keyPress("", 97);
    logd(r == null || r === "" ? "success" : r);
}
main();
```

### keyPressChar Character Key {#keypresschar-字符按键}

* Character key
* For HarmonyOS Next USB control center 3.2.0+
* @param prefix Modifier key; can be empty: `alt` / `ctrl` / `gui` / `r_ctrl` / `r_shift` / `shift`
* @param code Character, e.g. `a`
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    // Focus an input field first
    let r = bleEvent.keyPressChar("", "a");
    logd(r == null || r === "" ? "success" : r);
}
main();
```

## Touch {#触控}

### clickPoint Click Coordinates {#clickpoint-点击坐标}

* Click at absolute pixel coordinates
* For HarmonyOS Next USB control center 3.2.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    bleEvent.setScreenSize(1080, 2400);
    bleEvent.setSendCmdType(1);
    bleEvent.openSerial(15000);
    let r = bleEvent.clickPoint(540, 1200);
    logd(r == null || r === "" ? "success" : r);
}
main();
```

### doubleClickPoint Double-Click Coordinates {#doubleclickpoint-双击坐标}

* Double-click coordinates
* For HarmonyOS Next USB control center 3.2.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let r = bleEvent.doubleClickPoint(540, 1200);
    logd(r == null || r === "" ? "success" : r);
}
main();
```

### press Long Press Coordinates {#press-长按坐标}

* Long press coordinates
* For HarmonyOS Next USB control center 3.2.0+
* @param x X coordinate
* @param y Y coordinate
* @param delay Long press duration in milliseconds
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let r = bleEvent.press(540, 1200, 1500);
    logd(r == null || r === "" ? "success" : r);
}
main();
```

### swipeToPoint Swipe {#swipetopoint-滑动}

* Swipe from one coordinate to another
* For HarmonyOS Next USB control center 3.2.0+
* @param startX Start X coordinate
* @param startY Start Y coordinate
* @param endX End X coordinate
* @param endY End Y coordinate
* @param duration Duration in milliseconds
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let r = bleEvent.swipeToPoint(200, 800, 200, 400, 800);
    logd(r == null || r === "" ? "success" : r);
}
main();
```

### touchDown Touch Down {#touchdown-按下}

* Touch down at coordinates
* For HarmonyOS Next USB control center 3.2.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let r = bleEvent.touchDown(300, 900);
    logd(r == null || r === "" ? "success" : r);
}
main();
```

### touchMove Touch Move {#touchmove-移动}

* Move touch to coordinates
* For HarmonyOS Next USB control center 3.2.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let r = bleEvent.touchMove(320, 920);
    logd(r == null || r === "" ? "success" : r);
}
main();
```

### touchUp Touch Up {#touchup-抬起}

* Touch up at coordinates
* For HarmonyOS Next USB control center 3.2.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let r = bleEvent.touchUp(320, 920);
    logd(r == null || r === "" ? "success" : r);
}
main();
```

### multiTouch Multi-Touch {#multitouch-多点触摸}

* Multi-touch
* Touch parameters: action — 0 = down, 1 = up, 2 = move
* x: X coordinate / y: Y coordinate / pointer: finger index / delay: delay in ms
* For HarmonyOS Next USB control center 3.2.0+
* @param touch1 Touch point array
* @param timeout Multi-touch execution timeout in milliseconds
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let touch1 = [
        {"action": 0, "x": 500, "y": 1200, "pointer": 1, "delay": 20},
        {"action": 2, "x": 450, "y": 1100, "pointer": 1, "delay": 80},
        {"action": 2, "x": 400, "y": 1000, "pointer": 1, "delay": 80},
        {"action": 1, "x": 400, "y": 1000, "pointer": 1, "delay": 20}
    ];
    let r = bleEvent.multiTouch(touch1, 10000);
    logd(r == null || r === "" ? "success" : r);
}
main();
```

## Quick Connect Example {#快速连通示例}

```javascript showLineNumbers
function main() {
    let img = image.captureFullScreen();
    let w = img.getWidth();
    let h = img.getHeight();
    img.recycle();
    bleEvent.setScreenSize(w, h);
    bleEvent.setSendCmdType(1); // 1 serial 2 network
    let r = bleEvent.openSerial(15000);
    if (r != null && r !== "") {
        logw("communication failed: " + r);
        return;
    }
    bleEvent.systemKey("home");
    sleep(800);
    bleEvent.clickPoint(Math.floor(w / 2), Math.floor(h / 2));
}
main();
```
