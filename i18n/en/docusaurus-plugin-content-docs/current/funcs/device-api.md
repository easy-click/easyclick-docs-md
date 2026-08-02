---
title: Device Functions
description: EasyClick automation scripts — Android no root device functions
keywords:
 - EasyClick automation scripts Android no root device functions
 - device
 - return
 - getIMEI
 - IMEI
 - Android
 - ID
 - tcDeviceId
 - getScreenWidth
 - getScreenHeight
 - getAndroidId
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
- The device module uses the `device` prefix, e.g. `device.getIMEI()`

## device.tcDeviceId Third-Party Analytics Device ID

* Get the unique device ID defined by TalkingData analytics
* Deprecated in 10.25+; use another approach
* @return `{string}`

```javascript showLineNumbers
function main() {
    var d = device.tcDeviceId();
    // Cast to string when passing to objects, otherwise an error may occur
    d += ""
    toast(d);
}

main();
```

## device.getIMEI Get IMEI

* Do not call on recent Android versions — values may be unavailable
* Get the device IMEI
* @return string

```javascript showLineNumbers
function main() {
    var imei = device.getIMEI();
    toast(imei);
}

main();
```

## device.getScreenWidth Screen Width

* Get screen width
* @return int

```javascript showLineNumbers
function main() {
    var width = device.getScreenWidth();
    toast(width);
}

main();
```

## device.getScreenHeight Screen Height

* Get screen height
* @return int

```javascript showLineNumbers
function main() {
    var height = device.getScreenHeight();
    toast(height);
}

main();
```

## device.getAndroidId Get Android ID

* Get the Android ID
* @return string

```javascript showLineNumbers
function main() {
    var androidId = device.getAndroidId();
    toast(androidId);
}

main();
```

## device.getBrand Get Brand

* Get device brand
* @return string

```javascript showLineNumbers
function main() {
    var brand = device.getBrand();
    toast(brand);
}

main();
```

## device.getModel Get Model

* Get device model
* @return string

```javascript showLineNumbers
function main() {
    var model = device.getModel();
    toast(model);
}

main();
```

## device.getImsi Get SIM Number

* Do not call on recent Android versions — values may be unavailable
* Get the SIM card number
* @return string

```javascript showLineNumbers
function main() {
    var imsi = device.getImsi();
    toast(imsi);
}

main();
```

## device.getSerial Get Serial Number

* Do not call on recent Android versions — values may be unavailable
* Get the device serial number
* @return string

```javascript showLineNumbers
function main() {
    var serial = device.getSerial();
    toast(serial);
}

main();
```

## device.getSdkInt Get SDK Version

* Get the SDK version number, e.g. 23
* @return string

```javascript showLineNumbers
function main() {
    var sdkInt = device.getSdkInt();
    toast(sdkInt);
}

main();
```

## device.getOSVersion Get OS Version

* Get the OS version string, e.g. 6.0
* @return string

```javascript showLineNumbers
function main() {
    var osVersion = device.getOSVersion();
    toast(osVersion);
}

main();
```

## device.getMacAddress Get MAC Address

* Do not call on recent Android versions — may return a fixed or empty value
* Get the MAC address
* @return string

```javascript showLineNumbers
function main() {
    var res = device.getMacAddress();
    toast(res);
}

main();
```

## device.getBattery Get Battery Level

* Get battery level
* @return int

```javascript showLineNumbers
function main() {
    var res = device.getBattery();
    toast(res);
}

main();
```

## device.getTotalMem Get Total Memory

* Get total memory
* @return long

```javascript showLineNumbers
function main() {
    var res = device.getTotalMem();
    toast(res);
}

main();
```

## device.getAvailMem Get Available Memory

* Get available memory
* @return long

```javascript showLineNumbers
function main() {
    var res = device.getAvailMem();
    toast(res);
}

main();
```

## device.isCharging Is Charging

* Check whether the device is charging
* @return boolean

```javascript showLineNumbers
function main() {
    var res = device.isCharging();
    toast(res);
}

main();
```

## device.vibrate Vibrate

* Vibrate for the given duration in milliseconds

```javascript showLineNumbers
function main() {
    device.vibrate(1 * 1000);
}

main();
```

## device.cancelVibration Cancel Vibration

* Cancel vibration

```javascript showLineNumbers
function main() {
    device.cancelVibration();
}

main();
```

## device.keepAwake Keep Device Awake

* Keep the device awake
* @param flag See Android PowerManager wake flags

```javascript showLineNumbers
function main() {
    importClass(android.os.PowerManager)
    device.keepAwake(PowerManager.SCREEN_DIM_WAKE_LOCK | PowerManager.ACQUIRE_CAUSES_WAKEUP);
}

main();
```

## device.keepScreenOn Keep Screen On

* Keep the screen on

```javascript showLineNumbers
function main() {
    device.keepScreenOn();
}

main();
```

## device.keepScreenDim Keep Screen Dim

* Keep the screen in a dim state

```javascript showLineNumbers
function main() {
    device.keepScreenDim();
}

main();
```

## device.cancelKeepingAwake Cancel Keep Awake

* Cancel keep-awake state

```javascript showLineNumbers
function main() {
    device.cancelKeepingAwake();
}

main();
```
