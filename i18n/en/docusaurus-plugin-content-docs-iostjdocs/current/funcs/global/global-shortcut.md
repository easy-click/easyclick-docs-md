---
title: EasyClick Automation Scripts — iOS Scripts — No Jailbreak — No Hardware — Global Shortcut Events
hide_title: false
hide_table_of_contents: false
sidebar_label: Global Shortcut Events
description: EasyClick automation scripts — iOS no jailbreak — global shortcut events
keywords:
 - EasyClick automation scripts
 - iOS scripts
 - iOS no jailbreak
 - iOS no hardware
 - global shortcut events
 - param
 - return
 - iOS
 - clickPoint
 - clickPointPressure
 - longClickPoint
 - doubleClickPoint
 - press
 - EasyClick
 - multiTouch
 - mobile automation
 - test automation
 - script development
---

# Global Shortcut Events {#全局快捷事件}

## Overview {#说明}

Shortcut events encapsulated in the global module.

## Click Functions {#点击函数}

### clickPoint Click by coordinates {#clickpoint-坐标点击}

* Click at coordinates
* @param x X coordinate
* @param y Y coordinate
* @return boolean

```javascript showLineNumbers
function main() {
    var result = clickPoint(100, 100);
    if (result) {
        logd("Click succeeded");
    } else {
        logd("Click failed");
    }
}

main();
```

### clickPointPressure Click coordinates with pressure {#clickpointpressure-Click coordinates with pressure}

* Click at coordinates with pressure
* Requires EC standalone 2.1.0+
* @param x X coordinate
* @param y Y coordinate
* @param pressure Pressure value in the range 0–1
* @return `{boolean}`

```javascript showLineNumbers
function main() {
    var result = clickPointPressure(100, 100, 0.2);
    if (result) {
        logd("Click succeeded");
    } else {
        logd("Click failed");
    }
}

main();
```

### longClickPoint Long-click by coordinates {#longclickpoint-坐标长点击}

* Long-click at coordinates
* @param x X coordinate
* @param y Y coordinate
* @return `{boolean}`

```javascript showLineNumbers
function main() {
    var result = longClickPoint(100, 100);
    if (result) {
        logd("Click succeeded");
    } else {
        logd("Click failed");
    }
}

main();
```

### doubleClickPoint Double-click by coordinates {#doubleclickpoint-坐标双击}

* Double-click at coordinates
* @param x X coordinate
* @param y Y coordinate
* @return `{boolean}`

```javascript showLineNumbers
function main() {
    var result = doubleClickPoint(100, 100);
    if (result) {
        logd("Click succeeded");
    } else {
        logd("Click failed");
    }
}

main();
```

### press Long-press by coordinates {#press-坐标长按}

* Long-press event
* @param x X coordinate
* @param y Y coordinate
* @param delay Long-press duration in milliseconds
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    var result = press(100, 100, 5000);
    if (result) {
        logd("Long press succeeded");
    } else {
        logd("Long press failed");
    }
}

main();
```

## Multi-touch {#多点触摸}

### multiTouch Multi-touch {#multitouch-多点触摸}

* Multi-touch
* Touch parameters: action — generally 0 = down, 1 = up, 2 = move, 3 = pause
* x: X coordinate
* y: Y coordinate
* pointer: Which finger touch point (1, 2, 3, etc.)
* delay: How many milliseconds to delay before executing this action
* @param touch1 Touch point array for finger 1, for example:
 ```[{"action":0,"x":1,"y":1,"pointer":1,"delay":20},{"action":2,"x":1,"y":1,"pointer":1,"delay":20}]```
* @param touch2 Touch point array for finger 2
* @param touch3 Touch point array for finger 3
* @param touch4 Touch point array for finger 4
* @param touch5 Touch point array for finger 5
* @param timeout Total multi-touch execution timeout in milliseconds
* @return `{boolean}`

```javascript showLineNumbers
function main() {
 // First style: array-based
 var touch1 = [
 {"action": 0, "x": 500, "y": 1200, "pointer": 1, "delay": 1},
 {"action": 2, "x": 500, "y": 1100, "pointer": 1, "delay": 20},
 {"action": 2, "x": 500, "y": 1000, "pointer": 1, "delay": 20},
 {"action": 1, "x": 1, "y": 1, "pointer": 1, "delay": 20}
 ]

 // Second style: chained calls
 var touch1 = MultiPoint
 .get()
 .action(0).x(500).y(1200).pointer(1).delay(1)
 .next()
 .action(2).x(500).y(1100).pointer(1).delay(1)
 .next()
 .action(2).x(500).y(1000).pointer(1).delay(1)
 .next()
 .action(2).x(500).y(900).pointer(1).delay(1)
 .next()
 .action(1).x(500).y(800).pointer(1).delay(1);
 var touch2 = MultiPoint
 .get()
 .action(0).x(300).y(1200).pointer(2).delay(1)
 .next()
 .action(2).x(300).y(1100).pointer(2).delay(1)
 .next()
 .action(2).x(300).y(1000).pointer(2).delay(1)
 .next()
 .action(2).x(300).y(900).pointer(2).delay(1)
 .next()
 .action(1).x(300).y(800).pointer(2).delay(1);
 var x = multiTouch(touch1, touch2, null, null, null, 30000);
 logd("xxs " + x);
}

main();
```

## Swipe Functions {#滑动函数}

### swipeToPoint Swipe between coordinate points {#swipetopoint-坐标点滑动}

* Swipe from one coordinate to another
* @param startX Start X coordinate
* @param startY Start Y coordinate
* @param endX End X coordinate
* @param endY End Y coordinate
* @param duration Duration in milliseconds
* @return `{boolean}` true if swipe succeeded, false if swipe failed

```javascript showLineNumbers
function main() {
 var result = swipeToPoint(10, 10, 100, 100, 200);
 if (result) {
 logd("Swipe succeeded");
 } else {
 logd("Swipe failed");
 }
}

main();
```

### swipeToPointPressure Swipe between points with pressure {#swipetopointpressure-带压力坐标点滑动}

* Swipe from one coordinate to another
* Supported in EC standalone 2.1.0+
* @param startX Start X coordinate
* @param startY Start Y coordinate
* @param endX End X coordinate
* @param endY End Y coordinate
* @param duration Duration in milliseconds
* @param pressure Pressure, between 0 and 1
* @return true if swipe succeeded, false if swipe failed

```javascript showLineNumbers
function main() {
 var result = swipeToPointPressure(10, 10, 100, 100, 200, 0.2);
 if (result) {
 logd("Swipe succeeded");
 } else {
 logd("Swipe failed");
 }
}

main();
```

## Input Data {#输入数据}

### inputText Input text {#inputtext-输入数据}

* Enter text
* @param content Content
* @param duration Execution time in milliseconds
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 var result = inputText("My content", 100);
 if (result) {
 logd("Yes");
 } else {
 logd("No");
 }
}

main();
```

### ioHIDEvent Simulate keyboard {#iohidevent-模拟键盘}

* Simulate human–machine interaction, such as keyboard input and shortcuts. See [key values](https://ieasyclick.com/iosdocs/advance/keyboard) for details.
* @param eventPageID Human–machine interaction type
* @param eventUsageID Human–machine interaction value
* @param delay Duration; 0.2 is usually sufficient (there may be latency)
* @return `{boolean}`

```javascript showLineNumbers
function main() {
 let x = ioHIDEvent("0x07", "0x11", 0.2)
 logd(x)

}

main();
```

## Screen Orientation {#屏幕方向}

### setOrientation Set screen orientation {#setorientation-设置屏幕方向}

* Set screen orientation; landscape supports only 90° clockwise rotation
* @param orientation 1 = normal portrait, 2 = 90° clockwise (landscape)
* @return `{boolean}`

```javascript showLineNumbers
function main() {
 let x = setOrientation(1)
 logd(x)

}

main();
```

### getOrientation Get screen orientation {#getorientation-获取屏幕方向}

* Get screen orientation
* @return int | 0 = portrait, 1 = landscape (90° clockwise)

```javascript showLineNumbers
function main() {
 let x = getOrientation()
 logd(x)
}

main();
```

## System Key Functions {#系统按键相关}

### home Go to home screen {#home-Go to home screen}

* Go to home screen
* @return `{boolean}`

```javascript showLineNumbers
function main() {
 var result = home();
 if (result) {
 logd("Success");
 } else {
 logd("Failed");
 }
}

main();
```

### homeScreen Force go to home screen {#homescreen-强制进入主页}

* Force go to home screen
* @return `{boolean}`

```javascript showLineNumbers
function main() {
 var result = homeScreen();
 if (result) {
 logd("Success");
 } else {
 logd("Failed");
 }
}

main();
```

### isLocked Whether screen is locked {#islocked-屏幕是否是锁定状态}

* Whether the screen is locked
* @return `{boolean}`

```javascript showLineNumbers
function main() {
 var result = isLocked();
 if (result) {
 logd("Success");
 } else {
 logd("Failed");
 }
}

main();
```

### lockScreen Lock screen {#lockscreen-锁定屏幕}

* Lock screen
* @return `{boolean}`

```javascript showLineNumbers
function main() {
 var result = lockScreen();
 if (result) {
 logd("Success");
 } else {
 logd("Failed");
 }
}

main();
```

### unlockScreen Unlock screen {#unlockscreen-解锁屏幕}

* Unlock screen; the screen must not have a password, etc.
* @return `{boolean}`

```javascript showLineNumbers
function main() {
 var result = unlockScreen();
 if (result) {
 logd("Success");
 } else {
 logd("Failed");
 }
}

main();
```

### appLaunch Launch app {#applaunch-运行程序}

* Launch an app
* @param bundleId App bundle ID
* @param ignoreState 1 = ignore previous open state and open directly; otherwise use ""
* @return `{boolean}` true on success

```javascript showLineNumbers
function main() {
 var result = appLaunch("com.tencent.xin", "1");
 logd("result " + result);
}

main();
```

### appKillByBundleId Kill app by bundle ID {#appkillbybundleid-杀死程序}

* Kill a process by bundle ID
* @param bundleId App bundle ID
* @param ignoreState 1 = ignore previous open state and kill directly; otherwise use ""
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
 var result = appKillByBundleId("com.tencent.xin", "1");
 if (result) {
 logd("Success");
 } else {
 logd("Failed");
 }
}

main();
```

### setAgentTimeout Set agent request timeout {#setagenttimeout-设置代理请求超时}

* @param envTimeout Automation startup timeout in milliseconds; can be set to 10000–15000
* @param readTimeout Other request timeout in milliseconds; can be set to 2000–5000
* @return `{boolean}` true on success

```javascript showLineNumbers
function main() {
 setAgentTimeout(10000, 3000);
}

main();
```

### setAgentPort Set agent port {#setagentport-设置代理运行的端口}

* @param port Port as an integer; must be greater than 1024
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 setAgentPort(12008);
}

main();
```

### setComputeMode Set compute mode {#setcomputemode-设置算力模式}

* If you do not understand this function or run into issues, use 2 or the default.
* Set compute mode; default is 2
* @param type: 1 = compute in agent, 2 = compute in app

```javascript showLineNumbers
function main() {
 setComputeMode(2);
}

main();
```

## Control Center Functions {#中控相关函数}

### getCenterTaskInfo Get control center task parameters {#getcentertaskinfo-获取中控任务参数}

* Get task parameter information sent from the control center
* When the control center starts a script, it can configure parameters; use this function to read them in the script
* Requires EC iOS standalone 3.8.0+
* Note: Parameter configuration is required. Read order: per-device config first; if empty, read global config.
* If the returned parameters contain a key like `__from_global__`, the value comes from global config
* @return `{json}` object

```javascript showLineNumbers
function main() {
 let taskInfo = getCenterTaskInfo();
 logd(JSON.stringify(taskInfo))
 if (taskInfo) {
 // Get task parameters
 let value = taskInfo["valueJson"]
 // Get a parameter value, e.g. name
 // let xm = value["name"]
 logd(JSON.stringify(value))
 }
}

main();
```

## Coordinate Conversion {#坐标系转换}

### convertPointToClickable Convert landscape coordinates to portrait click coordinates {#convertpointtoclickable-横屏坐标转竖屏点击坐标}

* Convert landscape coordinates to clickable portrait coordinates
* See the FAQ for when conversion is needed
* @param x Landscape X coordinate
* @param y Portrait Y coordinate
* @returns `{json}` x = converted X, y = converted Y

```javascript showLineNumbers
function main() {
 let d = convertPointToClickable(100, 300);
 logd("x {} y {}", d.x, d.y)
}

main();
```
