---
title: Bluetooth HID Events
description: EasyClick automation scripts — Android no root HID events
keywords:
 - EasyClick automation scripts Android no root HID events
 - HID
 - app
 - startConnect
 - bleEvent
 - ble
 - hid
 - param
 - stopConnect
 - setHeartbeatTimeout
 - isConnected
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview {#说明}

- All Bluetooth HID event module functions require hardware to use
- See Bluetooth HID setup in Advanced — Bluetooth HID Hardware: [Bluetooth HID Hardware](/docs/advance/blehid)
- The module object prefix is `bleEvent`, e.g. `bleEvent.click()`

:::tip

- BLE HID is one click mode; accessibility, proxy mode, and root all support click and node fetch. If nodes are unavailable, use image/color matching
- For image capture permission, use `image.requestScreenCapture` with type=1 (permission-based capture)
- BLE HID cannot use nodes; other features work the same with no special handling
 :::

## startConnect Connect Bluetooth Device {#startconnect-连接蓝牙设备}

* Connect to a Bluetooth device
* Can be used without pre-configuring in app settings; this function scans, connects, and saves
* If you do not want per-device setup, use this function
* Requires EC Android 11.37.0+
* @param bleDeviceName Bluetooth device name; omit to read from app settings — last 8 chars of board MAC, lowercase
* @param save Save to app Bluetooth device name settings; set true to persist
* @param timeout Connection timeout in milliseconds
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
function main() {
    logd("connecting ble...")
    let cr = connect_ble();
    if (cr == null || cr == "") {
        logd("ble connected")
    } else {
        logw("ble connect failed " + cr)
        return
    }
    sleep(1000)
    // Enable pointer location in Developer Options to see tap effects
    logd("testing clickPoint")
    // let crc = bleEvent.clickPoint(399, 4525);
    // if (crc == null || crc == "") {
    // logd("click success ")
    // } else {
    // logd("click failed " + crc)
    // }
    // sleep(3000)
    // logd("testing press")
    // let crc2 = bleEvent.press(399, 455, 5000);
    // if (crc2 == null || crc2 == "") {
    // logd("press success ")
    // } else {
    // logd("press failed " + crc2)
    // }
    // sleep(6000)
    // sleep(1000)
    // logd("testing doubleClick")
    // let doubleClickr = bleEvent.doubleClick(666, 1000, 30);
    // if (doubleClickr == null || doubleClickr == "") {
    // logd("doubleClick success ")
    // } else {
    // logd("doubleClick failed " + doubleClickr)
    // }
    // sleep(6000)
    //
    // test_Swipe();
    // touchTest()
    // mtouch();
    test_Swipe();
    testSystemKey();
    
    // Input character a
    bleEvent.keyPressChar("","a")
    
    testPressKey();
    //bleEvent.stopConnect()
    sleep(100000)
    logd("0---end ");
}


function testPressKey() {
    sleep(1000)
    let press_a = bleEvent.keyPress("", 65);
    console.log("press_a {}", press_a)
}

function testSystemKey() {

    sleep(1000)

    logd("home " + bleEvent.home())
    sleep(1000)

    logd("back " + bleEvent.back())

    sleep(1000)
    logd("recents " + bleEvent.recents())

}

function test_Swipe() {
    sleep(1000)
    let td = bleEvent.swipe(300, 400, 500, 600, 5000)
    logd("swipe result: " + td)
    sleep(6000)
}

function mtouch() {
    sleep(1000)

    let data = [
        {"action": 0, "x": 250, "y": 1800, "pointer": 1, "delay": 100},
        {"action": 2, "x": 250, "y": 1700, "pointer": 1, "delay": 100},
        {"action": 2, "x": 330, "y": 1650, "pointer": 1, "delay": 200},
        {"action": 2, "x": 330, "y": 1640, "pointer": 1, "delay": 200},
        {"action": 2, "x": 330, "y": 1620, "pointer": 1, "delay": 200},
        {"action": 2, "x": 322, "y": 1500, "pointer": 1, "delay": 200},
        {"action": 2, "x": 322, "y": 1400, "pointer": 1, "delay": 200},
        {"action": 2, "x": 334, "y": 1000, "pointer": 1, "delay": 200},
        {"action": 2, "x": 339, "y": 1000, "pointer": 1, "delay": 200},
        {"action": 2, "x": 330, "y": 1000, "pointer": 1, "delay": 200},
        {"action": 2, "x": 453, "y": 1000, "pointer": 1, "delay": 200},
        {"action": 2, "x": 555, "y": 1000, "pointer": 1, "delay": 200},
        {"action": 2, "x": 557, "y": 600, "pointer": 1, "delay": 200},
        {"action": 1, "x": 600, "y": 400, "pointer": 1, "delay": 100}
    ]

    let tou = bleEvent.multiTouch(data, 1000)
    sleep(5000)
    if (tou == null) {
        logd("multi-touch success")
    } else {
        loge("multi-touch failed:" + tou);
        return false
    }
}

function touchTest() {
    sleep(6000)
    let td = bleEvent.touchDown(300, 400)
    console.log(" touchDown result " + td)
    sleep(1000)
    bleEvent.touchMove(350, 460)
    sleep(1000)
    bleEvent.touchMove(400, 500)
    sleep(1000)
    logd("touchUp " + bleEvent.touchUp(400, 500))
}


/**
 * Connect ble
 * @returns {null}
 */
function connect_ble() {
    if (bleEvent.isConnected()) {
        return null;
    }
    bleEvent.stopConnect();
    return bleEvent.startConnect("", false, 15000);

}

// Normal devices do not need this
// bleEvent.setWidthEqualsHeight(true)

main()
```

## stopConnect Disconnect {#stopconnect-断开连接}

* Disconnect
* Requires EC Android 11.37.0+
* @return `{string}` null on success; otherwise an error message

```javascript showLineNumbers
    // See startConnect function example
```

## setHeartbeatTimeout Set Heartbeat Timeout {#setheartbeattimeout-断开连接}

* Set heartbeat timeout
* If heartbeat exceeds the set time, connection is considered lost; default 30s
* @param tt Timeout in milliseconds
* Requires EC Android 11.37.0+

```javascript showLineNumbers
    // See startConnect function example
```

## isConnected Connection Status {#isconnected-链接状态}

* Connection status
* Requires EC Android 11.37.0+
* @returns `{boolean}` true when connected, false when not

```javascript showLineNumbers
    // See startConnect function example
```

## setWidthEqualsHeight Coordinate Width Equals Height {#setwidthequalsheight-坐标系的宽度等于高度}

* Set coordinate system width equal to height
* Usually not needed; set if tap coordinates are wrong, e.g. iQOO series
* Requires EC Android 11.37.0+
* @param r true to enable
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // See startConnect function example
```

## clickPoint Click {#clickpoint-点击}

* Click
* Requires EC Android 11.37.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // See startConnect function example
```

## press Long Press {#press-长按}

* Long press
* Note: async with script; call sleep(delay) afterward to avoid event conflicts
* Requires EC Android 11.37.0+
* @param x X coordinate
* @param y Y coordinate
* @param delay Hold duration in milliseconds
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // See startConnect function example
```

## doubleClick Double Click {#doubleclick-双击}

* Double click
* Requires EC Android 11.37.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // See startConnect function example
```

## touchDown Touch Down {#touchdown-按下}

* Touch down
* Requires EC Android 11.37.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // See startConnect function example
```

## touchMove Touch Move {#touchmove-移动}

* Touch move
* Requires EC Android 11.37.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // See startConnect function example
```

## touchUp Touch Up {#touchup-抬起}

* Touch up
* Requires EC Android 11.37.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // See startConnect function example
```

## swipe Swipe {#swipe-滑动}

* Swipe
* Note: async with script; call sleep(delay) afterward to avoid event conflicts
* Requires EC Android 11.37.0+
* @param x Start X coordinate
* @param y Start Y coordinate
* @param ex End X coordinate
* @param ey End Y coordinate
* @param delay Total swipe duration in milliseconds
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // See startConnect function example
```

## multiTouch Multi-Touch {#multitouch-多点触摸}

* Multi-touch
* Note: async with script; sleep for total swipe duration to avoid event conflicts
* Requires EC Android 11.37.0+
* Touch parameters: action — 0 = down, 1 = up, 2 = move
* x: X coordinate
* y: Y coordinate
* pointer: Finger index 1, 2, 3, etc.
* delay: Delay before this action in ms; use >40ms to avoid coordinate drift
* @param touch1 Touch point array for finger 1, e.g. `[{"action":0,"x":1,"y":1,"pointer":1,"delay":30},{"action":2,"x":1,"y":1,"pointer":1,"delay":30}]`
* @param timeout Multi-touch execution timeout in milliseconds
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // See startConnect function example
```

## systemKey System Key {#systemkey-系统按键}

* System key
* Requires EC Android 11.37.0+
* @param key Values: home, back, recents
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // See startConnect function example
```

## recents Recents {#recents-任务列表}

* Recents / task list
* May not work on all devices (non-standard key)
* Requires EC Android 11.37.0+
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // See startConnect function example
```

## back Back {#back-返回}

* Back
* Requires EC Android 11.37.0+
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // See startConnect function example
```

## home Home {#home-主页}

* Home
* Requires EC Android 11.37.0+
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // See startConnect function example
```

## keyPress Keyboard Key {#keypress-键盘按键}

* Keyboard key
* Requires EC Android 11.37.0+
* @param prefix Modifier prefix; omit or null for normal key: alt, ctrl, gui (Win), r_ctrl (right Ctrl), r_shift (right Shift), shift
* @param code ASCII code
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // See startConnect function example
```

## keyPressChar Keyboard Character {#keypresschar-键盘按键字符}

* Keyboard character key
* Requires EC Android 11.37.0+
* @param prefix Modifier prefix; omit or null for normal key: alt, ctrl, gui (Win), r_ctrl (right Ctrl), r_shift (right Shift), shift
* @param c Single character, e.g. a; converted to ASCII internally. See https://tool.oschina.net/commons?type=4
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    
    // See startConnect function example
```

## hideBleName Hide Bluetooth Name {#hideblename-隐藏蓝牙的名称}

* Hide Bluetooth name to prevent discovery
* Requires EC Android 11.38.0+
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // Combined with startConnect example
    // Call after successful connection
    bleEvent.hideBleName();
    sleep(1000)
    // Call again
    let cr = bleEvent.hideBleName();
    if (cr == null || cr == "") {
        logd("hide success")
    } else {
        logw("hide failed " + cr)
        return
    }
```

## showBleName Show Bluetooth Name {#showblename-显示蓝牙的名称}

* Show Bluetooth name
* If BLE communication fails, display may fail — manually reset the board
* Requires EC Android 11.38.0+
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // Combined with startConnect example
    // Call after successful connection
    bleEvent.showBleName();
    sleep(1000)
    // Call again
    let cr = bleEvent.showBleName();
    if (cr == null || cr == "") {
        logd("success")
    } else {
        logw("failed " + cr)
        return
    }
```

## getConfigBleName Get App-Configured Bluetooth Name {#getconfigblename-获取app配置的蓝牙名称}

* Get the Bluetooth name configured in the app
* Requires EC Android 11.38.0+
* @returns `{string|null}` Name string

```javascript showLineNumbers
    let cr = bleEvent.getConfigBleName();
    logd("cr "+cr)
```

## setWifi Set WiFi {#setwifi-设置wifi}

* Set WiFi credentials
* Requires EC Android 11.39.0+
* Board must be restarted after setting to connect to network
* @param name WiFi SSID
* @param pwd WiFi password
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // Call after successful Bluetooth connection

    let cr = bleEvent.setWifi("112","333");
    logd("cr "+cr)
```

## reset Reset Board {#reset-重启开发板}

* Reset the development board
* Requires EC Android 11.39.0+
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // Call after successful Bluetooth connection
    let cr = bleEvent.reset();
    logd("cr "+cr)
```
