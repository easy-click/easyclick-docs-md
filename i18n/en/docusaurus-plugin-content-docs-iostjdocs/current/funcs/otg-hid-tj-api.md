---
title: EasyClick Automation Scripts — iOS Scripts — iOS No Jailbreak — iOS No Hardware — OTG HID Functions
hide_title: false
hide_table_of_contents: false
sidebar_label: OTG HID Functions
description: EasyClick automation scripts — iOS no jailbreak — OTG HID functions
keywords:
 - EasyClick automation scripts
 - iOS scripts
 - iOS no jailbreak
 - iOS no hardware
 - OTG HID functions
 - otgEvent
 - OTG
 - HID
 - otg
 - iOS
 - EC
 - 6.6.0
 - isConnected
 - setUrl
 - setTimeout
 - EasyClick
 - mobile automation
 - test automation
---

# OTG HID Functions {#OTG HID函数}

## Overview {#说明}

- OTG module functions mainly simulate a network adapter, keyboard, and mouse. Once connected, you can use wired networking, a mouse, and a keyboard at the same time
- The OTG module uses the `otgEvent` prefix
- For OTG HID hardware configuration, see [OTG HID Usage Guide](/iostjdocs/advance/tj-otg-starter)

## otgEvent.isConnected Check OTG Connection {#otgeventisconnected-otg是否连接}

* Check whether OTG is connected
* Requires EC standalone 6.6.0+
* @returns `{boolean}` `true` if connected, `false` if not connected

```javascript showLineNumbers

function _isOtgResultOk(r) {
    return r == null || r == "";
}

function reset_screen_size_by_activeself() {
    // You can also use device module functions to get device width and height
    let ss = activeSelf.deviceOrientation(10000)
    if (ss) {
        logd("Current screen orientation: " + ss["data"])
    }
    logd("Start screenshot ")
    console.time(1)
    let img = activeSelf.screenshot(10000)
    logd(img)
    if (img) {
        logd("Image dimensions: ", img.getWidth() + "," + img.getHeight())
        let set = otgEvent.setScreenSize(img.getWidth(), img.getHeight())
        if (_isOtgResultOk(set)) {
            logd("Set screen size succeeded")
        } else {
            logw("Set screen size failed ")
        }
        img.recycle()
    }
    logd("end, elapsed time (ms) ", console.timeEnd(1))
}


function testSystemKey() {
    logd("Test system key functions")
    sleep(200)
    let rc = otgEvent.systemKey("recents")
    if (_isOtgResultOk(rc)) {
        logd("Execute recents succeeded")
    } else {
        logw("Execute recents failed")
    }
    sleep(3000)
    let hm = otgEvent.systemKey("home");
    if (_isOtgResultOk(hm)) {
        logd("Execute home succeeded")
    } else {
        logw("Execute home failed")
    }

}

function test_click() {
    logd("Start testing click and gesture actions")
    sleep(1000)
    logd("Reset coordinates to 0,0")
    if (!_isOtgResultOk(otgEvent.resetZero())) {
        logw("Mouse reset to zero failed")
        return
    }
    logd("Mouse reset to zero succeeded")
    otgEvent.setStep(100)
    sleep(1000)
    if (!_isOtgResultOk(otgEvent.clickPoint(300, 420))) {
        logw("Mouse click 300,420 failed")
        return;
    }
    logd("Mouse click 300,420 succeeded")


    sleep(1000)
    if (!_isOtgResultOk(otgEvent.doubleClickPoint(600, 500))) {
        logw("Mouse double-click 600,500 failed")
        return;
    }
    logd("Mouse double-click 600,500 succeeded")

    sleep(1000)
    if (!_isOtgResultOk(otgEvent.press(620, 520, 5000))) {
        logw("Mouse long-press 620,520 failed")
        return;
    }
    logd("Mouse long-press 620,520 succeeded")

    sleep(1000)
    if (!_isOtgResultOk(otgEvent.mouseMoveDistance(10, 10, true))) {
        logw("Mouse press-and-move 10,10 pixels failed")
        return;
    }
    logd("Mouse press-and-move 10,10 pixels succeeded")
    sleep(1000)
    if (!_isOtgResultOk(otgEvent.mouseMoveDistance(10, 10, false))) {
        logw("Mouse move 10,10 pixels failed")
        return;
    }
    logd("Mouse move 10,10 pixels succeeded")
}

function testOtgMtouch() {
    let touch1 = [
        {
            "action": 0, "x": 300,
            "y": 900, "pointer": 1, "delay": 1
        },
        {
            "action": 2,
            "x": 500,
            "y": 800,
            "pointer": 1,
            "delay": 20
        }, {
            "action": 2,
            "x": 500,
            "y": 500,
            "pointer": 1,
            "delay": 20
        },
        {
            "action": 1,
            "x": 100,
            "y": 300,
            "pointer": 1,
            "delay": 20
        }];

    let m = otgEvent.multiTouch(touch1, 10000)
    if (_isOtgResultOk(m)) {
        logd("Test multiTouch succeeded")
    } else {
        logw("Test multiTouch failed " + m)
    }
}

function test_touch() {
    logd("Start testing touch actions")
    sleep(1000)

    if (_isOtgResultOk(otgEvent.touchDown(200, 300))) {
        logd("Test touchDown 200,300 succeeded")
    } else {
        logw("Test touchDown 200,300 failed")
    }
    sleep(100)
    if (_isOtgResultOk(otgEvent.touchMove(220, 320))) {
        logd("Test touchMove 220,320 succeeded")
    } else {
        logw("Test touchMove 220,320 failed")
    }
    sleep(100)
    if (_isOtgResultOk(otgEvent.touchMove(250, 360))) {
        logd("Test touchMove 250,360 succeeded")
    } else {
        logw("Test touchMove 250,360 failed")
    }
    sleep(100)
    if (_isOtgResultOk(otgEvent.touchUp(250, 360))) {
        logd("Test touchUp 250,360 succeeded")
    } else {
        logw("Test touchUp 250,360 failed")
    }
    logd("Start testing swipe")
    sleep(2000)

    if (_isOtgResultOk(otgEvent.swipeToPoint(200, 400, 290, 800, 2000))) {
        logd("Test swipe swipeToPoint 200,400,290,800 succeeded")
    } else {
        logw("Test swipe swipeToPoint 200,400,290,800 failed")
    }
}

function test_otg_keyboard() {
    logd("Find an input field to test")
    sleep(2000)
    if (_isOtgResultOk(otgEvent.keyPressChar("", "a"))) {
        logd("Input a succeeded ")
    } else {
        logw("Input a failed ")
    }
    sleep(1000)
    if (_isOtgResultOk(otgEvent.pressMouseBtn(8))) {
        logd("Input pressMouseBtn 8 succeeded ")
    } else {
        logw("Input pressMouseBtn 8 failed ")
    }

    sleep(1000)
    if (_isOtgResultOk(otgEvent.toggleSoftKeyboard())) {
        logd("Input toggleSoftKeyboard succeeded ")
    } else {
        logw("Input toggleSoftKeyboard failed ")
    }

}


function test_otg() {
    logd("Start testing OTG functions")
    sleep(1000)
    logd("Set timeout to 20000 ms")
    otgEvent.setTimeout(20000)
    sleep(1000)

    if (!otgEvent.isConnected()) {
        logw("OTG device not ready")
        return
    }
    logd("OTG is ready")

    let ok = otgEvent.setLastScale()
    logd("setLastScale : {}", ok)
    if (!_isOtgResultOk(ok)) {
        console.log("If you are using absolute-coordinate firmware, set scale to 1")
        let scale = bleEvent.getIPhoneScale();
        let setscle = otgEvent.setScale(scale, scale)
        if (!_isOtgResultOk(setscle)) {
            logd("Failed to set compensation ratio")
            return;
        }
    } else {
        logd("Using calibrated compensation ratio")
    }


    reset_screen_size_by_activeself();
    console.log("reset " + otgEvent.resetZero())
    sleep(1000)

    // testSystemKey();
    //
    test_click();
    test_touch();
    //
    // logd("Start testing multi-touch ")
    // otgEvent.resetZero()
    // sleep(2000)
    // testOtgMtouch();


    test_otg_keyboard();

}


test_otg()
```

## otgEvent.setUrl Set OTG Device Address {#otgeventseturl-设置otg设备地址}

* Set the OTG device address
* Requires EC standalone 6.6.0+
* Usually not needed; this is a reserved interface. Default is `http://172.31.255.1/`
* @param iur OTG device address
* @returns `{string}` empty string on success, non-empty error message on failure

```javascript showLineNumbers
    // See the full example code in otgEvent.isConnected
```

## otgEvent.setTimeout Set Timeout {#otgeventsettimeout-设置超时}

* Set timeout
* Set request timeout in milliseconds for OTG requests
* Requires EC standalone 6.6.0+
* @param timeoutMs timeout in milliseconds
* @returns `{string}` empty string on success, non-empty error message on failure

```javascript showLineNumbers
    // See the full example code in otgEvent.isConnected
```

## otgEvent.clickPoint Click {#otgeventclickpoint-点击}

* Click
* Requires EC standalone 6.6.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string}` empty string on success, non-empty error message on failure

```javascript showLineNumbers
    // See the full example code in otgEvent.isConnected
```

## otgEvent.mouseMove Move Mouse {#otgeventmousemove-鼠标移动}

* Move the mouse
* Requires EC standalone 6.6.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string}` empty string on success, non-empty error message on failure

```javascript showLineNumbers
    // See the full example code in otgEvent.isConnected
```

## otgEvent.resetZero Reset Mouse to Zero {#otgeventresetzero-鼠标归零}

* Reset mouse to zero
* Requires EC standalone 6.6.0+
* @returns `{string}` empty string on success, non-empty error message on failure

```javascript showLineNumbers
    // See the full example code in otgEvent.isConnected
```

## otgEvent.setStep Step Size {#otgeventsetstep-步伐大小}

* Step size
* Requires EC standalone 6.6.0+
* Larger step size means faster movement
* @param step between 1 and 120
* @returns `{string}` empty string on success, non-empty error message on failure

```javascript showLineNumbers
    // See the full example code in otgEvent.isConnected
```

## otgEvent.setScale Set Mouse Compensation Ratio {#otgeventsetscale-设置鼠标补偿比率}

* Set mouse compensation ratio
* For absolute-coordinate firmware, set to 1.0
* Note: you can calibrate OTG coordinates in the app settings; the compensation ratio is saved and can be applied on the next script run via `setLastScale`
* Ratio of how many pixels the cursor moves per mouse unit; default is 2.0
* iPhone 6/7/8 375 x 667 — set to 2.0, standard 16:9, no safe area interference
* iPhone 11 / XR 414 x 896 — set to 1.96; taller screen, system accelerates Y axis compensation
* iPhone X/XS/11Pro 375 x 812 — set to 1.98; 19.5:9 aspect ratio with slight acceleration
* iPhone 12/13/14/15 390 x 844 — set to 1.97; different logical points from iPhone 11, slightly different acceleration curve
* Plus / Max series 414 x 896 / 430 x 932 — set to 1.94 ~ 1.95; tallest screens, system increases Y axis gain significantly for usability
* @param x_scale X-axis scale (float)
* @param y_scale Y-axis scale (float)
* @returns `{string}` `null` or empty string on success, otherwise error message

```javascript showLineNumbers
    // See the full example code in otgEvent.isConnected
```

## otgEvent.setLastScale Use Previous Compensation Ratio {#otgeventsetlastscale-使用上一次补偿率}

* Use the previous compensation ratio
* Requires EC standalone 6.6.0+
* If this fails, use `setScale`, or calibrate OTG coordinates in the app settings
* @returns `{*|string}` `null` or empty string on success, otherwise error message

```javascript showLineNumbers
    // See the full example code in otgEvent.isConnected
```

## otgEvent.press Long-Press Coordinates {#otgeventpress-长按坐标}

* Long-press coordinates
* Requires EC standalone 6.6.0+
* @param x X coordinate
* @param y Y coordinate
* @param delay long-press duration in milliseconds
* @returns `{string}` `null` or empty string on success, otherwise error message

```javascript showLineNumbers
    // See the full example code in otgEvent.isConnected
```

## otgEvent.setScreenSize Set Screen Size {#otgeventsetscreensize-设置屏幕尺寸}

* Set screen size
* Requires EC standalone 6.6.0+
* Prevents the mouse from moving off-screen and causing offset; absolute-coordinate firmware uses this to compute ratios
* If screen size is unknown, use the width and height from a screenshot image
* Or use functions from the device module
* @param w screen width
* @param h screen height
* @returns `{string}` `null` or empty string on success, otherwise error message

```javascript showLineNumbers
    // See the full example code in otgEvent.isConnected
```

## otgEvent.mouseMoveDistance Move Mouse (With Press Parameter) {#otgeventmousemovedistance-移动鼠标带按下参数}

* Move mouse (with press parameter)
* Requires EC standalone 6.6.0+
* Move the mouse only; optional press parameter
* If both x and y are 0, two moves are sent — first with `press=true`, then `press=false` — which represents a click
* Requires EC iOS standalone 6.5.0+
* @param x_dis X pixel distance; must not exceed 127
* @param y_dis Y pixel distance; must not exceed 127
* @param press `true` to press
* @returns `{string}` `null` or empty string on success, otherwise error message

```javascript showLineNumbers
    // See the full example code in otgEvent.isConnected
```

## otgEvent.pressMouseBtn Press Mouse Button {#otgeventpressmousebtn-点击鼠标键}

* Press a mouse button
* Requires EC standalone 6.6.0+
* Can be used for custom AssistiveTouch — Customize Actions — More buttons
* To avoid conflicts, use button numbers from 4 to 8
* @param b mouse button number starting from 1; typically 1 = left, 2 = right, 3 = middle scroll wheel, 4–8 = additional buttons
* @returns `{string}` `null` or empty string on success, otherwise error message

```javascript showLineNumbers
    // See the full example code in otgEvent.isConnected
```

## otgEvent.doubleClickPoint Double-Click Coordinates {#otgeventdoubleclickpoint-双击坐标}

* Double-click coordinates
* Requires EC standalone 6.6.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string}` `null` or empty string on success, otherwise error message

```javascript showLineNumbers
    // See the full example code in otgEvent.isConnected
```

## otgEvent.multiTouch Multi-Touch {#otgeventmultitouch-多点触摸}

* Multi-touch
* Requires EC standalone 6.6.0+
* Touch parameters: `action` — typically 0 = down, 1 = up, 2 = move
* x: X coordinate
* y: Y coordinate
* pointer: finger index — 1, 2, 3, etc. for the nth finger<br/>
* delay: delay in milliseconds before this action executes
* @param touch1 touch point array for finger 1, e.g.:
 `[{"action":0,"x":1,"y":1,"pointer":1,"delay":20},{"action":2,"x":1,"y":1,"pointer":1,"delay":20}]`
* @param timeout multi-touch execution timeout in milliseconds
* @returns `{string}` `null` or empty string on success, otherwise error message

```javascript showLineNumbers
    // See the full example code in otgEvent.isConnected
```

## otgEvent.swipeToPoint Swipe {#otgeventswipetopoint-滑动}

* Swipe
* Requires EC standalone 6.6.0+
* @param x start X coordinate
* @param y start Y coordinate
* @param ex end X coordinate
* @param ey end Y coordinate
* @param delay duration in milliseconds
* @returns `{string}` `null` or empty string on success, otherwise error message

```javascript showLineNumbers
    // See the full example code in otgEvent.isConnected
```

## otgEvent.touchDown Touch Down {#otgeventtouchdown-按下坐标点}

* Touch down at coordinates
* Requires EC standalone 6.6.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string}` `null` or empty string on success, otherwise error message

```javascript showLineNumbers
    // See the full example code in otgEvent.isConnected
```

## otgEvent.touchMove Touch Move {#otgeventtouchmove-移动坐标点}

* Move touch point
* Requires EC standalone 6.6.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string}` `null` or empty string on success, otherwise error message

```javascript showLineNumbers
    // See the full example code in otgEvent.isConnected
```

## otgEvent.touchUp Touch Up {#otgeventtouchup-抬起坐标点}

* Lift touch at coordinates
* Requires EC standalone 6.6.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string}` `null` or empty string on success, otherwise error message

```javascript showLineNumbers
    // See the full example code in otgEvent.isConnected
```

## otgEvent.systemKey System Key {#otgeventsystemkey-系统按键}

* System key
* Requires EC standalone 6.6.0+
* @param key currently supports `home`, `recents` = recent tasks
* @returns `{string}` `null` or empty string on success, otherwise error message

```javascript showLineNumbers
    // See the full example code in otgEvent.isConnected
```

## otgEvent.keyPressChar Character Key Press {#otgeventkeypresschar-字符按键}

* Character key press
* Requires EC standalone 6.6.0+
* @param prefix modifier key; can be empty — `alt` = Alt key, `ctrl` = Ctrl key, `gui` = Win or Command key, `r_ctrl` = right Ctrl key, `r_shift` = right Shift key, `shift` = Shift key
* @param code character, e.g. `a`
* @returns `{string}` `null` or empty string on success, otherwise error message

```javascript showLineNumbers
    // See the full example code in otgEvent.isConnected
```

## otgEvent.keyPress Key Press {#otgeventkeypress-按键}

* Key press
* Requires EC standalone 6.6.0+
* @param prefix modifier key; can be empty — `alt` = Alt key, `ctrl` = Ctrl key, `gui` = Win or Command key, `r_ctrl` = right Ctrl key, `r_shift` = right Shift key, `shift` = Shift key
* @param code integer, e.g. 65 (ASCII code); see https://tool.oschina.net/commons?type=4
* @returns `{string}` `null` or empty string on success, otherwise error message

```javascript showLineNumbers
    // See the full example code in otgEvent.isConnected
```

## otgEvent.toggleSoftKeyboard Toggle Soft Keyboard {#otgeventtogglesoftkeyboard-开关软键盘}

* Toggle soft keyboard
* Requires EC standalone 6.6.0+
* On iPhone 7 in testing, input fields could not show the soft keyboard for standalone host app input; try this method. iPhone 11 did not have this issue — behavior depends on iOS version
* Ignore this method if you are not using the standalone host app as the input method
* Requires EC iOS standalone 6.5.0+
* @returns `{string}` `null` or empty string on success, otherwise error message

```javascript showLineNumbers
    // See the full example code in otgEvent.isConnected
```




## otgEvent.startKeepAlive Mouse Keep-Alive {#otgeventstartkeepalive-鼠标心跳}

* Mouse keep-alive heartbeat
* Keeps the mouse cursor visible
* Under the hood, moves the mouse away and back
* If other actions are running, this heartbeat move is not sent
* Absolute-coordinate firmware on iOS 17+ does not need this function
* @param keepAliveTime heartbeat interval in milliseconds; e.g. 5000 for every 5 seconds
* @param moveValue how much to move each time; use 1 or 2; must not exceed 100
* @returns `{*|string}` empty string on success, non-empty error message on failure
```javascript showLineNumbers
    // Call directly after Bluetooth connection completes
otgEvent.startKeepAlive(5000,1)
```



## otgEvent.stopKeepAlive Stop Keep-Alive {#otgeventstopkeepalive-停止心跳}
* Stop keep-alive heartbeat
* @returns `{string}` empty string on success, non-empty error message on failure
```javascript showLineNumbers
    // Call directly
    otgEvent.stopKeepAlive()
```


## otgEvent.getMacAddress Get MAC Address {#otgeventgetmacaddress-获取mac地址}

* Get MAC address
* @returns `{string}` non-empty value means MAC address was retrieved
```javascript showLineNumbers
    // Call directly
    logd(otgEvent.getMacAddress())
```

