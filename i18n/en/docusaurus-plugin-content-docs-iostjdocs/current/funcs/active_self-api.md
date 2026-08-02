---
title: Self-Activation Functions
description: EasyClick automation scripts — iOS no jailbreak — self-activation functions
keywords:
 - EasyClick automation scripts — iOS no jailbreak — self-activation functions
 - activeSelf
 - LocalDevVpn
 - openLocalDevVpn
 - disableLocalDevVpn
 - mountImageOk
 - openApp
 - killApp
 - IPA
 - VPN
 - EC
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview

- The self-activation module enables maximum-privilege device operations without a computer, such as rebooting the device
- All features in this module require self-activation setup first. See [Advanced — Self-Activation Configuration](/iostjdocs/advance/activemyself)

:::tip

- To keep a proxy IPA running reliably, complete self-activation setup first. If the proxy stops responding, use `killApp` to terminate it, then use `openApp` to restart the proxy IPA
:::

:::tip Self-Activation Video Tutorial
- URL: https://www.bilibili.com/video/BV18vgC6eEC6/
:::

## activeSelf.openLocalDevVpn Open LocalDevVpn

* Open LocalDevVpn and connect VPN
* The main app must be in the foreground
* Available in EC standalone 6.1.0+
* @returns `{boolean}` `true` on success, `false` on failure

```javascript showLineNumbers
function main() {
    // Open the external LocalDevVpn app and connect
    var data = activeSelf.openLocalDevVpn();
    logd(data);
    sleep(3000)
    activeSelf.disableLocalDevVpn();
}

main();
```

## activeSelf.disableLocalDevVpn Disconnect LocalDevVpn

* Disconnect LocalDevVpn VPN
* The main app must be in the foreground
* Available in EC standalone 6.1.0+
* @returns `{boolean}` `true` on success, `false` on failure

```javascript showLineNumbers
function main() {
    // Open the external LocalDevVpn app and connect
    var data = activeSelf.disableLocalDevVpn();
    logd(data);
    sleep(3000)
    activeSelf.disableLocalDevVpn();
}

main();
```

## activeSelf.mountImageOk Check Developer Image Mount

* Whether the developer image was mounted successfully
* Available in EC standalone 6.1.0+
* @returns `{boolean}` `true` on success, `false` on failure

```javascript showLineNumbers
function main() {
    var data = activeSelf.mountImageOk();
    logd(data);
}

main();
```

## activeSelf.startActiveMySelf Start Self-Activation

* Start self-activation. See the self-activation configuration docs for details
* Available in EC standalone 6.1.0+
* @param timeout timeout in milliseconds
* @returns `{JSON}` `code = 0` on success; `msg` contains error information

```javascript showLineNumbers
function testactive1_ext_vpn() {
    logd("testactive1_self_vpn")

    activeSelf.openSelfVpn();
    // Sleep briefly so VPN error messages can be captured
    sleep(3000)
    let ar = activeSelf.getOpenSelfVpnError()
    if (ar != "") {
        loge("Failed to open built-in VPN")
        loge("Error message: " + activeSelf.getOpenSelfVpnError())
        return false;
    }
    sleep(2000)

    let acData = activeSelf.startActiveMySelf(30000);
    if (!acData) {

        loge("Self-activation failed, no response");
        return false;
    }
    if (acData["code"] != 0) {
        loge("Self-activation failed, err : " + acData["msg"]);
        return false;
    }
    logd("Self-activation succeeded!!")
    logd("mount -> " + activeSelf.mountImageOk())
    activeSelf.disableSelfVpn();
    return true;
}

function main() {
    testactive1_ext_vpn();
}

main();
```

## activeSelf.openApp Open App

* Open an app by bundleId
* If opening a proxy IPA:
* Call `activeSelf.startActiveMySelf` for self-activation first
* Mount the developer image so the proxy IPA can start
* Available in EC standalone 6.1.0+
* @param bundleId bundle ID of the app to launch
* @param start_suspended not very meaningful; use `0`
* @param kill_existing `1` = kill existing process (may not work); use `0`
* @param timeout timeout in milliseconds
* @returns `{JSON}` `code = 0` on success; `msg` contains error information; `data` contains PID on success

```javascript showLineNumbers
function testOpenApp() {
    // If opening a proxy IPA:
    // Call activeSelf.startActiveMySelf for self-activation first
    // Mount the developer image so the proxy IPA can start
    logd("testOpenApp")
    activeSelf.openSelfVpn();
    // Sleep briefly so VPN error messages can be captured
    sleep(5000)
    let ar = activeSelf.getOpenSelfVpnError()
    if (ar != "") {
        loge("Failed to open built-in VPN")
        loge("Error message: " + activeSelf.getOpenSelfVpnError())
        activeSelf.disableSelfVpn();
        return false;
    }
    sleep(2000)
    // Proxy IPA bundleId
    let runner = "com.cy.ceshi";

    let rsd = activeSelf.openApp(runner, 0, 0,30000)
    if (!rsd) {
        loge("Failed to open app")
        activeSelf.disableSelfVpn();
        return false;
    }
    if (rsd["code"] != 0) {
        loge("Failed to open app, err : " + rsd["msg"]);
        activeSelf.disableSelfVpn();
        return false;
    }
    logd(JSON.stringify(rsd))

    logd("App opened successfully")
    activeSelf.disableSelfVpn();
    return true;
}

function main() {
    testOpenApp();
}

main();
```

## activeSelf.killApp Kill App

* Kill an app by bundleId
* Available in EC standalone 6.1.0+
* @param bundleId bundle ID of the app to kill
* @param timeout timeout in milliseconds
* @returns `{JSON}` `code = 0` on success; `msg` contains error information

```javascript showLineNumbers
function testKillApp() {
    logd("testKillApp")
    activeSelf.openSelfVpn();
    // Sleep briefly so VPN error messages can be captured
    sleep(5000)
    let ar = activeSelf.getOpenSelfVpnError()
    if (ar != "") {
        loge("Failed to open built-in VPN")
        loge("Error message: " + activeSelf.getOpenSelfVpnError())
        activeSelf.disableSelfVpn();
        return false;
    }
    sleep(2000)
    // Proxy IPA bundleId
    let runner = "com.cy.ceshi";
    let rsd = activeSelf.killApp(runner,30000)
    if (!rsd) {
        loge("Failed to kill process")
        activeSelf.disableSelfVpn();
        return false;
    }
    if (rsd["code"] != 0) {
        loge("Failed to kill process, err : " + rsd["msg"]);
        activeSelf.disableSelfVpn();
        return false;
    }
    logd(JSON.stringify(rsd))
    logd("App process killed successfully")
    activeSelf.disableSelfVpn();
    return true;
}

function main() {
    testKillApp();
}

main();
```

## activeSelf.listApps Get App List

* Get installed app list
* Available in EC standalone 6.1.0+
* Return field descriptions:
* ApplicationType = app type
* CFBundleIdentifier = bundleId (package name)
* CFBundleDisplayName = app display name
* CFBundleShortVersionString = version
* CFBundleURLSchemes = URL schemes
* @param type `System` = system apps, `User` = user-installed apps, `Any` = all apps
* @param timeout timeout in milliseconds
* @returns `{JSON}` `code = 0` on success; `msg` contains error information; `data` is the app list array

```javascript showLineNumbers
function testAppList() {
    logd("testAppList")
    activeSelf.openSelfVpn();
    // Sleep briefly so VPN error messages can be captured
    sleep(5000)
    let ar = activeSelf.getOpenSelfVpnError()
    if (ar != "") {
        loge("Failed to open built-in VPN")
        loge("Error message: " + activeSelf.getOpenSelfVpnError())
        activeSelf.disableSelfVpn();
        return false;
    }
    sleep(2000)
    let rsd = activeSelf.listApps("User",30000)
    if (!rsd) {
        loge("Failed to get app list")
        activeSelf.disableSelfVpn();
        return false;
    }
    if (rsd["code"] != 0) {
        loge("Failed to get app list, err : " + rsd["msg"]);
        activeSelf.disableSelfVpn();
        return false;
    }
    logd("App list: " + JSON.stringify(rsd.data))
    activeSelf.disableSelfVpn();
    return true;
}

function main() {
    testAppList();
}

main();
```

## activeSelf.listProcess Get Process List

* Get process list
* Available in EC standalone 6.1.0+
* name = process name
* isApplication = whether it is a visible application
* realAppName = actual runtime path
* pid = process ID
* @param timeout timeout in milliseconds
* @returns `{JSON}` `code = 0` on success; `msg` contains error information; `data` is the process list array

```javascript showLineNumbers
function testAppList() {
    logd("testAppList")
    activeSelf.openSelfVpn();
    // Sleep briefly so VPN error messages can be captured
    sleep(5000)
    let ar = activeSelf.getOpenSelfVpnError()
    if (ar != "") {
        loge("Failed to open built-in VPN")
        loge("Error message: " + activeSelf.getOpenSelfVpnError())
        activeSelf.disableSelfVpn();
        return false;
    }
    sleep(2000)
    let rsd = activeSelf.listProcess(30000)
    if (!rsd) {
        loge("Failed to get process list")
        activeSelf.disableSelfVpn();
        return false;
    }
    if (rsd["code"] != 0) {
        loge("Failed to get process list, err : " + rsd["msg"]);
        activeSelf.disableSelfVpn();
        return false;
    }
    logd("Process list: " + JSON.stringify(rsd.data))
    activeSelf.disableSelfVpn();
    return true;
}

function main() {
    testAppList();
}

main();
```

## activeSelf.reboot Reboot Device

* Reboot the device
* Available in EC standalone 6.1.0+
* @param timeout timeout in milliseconds
* @returns `{JSON}` `code = 0` on success; `msg` contains error information

```javascript showLineNumbers
function testReboot() {
    activeSelf.openSelfVpn();
    // Sleep briefly so VPN error messages can be captured
    sleep(5000)
    let ar = activeSelf.getOpenSelfVpnError()
    if (ar != "") {
        loge("Failed to open built-in VPN")
        loge("Error message: " + activeSelf.getOpenSelfVpnError())
        activeSelf.disableSelfVpn();
        return false;
    }
    sleep(2000)
    let rsd = activeSelf.reboot(30000)
    if (!rsd) {
        loge("Reboot failed")
        activeSelf.disableSelfVpn();
        return false;
    }
    if (rsd["code"] != 0) {
        loge("Reboot failed, err : " + rsd["msg"]);
        activeSelf.disableSelfVpn();
        return false;
    }
    logd("Reboot: " + JSON.stringify(rsd.data))
    return true;
}

function main() {
    testReboot();
}

main();
```

## activeSelf.killAppByPid Kill Process by PID

* Kill an app/process by PID
* Available in EC standalone 6.1.0+
* @param pid process PID (integer)
* @param timeout timeout in milliseconds
* @returns `{JSON}` `code = 0` on success; `msg` contains error information

```javascript showLineNumbers
function test_killAppByPid() {
    activeSelf.openSelfVpn();
    // Sleep briefly so VPN error messages can be captured
    sleep(5000)
    let ar = activeSelf.getOpenSelfVpnError()
    if (ar != "") {
        loge("Failed to open built-in VPN")
        loge("Error message: " + activeSelf.getOpenSelfVpnError())
        activeSelf.disableSelfVpn();
        return false;
    }
    sleep(2000)
    // PID from listProcess
    let rsd = activeSelf.killAppByPid(123,30000)
    if (!rsd) {
        loge("Failed to kill process")
        activeSelf.disableSelfVpn();
        return false;
    }
    if (rsd["code"] != 0) {
        loge("Failed to kill process, err : " + rsd["msg"]);
        activeSelf.disableSelfVpn();
        return false;
    }
    logd("Process killed: " + JSON.stringify(rsd.data))
    return true;
}

function main() {
    test_killAppByPid();
}

main();
```



## activeSelf.installApp Install IPA

* Install an application
* Available in EC standalone 6.2.0+
* @param path IPA file path
* @param timeout timeout in milliseconds
* @returns `{JSON}` `code = 0` on success; `msg` contains error information

```javascript showLineNumbers
function test_installApp() {
    activeSelf.openSelfVpn();
    // Sleep briefly so VPN error messages can be captured
    sleep(5000)
    let ar = activeSelf.getOpenSelfVpnError()
    if (ar != "") {
        loge("Failed to open built-in VPN")
        loge("Error message: " + activeSelf.getOpenSelfVpnError())
        activeSelf.disableSelfVpn();
        return false;
    }
    sleep(2000)
    // Self-activate once first so the image can mount
    activeSelf.startActiveMySelf(30000)
    let filepath = file.getSandBoxFilePath("ddd.ipa")
    let rsd = activeSelf.installApp(filepath, 30000)
    if (!rsd) {
        loge("Installation failed")
        activeSelf.disableSelfVpn();
        return false;
    }
    if (rsd["code"] != 0) {
        loge("Installation failed, err : " + rsd["msg"]);
        activeSelf.disableSelfVpn();
        return false;
    }
    logd("Installation succeeded")
    return true;
}

function main() {
    test_installApp();
}

main();
```



## activeSelf.uninstallApp Uninstall App

* Uninstall an application
* @param bundleId bundle ID to uninstall
* @param timeout timeout in milliseconds
* @returns `{JSON}` `code = 0` on success; `msg` contains error information

```javascript showLineNumbers
function test_uninstallApp() {
    activeSelf.openSelfVpn();
    // Sleep briefly so VPN error messages can be captured
    sleep(5000)
    let ar = activeSelf.getOpenSelfVpnError()
    if (ar != "") {
        loge("Failed to open built-in VPN")
        loge("Error message: " + activeSelf.getOpenSelfVpnError())
        activeSelf.disableSelfVpn();
        return false;
    }
    sleep(2000)
    // Self-activate once first so the image can mount
    activeSelf.startActiveMySelf(30000)
    let bundleId="com.cy.ceshi"
    let rsd = activeSelf.uninstallApp(bundleId, 30000)
    if (!rsd) {
        loge("Uninstall failed")
        activeSelf.disableSelfVpn();
        return false;
    }
    if (rsd["code"] != 0) {
        loge("Uninstall failed, err : " + rsd["msg"]);
        activeSelf.disableSelfVpn();
        return false;
    }
    logd("Uninstall succeeded")
    return true;
}

function main() {
    test_uninstallApp();
}

main();
```





## activeSelf.deviceOrientation Screen Orientation

*
* Screen orientation
* Available in EC standalone 6.5.0+
* @param timeout timeout in milliseconds
* @returns `{JSON}` `code = 0` on success; `msg` contains error information; `data` is orientation — `1` = portrait, `2` = landscape, `0` = unknown

```javascript showLineNumbers
function test_active_screenshot() {
    let ss = activeSelf.deviceOrientation(10000)
    if (ss) {
        logd("Current screen orientation: " + ss["data"])
    }
    logd("Starting screenshot")
    console.time(1)
    let img = activeSelf.screenshot(10000)
    logd(img)
    if (img) {
        logd(img.getWidth() + "," + img.getHeight())
        let path = file.getSandBoxFilePath("test.png")
        logd("Save to path: " + path)
        img.saveTo(path)
        img.recycle()
    }
    logd("end, elapsed (ms) ", console.timeEnd(1))
    sleep(300000)
}



function start() {
    activeSelf.openSelfVpn();
    // Sleep briefly so VPN error messages can be captured
    sleep(5000)
    let ar = activeSelf.getOpenSelfVpnError()
    if (ar != "") {
        loge("Failed to open built-in VPN")
        loge("Error message: " + activeSelf.getOpenSelfVpnError())
        activeSelf.disableSelfVpn();
        return false;
    }
    sleep(2000)
    // Self-activate once first so the image can mount
    activeSelf.startActiveMySelf(30000)
    test_active_screenshot()
    return true;
}

function main() {
    start();
}

main();
```






## activeSelf.screenshot Screenshot

* Take a screenshot
* Available in EC standalone 6.5.0+
* @param timeout timeout in milliseconds
* @returns `{AutoImage}` `null` if screenshot failed

```javascript showLineNumbers
function test_active_screenshot() {
    let ss = activeSelf.deviceOrientation(10000)
    if (ss) {
        logd("Current screen orientation: " + ss["data"])
    }
    logd("Starting screenshot")
    console.time(1)
    let img = activeSelf.screenshot(10000)
    logd(img)
    if (img) {
        logd(img.getWidth() + "," + img.getHeight())
        let path = file.getSandBoxFilePath("test.png")
        logd("Save to path: " + path)
        img.saveTo(path)
        img.recycle()
    }
    logd("end, elapsed (ms) ", console.timeEnd(1))
    sleep(300000)
}



function start() {
    activeSelf.openSelfVpn();
    // Sleep briefly so VPN error messages can be captured
    sleep(5000)
    let ar = activeSelf.getOpenSelfVpnError()
    if (ar != "") {
        loge("Failed to open built-in VPN")
        loge("Error message: " + activeSelf.getOpenSelfVpnError())
        activeSelf.disableSelfVpn();
        return false;
    }
    sleep(2000)
    // Self-activate once first so the image can mount
    activeSelf.startActiveMySelf(30000)
    test_active_screenshot()
    return true;
}

function main() {
    start();
}

main();
```
