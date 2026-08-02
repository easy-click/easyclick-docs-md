---
title: EasyClick Automation Scripts — iOS Scripts — iOS No Jailbreak — iOS No Hardware — Device Functions
hide_title: false
hide_table_of_contents: false
sidebar_label: Device Functions
description: EasyClick automation scripts — iOS no jailbreak — device functions
keywords:
 - EasyClick automation scripts
 - iOS scripts
 - iOS no jailbreak
 - iOS no hardware
 - device functions
 - device
 - ID
 - iOS
 - Ecid
 - EC
 - iostjdocs
 - zh
 - cn
 - advance
 - tjcenter
 - EasyClick
 - mobile automation
 - test automation
---

# Device Functions

## Overview

- Device module functions are related to device information
- The device module uses the `device` prefix

## device.getDeviceId Get Device ID

* Get the device ID
* Available in EC standalone 2.0.0+; requires the standalone activator to obtain the device ID correctly
* See [Advanced — Standalone Activator Tutorial](/iostjdocs/advance/tjcenter)
* @return string

```javascript showLineNumbers
function main() {
    var xx = device.getDeviceId();
    logd(xx);
}

main();
```

## device.getEcid Get ECID

* Get ECID
* Available in EC standalone 3.11.0+; requires the standalone activator to obtain the device ID correctly
* See [Advanced — Standalone Activator Tutorial](/iostjdocs/advance/tjcenter)
* @return string

```javascript showLineNumbers
function main() {
    var xx = device.getEcid();
    logd(xx);
}

main();
```

## device.getSerialNo Get Serial Number

* Get serial number
* Available in EC standalone 3.11.0+; requires the standalone activator to obtain the device ID correctly
* See [Advanced — Standalone Activator Tutorial](/iostjdocs/advance/tjcenter)
* @return string

```javascript showLineNumbers
function main() {
    var xx = device.getSerialNo();
    logd(xx);
}

main();
```

## device.getDeviceName Get Device Name

* Get device name (the phone's name)
* @return string

```javascript showLineNumbers
function main() {
    var xx = device.getDeviceName();
    logd(xx);
}

main();
```

## device.getDeviceName2 Get Device Name 2

* Get device name; on iOS 16+ the standard method may fail — use this function instead
* Available in EC standalone 2.0.0+; requires the standalone activator to obtain the device name correctly
* See [Advanced — Standalone Activator Tutorial](/iostjdocs/advance/tjcenter)
* @return string

```javascript showLineNumbers
function main() {
    var xx = device.getDeviceName2();
    logd(xx);
}

main();
```

## device.getScreenWidthHeight Screen Width and Height

* Screen width and height
* @return integer

```javascript showLineNumbers
function main() {
    let aa = device.getScreenWidthHeight()
    logd("getScreenWidthHeight " + aa)
    let bb = aa.split(",")
    logd("width " + bb[0])
    logd("height " + bb[1])
}

main();
```

## device.getScreenWidth Screen Width

* [Deprecated]
* Get screen width
* @return integer

```javascript showLineNumbers
function main() {
    var width = device.getScreenWidth();
    logd(width);
}

main();
```

## device.getScreenHeight Screen Height

* [Deprecated]
* Get screen height
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

* Get phone model
* @return string

```javascript showLineNumbers
function main() {
    var model = device.getModel();
    logd(model);
}

main();
```

## device.getOSVersion Get OS Version

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
* @return int

```javascript showLineNumbers
function main() {
    var res = device.getBattery();
    logd(res);
}

main();
```

## device.isCharging Is Charging

* Whether the device is charging
* @return boolean

```javascript showLineNumbers
function main() {
    var res = device.isCharging();
    logd(res);
}

main();
```

