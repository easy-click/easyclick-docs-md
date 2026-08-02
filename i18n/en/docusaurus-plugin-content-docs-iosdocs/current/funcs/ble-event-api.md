---
title: Bluetooth BLE Functions
description: EasyClick automation scripts — iOS no jailbreak Bluetooth BLE functions
keywords:
 - EasyClick automation scripts — iOS no jailbreak Bluetooth BLE functions
 - bleEvent
 - setScale
 - openSerial
 - iOS
 - USB
 - BLE
 - getIPhoneScale
 - setScreenSize
 - setSendCmdType
 - setWifiInfo
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview {#说明}

- This feature uses Bluetooth to simulate swiping, input, and other actions. Currently supports ESP32C3 development boards
- For firmware flashing and connection details, see the [iOS USB Bluetooth Tutorial](/iosdocs/advance/ios-usb-ble)

:::tip
- Note: Firmware comes in **relative coordinates** and **absolute coordinates** variants. If you use **absolute coordinate firmware**, you must set the compensation ratio to 1: `bleEvent.setScale(1, 1)`
- Relative coordinates have broader compatibility, but you must calculate the compensation ratio yourself. Call the reset-to-zero function if errors occur
- Absolute coordinates work well on iOS 17+ with no compensation ratio calculation needed; clicks are more precise

:::

## bleEvent.openSerial Open Serial Communication {#bleeventopenserial-打开串口通信}

* Open serial communication
* Compatible with EC iOS USB version 9.20.0+
* @param timeout Serial communication timeout in milliseconds; default 15 seconds
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    if (!isDeviceAuthOk(1))
    {
        logw("Device authorization has expired")
        return;
    }
    initMouse();
    if (!openConnect(1, 15000)) {
        logw("Failed to open communication; cannot proceed. Check whether the Bluetooth device is working")
        return
    }
    logd("Communication OK, starting execution")
    // Move mouse to 0,0
    let r = bleEvent.resetZero()
    if (_bleResultOk(r)) {
        logd("Restored mouse original coordinates successfully")
    } else {
        logd("Failed to restore mouse original coordinates {}", r)
        return
    }
// test_mouse_action();
// test_multiTouch();

    test_input();

    // test_wifi_and_reset();
}

function test_mouse_action() {

    logd("Testing mouse move")
    let mv = bleEvent.mouseMove(300, 588)
    if (_bleResultOk(mv)) {
        logd("Mouse move succeeded")
        sleep(3000)
    } else {
        logw("Mouse move failed {}", mv)
    }

    bleEvent.resetZero()

    logd("Testing click")
    let cr = bleEvent.clickPoint(300, 400)
    if (_bleResultOk(cr)) {
        logd("Click coordinates succeeded")
    } else {
        logw("Click coordinates failed {}", cr)
    }
    sleep(2000)
    logd("Testing long press")
    cr = bleEvent.press(400, 500, 5000)
    if (_bleResultOk(cr)) {
        logd("Long press coordinates succeeded")
    } else {
        logw("Long press coordinates failed {}", cr)
    }
    sleep(2000)
    logd("Testing double-click")
    cr = bleEvent.doubleClickPoint(212, 300)
    if (_bleResultOk(cr)) {
        logd("Double-click coordinates succeeded")
    } else {
        logw("Double-click coordinates failed {}", cr)
    }
    sleep(2000)
    logd("Testing swipe")
    cr = bleEvent.swipeToPoint(200, 300, 500, 600, 2000)
    if (_bleResultOk(cr)) {
        logd("Swipe succeeded")
    } else {
        logw("Swipe failed {}", cr)
    }

    sleep(2000)
    let tdx = 200;
    let tdy = 300;
    logd("Testing touch down x={} y={}", tdx, tdy)
    cr = bleEvent.touchDown(tdx, tdy)
    if (_bleResultOk(cr)) {
        logd("Touch down succeeded")
    } else {
        logw("Touch down failed {}", cr)
    }
    let endMoveX = tdx;
    let endMoveY = tdy;
    for (let i = 0; i < 10; i++) {
        sleep(20)
        let movex = tdx + (i * 10);
        let movey = tdy + (i * 10);
        endMoveX = movex;
        endMoveY = movey;
        cr = bleEvent.touchMove(movex, movey)
        if (_bleResultOk(cr)) {
            logd("Move succeeded x={} y={}", movex, movey)
        } else {
            logw("Move failed x={} y={}", movex, movey)
        }
    }

    logd("Testing touch up x={} y={}", endMoveX, endMoveY)
    cr = bleEvent.touchUp(endMoveX, endMoveY)
    if (_bleResultOk(cr)) {
        logd("Touch up succeeded")
    } else {
        logw("Touch up failed {}", cr)
    }

}


function test_multiTouch() {
    sleep(1000)
    logd("Starting multi-touch test")
    // Last touch-up should match last move to prevent offset errors
    let touch1 = [
        {"action": 0, "x": 500, "y": 1200, "delay": 10},
        {"action": 2, "x": 450, "y": 1100, "delay": 200},
        {"action": 2, "x": 400, "y": 1000, "delay": 100},
        {"action": 2, "x": 350, "y": 900, "delay": 100},
        {"action": 2, "x": 300, "y": 800, "delay": 100},
        {"action": 1, "x": 300, "y": 800, "delay": 2}
    ]

    let rr = bleEvent.multiTouch(touch1, 10000)

    if (_bleResultOk(rr)) {
        logd("Multi-touch succeeded")
    } else {
        logd("Multi-touch failed {}", rr)
    }


}

function test_input() {
    logd("Starting input test — focus an input field to see results, or use the Notes app")

    sleep(1000)
    logd("Starting input of character a")
    let sar = bleEvent.keyPressChar("", "a")
    if (_bleResultOk(sar)) {
        logd("Input character a succeeded")
    } else {
        logd("Input character a failed {}", sar)
    }

    sleep(1000)
    logd("Starting ASCII input of character a")
    sar = bleEvent.keyPress("", 97)
    if (_bleResultOk(sar)) {
        logd("ASCII input character a succeeded")
    } else {
        logd("ASCII input character a failed {}", sar)
    }


    sleep(1000)
    let home = bleEvent.systemKey("home")
    if (_bleResultOk(home)) {
        logd("Home key succeeded")
    } else {
        logd("Home key failed {}", home)
    }
}


function test_wifi_and_reset() {
    logd("Example for WiFi info and restart — usually set once, no need to repeat")
    let ir = bleEvent.setWifiInfo("wdes", "11111111")
    if (_bleResultOk(ir)) {
        logd("Set WiFi info succeeded")
    } else {
        logw("Set WiFi info failed {}", ir)
    }
    sleep(1000)
    let rr = bleEvent.resetBle()

    if (_bleResultOk(rr)) {
        logd("Reset development board succeeded")
    } else {
        logw("Reset development board failed {}", ir)
    }
}


function initMouse() {
    let msg = device.getDeviceMsg()
    let productType = "";
    let bleWifiIp = "";
    if (!_bleResultOk(msg)) {
        // Get iPhone type to determine scale standard
        let bb = JSON.parse(msg);
        productType = bb["productType"];
        bleWifiIp = bb["bleWifiIp"];
    }


    let scale = bleEvent.getIPhoneScale();
    logd("Current phone type: {}, scale ratio: {}, BLE board IP: {}", productType, scale, bleWifiIp)
    // Set scale ratio so mouse movement matches on-screen pixels
    bleEvent.setScale(scale, scale)
    let img = image.captureFullScreenNoAuto();
    if (img) {
        let w = img.getWidth();
        let h = img.getHeight();
        if (w > 0 && h > 0) {
            // Set screen width and height to prevent cursor drifting off-screen; otherwise control coordinates manually
            logd("Setting screen width and height")
            bleEvent.setScreenSize(w, h)
        }
    }
}

/**
 * Connection mode
 * @param type 1 = serial port, 2 = network
 * @param timeout Timeout
 * @return {boolean} true = success, false = failure
 */
function openConnect(type, timeout) {
    if (type == 2) {
        logd("Setting network communication mode...")
        bleEvent.setSendCmdType(2)
        return true;
    } else {
        logd("Preparing to open serial communication...")
        bleEvent.setSendCmdType(1)
        // Try opening three times; failure if still not working
        for (let i = 0; i < 3; i++) {
            // Close once first
            // bleEvent.closeSerial();
            sleep(1000 + (200 * 3))
            let rr = bleEvent.openSerial(timeout)
            if (_bleResultOk(rr)) {
                return true;
            }
        }

    }
    return false;
}


function _bleResultOk(r) {
    return r == null || r == "";
}

main();
```

## bleEvent.setScale Set Mouse Compensation Ratio {#bleeventsetscale-设置鼠标补偿比率}

* Set mouse compensation ratio
* Note: If you use **absolute coordinate firmware**, you must set the compensation ratio to 1
* If your device model is not listed, calculate it yourself by testing mouse movement
* The ratio of how many pixels move per mouse unit; default is 2.0
* iPhone 6/7/8 375 x 667: set to 2.0 — standard 16:9, no safe area interference.
* iPhone 11 / XR 414 x 896: set to 1.96 — taller screen, system Y-axis acceleration compensation.
* iPhone X/XS/11Pro 375 x 812: set to 1.98 — 19.5:9 aspect ratio, slight acceleration.
* iPhone 12/13/14/15 390 x 844: set to 1.97 — different logical points from 11, slightly different acceleration curve.
* Plus / Max series 414 x 896 / 430 x 932: set to 1.94 ~ 1.95 — tallest screens; system greatly increases Y-axis gain for operational efficiency.
* These values are experimental estimates, not algorithm-derived
* Compatible with EC iOS USB version 9.20.0+
* @param x_scale X-axis scale (float)
* @param y_scale Y-axis scale (float)
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See bleEvent.openSerial example
}

main();
```

## bleEvent.getIPhoneScale Get Calculated Scale Value {#bleeventgetiphonescale-获取计算缩放值}

* Returns scale value based on iPhone hardware identifier prefix (ignoring digits after comma)
* @returns `{number}` Float scale ratio

```javascript showLineNumbers
function main() {
    // See bleEvent.openSerial example
}

main();
```

## bleEvent.setScreenSize Set Screen Size {#bleeventsetscreensize-设置屏幕尺寸}

* Prevents mouse drift when the cursor moves off-screen
* If screen size is unknown, use the width and height from a screenshot
* Compatible with EC iOS USB version 9.20.0+
* @param w Screen width
* @param h Screen height
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See bleEvent.openSerial example
}

main();
```

## bleEvent.setSendCmdType Set Communication Mode {#bleeventsetsendcmdtype-设置通信方式}

* Set communication mode
* Sets whether to communicate with the development board via serial port or network
* Default is serial port
* Compatible with EC iOS USB version 9.20.0+
* @param tt 1 = serial port, 2 = network
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See bleEvent.openSerial example
}

main();
```

## bleEvent.setWifiInfo Set Network Info {#bleeventsetwifiinfo-设置网络信息}

* Set network info
* Allows the development board to connect to the network
* Compatible with EC iOS USB version 9.20.0+
* @param name WiFi name
* @param pwd WiFi password
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See bleEvent.openSerial example
}

main();
```

## bleEvent.resetBle Restart Development Board {#bleeventresetble-重启开发板}

* Restart development board
* Equivalent to pressing the development board RST button
* Compatible with EC iOS USB version 9.20.0+
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See bleEvent.openSerial example
}

main();
```

## bleEvent.closeSerial Close Serial Communication {#bleeventcloseserial-关闭串口通信}

* Close serial communication
* Compatible with EC iOS USB version 9.20.0+
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See bleEvent.openSerial example
}

main();
```

## bleEvent.mouseMove Move Mouse {#bleeventmousemove-移动鼠标}

* Move mouse
* Move mouse only; no press action
* Compatible with EC iOS USB version 9.20.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See bleEvent.openSerial example
}

main();
```

## bleEvent.resetZero Reset Mouse to Zero {#bleeventresetzero-鼠标归零}

* Reset mouse to zero
* Move mouse to the upper-right corner at 0,0
* Compatible with EC iOS USB version 9.20.0+
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See bleEvent.openSerial example
}

main();
```

## bleEvent.touchDown Touch Down at Coordinates {#bleeventtouchdown-按下坐标点}

* Touch down at coordinates
* Compatible with EC iOS USB version 9.20.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See bleEvent.openSerial example
}

main();
```


## bleEvent.touchMove Move Touch Point {#bleeventtouchmove-移动坐标点}

* Move touch point
* Compatible with EC iOS USB version 9.20.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See bleEvent.openSerial example
}

main();
```

## bleEvent.touchUp Touch Up at Coordinates {#bleeventtouchup-抬起坐标点}

* Touch up at coordinates
* Compatible with EC iOS USB version 9.20.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See bleEvent.openSerial example
}

main();
```


## bleEvent.clickPoint Click Coordinates {#bleeventclickpoint-点击坐标点}

* Click coordinates
* Compatible with EC iOS USB version 9.20.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See bleEvent.openSerial example
}

main();
```


## bleEvent.press Long Press Coordinates {#bleeventpress-长按坐标}

* Long press coordinates
* Compatible with EC iOS USB version 9.20.0+
* @param x X coordinate
* @param y Y coordinate
* @param delay Long press duration in milliseconds
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See bleEvent.openSerial example
}

main();
```



## bleEvent.doubleClickPoint Double-Click Coordinates {#bleeventdoubleclickpoint-双击坐标}

* Double-click coordinates
* Compatible with EC iOS USB version 9.20.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See bleEvent.openSerial example
}

main();
```

## bleEvent.swipeToPoint Swipe {#bleeventswipetopoint-滑动}

* Swipe from one coordinate to another
* Compatible with EC iOS USB version 9.20.0+
* @param startX Start X coordinate
* @param startY Start Y coordinate
* @param endX End X coordinate
* @param endY End Y coordinate
* @param duration Duration in milliseconds
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See bleEvent.openSerial example
}

main();

```


## bleEvent.multiTouch Multi-Touch {#bleeventmultitouch-多点触摸}

* Multi-touch<br/>
* Touch parameters: action — generally 0 = down, 1 = up, 2 = move<br/>
* x: X coordinate<br/>
* y: Y coordinate<br/>
* pointer: which finger touch point (1, 2, 3, etc., representing the nth finger)<br/>
* delay: milliseconds to delay before executing this action
* Compatible with EC iOS USB version 9.20.0+
* @param touch1 Touch point array for the first finger, e.g. `[{"action":0,"x":1,"y":1,"pointer":1,"delay":20},{"action":2,"x":1,"y":1,"pointer":1,"delay":20}]`
* @param timeout Multi-touch execution timeout in milliseconds
* @returns `{string}` null or empty string on success; otherwise an error message
```javascript showLineNumbers
function main() {
    // See bleEvent.openSerial example
}

main();

```



## bleEvent.systemKey System Key {#bleeventsystemkey-系统按键}

* System key
* Compatible with EC iOS USB version 9.20.0+
* @param key Currently: home, recents = recent tasks
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See bleEvent.openSerial example
}

main();

```


## bleEvent.keyPress Key Press {#bleeventkeypress-按键}

* Key press
* Compatible with EC iOS USB version 9.20.0+
* @param prefix Modifier key; can be empty. alt = Alt key, ctrl = Ctrl key, gui = Win or Command key, r_ctrl = right Ctrl key, r_shift = right Shift key, shift = Shift key
* @param code Integer, e.g. 65 (ASCII code). See https://tool.oschina.net/commons?type=4
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See bleEvent.openSerial example
}

main();

```


## bleEvent.keyPressChar Character Key Press {#bleeventkeypresschar-字符按键}

* Character key press
* Compatible with EC iOS USB version 9.20.0+
* @param prefix Modifier key; can be empty. alt = Alt key, ctrl = Ctrl key, gui = Win or Command key, r_ctrl = right Ctrl key, r_shift = right Shift key, shift = Shift key
* @param code Character, e.g. a
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See bleEvent.openSerial example
}

main();

```
## bleEvent.setStep Set Step Size {#bleeventsetstep-设置步伐大小}

* Set step size
* Compatible with EC iOS USB version 9.20.0+
* @param step 10–120 — maximum value per mouse move step; larger values move faster. Default is 100
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let r = bleEvent.setStep(50);
}
```


## bleEvent.toggleSoftKeyboard Toggle Soft Keyboard {#bleeventtogglesoftkeyboard-字符按键}

* Toggle soft keyboard
* On iPhone 7 in testing, after Bluetooth connection the input field cannot show the soft keyboard. Used with the standalone main program for input — try this method. iPhone 11 does not have this issue; related to system version
* Ignore this method if you do not use the standalone main program as the input method
* Compatible with EC iOS USB version 9.20.0+
* @returns `{string}` null or empty string on success; otherwise an error message


```javascript showLineNumbers
function main() {
    // Focus the input field, then run the function below
   let r = bleEvent.toggleSoftKeyboard();
   if(r == null || r == ""){
       logd("Show keyboard succeeded")
   }else{
       logd("Show keyboard failed "+r)
   }
}

main();

```

## bleEvent.showBleName Show Bluetooth Name {#bleeventshowblename-显示蓝牙名称}

* Show Bluetooth name
* Compatible with EC iOS USB version 9.21.0+
* Helps with discoverability during search
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
// See bleEvent.light code example
```


## bleEvent.hideBleName Hide Bluetooth Name {#bleeventhideblename-隐藏蓝牙名称}

* Hide Bluetooth name
* Anti-detection
* Compatible with EC iOS USB version 9.21.0+
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
// See bleEvent.light code example
```



## bleEvent.light Light LED {#bleeventlight-点亮led}

* Light LED
* Compatible with EC iOS USB version 9.21.0+
* @param num Number of blink cycles
* @param lightToOff Time from on to off in milliseconds
* @param offToLight Time from off to on in milliseconds
* @returns `{string}` null or empty string on success; otherwise an error message

```javascript showLineNumbers
function main() {
    initMouse();
    if (!openConnect(1, 15000)) {
        logw("Failed to open communication; cannot proceed. Check whether the Bluetooth device is working")
        return
    }
    logd("Communication OK, starting execution")
    // Move mouse to 0,0
    let r = bleEvent.resetZero()
    if (_bleResultOk(r)) {
        logd("Restored mouse original coordinates successfully")
    } else {
        logd("Failed to restore mouse original coordinates {}", r)
        return
    }


    test_light_and_hide()

}


function initMouse() {
    let msg = device.getDeviceMsg()
    let productType = "";
    let bleWifiIp = "";
    if (!_bleResultOk(msg)) {
        // Get iPhone type to determine scale standard
        let bb = JSON.parse(msg);
        productType = bb["productType"];
        bleWifiIp = bb["bleWifiIp"];
    }


    let scale = bleEvent.getIPhoneScale();
    logd("Current phone type: {}, scale ratio: {}, BLE board IP: {}", productType, scale, bleWifiIp)
    // Set scale ratio so mouse movement matches on-screen pixels
    // * Note: If you use **absolute coordinate firmware**, you must set the compensation ratio to 1
    bleEvent.setScale(scale, scale)
    let img = image.captureFullScreenNoAuto();
    if (img) {
        let w = img.getWidth();
        let h = img.getHeight();
        if (w > 0 && h > 0) {
            // Set screen width and height to prevent cursor drifting off-screen; otherwise control coordinates manually
            logd("Setting screen width and height")
            bleEvent.setScreenSize(w, h)
        }
    }
}

/**
 * Connection mode
 * @param type 1 = serial port, 2 = network
 * @param timeout Timeout
 * @return {boolean} true = success, false = failure
 */
function openConnect(type, timeout) {
    if (type == 2) {
        logd("Setting network communication mode...")
        bleEvent.setSendCmdType(2)
        return true;
    } else {
        logd("Preparing to open serial communication...")
        bleEvent.setSendCmdType(1)
        // Try opening three times; failure if still not working
        for (let i = 0; i < 3; i++) {
            // Close once first
            bleEvent.closeSerial();
            sleep(1000 + (200 * 3))
            let rr = bleEvent.openSerial(timeout)
            if (_bleResultOk(rr)) {
                return true;
            }
        }

    }
    return false;
}


function _bleResultOk(r) {
    return r == null || r == "";
}



function test_light_and_hide() {
    let show = bleEvent.showBleName();
    if (show == null || show == "") {
        logd("Show Bluetooth succeeded")
    } else {
        logw("Show Bluetooth name failed: " + show)
    }

    sleep(1000)

    let hide = bleEvent.hideBleName();
    if (hide == null || hide == "") {
        logd("Hide Bluetooth succeeded")
    } else {
        logw("Hide Bluetooth name failed: " + hide)
    }

    sleep(1000)
    logd("Starting LED blink")

    let lg = bleEvent.light(5, 1000, 1000);
    if (lg == null || lg == "") {
        logd("LED blink succeeded")
    } else {
        logw("LED blink failed: " + lg)
    }

    sleep(2000)

}

main()


```


## Type Text via Shortcuts {#利用快捷指令进行输入文字}
- Tested code example below

```javascript showLineNumbers
function main() {
    initMouse();
    if (!openConnect(1, 15000)) {
        logw("Failed to open communication; cannot proceed. Check whether the Bluetooth device is working")
        return
    }
    logd("Communication OK, starting execution")
    // Move mouse to 0,0
    let r = bleEvent.resetZero()
    if (_bleResultOk(r)) {
        logd("Restored mouse original coordinates successfully")
    } else {
        logd("Failed to restore mouse original coordinates {}", r)
        return
    }

    test_ble_shortcut_input()

}



function initMouse() {
    let msg = device.getDeviceMsg()
    let productType = "";
    let bleWifiIp = "";
    if (!_bleResultOk(msg)) {
        // Get iPhone type to determine scale standard
        let bb = JSON.parse(msg);
        productType = bb["productType"];
        bleWifiIp = bb["bleWifiIp"];
    }


    let scale = bleEvent.getIPhoneScale();
    logd("Current phone type: {}, scale ratio: {}, BLE board IP: {}", productType, scale, bleWifiIp)
    // Set scale ratio so mouse movement matches on-screen pixels
    // * Note: If you use **absolute coordinate firmware**, you must set the compensation ratio to 1
    bleEvent.setScale(scale, scale)
    let img = image.captureFullScreenNoAuto();
    if (img) {
        let w = img.getWidth();
        let h = img.getHeight();
        if (w > 0 && h > 0) {
            // Set screen width and height to prevent cursor drifting off-screen; otherwise control coordinates manually
            logd("Setting screen width and height")
            bleEvent.setScreenSize(w, h)
        }
    }
}

/**
 * Connection mode
 * @param type 1 = serial port, 2 = network
 * @param timeout Timeout
 * @return {boolean} true = success, false = failure
 */
function openConnect(type, timeout) {
    if (type == 2) {
        logd("Setting network communication mode...")
        bleEvent.setSendCmdType(2)
        return true;
    } else {
        logd("Preparing to open serial communication...")
        bleEvent.setSendCmdType(1)
        // Try opening three times; failure if still not working
        for (let i = 0; i < 3; i++) {
            // Close once first
            bleEvent.closeSerial();
            sleep(1000 + (200 * 3))
            let rr = bleEvent.openSerial(timeout)
            if (_bleResultOk(rr)) {
                return true;
            }
        }

    }
    return false;
}


function _bleResultOk(r) {
    return r == null || r == "";
}
function test_ble_shortcut_input() {
    let msg = device.getDeviceMsg()
    // Get BLE address as simple unique device identifier
    let bleMac = "";
    if (msg != null && msg != "") {
        // Get iPhone type to determine scale standard
        let bb = JSON.parse(msg);
        bleMac = bb["bleMac"];
    }
    if (bleMac == null || bleMac == "") {
        logw("Failed to get BLE MAC address")
        return
    }
    logd("Got BLE MAC address as unique identifier {}", bleMac)

    let data = {
        key: bleMac,
        "content": "My data " + new Date().toString()
    }
    // Submit to iOS Shortcuts helper service; use external URL if deployed externally
    let post_url = "http://192.168.2.26:8696/postText"
    let result = http.postJSON(post_url, data, 10 * 1000, {});
    if (result == null || result == "") {
        logw("Failed to submit data to iOS Shortcuts helper service")
        return;
    }
    result = JSON.parse(result)
    if (result["code"] != 0) {
        logw("Failed to submit data to iOS Shortcuts helper service: " + result["msg"])
        return;
    }
    // Run Shortcuts — previously bound shortcut key
    let gs = bleEvent.keyPressChar("gui", "u")
    if (gs != null && gs != "") {
        logw("Failed to run Shortcuts shortcut key " + gs)
        return
    }

    logd("Running Shortcuts, waiting for result")
    // Poll for result in a loop
    // Result polling URL
    let result_url = "http://192.168.2.26:8696/getResult?key=" + bleMac
    let getresult = false;
    for (let i = 0; i < 15; i++) {
        sleep(1000)
        let data = http.httpGet(result_url, {}, 5 * 1000, {})
        if (data != null && data != "") {
            try {
                let r1 = JSON.parse(data)
                // Got explicit success
                if (r1["data"] == "ok") {
                    getresult = true;
                    break
                }
            } catch (e) {

            }
        }
    }

    if (!getresult) {
        logw("Failed to get result")
        return;
    }

    // Paste with gui+v
    let gsv = bleEvent.keyPressChar("gui", "v")
    if (gsv != null && gsv != "") {
        logw("Failed to run paste shortcut key " + gs)
        return
    }

    logd("Paste succeeded")
}

main()





```



## Insert to Photos via Shortcuts {#利用快捷指令进行插入相册}
- Tested code example below

```javascript

function main() {
    initMouse();
    if (!openConnect(1, 15000)) {
        logw("Failed to open communication; cannot proceed. Check whether the Bluetooth device is working")
        return
    }
    logd("Communication OK, starting execution")
    // Move mouse to 0,0
    let r = bleEvent.resetZero()
    if (_bleResultOk(r)) {
        logd("Restored mouse original coordinates successfully")
    } else {
        logd("Failed to restore mouse original coordinates {}", r)
        return
    }


    test_ble_insert()

  
}








function initMouse() {
    let msg = device.getDeviceMsg()
    let productType = "";
    let bleWifiIp = "";
    if (!_bleResultOk(msg)) {
        // Get iPhone type to determine scale standard
        let bb = JSON.parse(msg);
        productType = bb["productType"];
        bleWifiIp = bb["bleWifiIp"];
    }


    let scale = bleEvent.getIPhoneScale();
    logd("Current phone type: {}, scale ratio: {}, BLE board IP: {}", productType, scale, bleWifiIp)
    // Set scale ratio so mouse movement matches on-screen pixels
    // * Note: If you use **absolute coordinate firmware**, you must set the compensation ratio to 1
    bleEvent.setScale(scale, scale)
    let img = image.captureFullScreenNoAuto();
    if (img) {
        let w = img.getWidth();
        let h = img.getHeight();
        if (w > 0 && h > 0) {
            // Set screen width and height to prevent cursor drifting off-screen; otherwise control coordinates manually
            logd("Setting screen width and height")
            bleEvent.setScreenSize(w, h)
        }
    }
}

/**
 * Connection mode
 * @param type 1 = serial port, 2 = network
 * @param timeout Timeout
 * @return {boolean} true = success, false = failure
 */
function openConnect(type, timeout) {
    if (type == 2) {
        logd("Setting network communication mode...")
        bleEvent.setSendCmdType(2)
        return true;
    } else {
        logd("Preparing to open serial communication...")
        bleEvent.setSendCmdType(1)
        // Try opening three times; failure if still not working
        for (let i = 0; i < 3; i++) {
            // Close once first
            bleEvent.closeSerial();
            sleep(1000 + (200 * 3))
            let rr = bleEvent.openSerial(timeout)
            if (_bleResultOk(rr)) {
                return true;
            }
        }

    }
    return false;
}


function _bleResultOk(r) {
    return r == null || r == "";
}



function test_ble_insert() {
    let msg = device.getDeviceMsg()
    let bleMac = "";
    if (msg != null && msg != "") {
        // Get iPhone type to determine scale standard
        let bb = JSON.parse(msg);
        bleMac = bb["bleMac"];
    }
    if (bleMac == null || bleMac == "") {
        logw("Failed to get BLE MAC address")
        return
    }
    logd("Got BLE MAC address as unique identifier {}", bleMac)

    let data = {
    }
    // Submit to iOS Shortcuts helper service upload endpoint; use external URL if deployed externally
    // key is MAC address as identifier
    let post_url = "http://192.168.2.26:8696/upload?key="+bleMac
    // Select a video file here
    let file = {
        "file":"c:/Downloads/QQ2025522-95310.mp4"
    }
    let result = http.httpPost(post_url, data,file, 20 * 1000, {});
    if (result == null || result == "") {
        logw("Failed to submit data to iOS Shortcuts helper service")
        return;
    }
    logd("Submit result: "+result)
    result = JSON.parse(result)
    if (result["code"] != 0) {
        logw("Failed to submit data to iOS Shortcuts helper service: " + result["msg"])
        return;
    }
    // Run Shortcuts — previously bound shortcut key
    let gs = bleEvent.keyPressChar("gui", "i")
    if (gs != null && gs != "") {
        logw("Failed to run Shortcuts shortcut key " + gs)
        return
    }

    logd("Running Shortcuts, waiting for result")
    // Poll for result in a loop
    // Result polling URL
    let result_url = "http://192.168.2.26:8696/getResult?key=" + bleMac
    let getresult = false;
    for (let i = 0; i < 15; i++) {
        sleep(1000)
        let data = http.httpGet(result_url, {}, 5 * 1000, {})
        if (data != null && data != "") {
            try {
                let r1 = JSON.parse(data)
                // Got explicit success
                if (r1["data"] == "ok") {
                    getresult = true;
                    break
                }
            } catch (e) {

            }
        }
    }

    if (!getresult) {
        logw("Failed to get result")
        return;
    }
    logd("Insert to Photos succeeded")
}
main()
```
