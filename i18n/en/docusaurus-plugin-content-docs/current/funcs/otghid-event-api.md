---
title: OTG HID Events
description: EasyClick automation scripts — Android no root HID events
keywords:
 - EasyClick automation scripts Android no root HID events
 - OTG
 - HID
 - init
 - connectFirst
 - otgEvent
 - hid
 - EC
 - 11.40.0
 - return
 - 'null'
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview {#说明}

- All OTG HID event module functions require hardware to use
- See OTG HID setup in Advanced — OTG HID Hardware: [OTGHID Hardware](/docs/advance/otghid)
- The module object prefix is `otgEvent`, e.g. `otgEvent.click()`

:::tip

- OTG HID is one click mode; accessibility, proxy mode, and root all support click and node fetch. If nodes are unavailable, use image/color matching
- For image capture permission, use `image.requestScreenCapture` with type=1 (permission-based capture)
- OTG HID cannot use nodes; other features work the same with no special handling
 :::

## init Initialize OTG Serial Port {#init-初始化-otg-串口}

* Initialize the OTG serial port
* Requires EC Android 11.40.0+
* @return `{null|string}` null on success; otherwise an error message

```javascript showLineNumbers
// Test example for reference
var OTG_STEP_DELAY_MS = 2000;

/** Whether to run navigation tests: home / back / recents / systemKey */
var OTG_TEST_RUN_NAV_KEYS = false;

/** Touch test coordinates (adjust for target resolution) */
var OTG_TEST_X = 200;
var OTG_TEST_Y = 200;
var OTG_TEST_X2 = 400;
var OTG_TEST_Y2 = 400;

function _ok(name, err) {
    if (err == null || err === "" || err === undefined) {
        logd("[OTG-TEST] OK " + name);
        return true;
    }
    loge("[OTG-TEST] FAIL " + name + " -> " + err);
    return false;
}

function runOtgEventTests() {
    logd("[OTG-TEST] ========== start ==========");

    _ok("init", otgEvent.init());
    sleep(OTG_STEP_DELAY_MS);

    _ok("connectFirst", otgEvent.connectFirst());
    sleep(OTG_STEP_DELAY_MS);

    logd("[OTG-TEST] isConnected = " + otgEvent.isConnected());

    
    // If unavailable, sleep 3s and retry
    logd("mac "+otgEvent.getMacAddress())
    
    _ok("setTimeouts(800,1500,2500)", otgEvent.setTimeouts(2000, 2000, 3000));
    sleep(OTG_STEP_DELAY_MS);

    _ok("clickPoint", otgEvent.clickPoint(200, 300));
    sleep(OTG_STEP_DELAY_MS);

    _ok("doubleClickPoint", otgEvent.doubleClickPoint(400, 500));
    sleep(OTG_STEP_DELAY_MS);

    _ok("press(600ms)", otgEvent.press(600, 700, 5000));
    sleep(OTG_STEP_DELAY_MS);

    _ok("swipe", otgEvent.swipe(OTG_TEST_X, OTG_TEST_Y, OTG_TEST_X2, OTG_TEST_Y2, 400));
    sleep(OTG_STEP_DELAY_MS);

    _ok("touchDown", otgEvent.touchDown(OTG_TEST_X, OTG_TEST_Y));
    sleep(OTG_STEP_DELAY_MS);
    _ok("touchMove(pressed=true)", otgEvent.touchMove(OTG_TEST_X + 30, OTG_TEST_Y + 30));
    sleep(OTG_STEP_DELAY_MS);
    _ok("touchMove(pressed=false)", otgEvent.touchMove(OTG_TEST_X + 60, OTG_TEST_Y + 60));
    sleep(OTG_STEP_DELAY_MS);
    _ok("touchUp", otgEvent.touchUp(OTG_TEST_X + 60, OTG_TEST_Y + 60));
    sleep(OTG_STEP_DELAY_MS);

    if (OTG_TEST_RUN_NAV_KEYS) {
        _ok("systemKey(home)", otgEvent.systemKey("home"));
        sleep(OTG_STEP_DELAY_MS);
        _ok("systemKey(back)", otgEvent.systemKey("back"));
        sleep(OTG_STEP_DELAY_MS);
        _ok("systemKey(recents)", otgEvent.systemKey("recents"));
        sleep(OTG_STEP_DELAY_MS);

        _ok("recents()", otgEvent.recents());
        sleep(OTG_STEP_DELAY_MS);
        _ok("back()", otgEvent.back());
        sleep(OTG_STEP_DELAY_MS);
        _ok("home()", otgEvent.home());
        sleep(OTG_STEP_DELAY_MS);
    } else {
        logd("[OTG-TEST] skip systemKey / recents / back / home (set OTG_TEST_RUN_NAV_KEYS = true to run)");
    }

    _ok("keyPressChar a", otgEvent.keyPressChar("", "a"));
    sleep(OTG_STEP_DELAY_MS);
    _ok("keyPress 97 (a)", otgEvent.keyPress("", 97));
    sleep(OTG_STEP_DELAY_MS);

    var seq = [
        {"action": 0, "x": OTG_TEST_X, "y": OTG_TEST_Y, "pointer": 1, "delay": 40},
        {"action": 2, "x": OTG_TEST_X + 50, "y": OTG_TEST_Y + 50, "pointer": 1, "delay": 40},
        {"action": 2, "x": OTG_TEST_X + 100, "y": OTG_TEST_Y + 100, "pointer": 1, "delay": 40},
        {"action": 1, "x": OTG_TEST_X + 100, "y": OTG_TEST_Y + 100, "pointer": 1, "delay": 40}
    ];
    _ok("multiTouch", otgEvent.multiTouch(seq, 5000));
    sleep(OTG_STEP_DELAY_MS);

    _ok("close", otgEvent.close());
    sleep(OTG_STEP_DELAY_MS);

    logd("[OTG-TEST] ========== end ==========");
}

runOtgEventTests();

```

## connectFirst Connect First Serial Device {#connectfirst-连接第一个串口设备}

* Connect to the first serial device
* Requires EC Android 11.40.0+
* Note: If not authorized, a permission dialog appears and an error is returned; call again after granting permission
* @return `{null|string}` null on success; otherwise an error message

```javascript showLineNumbers
    // See init function example
```

## isConnected Connection Status {#isconnected-连接状态}

* Connection status
* Requires EC Android 11.40.0+
* @return `{boolean}` true when connected

```javascript showLineNumbers
    // See init function example
```

## setTimeouts Set Timeouts {#settimeouts-设置超时}

* Set timeouts (global)
* Requires EC Android 11.40.0+
* @param writeTimeoutMs Write timeout (ms); default 1000
* @param replyTimeoutMs Reply wait timeout (ms); default 2000
* @param durationExtraTimeoutMs Extra wait for press/swipe actions with duration (ms); default 3000
* @return `{null|string}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // See init function example
```

## clickPoint Click {#clickpoint-点击}

* Click
* Requires EC Android 11.40.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // See init function example
```

## press Long Press {#press-长按}

* Long press
* Requires EC Android 11.40.0+
* @param x X coordinate
* @param y Y coordinate
* @param holdMs Hold duration in milliseconds
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // See init function example
```

## doubleClick Double Click {#doubleclick-双击}

* Double click
* Requires EC Android 11.40.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // See init function example
```

## touchDown Touch Down {#touchdown-按下}

* Touch down
* Requires EC Android 11.40.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // See init function example
```

## touchMove Touch Move {#touchmove-移动}

* Touch move
* Requires EC Android 11.40.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // See init function example
```

## touchUp Touch Up {#touchup-抬起}

* Touch up
* Requires EC Android 11.40.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // See init function example
```

## swipe Swipe {#swipe-滑动}

* Swipe
* Requires EC Android 11.40.0+
* @param x Start X coordinate
* @param y Start Y coordinate
* @param ex End X coordinate
* @param ey End Y coordinate
* @param delay Total swipe duration in milliseconds
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // See init function example
```

## multiTouch Multi-Touch {#multitouch-多点触摸}

* Multi-touch
* Touch parameters: action — 0 = down, 1 = up, 2 = move
* x: X coordinate
* y: Y coordinate
* pointer: Finger index 1, 2, 3, etc.
* delay: Delay before this action in ms; use >40ms to avoid coordinate drift
* @param touch1 Touch point array for finger 1, e.g.:
 `[{"action":0,"x":1,"y":1,"pointer":1,"delay":30},{"action":2,"x":1,"y":1,"pointer":1,"delay":30}]`
* @param timeout Multi-touch execution timeout in milliseconds
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // See init function example
```

## systemKey System Key {#systemkey-系统按键}

* System key
* Requires EC Android 11.40.0+
* @param key Values: home, back, recents
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // See init function example
```

## recents Recents {#recents-任务列表}

* Recents / task list
* May not work on all devices (non-standard key)
* Requires EC Android 11.40.0+
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // See init function example
```

## back Back {#back-返回}

* Back
* Requires EC Android 11.40.0+
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // See init function example
```

## home Home {#home-主页}

* Home
* Requires EC Android 11.40.0+
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // See init function example
```

## keyPress Keyboard Key {#keypress-键盘按键}

* Keyboard key
* Requires EC Android 11.40.0+
* @param prefix Modifier prefix; omit or null for normal key: alt, ctrl, gui (Win), r_ctrl (right Ctrl), r_shift (right Shift), shift
* @param code ASCII code
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers
    // See init function example
```

## keyPressChar Keyboard Character {#keypresschar-键盘按键字符}

* Keyboard character key
* Requires EC Android 11.40.0+
* @param prefix Modifier prefix; omit or null for normal key: alt, ctrl, gui (Win), r_ctrl (right Ctrl), r_shift (right Shift), shift
* @param c Single character, e.g. a; converted to ASCII internally. See https://tool.oschina.net/commons?type=4
* @returns `{string|null}` null or empty on success; otherwise an error message

```javascript showLineNumbers

// See init function example
```

## getMacAddress Firmware MAC Address {#getmacaddress-固件mac地址}

* Read firmware MAC address
* Requires EC Android 11.41.0+
* @returns `{string|null}` aa:bb:cc:dd:ee:ff on success; null on failure

```javascript showLineNumbers

// See init function example
```
