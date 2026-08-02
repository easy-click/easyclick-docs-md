---
title: Device Functions
description: EasyClick automation scripts — iOS no jailbreak device functions
keywords:
 - EasyClick automation scripts iOS no jailbreak device functions
 - device
 - EC
 - iOS
 - getDeviceMsg
 - getOrientationNoAuto
 - getDeviceInfo
 - getScreenWidth
 - USB
 - 9.20.0
 - returns
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview

- Device module functions provide device information
- The device module uses the `device` prefix

## device.getDeviceMsg Get All Runtime Information for the Current Device

* Get all runtime information for the current device
* Supported EC iOS USB 9.20.0+
* @returns `{string}` JSON string

```javascript showLineNumbers
function main() {
    var xx = device.getDeviceMsg();
    logd(xx);
}

main();
```

## device.getOrientationNoAuto Get Screen Orientation

* Get screen orientation
* Does not require the automation service
* Supported EC iOS USB 9.20.0+
* @returns `{*|number}` 1 portrait, 2 landscape

```javascript showLineNumbers
function main() {
    var xx = device.getOrientationNoAuto();
    logd(xx);
}

main();
```

## device.getDeviceInfo Get Device Information

* Get device information
* Supported EC iOS 3.2.0+
* Returns JSON
* orientation: orientation — 1 portrait, 2 landscape
* screenWidth: screen width
* screenHeight: screen height
* orientationClick: current coordinate system orientation — 1 portrait, 2 landscape
* deviceId: device ID
* serialNo: device serial number
* deviceName: device name
* productVersion: device OS version
* model: device model
* @return JSON string

```javascript showLineNumbers
function main() {
    var xx = device.getDeviceInfo();
    logd(xx);
}

main();
```

## device.getDeviceId Get ID

* Get the device ID
* @return string

```javascript showLineNumbers
function main() {
    var xx = device.getDeviceId();
    logd(xx);
}

main();
```

## device.getDeviceAlias Get Control Center Device Alias

* Get the control center device alias
* @return string

```javascript showLineNumbers
function main() {
    var xx = device.getDeviceAlias();
    logd(xx);
}

main();
```

## device.getSerialNo Get Device Serial Number

* Get the device serial number (visible in the device Settings app)
* @return string

```javascript showLineNumbers
function main() {
    var xx = device.getSerialNo();
    logd(xx);
}

main();
```

## device.getDeviceName Get Device Name

* Get the device name (the name shown for the phone)
* @return string

```javascript showLineNumbers
function main() {
    var xx = device.getDeviceName();
    logd(xx);
}

main();
```

## device.applist Get Installed App List

* Get the list of apps installed on the current device
* @return `{string}` JSON string

```javascript showLineNumbers
function main() {
    var applistx = device.applist();
    logd(applistx);
}

main();
```

## device.getScreenWidthHeightText Get Screen Width and Height

* Get screen width and height
* @return `{string}` e.g. 750,1334

```javascript showLineNumbers
function main() {
    var wh = device.getScreenWidthHeightText();
    logd(wh);
}

main();
```

## device.getScreenWidth Screen Width

* Get screen width
* [Deprecated] Has boundary issues; use getScreenWidthHeightText instead
* @return integer

```javascript showLineNumbers
function main() {
    var width = device.getScreenWidth();
    logd(width);
}

main();
```

## device.getScreenWidth Screen Width

* Get screen width
* [Deprecated] Has boundary issues; use getScreenWidthHeightText instead
* @return integer

```javascript showLineNumbers
function main() {
    var width = device.getScreenWidth();
    logd(width);
}

main();
```

## device.getScreenHeight Screen Height

* Get screen height
* [Deprecated] Has boundary issues; use getScreenWidthHeightText instead
* @return integer

```javascript showLineNumbers
function main() {
    var height = device.getScreenHeight();
    logd(height);
}

main();
```

## device.getScale Screen Scale Factor

* Screen scale factor
* @return `{float}`

```javascript showLineNumbers
function main() {
    var d = device.getScale();
    logd(d);
}

main();
```

## device.getModel Get Device Model

* Get the device model
* @return string

```javascript showLineNumbers
function main() {
    var model = device.getModel();
    logd(model);
}

main();
```

## device.getOSVersion Get OS Version

* Get the device OS version
* @return string

```javascript showLineNumbers
function main() {
    var osVersion = device.getOSVersion();
    logd(osVersion);
}

main();
```

## device.getBattery Get Battery Level

* Get battery level
* @return integer

```javascript showLineNumbers
function main() {
    var res = device.getBattery();
    logd(res);
}

main();
```

## device.isCharging Check Whether Charging

* Check whether the device is charging
* @return boolean

```javascript showLineNumbers
function main() {
    var res = device.isCharging();
    logd(res);
}

main();
```
