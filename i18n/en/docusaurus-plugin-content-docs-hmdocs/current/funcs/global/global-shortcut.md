---
title: Global Shortcut Events
description: EasyClick automation scripts HarmonyOS Next automation Global Shortcut Events resource download
keywords:
 - EasyClick automation scripts HarmonyOS Next automation Global Shortcut Events resource download
 - UI
 - boolean
 - 'null'
 - readAllUIConfig2
 - param
 - bool
 - clickPoint
 - longClickPoint
 - doubleClickPoint
 - press
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

### readAllUIConfig2 Read UI config (new UI) {#readalluiconfig2-读取ui第二种ui}

* Read UI parameter config
* Configure in designer: Control Center → UI Parameters (New)
* Requires EC HarmonyOS Next 1.0.0+
* Note: Requires new UI config. Read order: per-device first, then global if empty.
* If params contain `__from_global__`, the value comes from global config
* @param tmplName Parameter group name
* @param forceGlobal Force global config; true = ignore per-device config
* @return `{json}` JSON data

```javascript showLineNumbers
function main() {
    var result = readAllUIConfig2("UI example", false);
    logd(result);
    logd(JSON.stringify(result));
}

main();
```
- Global config return value
```json
{"__from_global__":true,"Input":"Input content","MultiSelect":["Option 3"],"Dropdown":"Option 1"}
```
- Per-device return value
````json
{"Input":"Input content","MultiSelect":["Option 3"],"Dropdown":"Option 1"}
````
## Click Functions {#点击函数}

### clickPoint Click by coordinates {#clickpoint-坐标点击}

* Click at coordinates
* Requires EC HarmonyOS Next 1.0.0+
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


### longClickPoint Long click by coordinates {#longclickpoint-坐标长点击}

* Long-click at coordinates
* Requires EC HarmonyOS Next 1.0.0+
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
* Requires EC HarmonyOS Next 1.0.0+
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
* @param speed Swipe speed; lower = slower
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



## Drag Functions {#拖动函数}

### drag Drag coordinates {#drag-拖动坐标}

* Drag from one coordinate to another
* @param startX Start X coordinate
* @param startY Start Y coordinate
* @param endX End X coordinate
* @param endY End Y coordinate
* @param speed Swipe speed; lower value = slower
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
* Requires EC HarmonyOS Next 1.0.0+
* **Input method notes**: ASCII characters can use key injection; non-ASCII content is written to the focused field via **clipboard + paste**. Set the device IME to the **system default**. Third-party IMEs (e.g. iFlytek) may only copy to clipboard when the soft keyboard is open — switch back to the system IME. With a Bluetooth HID keyboard the system often skips the soft keyboard and agent input is more stable; for reliable Chinese with a third-party IME, use Bluetooth HID input (e.g. `bleEvent`).
* @param content Content
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 var result = inputText("My content");
 if (result) {
 logd("Yes");
 } else {
 logd("No");
 }
}

main();
```

### combineKeys Combined key input {#combinekeys-组合键输入数据}

* Requires EC HarmonyOS Next 1.0.0+
* See [key codes](/hmdocs/advance/keycode)
* @param key1 Key 1
* @param key2 Key 2; default 0
* @param key3 Key 3; default 0
* @return boolean | true on success, false on failure

```javascript showLineNumbers
function main() {
 var result = combineKeys(2022, 0, 0);
 if (result) {
 logd("Yes");
 } else {
 logd("No");
 }
}

main();
```


## Screen Orientation {#屏幕方向}

### setOrientation Set screen orientation {#setorientation-设置屏幕方向}

* Set orientation; landscape supports 90° clockwise only
* Requires EC HarmonyOS Next 1.0.0+
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
* Requires EC HarmonyOS Next 1.0.0+
* @return int | 1 = portrait, 2 = landscape (90° clockwise))

```javascript showLineNumbers
function main() {
 let x = getOrientation()
 logd(x)
}

main();
```


### lockNode Lock current node {#locknode-锁定当前节点}

* Lock current node; after lock, node info stays stale on UI refresh until releaseNode

```javascript showLineNumbers
function main() {
 logd("Lock node...")
 // Lock node; UI refresh does not change it
 console.time("1")
 lockNode()
 for (let i = 0; i < 10; i++) {
 let n = text("Settings").getOneNodeInfo(1000)
 logd("lock " + n)
 }
 logd("Release node lock...")
 // Release node lock
 releaseNode()
 logd(console.timeEnd("1"))

 console.time("1")
 for (var i = 0; i < 10; i++) {
 let n = text("Settings").getOneNodeInfo(1000)
 logd("unlocked " + n)
 }
 logd(console.timeEnd("1"))
 // Locked fetch is noticeably faster
}

main();
```

### releaseNode Release node lock {#releasenode-释放节点的锁}

* Release node lock; node info updates on next UI refresh

```javascript showLineNumbers
function main() {
 logd("Lock node...")
 // Lock node; UI refresh does not change it
 console.time("1")
 lockNode()
 for (let i = 0; i < 10; i++) {
 let n = text("Settings").getOneNodeInfo(1000)
 logd("lock " + n)
 }
 logd("Release node lock...")
 // Release node lock
 releaseNode()
 logd(console.timeEnd("1"))

 console.time("1")
 for (var i = 0; i < 10; i++) {
 let n = text("Settings").getOneNodeInfo(1000)
 logd("unlocked " + n)
 }
 logd(console.timeEnd("1"))
 // Locked fetch is noticeably faster
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



### lock Lock screen {#lock-锁定屏幕}

* Lock screen
* @return `{null|Boolean}`

```javascript showLineNumbers
function main() {
 var result = lock();
 if (result) {
 logd("Success");
 } else {
 logd("Failed");
 }
}

main();
```

### unlock Unlock screen {#unlock-解锁屏幕}

* Unlock screen; must not have password, etc.
* Simulated swipe; HarmonyOS Next does not have native unlock
* Requires EC HarmonyOS Next 1.0.0+
* @return `{null|Boolean}`

```javascript showLineNumbers
function main() {
 var result = unlock();
 if (result) {
 logd("Success");
 } else {
 logd("Failed");
 }
}

main();
```

### openApp Open app by bundle ID {#openapp-使用bundleid-打开app}

* Open app by bundle ID
* @param bundleId App bundle ID
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
 var result = openApp("com.tencent.wechat");
 if (result) {
 logd("Success");
 } else {
 logd("Failed");
 }
}

main();
```

### openUrl Open URL {#openurl-打开url}

* Open URL
* @param url URL
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
 var r = openUrl("http://baidu.com");
 logd(r)
}

main();
```

### stopApp Stop app by bundle ID {#stopapp-使用bundleid-停止app}

* Stop app by bundle ID
* @param bundleId App bundle ID
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
 var result = stopApp("com.tencent.wechat");
 if (result) {
 logd("Success");
 } else {
 logd("Failed");
 }
}

main();
```


### installApp Install app by path {#installapp-使用-路径-安装app}

* Install app by path
* @param path HAP path on the same PC as the bridge
* @return `{string}` "ok" = success; other string = failure

```javascript showLineNumbers
function main() {
 var result = installApp("c:/a.hap");
 logd("result " + result);
 if (result === "ok") {
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
 var result = uninstallApp("com.test.wechat");
 logd("result " + result);
 if (result === "ok") {
 logd("Success");
 } else {
 logd("Failed");
 }
}

main();
```

## other Functions {#其他函数}
### reconnectUsb Reconnect USB {#reconnectusb-闪断usb}

* Flash-disconnect USB and reconnect (like unplugging cable)
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
 var result = reconnectUsb();
 logd(result);
}

main();
```
### isReleaseIec Whether script is release version {#isreleaseiec-脚本是否是release版本}

* Check whether script is release version
* Requires EC HarmonyOS Next 2.8.0+
* @return `{boolean}` true = release, false = debug

```javascript showLineNumbers
function main() {
 var result = isReleaseIec();
 logd(result)
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



### getLastToast Get toast data {#getlasttoast-获取toast数据}

* Get toast data
* [Automation must be started]
* Requires EC HarmonyOS Next 1.2.0+
* @param timeout Timeout in milliseconds
* @return `{string}` JSON string

```javascript showLineNumbers
function main() {
 let d = getLastToast(5000);
 logd(d);
}

main();
```


## Gallery Operations {#相册操作}

### uploadInsertImage Insert image into gallery {#uploadinsertimage-插入图片到相册}

* Insert image into gallery
* HarmonyOS Next gallery and files are isolated; this pushes to File Manager only
* After push, File Manager opens; use a script to share image/video to Gallery
* Method 1 (single): File Manager → Recent → Share → Save to Gallery
* Method 2 (batch): File Manager → Browse → Download → multi-select → Share → Save to Gallery
* Requires EC HarmonyOS Next 1.0.0+
* @param localPath Local file path on PC
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
 let d = uploadInsertImage("D:/a.jpg");
 logd(d);
}

main();
```

### uploadInsertVideo Insert video into gallery {#uploadinsertvideo-插入视频到相册}

* Insert video into gallery
* HarmonyOS Next gallery and files are isolated; this pushes to File Manager only
* After push, File Manager opens; use a script to share image/video to Gallery
* Method 1 (single): File Manager → Recent → Share → Save to Gallery
* Method 2 (batch): File Manager → Browse → Download → multi-select → Share → Save to Gallery
* Requires EC HarmonyOS Next 1.0.0+
* @param localPath Local file path on PC
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
 let d = uploadInsertVideo("D:/a.mp4");
 logd(d);
}

main();
```

## File Operations {#文件操作}


### pushFile Push file {#pushfile-推送文件}

* Push local PC file to remote device
* Requires EC HarmonyOS Next 1.0.0+
* @param localPath Local file path on PC
* @param remotePath Remote path on device
* @return `{boolean}` true on success, false on failure
```javascript showLineNumbers
function main() {
 // List all files under /
 let d = pushFile("c:\\a.jpg", "/data/local/tmp/");
 logd(d);
}

main();
```

### pullFile Pull file {#pullfile-推送获取文件}

* Pull remote file to local PC
* Requires EC HarmonyOS Next 1.0.0+
* @param localPath Local file path on PC
* @param remotePath Remote path on device
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
 // Pull file to local machine
 let d = pullFile("/data/local/tmp/a.txt", "c:\\bb.txt");
 logd(d);

}

main();
```
