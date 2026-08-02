---
title: Device Functions
description: EasyClick automation scripts — HarmonyOS Next automation device functions
keywords:
 - EasyClick automation scripts HarmonyOS Next automation device functions
 - device
 - return
 - id
 - EC
 - Next
 - 1.0.0
 - getDeviceInfo
 - getDeviceId
 - getDeviceAlias
 - getScreenState
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

## device.getDeviceInfo Get Device Information

* Get device information
* Requires EC HarmonyOS Next 1.0.0+
* Example:

```json showLineNumbers
{
  "screenWidth": "1224",
  "connectType": "USB",
  "deviceAlias": "",
  "groupId": "",
  "screenHeight": "2776",
  "deviceNo": "f615f3adccbe993fd02a33a93d3f5af7c64faf102039bb4f5756a474fdd62bac",
  "deviceId": "f615f3adccbe993fd02a33a93d3f5af7c64faf102039bb4f5756a474fdd62bac",
  "productName": "nova 12 Ultra",
  "serialNo": "2UCUT23C23001051",
  "groupName": "",
  "productVersion": "ADL-AL00U 5.0.0.112(SP1C00E110R1P12)",
  "port": "8026",
  "online": "1",
  "model": "ADA-AL00U",
  "sdkVersion": "13",
  "brand": "HUAWEI"
}
```

* screenWidth: screen width
* screenHeight: screen height
* deviceId: device ID
* serialNo: device serial number
* deviceName: device name
* productVersion: device version
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
* Requires EC HarmonyOS Next 1.0.0+
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

## device.getScreenState Get Screen State

* Get screen state
* Requires EC HarmonyOS Next 1.0.0+
* @return `{string}` e.g. INACTIVE, SLEEP, AWAKE

```javascript showLineNumbers
function main() {
  var xx = device.getScreenState();
  logd(xx);
}

main();
```

## device.getSerialNo Get Device Serial Number

* Get device serial number
* @return string

```javascript showLineNumbers
function main() {
  var xx = device.getSerialNo();
  logd(xx);
}

main();
```

## device.applist Get Installed App List

* Get the list of installed apps on the current device
* @return `{string}` package names separated by commas

```javascript showLineNumbers
function main() {
    let applist = device.applist();
    if (applist) {
        let applist_arr = applist.split(",")
        for (let i = 0; i < applist_arr.length; i++) {
            logd(applist_arr[i])
        }
    }
}

main();
```

## device.getScreenSize Get Screen Width and Height

* Get screen width and height
* @return `{string}` e.g. 750,1334

```javascript showLineNumbers
function main() {
  var width = device.getScreenSize();
  logd(width);
}

main();
```

## device.getModel Get Model

* Get device model
* @return string

```javascript showLineNumbers
function main() {
  var model = device.getModel();
  logd(model);
}

main();
```

## device.getSdkVersion Get SDK Version

* Get SDK version string, e.g. 10
* @return string

```javascript showLineNumbers
function main() {
  var osVersion = device.getSdkVersion();
  logd(osVersion);
}

main();
```

## device.getBattery Get Battery Level

* Get battery level
* @return `{int}` 1 - 100

```javascript showLineNumbers
function main() {
  var res = device.getBattery();
  logd(res);
}

main();
```

## device.isCharging Is Charging

* Check whether the device is charging
* @return boolean

```javascript showLineNumbers
function main() {
  var res = device.isCharging();
  logd(res);
}

main();
```
