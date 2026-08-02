---
title: OTG HID Functions
description: EasyClick automation scripts — iOS no jailbreak OTG HID functions
keywords:
 - EasyClick automation scripts iOS no jailbreak OTG HID functions
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

## Overview

- This feature simulates swipe, input, and other actions via OTG; currently supports ESP32S3 development boards
- For firmware flashing and connection setup, see [iOS USB OTG-HID Tutorial](/iosdocs/advance/ios-usb-otg)

:::tip

- Note: firmware only includes **absolute-coordinate** firmware — use iOS 17+ systems
 :::

## otgEvent.isOtgConnect OTG Connection Status

* OTG connection status
* Available in EC iOS USB edition 9.32.0+
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
let current_screen_width = 0;
let current_screen_height = 0;


function otg_test() {
    //scanOtgIp();
    logd("Start test otg functions")
    if (!connectOtg()) {
        return
    }

    logd("device.getOrientationNoAuto() " + device.getOrientationNoAuto())
    captureScreenAndResetScreenSize();
    testSystemKey();
    testInput();
    click_swipe();
    test_touch();
    test_multiTouch()
    logd("test otg end")
}

function test_multiTouch() {
    sleep(1000)
    logd("Start testing multi-touch function")
    // Keep the final lift aligned with the last move to avoid offset errors
    let touch1 = [
        {"action": 0, "x": 500, "y": 1200, "delay": 10},
        {"action": 2, "x": 480, "y": 1100, "delay": 200},
        {"action": 2, "x": 460, "y": 1000, "delay": 100},
        {"action": 2, "x": 430, "y": 1000, "delay": 100},
        {"action": 2, "x": 430, "y": 950, "delay": 100},
        {"action": 2, "x": 350, "y": 900, "delay": 100},
        {"action": 2, "x": 300, "y": 800, "delay": 100},
        {"action": 1, "x": 300, "y": 800, "delay": 2}
    ]

    let rr = otgEvent.multiTouch(touch1, 1000)

    if (_isEmpty(rr)) {
        logd("Multi-touch succeeded")
    } else {
        logd("Multi-touch failed {}", rr)
    }
}

function test_touch() {
    sleep(1000)
    let startx = 200;
    let starty = 400;
    let touchDown = otgEvent.touchDown(startx, starty)
    logd("touchDown, {},{} :{} ", startx, starty, (_isEmpty(touchDown) ? "OK" : touchDown))
    for (let i = 0; i < 50; i++) {
        sleep(10)
        startx = startx + 1;
        starty = starty + 10;
        let touchMove = otgEvent.touchMove(startx, starty)
        logd("touchMove {},{} :{} ", startx, starty, (_isEmpty(touchMove) ? "OK" : touchMove))

    }
    sleep(100)
    let touchUp = otgEvent.touchUp(startx, starty)
    logd("touchUp {},{} :{} ", startx, starty, (_isEmpty(touchUp) ? "OK" : touchUp))


}

function click_swipe() {
    sleep(1000)
    let click = otgEvent.clickPoint(508, 411)
    logd("clickPoint: {}", (_isEmpty(click) ? "OK" : click))

    sleep(1000)
    let press = otgEvent.press(719, 792, 5000)
    logd("press : {}", (_isEmpty(press) ? "OK" : press))

    sleep(1000)
    let doubleClickPoint = otgEvent.doubleClickPoint(500, 792)
    logd("doubleClickPoint : {}", (_isEmpty(doubleClickPoint) ? "OK" : doubleClickPoint))


    sleep(1000)
    let swipeToPoint = otgEvent.swipeToPoint(300, 400, 700, 900, 1000)
    logd("swipeToPoint : {}", (_isEmpty(swipeToPoint) ? "OK" : swipeToPoint))

}


function moveMouse() {
    sleep(1000)
    let resetZero = otgEvent.resetZero()
    logd("resetZero {}", (_isEmpty(resetZero) ? "OK" : resetZero))

    sleep(1000)
    let mouseMove = otgEvent.mouseMove(300, 500)
    logd("mouseMove {}", (_isEmpty(mouseMove) ? "OK" : mouseMove))
}

function testSystemKey() {
    sleep(1000)
    let recents_r = otgEvent.systemKey("recents");
    logd("Recent tasks {} ", _isEmpty(recents_r) ? "ok" : recents_r)
    sleep(2000)
    let home_result = otgEvent.systemKey("home");
    logd("Simulate HOME {} ", _isEmpty(home_result) ? "ok" : home_result)
}

function testInput() {

    let keyPress1 = otgEvent.keyPress("", 97)

    logd("keyPress {} ", _isEmpty(keyPress1) ? "ok" : keyPress1)
    otgEvent.keyPressChar("", "Enter")
    sleep(2000)
    let keyPressChar = otgEvent.keyPressChar("alt", "b")
    logd("keyPressChar {} ", _isEmpty(keyPressChar) ? "ok" : keyPressChar)
    sleep(2000)
    otgEvent.keyPressChar("", "t")
    sleep(2000)
    logd("toggleSoftKeyboard {}", otgEvent.toggleSoftKeyboard())

}

function connectOtg() {

    let oldIp = otgEvent.getOtgIp();
    if (!_isEmpty(oldIp)) {
        logd("Current OTG device IP {} getMacAddress={}", oldIp, otgEvent.getMacAddress())
        if (_isEmpty(otgEvent.isOtgConnect())) {
            logd("OTG device is ready")
            return true;
        }
        logd("OTG device not ready — start rescanning")
    }

    let ips = utils.getPCIps();
    if (_isEmpty(ips)) {
        logd("Could not find this PC's IP")
        return false;
    }
    logd("This PC's IP: {}", ips)
    ips = ips.split(",")
    for (let i = 0; i < ips.length; i++) {
        let ipsKey = ips[i];
        let iparr = ipsKey.split(".")
        let range = iparr[0] + "." + iparr[1] + "." + iparr[2] + ".2-" + iparr[0] + "." + iparr[1] + "." + iparr[2] + ".254"
        logd("-- scan ip rang: {}", range)
        let scanr = otgEvent.scanOtgDevice(range)
        if (_isEmpty(scanr)) {
            logd("IP range scan finished: {}", range)
        } else {
            logd("IP range scan error: {}", scanr)
        }
    }
    logd("Waiting for OTG to be ready...")
    sleep(5000)
    oldIp = otgEvent.getOtgIp();
    if (!_isEmpty(oldIp)) {
        if (_isEmpty(otgEvent.isOtgConnect())) {
            logd("Current OTG device IP {} getMacAddress={}", oldIp, otgEvent.getMacAddress())
            logd("OTG device is ready")
            return true;
        }
        logd("OTG device not ready")
    } else {
        logw("OTG device not ready — configure network on the development board first, then scan")
    }
    return false
}


function captureScreenAndResetScreenSize() {
    logd("Start setting screen size to prevent coordinate offset")
    let img = image.captureFullScreenNoAuto()
    if (!img) {
        image.resetCaptureScreenNoAutoEnv()
        sleep(1000)
        img = image.captureFullScreenNoAuto()
    }
    if (img) {
        let w = img.getWidth();
        let h = img.getHeight();
        if (w != current_screen_height || h != current_screen_height) {
            let sets = otgEvent.setScreenSize(w, h)
            if (_isEmpty(sets)) {
                logd("Screen size set successfully w:{} h:{}", w, h)
                current_screen_width = w;
                current_screen_height = h;
            }
        }
        img.recycle()
    } else {
        logw("Screenshot failed — screen size may not be set, coordinates may be offset")
    }


}

otg_test()
```

## otgEvent.scanOtgDevice Scan OTG Device IP

* Scan for OTG device IP
* Available in EC iOS USB edition 9.32.0+
* @param ip_ranges IP range, e.g. 192.168.2.1-192.168.2.255
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See otgEvent.isOtgConnect example
}

main();
```

## otgEvent.getOtgIp Get OTG IP

* Get OTG IP
* Available in EC iOS USB edition 9.32.0+
* @returns `{string}` null or empty string means no IP — rescan required

```javascript showLineNumbers
function main() {
    // See otgEvent.isOtgConnect example
}

main();
```

## otgEvent.setScreenSize Set Screen Size

* Set screen size
* Prevents the mouse from moving off-screen and causing offset
* If screen size is unknown, use the width and height from a screenshot
* Available in EC iOS USB edition 9.32.0+
* @param w Screen width
* @param h Screen height
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See otgEvent.isOtgConnect example
}

main();
```

## otgEvent.restart Restart Development Board

* Restart the development board
* Equivalent to pressing the RST button on the board
* Available in EC iOS USB edition 9.32.0+
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
function main() {
    // Call directly
    logd(otgEvent.restart())
}

main();
```

## otgEvent.mouseMove Move Mouse

* Move the mouse
* Moves only — no press action
* Available in EC iOS USB edition 9.32.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See otgEvent.isOtgConnect example
}

main();
```

## otgEvent.resetZero Reset Mouse to Zero

* Reset mouse to zero
* Moves the mouse to the top-right corner at (0, 0)
* Available in EC iOS USB edition 9.32.0+
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See otgEvent.isOtgConnect example
}

main();
```

## otgEvent.touchDown Touch Down at Coordinates

* Touch down at coordinates
* Available in EC iOS USB edition 9.32.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string} ` null or empty string means success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See otgEvent.isOtgConnect example
}

main();
```

## otgEvent.touchMove Move Touch Point

* Move touch point
* Available in EC iOS USB edition 9.32.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string} ` null or empty string means success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See otgEvent.isOtgConnect example
}

main();
```

## otgEvent.touchUp Touch Up at Coordinates

* Touch up at coordinates
* Available in EC iOS USB edition 9.32.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string} ` null or empty string means success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See otgEvent.isOtgConnect example
}

main();
```

## otgEvent.clickPoint Click Coordinates

* Click coordinates
* Available in EC iOS USB edition 9.32.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string} ` null or empty string means success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See otgEvent.isOtgConnect example
}

main();
```

## otgEvent.press Long Press Coordinates

* Long press coordinates
* Available in EC iOS USB edition 9.32.0+
* @param x X coordinate
* @param y Y coordinate
* @param delay Long press duration in milliseconds
* @returns `{string} ` null or empty string means success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See otgEvent.isOtgConnect example
}

main();
```

## otgEvent.doubleClickPoint Double-Click Coordinates

* Double-click coordinates
* Available in EC iOS USB edition 9.32.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string} ` null or empty string means success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See otgEvent.isOtgConnect example
}

main();
```

## otgEvent.swipeToPoint Swipe Between Coordinates

* Swipe between coordinates
* Available in EC iOS USB edition 9.32.0+
* @param startX Start X coordinate
* @param startY Start Y coordinate
* @param endX End X coordinate
* @param endY End Y coordinate
* @param duration Duration in milliseconds
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See otgEvent.isOtgConnect example
}

main();
```

## otgEvent.multiTouch Multi-Touch

* Multi-touch
* Touch parameters: action — typically 0=down, 1=up, 2=move
* x: X coordinate
* y: Y coordinate
* pointer: finger index — 1, 2, 3, etc.
* delay: delay before this action in milliseconds
* Available in EC iOS USB edition 9.32.0+
* @param touch1 Touch point array for finger 1, e.g.:
 `[{"action":0,"x":1,"y":1,"pointer":1,"delay":20},{"action":2,"x":1,"y":1,"pointer":1,"delay":20}]`
* @param timeout Multi-touch execution timeout in milliseconds
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See otgEvent.isOtgConnect example
}

main();
```

## otgEvent.systemKey System Key

* System key
* Available in EC iOS USB edition 9.32.0+
* @param key Currently supports home, recents (recent tasks)
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See otgEvent.isOtgConnect example
}

main();
```

## otgEvent.keyPress Key Press

* Key press
* Available in EC iOS USB edition 9.32.0+
* @param prefix Modifier key; can be empty — alt=Alt, ctrl=Ctrl, gui=Win/Command, r_ctrl=right Ctrl, r_shift=right Shift, shift=Shift
* @param code Integer, e.g.
 65 (ASCII); see [https://tool.oschina.net/commons?type=4](https://tool.oschina.net/commons?type=4)
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See otgEvent.isOtgConnect example
}

main();
```

## otgEvent.keyPressChar Character Key Press

* Character key press
* Available in EC iOS USB edition 9.32.0+
* @param prefix Modifier key; can be empty — alt=Alt, ctrl=Ctrl, gui=Win/Command, r_ctrl=right Ctrl, r_shift=right Shift, shift=Shift
* @param code Character, e.g. a; BS=Backspace, LF=line feed
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See otgEvent.isOtgConnect example
}

main();
```

## otgEvent.toggleSoftKeyboard Toggle Soft Keyboard

* Toggle soft keyboard
* On iPhone 7 in testing, after connection, input fields may not show the soft keyboard when using the standalone main app for input — try this method; iPhone 11 did not have this issue; behavior depends on iOS version
* Ignore this method if you do not use the standalone main app as the input method
* Available in EC iOS USB edition 9.32.0+
* @returns `{string}` null or empty string means success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See otgEvent.isOtgConnect example
}

main();
```

## otgEvent.getMacAddress Get MAC Address

* Get MAC address
* Available in EC iOS USB edition 9.32.0+
* @returns `{string}` MAC address string

```javascript showLineNumbers
function main() {
    // See otgEvent.isOtgConnect example
}

main();
```



## otgEvent.pressMouseBtn Press Mouse Button

* Press a mouse button
* Available in EC iOS USB edition 9.32.0+
* Can be used for AssistiveTouch custom actions — Custom Actions
* To avoid conflicts, use button numbers 4 through 8
* @param btn Mouse button number starting from 1; typically 1=left, 2=right, 3=middle scroll, 4–8=custom
* @returns `{string}` null or empty string means success; otherwise an error message
```javascript showLineNumbers
function main() {
    // See otgEvent.isOtgConnect example
}

main();
```


## Type Text via Shortcuts {#利用快捷指令进行输入文字}

- Verified test code below

```javascript showLineNumbers


function main() {
    logd("Start test otg functions")
    if (!connectOtg()) {
        return
    }
    logd("Communication OK — starting execution")
    test_otg_shortcut_input()

}

function test_otg_shortcut_input() {
    let msg = device.getDeviceMsg()
    // BLE and OTG share the bleMac field — the MAC bound in the control center
    let bleMac = "";
    if (msg != null && msg != "") {
        // Get iPhone type for scale calibration
        let bb = JSON.parse(msg);
        bleMac = bb["bleMac"];
    }
    if (bleMac == null || bleMac == "") {
        logw("Could not get OTG MAC address")
        return
    }
    logd("Using control-center-bound OTG MAC as unique ID {}", bleMac)

    let data = {
        key: bleMac,
        "content": "Sample data " + new Date().toString()
    }
    // Submit to iOS Shortcuts assistant service — use your external URL if deployed remotely
    let post_url = "http://192.168.2.26:8696/postText"
    let result = http.postJSON(post_url, data, 10 * 1000, {});
    if (result == null || result == "") {
        logw("Failed to submit data to iOS Shortcuts assistant service")
        return;
    }
    result = JSON.parse(result)
    if (result["code"] != 0) {
        logw("Failed to submit data to iOS Shortcuts assistant service," + result["msg"])
        return;
    }
    // Run the previously bound shortcut hotkey
    let gs = otgEvent.keyPressChar("gui", "u")
    if (gs != null && gs != "") {
        logw("Failed to run shortcut hotkey " + gs)
        return
    }

    logd("Running shortcut — waiting for result")
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
                // Confirmed success
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
    let gsv = otgEvent.keyPressChar("gui", "v")
    if (gsv != null && gsv != "") {
        logw("Failed to run paste shortcut hotkey " + gs)
        return
    }

    logd("Paste succeeded")
}


function connectOtg() {

    let oldIp = otgEvent.getOtgIp();
    if (!_isEmpty(oldIp)) {
        logd("Current OTG device IP {} getMacAddress={}", oldIp, otgEvent.getMacAddress())
        if (_isEmpty(otgEvent.isOtgConnect())) {
            logd("OTG device is ready")
            return true;
        }
        logd("OTG device not ready — start rescanning")
    }

    let ips = utils.getPCIps();
    if (_isEmpty(ips)) {
        logd("Could not find this PC's IP")
        return false;
    }
    logd("This PC's IP: {}", ips)
    ips = ips.split(",")
    for (let i = 0; i < ips.length; i++) {
        let ipsKey = ips[i];
        let iparr = ipsKey.split(".")
        let range = iparr[0] + "." + iparr[1] + "." + iparr[2] + ".2-" + iparr[0] + "." + iparr[1] + "." + iparr[2] + ".254"
        logd("-- scan ip rang: {}", range)
        let scanr = otgEvent.scanOtgDevice(range)
        if (_isEmpty(scanr)) {
            logd("IP range scan finished: {}", range)
        } else {
            logd("IP range scan error: {}", scanr)
        }
    }
    logd("Waiting for OTG to be ready...")
    sleep(5000)
    oldIp = otgEvent.getOtgIp();
    if (!_isEmpty(oldIp)) {
        if (_isEmpty(otgEvent.isOtgConnect())) {
            logd("Current OTG device IP {} getMacAddress={}", oldIp, otgEvent.getMacAddress())
            logd("OTG device is ready")
            return true;
        }
        logd("OTG device not ready")
    }
    return false
}

main()

```

## Insert to Photos via Shortcuts {#利用快捷指令进行插入相册}

- Verified test code below

```javascript showLineNumbers
function main() {
    logd("Start test otg functions")
    if (!connectOtg()) {
        return
    }
    logd("Communication OK — starting execution")

    test_otg_insert()


}


function connectOtg() {

    let oldIp = otgEvent.getOtgIp();
    if (!_isEmpty(oldIp)) {
        logd("Current OTG device IP {} getMacAddress={}", oldIp, otgEvent.getMacAddress())
        if (_isEmpty(otgEvent.isOtgConnect())) {
            logd("OTG device is ready")
            return true;
        }
        logd("OTG device not ready — start rescanning")
    }

    let ips = utils.getPCIps();
    if (_isEmpty(ips)) {
        logd("Could not find this PC's IP")
        return false;
    }
    logd("This PC's IP: {}", ips)
    ips = ips.split(",")
    for (let i = 0; i < ips.length; i++) {
        let ipsKey = ips[i];
        let iparr = ipsKey.split(".")
        let range = iparr[0] + "." + iparr[1] + "." + iparr[2] + ".2-" + iparr[0] + "." + iparr[1] + "." + iparr[2] + ".254"
        logd("-- scan ip rang: {}", range)
        let scanr = otgEvent.scanOtgDevice(range)
        if (_isEmpty(scanr)) {
            logd("IP range scan finished: {}", range)
        } else {
            logd("IP range scan error: {}", scanr)
        }
    }
    logd("Waiting for OTG to be ready...")
    sleep(5000)
    oldIp = otgEvent.getOtgIp();
    if (!_isEmpty(oldIp)) {
        if (_isEmpty(otgEvent.isOtgConnect())) {
            logd("Current OTG device IP {} getMacAddress={}", oldIp, otgEvent.getMacAddress())
            logd("OTG device is ready")
            return true;
        }
        logd("OTG device not ready")
    }
    return false
}


function test_otg_insert() {
    let msg = device.getDeviceMsg()
    let bleMac = "";
    if (msg != null && msg != "") {
        // Get iPhone type for scale calibration
        let bb = JSON.parse(msg);
        bleMac = bb["bleMac"];
    }
    if (bleMac == null || bleMac == "") {
        logw("Could not get OTG MAC address")
        return
    }
    logd("Using control-center-bound OTG MAC as unique ID {}", bleMac)

    let data = {}
    // Submit to iOS Shortcuts assistant service — upload image or video endpoint; use external URL if deployed remotely
    // key is the MAC address as identifier
    let post_url = "http://192.168.2.26:8696/upload?key=" + bleMac
    // Select a video file here
    let file = {
        "file": "c:/Downloads/QQ2025522-95310.mp4"
    }
    let result = http.httpPost(post_url, data, file, 20 * 1000, {});
    if (result == null || result == "") {
        logw("Failed to submit data to iOS Shortcuts assistant service")
        return;
    }
    logd("Submit result: " + result)
    result = JSON.parse(result)
    if (result["code"] != 0) {
        logw("Failed to submit data to iOS Shortcuts assistant service," + result["msg"])
        return;
    }
    // Run the previously bound shortcut hotkey
    let gs = otgEvent.keyPressChar("gui", "i")
    if (gs != null && gs != "") {
        logw("Failed to run shortcut hotkey " + gs)
        return
    }

    logd("Running shortcut — waiting for result")
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
                // Confirmed success
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
