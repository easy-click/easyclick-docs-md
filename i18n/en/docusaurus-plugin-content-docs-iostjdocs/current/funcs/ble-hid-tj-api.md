---
title: EasyClick Automation Scripts — iOS Scripts — iOS No Jailbreak — iOS No Hardware — BLE Functions
hide_title: false
hide_table_of_contents: false
sidebar_label: BLE Functions
description: EasyClick automation scripts — iOS no jailbreak — BLE functions
keywords:
 - EasyClick automation scripts
 - iOS scripts
 - iOS no jailbreak
 - iOS no hardware
 - BLE functions
 - bleEvent
 - iOS
 - BLE
 - EC
 - 6.5.0
 - returns
 - param
 - isConnected
 - startConnect
 - stopConnect
 - EasyClick
 - mobile automation
 - test automation
---

# BLE Functions

## Overview

- BLE module functions are mainly used for Bluetooth gesture actions
- The BLE module uses the `bleEvent` prefix
- For BLE hardware configuration, see [BLE Getting Started](/iostjdocs/advance/tj-ble-starter)

## bleEvent.isConnected BLE Connection Status

* BLE connection status
* Available in EC iOS standalone edition 6.5.0+
* @returns `{boolean}` true if connected, false if not connected

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.startConnect Connect to BLE Device

* Connect to a BLE device
* Available in EC iOS standalone edition 6.5.0+
* @param bleDeviceName BLE device name; if omitted, read from app system settings
* @param save Whether to save the configured BLE device name
* @param timeout Connection timeout in milliseconds
* @returns `{string|null}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
function testble() {

    // Set this to true if you need network connection mode
    // The code below is test code only — fill in your own logic
    let useNetwork = false
    if (useNetwork) {
        if (!useNetworkBle()) {
            return;
        }
    } else {
        if (!connectBle()) {
            return
        }
    }


    testMoveDistance()

    sleep(3000)

    logd("hide ble Name : " + bleEvent.hideBleName())
    sleep(1000)
    let zr = bleEvent.resetZero();
    if (_isBleResultOk(zr)) {
        logd("Mouse reset to zero succeeded")
    } else {
        logw("Mouse reset to zero failed")
    }

    let rr = bleEvent.setLastScale()
    if (rr != null && rr != "") {
        // Scale was never set before
        logd("Current device type: " + device.getDeviceIdentifier())
        let scale = bleEvent.getIPhoneScale();
        logd("scale is " + scale)
        // If you use relative-coordinate firmware, set the compensation scale
        bleEvent.setScale(scale, scale)
    }
    
    logd("scale "+bleEvent.getScale())


    // If you use absolute-coordinate firmware, set this to 1
    // bleEvent.setScale(1,1)

    resetScreenSize_auto();
    sleep(1000)
    logd("Test LED blink")
    logd("light: " + bleEvent.light(10, 100, 100))

    // Set step size — smaller values move slower
    bleEvent.setStep(20)

    testClick();
    sleep(5000)
    logd("Start testing multi-touch")
    testBleMtouch();
    sleep(3000)
    logd("Start testing basic gestures")
    testMove()

    sleep(3000)
    logd("Start testing keys and keyboard")

    testBleKey();
    sleep(3000)

    logd("showBleName ble Name : " + bleEvent.showBleName())

    sleep(5000)
    logd("Toggle soft keyboard: " + bleEvent.toggleSoftKeyboard())

    sleep(2000)
    logd("Toggle soft keyboard: " + bleEvent.toggleSoftKeyboard())
    sleep(2000)

    logd("Restart development board: " + bleEvent.resetBle())

    bleEvent.clickPoint(300, 400)

}


function resetScreenSize_auto() {
    let o = agentEvent.getOrientation();
    let img = image.captureFullScreen();
    if (img == null) {
        return
    }
    // You can also use device module functions to get width and height
    let w = img.getWidth();
    let h = img.getHeight();
    logd("Current screen width and height: " + w + "," + h + " orientation: " + o)
    let rw = w;
    let rh = h;
    if (o == "2") {
        // Landscape
        if (w < h) {
            rh = w;
            rw = h;
        }
    } else {
        // Portrait
        if (w > h) {
            rh = w;
            rw = h;
        }
    }
    logd("Set BLE screen parameters " + rw + " " + rh)
    bleEvent.setScreenSize(rw, rh)
}

function connectBle() {
    bleEvent.sendCmdType(1)
    logd("Start connecting BLE " + bleEvent.getConfigBleName())
    if (bleEvent.isConnected()) {
        return true;
    }
    bleEvent.stopConnect();
    let cr = bleEvent.startConnect("", false, 15000)
    if (_isBleResultOk(cr)) {
        logd("BLE connected successfully " + bleEvent.getConfigBleName())
        return true
    }
    logw("BLE connection failed " + cr)


    return false

}

function testClick() {
    sleep(1000)
    logd("Start testing gesture actions")
    logd("Start testing click")
    bleEvent.resetZero();
    let ck = bleEvent.clickPoint(300, 400)

    if (_isBleResultOk(ck)) {
        logd("clickPoint test succeeded")
    } else {
        logw("clickPoint test failed " + ck)
    }
    sleep(2000)
    ck = bleEvent.press(310, 420, 4000)
    if (_isBleResultOk(ck)) {
        logd("press test succeeded")
    } else {
        logw("press test failed " + ck)
    }

    sleep(2000)
    ck = bleEvent.doubleClickPoint(200, 500)
    if (_isBleResultOk(ck)) {
        logd("doubleClickPoint test succeeded")
    } else {
        logw("doubleClickPoint test failed " + ck)
    }

    sleep(2000)
    ck = bleEvent.swipeToPoint(200, 500, 600, 900, 5000)
    if (_isBleResultOk(ck)) {
        logd("swipeToPoint test succeeded")
    } else {
        logw("swipeToPoint test failed " + ck)
    }

    sleep(2000)
}

function testMove() {
    sleep(2000)
    logd("start move ...")
    let m = bleEvent.mouseMove(100, 100)
    if (_isBleResultOk(m)) {
        logd("mouseMove test succeeded")
    } else {
        logw("mouseMove test failed " + m)
    }

    sleep(2000)
    logd("start mouseMoveByDistance ...")
    m = bleEvent.mouseMoveByDistance(10, 20)
    if (_isBleResultOk(m)) {
        logd("mouseMoveByDistance test succeeded")
    } else {
        logw("mouseMoveByDistance test failed " + m)
    }

    sleep(2000)
    logd("start touchDown ...")
    m = bleEvent.touchDown(101, 121)
    if (_isBleResultOk(m)) {
        logd("touchDown test succeeded")
    } else {
        logw("touchDown test failed " + m)
    }

    sleep(2000)
    logd("start touchMove ...")
    m = bleEvent.touchMove(130, 150)
    if (_isBleResultOk(m)) {
        logd("touchMove test succeeded")
    } else {
        logw("touchMove test failed " + m)
    }

    sleep(2000)
    logd("start touchUp ...")
    m = bleEvent.touchUp(130, 150)
    if (_isBleResultOk(m)) {
        logd("touchUp test succeeded")
    } else {
        logw("touchUp test failed " + m)
    }

}

function testBleMtouch() {
    let touch1 = [
        {
            "action": 0, "x": 500,
            "y": 1200, "pointer": 1, "delay": 1
        },
        {
            "action": 2,
            "x": 500,
            "y": 1100,
            "pointer": 1,
            "delay": 20
        }, {
            "action": 2,
            "x": 500,
            "y": 1000,
            "pointer": 1,
            "delay": 20
        },
        {
            "action": 1,
            "x": 1,
            "y": 1,
            "pointer": 1,
            "delay": 20
        }];

    let m = bleEvent.multiTouch(touch1, 10000)
    if (_isBleResultOk(m)) {
        logd("multiTouch test succeeded")
    } else {
        logw("multiTouch test failed " + m)
    }
}

function testBleKey() {

    sleep(1000)
    let kc = bleEvent.keyPressChar("", "a")
    if (_isBleResultOk(kc)) {
        logd("Press a succeeded")
    } else {
        logw("Press a failed: " + kc)
    }

    sleep(1000)
    let kc2 = bleEvent.keyPressChar("shift", "a")
    if (_isBleResultOk(kc2)) {
        logd("Press SHIFT+a succeeded")
    } else {
        logw("Press SHIFT+a failed: " + kc2)
    }

    sleep(1000)
    let kc3 = bleEvent.keyPress("", 97)
    if (_isBleResultOk(kc3)) {
        logd("keyPress 97 succeeded")
    } else {
        logw("keyPress 97 failed: " + kc3)
    }

    sleep(1000)
    let k1 = bleEvent.systemKey("home")
    if (_isBleResultOk(k1)) {
        logd("Press HOME succeeded")
    } else {
        logw("Press HOME failed: " + k1)
    }
    sleep(1000)
    let k2 = bleEvent.systemKey("recents")
    if (_isBleResultOk(k2)) {
        logd("Press appSwitch succeeded")
    } else {
        logw("Press appSwitch failed: " + k2)
    }

}


function useNetworkBle() {
    let ip = bleEvent.searchBleIp(false, 10 * 1000)
    if (ip == null || ip == "") {
        logw("Failed to find development board IP")
        return false
    }
    logd("Development board IP: " + ip + " BLE name: " + bleEvent.getConfigBleName())
    // Switch to network request mode
    bleEvent.sendCmdType(2)
    return true;


}

function testMoveDistance() {
    bleEvent.resetZero();
    bleEvent.mouseMoveDistance(100, 100, false)
    bleEvent.mouseMoveDistance(100, 100, false)
    bleEvent.mouseMoveDistance(100, 100, false)
    sleep(1000)
    bleEvent.mouseMoveDistance(100, 100, false)
    sleep(3000)
    sleep(1000)
    bleEvent.mouseMoveDistance(100, 100, false)
    logd(bleEvent.mouseMoveDistance(0, 0, true))
    sleep(1000)
    logd(bleEvent.mouseMoveDistance(0, 0, false))
}


function _isBleResultOk(r) {
    return r == null || r == ""
}


testble()

```

## bleEvent.stopConnect Disconnect

* Disconnect
* Available in EC iOS standalone edition 6.5.0+
* @returns `{string|null`} null or empty string means success; otherwise an error message

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.searchBleIp Search Development Board IP

* Search for the development board IP
* Available in EC iOS standalone edition 6.5.0+
* @param force Force search to prevent stale cache
* @param timeout Timeout in milliseconds
* @returns `{string|null}` null means not found

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.getIPhoneScale Mouse Compensation Scale

* Returns a scale value based on the iPhone hardware identifier prefix (digits after the comma are ignored)
* @returns `{number}` float scale ratio

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.setScale Set Mouse Compensation Scale

* Set the mouse compensation scale
* Ratio of pixel movement per mouse unit; default is 2.0
* iPhone 6/7/8 375 x 667 — set to 2.0; standard 16:9, no safe area interference
* iPhone 11 / XR 414 x 896 — set to 1.96; taller screen, system accelerates Y axis compensation
* iPhone X/XS/11 Pro 375 x 812 — set to 1.98; 19.5:9 aspect ratio with slight acceleration
* iPhone 12/13/14/15 390 x 844 — set to 1.97; different logical points from iPhone 11, slightly different acceleration curve
* Plus / Max series 414 x 896 / 430 x 932 — set to 1.94 ~ 1.95; tallest screens, system increases Y axis gain for usability
* Available in EC iOS standalone edition 6.5.0+
* @param x_scale X-axis float
* @param y_scale Y-axis float
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.getScale Get Stored Compensation Scale

* Get the stored compensation scale
* Available in EC iOS standalone edition 6.6.0+
* May come from app calibration settings or from code
* @returns `{string}` null or empty means never set; otherwise a JSON string with the stored value

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.setLastScale Use Previous Compensation Scale

* Apply the previously saved compensation scale
* Available in EC iOS standalone edition 6.6.0+
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.setScreenSize Set Screen Size

* Set screen size
* Prevents the mouse from moving off-screen and causing offset
* If screen size is unknown, use the width and height from a screenshot
* Or use functions from the device module
* Available in EC iOS standalone edition 6.5.0+
* @param w Screen width
* @param h Screen height
* @returns `{string} ` null or empty string means success; otherwise an error message

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.setWifiInfo Set Network Information

* Set network information
* Helps the development board connect to Wi-Fi
* Available in EC iOS standalone edition 6.5.0+
* @param name Wi-Fi name
* @param pwd Wi-Fi password
* @returns `{string} ` null or empty string means success; otherwise an error message

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.resetBle Restart Development Board

* Restart the development board
* Equivalent to pressing the RST button on the board
* Available in EC iOS standalone edition 6.5.0+
* @returns `{string} ` null or empty string means success; otherwise an error message

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.mouseMove Move Mouse

* Move the mouse
* Moves only — no press action
* Available in EC iOS standalone edition 6.5.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.mouseMoveByDistance Move Mouse (Pixel Distance)

* Move the mouse by pixel distance
* Moves only — no press action
* Available in EC iOS standalone edition 6.5.0+
* @param x_dis X pixel distance; must not exceed 127
* @param y_dis Y pixel distance; must not exceed 127
* @returns `{string} ` null or empty string means success; otherwise an error message

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.mouseMoveDistance Move Mouse (With Press Parameter)

* Move the mouse with optional press parameter
* Moves only; press can be included
* If both x and y are 0, send twice — first with press=true, then press=false — to simulate a click
* Available in EC iOS standalone edition 6.5.0+
* @param x_dis X pixel distance; must not exceed 127
* @param y_dis Y pixel distance; must not exceed 127
* @param press true to press
* @returns `{string} ` null or empty string means success; otherwise an error message

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.resetZero Reset Mouse to Zero

* Reset mouse to zero
* Moves the mouse to the top-right corner at (0, 0)
* Available in EC iOS standalone edition 6.5.0+
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.touchDown Touch Down at Coordinates

* Touch down at coordinates
* Available in EC iOS standalone edition 6.5.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.touchMove Move Touch Point

* Move touch point
* Available in EC iOS standalone edition 6.5.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.touchUp Touch Up at Coordinates

* Touch up at coordinates
* Available in EC iOS standalone edition 6.5.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.clickPoint Click Coordinates

* Click coordinates
* Available in EC iOS standalone edition 6.5.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.press Long Press Coordinates

* Long press coordinates
* Available in EC iOS standalone edition 6.5.0+
* @param x X coordinate
* @param y Y coordinate
* @param delay Long press duration in milliseconds
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.doubleClickPoint Double-Click Coordinates

* Double-click coordinates
* Available in EC iOS standalone edition 6.5.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.pressMouseBtn Press Mouse Button

* Press a mouse button
* Can be used for AssistiveTouch custom actions — Custom Actions
* To avoid conflicts, use button numbers 4 through 8
* Available in EC iOS standalone edition 6.5.0+
* @param b Mouse button number starting from 1; typically 1=left, 2=right, 3=middle scroll, 4–8=custom
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers

function main() {
    // BLE connection code omitted — see startConnect example
    // If you configured Settings > Accessibility > Touch > AssistiveTouch > Devices > select connected device > Custom Actions,
    // sending mouse button 4 here triggers the assigned action
    let r = bleEvent.pressMouseBtn(4);
    logd(r)
}

main();
```

## bleEvent.swipeToPoint Swipe

* Swipe
* Available in EC iOS standalone edition 6.5.0+
* @param startX Start X coordinate
* @param startY Start Y coordinate
* @param endX End X coordinate
* @param endY End Y coordinate
* @param duration Duration in milliseconds
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.multiTouch Multi-Touch

* Multi-touch<br/>
* Touch parameters: action — typically 0=down, 1=up, 2=move<br/>
* x: X coordinate<br/>
* y: Y coordinate<br/>
* pointer: finger index — 1, 2, 3, etc.<br/>
* delay: delay before this action in milliseconds
* Available in EC iOS standalone edition 6.5.0+
* @param touch1 Touch point array for finger 1, e.g.:
 `[{"action":0,"x":1,"y":1,"pointer":1,"delay":20},{"action":2,"x":1,"y":1,"pointer":1,"delay":20}]`
* @param timeout Multi-touch execution timeout in milliseconds
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.systemKey System Key

* System key
* Available in EC iOS standalone edition 6.5.0+
* @param key Currently supports home, recents (recent tasks)
* @returns `{string} ` null or empty string means success; otherwise an error message

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.keyPress Key Press

* Key press
* Available in EC iOS standalone edition 6.5.0+
* @param prefix Modifier key; can be empty — alt=Alt, ctrl=Ctrl, gui=Win/Command, r_ctrl=right Ctrl, r_shift=right Shift, shift=Shift
* @param code Integer, e.g. 65 (ASCII); see https://tool.oschina.net/commons?type=4
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.keyPressChar Character Key Press

* Character key press
* Available in EC iOS standalone edition 6.5.0+
* @param prefix Modifier key; can be empty — alt=Alt, ctrl=Ctrl, gui=Win/Command, r_ctrl=right Ctrl, r_shift=right Shift, shift=Shift
* @param code Character, e.g. a
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.toggleSoftKeyboard Toggle Soft Keyboard

* Toggle soft keyboard
* On iPhone 7 in testing, after BLE connects, input fields may not show the soft keyboard when using the standalone main app for input — try this method; iPhone 11 did not have this issue; behavior depends on iOS version
* Ignore this method if you do not use the standalone main app as the input method
* Available in EC iOS standalone edition 6.5.0+
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.setStep Set Step Size

* Set step size
* Available in EC iOS standalone edition 6.5.0+
* @param step 10–120; maximum value per mouse move step — larger values move faster; default 100
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.light Blink LED

* Blink LED
* Available in EC iOS standalone edition 6.5.0+
* @param num Number of blink cycles
* @param lightToOff Duration from on to off in milliseconds
* @param offToLight Duration from off to on in milliseconds
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.showBleName Show BLE Name

* Show BLE name
* Available in EC iOS standalone edition 6.5.0+
* Helps the device be discoverable
* @returns `{string} ` null or empty string means success; otherwise an error message

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.hideBleName Hide BLE Name

* Hide BLE name
* Anti-detection
* Available in EC iOS standalone edition 6.5.0+
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.getConfigBleName Get App-Configured BLE Name

* Get the BLE name configured in the app
* Available in EC iOS standalone edition 6.5.0+
* @returns `{string}` configured BLE name

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```

## bleEvent.sendCmdType Set Communication Mode

* Set communication mode
* How the app sends commands to the development board
* Available in EC iOS standalone edition 6.5.0+
* @param tt 1=Bluetooth, 2=network
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
    // See the full example code in bleEvent.startConnect
```


## bleEvent.startKeepAlive Mouse Keep-Alive

* Mouse keep-alive heartbeat
* Keeps the mouse cursor visible
* Under the hood, moves the mouse away and back
* Skipped when other actions are in progress
* Not needed on iOS 17+ with absolute-coordinate firmware
* @param keepAliveTime Heartbeat interval in milliseconds; e.g. 5000 for every 5 seconds
* @param moveValue Distance per heartbeat move; use 1 or 2; must not exceed 100
* @returns `{*|string} ` empty string on success; non-empty error message on failure
```javascript showLineNumbers
    // Call directly after BLE connection completes
    bleEvent.startKeepAlive(5000,1)
```



## bleEvent.stopKeepAlive Stop Keep-Alive

* Stop keep-alive heartbeat
* @returns `{string}` empty string on success; non-empty error message on failure
```javascript showLineNumbers
    // Call directly
    bleEvent.stopKeepAlive()
```
