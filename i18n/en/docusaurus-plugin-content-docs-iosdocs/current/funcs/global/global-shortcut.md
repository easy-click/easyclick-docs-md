---
title: Global Shortcut Events
description: EasyClick automation scripts iOS no jailbreak global shortcut events resource download
keywords:
 - EasyClick automation scripts iOS no jailbreak global shortcut events resource download
 - UI
 - param
 - return
 - readAllUIConfig
 - readAllUIConfig2
 - clickPoint
 - clickPointPressure
 - tmplName
 - JSON
 - longClickPoint
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview {#说明}

Shortcut events encapsulated in the global module.

## Read UI {#读取ui}

### readAllUIConfig Read UI {#readalluiconfig-读取ui}

* Read all UI config
* @param tmplName UI template file name
* @return `{json}` JSON data

```javascript showLineNumbers
function main() {
    var result = readAllUIConfig("Douyin template");
    logd(result);
    logd(JSON.stringify(result));
}

main();
```

### readAllUIConfig2 Read UI config (new UI) {#readalluiconfig2-读取ui第二种ui}

* Read UI parameter config
* Configure in designer: Control Center → UI Parameters (New)
* Requires EC iOS USB 6.28.0++
* Note: Requires new UI config. Read order: per-device first, then global if empty.
* If params contain `__from_global__`, the value comes from global config
* @param tmplName Parameter group name
* @param forceGlobal Force global config; true = ignore per-device config
* @return `{json}` JSON data

```javascript showLineNumbers
function main() {
    var result = readAllUIConfig2("Douyin template", false);
    logd(result);
    logd(JSON.stringify(result));
}

main();
```

## Click Functions {#点击函数}

### clickPoint Click by coordinates {#clickpoint-坐标点击}

* Click at coordinates
* @param x X coordinate
* @param y Y coordinate
* @return `{boolean}`

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

### clickPointPressure Click coordinates with pressure {#clickpointpressure-带压力点击坐标}

* Click coordinates with pressure
* Requires EC iOS 6.4.0+
* @param x X coordinate
* @param y Y coordinate
* @param pressure Pressure value in range 0–1
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

### longClickPoint Long click by coordinates {#longclickpoint-坐标长点击}

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
* Touch params: action — 0 = down, 1 = up, 2 = move,3 = pause
* x: X coordinate
* y: Y coordinate
* pointer: finger index (1, 2, 3, … for nth finger)
* delay: delay in ms before this action runs
* @param touch1
 First finger touch point array, e.g.:
 ```[{"action":0,"x":1,"y":1,"pointer":1,"delay":20},{"action":2,"x":1,"y":1,"pointer":1,"delay":20}]```
* @param touch2 Second finger touch point array
* @param touch3 Third finger touch point array
* @param touch4 Fourth finger touch point array
* @param touch5 Fifth finger touch point array
* @param timeout Multi-touch total timeout in milliseconds
* @return `{boolean}`

```javascript showLineNumbers
function main() {
 // First style: array-based
 var touch1 = [
 {"action": 0, "x": 500, "y": 1200, "pointer": 1, "delay": 1},
 {"action": 2, "x": 500, "y": 1100, "pointer": 1, "delay": 20},
 {"action": 2, "x": 500, "y": 1000, "pointer": 1, "delay": 20},
 {"action": 1, "x": 1, "y": 1, "pointer": 1, "delay": 2}
 ]
 // Second style: chained calls
 var touch1 = MultiPoint
 .get()
 .action(0).x(500).y(1200).pointer(1).delay(100)
 .next()
 .action(2).x(500).y(1100).pointer(1).delay(100)
 .next()
 .action(2).x(500).y(1000).pointer(1).delay(100)
 .next()
 .action(2).x(500).y(900).pointer(1).delay(100)
 .next()
 .action(1).x(500).y(800).pointer(1).delay(100);
 var touch2 = MultiPoint
 .get()
 .action(0).x(300).y(1200).pointer(2).delay(100)
 .next()
 .action(2).x(300).y(1100).pointer(2).delay(100)
 .next()
 .action(2).x(300).y(1000).pointer(2).delay(100)
 .next()
 .action(2).x(300).y(900).pointer(2).delay(100)
 .next()
 .action(1).x(300).y(800).pointer(2).delay(100);
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
* @return Boolean true Swipe succeeded, false Swipe failed

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
* Requires EC control center 6.4.0+
* @param startX Start X coordinate
* @param startY Start Y coordinate
* @param endX End X coordinate
* @param endY End Y coordinate
* @param duration Duration in milliseconds
* @param pressure pressure 0–1
* @return Boolean true Swipe succeeded, false Swipe failed

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

## Drag Functions {#拖动函数}

### drag Drag coordinates {#drag-拖动坐标}

* Drag from one coordinate to another
* @param startX Start X coordinate
* @param startY Start Y coordinate
* @param endX End X coordinate
* @param endY End Y coordinate
* @param duration Duration in milliseconds
* @return Boolean true Drag succeeded, false Drag failed

```javascript showLineNumbers
function main() {
 var result = drag(10, 10, 100, 100, 200);
 if (result) {
 logd("Drag succeeded");
 } else {
 logd("Drag failed");
 }
}

main();
```

## Input Data {#输入数据}

### inputText Input text {#inputtext-输入数据}

* Enter text
* @param content Content
* @param duration execution time in milliseconds
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

### typingText Input text {#typingtext-输入数据}

* Enter text; simulates typing
* EC IOS 6.1.0+
* @param content Content
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 var result = typingText("My content");
 if (result) {
 logd("Yes");
 } else {
 logd("No");
 }
}

main();
```

### ioHIDEvent Simulate keyboard {#iohidevent-模拟键盘}

* Simulate HID events, e.g. keyboard and shortcuts; see key values at
* <a href="https://ieasyclick.com/iosdocs/advance/keyboard">https://ieasyclick.com/iosdocs/advance/keyboard</a>
* @param eventPageID HID type
* @param eventUsageID HID usage
* @param delay typically 0.2; may have delay
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

* Set orientation; landscape supports 90° clockwise only
* Requires EC iOS control center 3.0.0+
* @param orientation 1 = portrait, 2 = 90° clockwise landscape
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
* Requires EC iOS control center 3.0.0+
* @return int | 0 = portrait, 1 = landscape (90° clockwise))

```javascript showLineNumbers
function main() {
 let x = getOrientation()
 logd(x)
}

main();
```

### adjustScreenOrientation Adjust screen orientation {#adjustscreenorientation-校正屏幕方向}

* Adjust screen orientation and coordinate system
* Requires EC iOS control center 3.0.0+
* @param orientation 0 = auto; 1 = forced portrait; 2 = forced 90° clockwise landscape
* @return JSON string,keys: orientation, screenWidth, screenHeight

```javascript showLineNumbers
function main() {
 logd(setOrientation(1))
 sleep(1000)
 logd(getOrientation())
 logd("adjustScreenOrientation {}", adjustScreenOrientation(0))
}

main();
```

## System Key Functions {#系统按键相关}

### home Go to home screen {#home-返回主页}

* Go to home screen
* @return `{null|Boolean}`

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

### reboot Reboot device {#reboot-重启设备}

* Reboot device
* Requires EC iOS 3.5.0+
* @return `{null|Boolean}`

```javascript showLineNumbers
function main() {
 var result = reboot();
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
* Requires EC iOS control center 3.0.0+
* @return `{null|Boolean}`

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

* Whether screen is locked
* Requires EC iOS control center 3.0.0+
* @return `{null|Boolean}`

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
* Requires EC iOS control center 3.0.0+
* @return `{null|Boolean}`

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

* Unlock screen; must not have password, etc.
* Requires EC iOS control center 3.0.0+
* @return `{null|Boolean}`

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

### openApp Open app by bundle ID {#openapp-使用bundleid-打开app}

* Open app by bundle ID; unlike appLaunch, uses command channel
* @param bundleId App bundle ID
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
 var result = openApp("com.tencent.xin");
 if (result) {
 logd("Success");
 } else {
 logd("Failed");
 }
}

main();
```

### openUrl Open URL {#openurl-打开url}

* Open URL; call takeMeToFront first to bring this app to foreground
* @param url URL
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
 takeMeToFront()
 sleep(1000)
 var r = openUrl("http://baidu.com");
 logd(r)
}

main();
```

### stopApp Stop app by bundle ID {#stopapp-使用bundleid-停止app}

* Stop app by bundle ID; unlike appKillByBundleId, uses command channel
* @param bundleId App bundle ID
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
 var result = stopApp("com.tencent.xin");
 if (result) {
 logd("Success");
 } else {
 logd("Failed");
 }
}

main();
```

### appLaunchByPrefix Launch app by prefix {#applaunchbyprefix-按前缀launch-app}

* Find and launch app by bundle ID prefix
* @param bundleIdPrefix App bundle ID prefix; comma-separated
* @return `{boolean}` true on success

```javascript showLineNumbers
function main() {
 var result = appLaunchByPrefix("com.tencent.xin,123");
 if (result) {
 logd("Success");
 } else {
 logd("Failed");
 }
}

main();
```

### appLaunch Launch app {#applaunch-运行程序}

* Launch app
* Automation not required
* @param bundleId App bundle ID
* @return `{int}` process ID

```javascript showLineNumbers
function main() {
 var result = appLaunch("com.tencent.xin");
 if (result > 0) {
 logd("Success");
 } else {
 logd("Failed");
 }
}

main();
```

### appLaunchEx Open an app {#applaunchex-打开一个app}

* Open an app
* Requires EC iOS USB 6.25.0+
* Automation required
* @param bundleId App bundle ID
* @param ignoreState 1 = ignore previous state and open directly; otherwise ""
* @return `{boolean}` true on success

```javascript showLineNumbers
function main() {
 var result = appLaunchEx("com.tencent.xin", "1");
 if (result) {
 logd("Success");
 } else {
 logd("Failed");
 }
}

main();
```

### appKillByBundleId Kill app by bundle ID {#appkillbybundleid-杀死程序}

* Kill process by bundle ID
* Automation not required
* @param bundleId App bundle ID
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
 var result = appKillByBundleId("com.tencent.xin");
 if (result) {
 logd("Success");
 } else {
 logd("Failed");
 }
}

main();
```

### appKillByBundleIdEx Kill app by bundle ID {#appkillbybundleidex-杀死程序}

* Kill process by bundle ID
* Requires EC iOS USB 6.25.0+
* Automation required
* @param bundleId App bundle ID
* @param ignoreState 1 = ignore previous state and kill process; otherwise ""
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
 var result = appKillByBundleIdEx("com.tencent.xin", "1");
 if (result) {
 logd("Success");
 } else {
 logd("Failed");
 }
}

main();
```

### installApp Install app by path {#installapp-使用-路径-安装app}

* Install app by path (automation not required)
* @param bundleId App bundle ID
* @param path IPA path on same PC as bridge
* @return `{string}` "ok" = success; other string = failure

```javascript showLineNumbers
function main() {
 var result = installApp("com.test.xin", "c:/a.ipa");
 logd("result " + result);
 if (result == "ok") {
 logd("Success");
 } else {
 logd("Failed");
 }
}

main();
```

### uninstallApp Uninstall app by bundle ID {#uninstallapp-使用bundleid-卸载app}

* Uninstall app by bundle ID (automation not required)
* @param bundleId App bundle ID
* @return `{string}` "ok" = success; other string = failure

```javascript showLineNumbers
function main() {
 var result = uninstallApp("com.test.xin");
 logd("result " + result);
 if (result == "ok") {
 logd("Success");
 } else {
 logd("Failed");
 }
}

main();
```

## other Functions {#其他函数}

### setAssistiveTouch AssistiveTouch toggle {#setassistivetouch-悬浮球开关}

* Toggle AssistiveTouch floating ball
* Requires EC IOS 6.0.0+
* @param open true = show, false = hide
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
 var result = setAssistiveTouch(true);
 logd(result);
}

main();
```

### resetUsbConn Reset USB connection {#resetusbconn-重置usb链接}

* Reset USB connection; try when automation is enabled
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
 var result = resetUsbConn();
 logd(result);
}

main();
```

### reconnectUsb Reconnect USB {#reconnectusb-闪断usb}

* Flash-disconnect USB and reconnect (like unplugging cable)
* Requires EC 7.2.0+
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
 var result = reconnectUsb();
 logd(result);
}

main();
```

### setAgentSetting Set agent program config {#setagentsetting-设置代理程序的配置}

* Set agent program config
* @param ext is a map,e.g. ```{"screenStreamQuality":100}```
* screenStreamQuality: screen mirroring quality 1–100
* screenStreamFramerate: screen mirroring framerate 10–60
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 // Omit map keys for properties you do not want to set
 var result = setAgentSetting({"screenStreamQuality": 60, "screenStreamFramerate": 20});
 logd(result);
}

main();
```

### setAgentTimeout Set agent request timeout {#setagenttimeout-设置代理请求超时}

* @param envTimeout Automation startup timeout (ms); 10000–15000
* @param readTimeout other request timeout (ms); 2000–5000
* @return `{boolean}` true on success

```javascript showLineNumbers
function main() {
 setAgentTimeout(10000, 3000);
}

main();
```

### activeAppInfo Current running app bundle ID {#activeappinfo-当前运行的程序-bundleid}

* @param Current running app bundleId
* @return `{string}` current running app bundleId

```javascript showLineNumbers
function main() {
 let d = activeAppInfo();
 logd(d);
}

main();
```


### erasePhone Factory reset {#erasephone-恢复出厂设置}

* Factory reset (wipe device)
* Requires EC iOS USB 8.10.0+
* Use with caution; wipes device data and reboots automatically
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
 let d = erasePhone();
 logd(d);
}

main();
```


## Gallery Operations {#相册操作}

### uploadInsertImage Insert image into gallery {#uploadinsertimage-插入图片到相册}

* Insert image into gallery via agent IPA
* Requires EC IOS 6.5.0 +
* @param path image path
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
 let d = uploadInsertImage("D:/a.jpg");
 logd(d);
}

main();
```

### uploadInsertVideo Insert video into gallery {#uploadinsertvideo-插入视频到相册}

* Insert video into gallery via agent IPA
* Requires EC IOS 6.5.0 +
* @param path video path
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
 let d = uploadInsertVideo("D:/a.mp4");
 logd(d);
}

main();
```

## File Operations {#文件操作}

:::tip
Due to iOS sandbox isolation,file operations are limited,to app-shared or system directories only,e.g. /Downloads, /DCIM
:::

### fsyncFileOpr File operations on iOS device {#fsyncfileopr-ios设备中文件操作}

* File operations on iOS device
* Requires EC 7.2.0+
* @param action Actions: list= list files or folders,rm= delete file or folder,mkdir = create folder
* @param path file path
* @return `{JSON}` code=0 = success; data = payload; when action=list, data is an array with file path, size, etc.

```javascript showLineNumbers
function main() {
 // List all files under /
 let d = fsyncFileOpr("list", "/");
 logd(JSON.stringify(d));
 // List all files under /Downloads
 d = fsyncFileOpr("list", "/Downloads");
 logd(JSON.stringify(d));


 // Create folder
 d = fsyncFileOpr("mkdir", "/Downloads/123");
 logd(JSON.stringify(d));

 // Delete file or folder
 d = fsyncFileOpr("rm", "/Downloads/123.txt");
 logd(JSON.stringify(d));
}

main();
```

### fsyncFilePushPull Pull file {#fsyncfilepushpull-推送获取文件}

* Push/pull files to/from iOS device
* Requires EC 7.2.0+
* @param action Actions: push = push file from PC to remote device,pull = pull file from device to PC
* @param srcPath Source path; for push = PC path; for pull = iOS path
* @param destPath Destination path; for push = iOS path; for pull = PC path
* @return `{json}` code=0 means success

```javascript showLineNumbers
function main() {
 // Push C:/a.txt to iOS device at /Downloads/a.txt
 let d = fsyncFilePushPull("push", "C:/a.txt", "/Downloads/a.txt");
 logd(JSON.stringify(d));
 // Pull file to local machine
 d = fsyncFilePushPull("pull", "/Downloads/a.txt", "c:/bb.txt");
 logd(JSON.stringify(d));

}

main();
```
