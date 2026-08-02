---
title: ADB Functions
description: EasyClick automation scripts — Android no root ADB interaction module
keywords:
 - EasyClick automation scripts Android no root ADB interaction module
 - adbClient
 - adb
 - ADB
 - connectHistory
 - https
 - ieasyclick
 - com
 - startScan
 - stopScan
 - closeAdbConnect
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview

- The ADB interaction module connects to ADB over wireless debugging and executes various ADB commands
- No USB cable is required

:::tip
- On devices below Android 11, enable network debugging from a PC with `adb tcpip 5555`
 - If you cannot download ADB to enable network debugging
 - Use this page to enable it online --> [https://adb.ieasyclick.com/](https://adb.ieasyclick.com/)
- On Android 11+, go to Settings → Developer options → Wireless debugging → Pair device with pairing code
- Pairing is required; use the app system settings ADB wireless debugging feature, follow the prompts to scan and connect, then use functions to run other ADB commands
- Android 11+ uses pairing; below Android 11 uses scan-and-authorize
- After connecting or pairing once, `connectHistory` is usually enough for scripts without re-pairing
:::

## adbClient.connectHistory Connect History

* Connect using a previous session
* Requires EC Android 11.33.0+
* @param timeout Timeout in milliseconds
* @returns `{*|boolean}` true if connected, false on failure

```javascript showLineNumbers
function testadb() {

 adbClient.closeAdbConnect();

 let connected = adbClient.connectHistory(10000)
 logd("connectHistory " + connected)
 if (connected) {
 logd("adb connected")
 let lsr = adbClient.runShell("ls -alh /sdcard/", 30000)
 // Reboot
 //adbClient.runShell("reboot",10000)
 logd(lsr)
 startEnvNow();
 return;
 }


 let scanResult = "";

 if (device.getSdkInt() > 29) {
 scanResult = adbClient.startScan(0, 0, 60 * 1000)
 } else {
 logd("Starting scan... if a permission dialog appears on the phone, allow it")
 scanResult = adbClient.startScan(5555, 6000, 60 * 1000)
 }
 logd("Scan result: " + scanResult)
 if (scanResult == null || scanResult == undefined || scanResult == "") {
 logd("Scan returned no data")
 return;
 }
 let scr = JSON.parse(scanResult)
 if (scr["code"] != 0) {
 // No scan data
 logd("Scan error: " + scr["msg"])
 return;
 }

 logd("adb connected successfully")
 startEnvNow();
}

function startEnvNow() {
 if (!adbClient.isAdbConnected()) {
 logw("adb not connected, please reconnect")
 return false;
 }
 let act = adbClient.activeSelf(1, 20000)
 logd("Self-activation result: " + act)
 if (act == null || act == undefined || act == "") {
 logd("Self-activation returned no data")
 return false;
 }
 let act2 = JSON.parse(act)
 if (act2["code"] != 0) {
 logd("Self-activation error: " + act2["msg"])
 return false;
 }
 logd("Starting automation environment")
 let str = startEnv()
 logd("Automation environment started: " + str)
 return str;
}


function main() {

 testadb();

 //logd(adbClient.openNotificationPermissionPage())

}


main();

```

## adbClient.startScan Start Scan

* Start scanning
* On Android 11+, leave port parameters empty for pairing mode; after scanning, open wireless debugging and use "Pair device with pairing code"; a notification will prompt for the pairing code
* Below Android 11, set start and end ports; scanning is automatic (port 5555 is common); allow the authorization dialog when prompted
* If it fails, retry. Prefer completing ADB wireless debugging pairing in app system settings first, so you may not need this function
* Requires EC Android 11.33.0+
* @param startPort Network debugging start port
* @param endPort Network debugging end port
* @param timeout Timeout in milliseconds
* @returns `{string}` JSON string; code=0 means success, data is returned data, msg is the message

```javascript showLineNumbers
    // See the adbClient.connectHistory example
```

## adbClient.stopScan Stop Scan

* Stop scanning
* Requires EC Android 11.33.0+
* @returns `{*|boolean}` true on success, false on failure

```javascript showLineNumbers
    // See the adbClient.connectHistory example
```

## adbClient.closeAdbConnect Close ADB Connection

* Close the ADB connection
* Requires EC Android 11.33.0+
* @returns `{*|boolean}` true on success, false on failure

```javascript showLineNumbers
    // See the adbClient.connectHistory example
```

## adbClient.isAdbConnected Check ADB Connection

* Check whether ADB is connected
* Requires EC Android 11.33.0+
* @returns `{*|boolean}` true if connected, false otherwise

```javascript showLineNumbers
    // See the adbClient.connectHistory example
```

## adbClient.runShell Run Shell Command

* Run a shell command
* Requires EC Android 11.33.0+
* @param command Command, e.g. `ls /sdcard/` or `pm install /sdcard/xxx.apk`
* @param timeout Timeout in milliseconds
* @returns `{string}` JSON string; code=0 means success, data is returned data, msg is the message

```javascript showLineNumbers
    // See the adbClient.connectHistory example
```

## adbClient.activeSelf Activate Self

* Activate the app itself
* Requires EC Android 11.33.0+
* @param type Values 1 or 2; different activation methods
* @param timeout Timeout in milliseconds
* @returns `{string}` JSON string; code=0 means success, data is returned data, msg is the message
*
```javascript showLineNumbers
    // See the adbClient.connectHistory example
```

## adbClient.openAdbWifiDebugPage Open Wireless Debugging Page

* Open the wireless debugging settings page
* Requires EC Android 11.33.0+
* @returns `{*|boolean}` true on success, false on failure

```javascript showLineNumbers
    function main() {
        // With the app in foreground, call directly
        let r = adbClient.openAdbWifiDebugPage();
        logd(r)
    }
    
    main();
```

## adbClient.openNotificationPermissionPage Open Notification Permission Page

* Open the notification permission settings page
* Requires EC Android 11.33.0+
* @returns `{*|boolean}` true on success, false on failure

```javascript showLineNumbers
    function main() {
        // With the app in foreground, call directly
        let r = adbClient.openNotificationPermissionPage();
        logd(r)
    }
    
    main();
```
