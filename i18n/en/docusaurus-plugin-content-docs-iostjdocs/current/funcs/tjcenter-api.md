---
title: EasyClick Automation Scripts — iOS Scripts — iOS No Jailbreak — iOS No Hardware — Activator Functions
hide_title: false
hide_table_of_contents: false
sidebar_label: Activator Functions
description: EasyClick automation scripts — iOS no jailbreak activator functions
keywords:
 - EasyClick automation scripts
 - iOS scripts
 - iOS no jailbreak
 - iOS no hardware
 - activator functions
 - tjCenter
 - iOS
 - app
 - param
 - setCenterUrl
 - appLaunch
 - WIFI
 - EC
 - '2.0'
 - return
 - EasyClick
 - mobile automation
 - test automation
---

# Activator Functions {#激活器函数}

## Overview {#说明}

- Activator module functions operate the phone through an activator program running on a PC
- The activator module object prefix is `tjCenter`
- Phone and activator must be on the same LAN with network connectivity; connect the phone via WiFi or USB
- For standalone activator, see [Advanced — Standalone Activator Tutorial](/iostjdocs/advance/tjcenter)

## tjCenter.setCenterUrl Set Standalone Activator Address {#tjcentersetcenterurl-设置脱机激活器地址}

* Set the address of the standalone activator
* Requires EC iOS standalone 2.0+
* @param url Activator address
* @return `{string}` null or "" on success; otherwise an error message

```javascript showLineNumbers
function main() {
    // Activator port is usually 8020; change only the PC IP
    let set = tjCenter.setCenterUrl("http://192.168.2.6:8020")
    if (set == null || set == "") {
        logd("setCenterUrl success: " + set)
    } else {
        logd("setCenterUrl failed: " + set)
        return
    }
}

main();
```

## tjCenter.appLaunch Launch App {#tjcenterapplaunch-启动app}

* Launch an app through the standalone activator
* Requires EC iOS standalone 2.0+
* @param deviceId Device ID
* @param bundleId Bundle ID
* @param killExist Kill existing process
* @return `{string}` null or "" on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let set = tjCenter.setCenterUrl("http://192.168.2.6:8020")
    if (set == null || set == "") {
        logd("setCenterUrl success: " + set)
    } else {
        logd("setCenterUrl failed: " + set)
        return
    }
    let deviceId = device.getDeviceId()
    logd("current deviceId : " + deviceId)

    let appLaunch = tjCenter.appLaunch(deviceId, "com.tencent.mttlite", false)
    if (appLaunch == null || appLaunch == "") {
        logd("appLaunch success ")
    } else {
        logd("appLaunch failed: " + appLaunch)
        return
    }
}

main();
```

## tjCenter.appKillByBundleId Kill App by Bundle ID {#tjcenterappkillbybundleid-启动app}

* Kill an app through the standalone activator
* Requires EC iOS standalone 2.0+
* @param deviceId Device ID
* @param bundleId Bundle ID
* @return `{string}` null or "" on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let set = tjCenter.setCenterUrl("http://192.168.2.6:8020")
    if (set == null || set == "") {
        logd("setCenterUrl success: " + set)
    } else {
        logd("setCenterUrl failed: " + set)
        return
    }
    let deviceId = device.getDeviceId()
    logd("current deviceId : " + deviceId)

    let appKillByBundleId = tjCenter.appKillByBundleId(deviceId, "com.tencent.mttlite")
    if (appKillByBundleId == null || appKillByBundleId == "") {
        logd("appKillByBundleId success")
    } else {
        logd("appKillByBundleId failed: " + appKillByBundleId)
        return
    }
}

main();
```

## tjCenter.stopApp Kill App {#tjcenterstopapp-杀死app}

* Kill an app through the standalone activator (alternative implementation)
* Requires EC iOS standalone 2.0+
* @param deviceId Device ID
* @param bundleId Bundle ID
* @return `{string}` null or "" on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let set = tjCenter.setCenterUrl("http://192.168.2.6:8020")
    if (set == null || set == "") {
        logd("setCenterUrl success: " + set)
    } else {
        logd("setCenterUrl failed: " + set)
        return
    }
    let deviceId = device.getDeviceId()
    logd("current deviceId : " + deviceId)

    let stopApp = tjCenter.stopApp(deviceId, "com.tencent.mttlite")
    if (stopApp == null || stopApp == "") {
        logd("stopApp success")
    } else {
        logd("stopApp failed: " + stopApp)
        return
    }
}

main();
```

## tjCenter.flushDevImage Flash Developer Image {#tjcenterflushdevimage-刷入开发者镜像}

* Flash the developer image through the standalone activator
* Requires EC iOS standalone 2.0+
* @param deviceId Device ID
* @return `{string}` null or "" on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let set = tjCenter.setCenterUrl("http://192.168.2.6:8020")
    if (set == null || set == "") {
        logd("setCenterUrl success: " + set)
    } else {
        logd("setCenterUrl failed: " + set)
        return
    }
    let deviceId = device.getDeviceId()
    logd("current deviceId : " + deviceId)

    let devImage = tjCenter.flushDevImage(deviceId)
    if (devImage == null || devImage == "") {
        logd("flushDevImage success")
    } else {
        logd("flushDevImage failed: " + devImage)
        return
    }
}

main();
```

## tjCenter.startAgent Start Agent for Automation {#tjcenterstartagent-开启agent程序启动自动化}

* Start the agent program through the standalone activator
* Requires setting the agent bundle ID on the activator web page first
* See [Advanced — Standalone Activator Tutorial](/iostjdocs/advance/tjcenter)
* Requires EC iOS standalone 2.0+
* @param deviceId Device ID
* @return `{string}` null or "" on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let set = tjCenter.setCenterUrl("http://192.168.2.6:8020")
    if (set == null || set == "") {
        logd("setCenterUrl success: " + set)
    } else {
        logd("setCenterUrl failed: " + set)
        return
    }
    let deviceId = device.getDeviceId()
    logd("current deviceId : " + deviceId)
    let agent = tjCenter.startAgent(deviceId)
    if (agent == null || agent == "") {
        logd("startAgent success: " + set)
    } else {
        logd("startAgent failed: " + set)
        return
    }
}

main();
```

## tjCenter.authInit Initialize Device {#tjcenterauthinit-初始化设备}

* Initialize the device through the standalone activator
* Requires setting the main app bundle ID on the activator web page first
* Usually not needed if initialization was done on the web page
* See [Advanced — Standalone Activator Tutorial](/iostjdocs/advance/tjcenter)
* This kills and restarts the current main app process
* Requires EC iOS standalone 2.0+
* @param deviceId Device ID
* @return `{string}` null or "" on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let set = tjCenter.setCenterUrl("http://192.168.2.6:8020")
    if (set == null || set == "") {
        logd("setCenterUrl success: " + set)
    } else {
        logd("setCenterUrl failed: " + set)
        return
    }
    let deviceId = device.getDeviceId()
    logd("current deviceId : " + deviceId)
    let d = tjCenter.authInit(deviceId)
    if (d == null || d == "") {
        logd("authInit success: " + set)
    } else {
        logd("authInit failed: " + set)
        return
    }
}

main();
```

## tjCenter.setWifiCon Enable or Disable WiFi Connection to PC {#tjcentersetwificon-开启或关闭wifi链接电脑}

* Enable or disable WiFi connection to the PC
* Requires EC iOS standalone 2.0+
* @param deviceId Device ID
* @param status 1 enable, 2 disable
* @return `{string}` null or "" on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let set = tjCenter.setCenterUrl("http://192.168.2.6:8020")
    if (set == null || set == "") {
        logd("setCenterUrl success: " + set)
    } else {
        logd("setCenterUrl failed: " + set)
        return
    }
    let deviceId = device.getDeviceId()
    logd("current deviceId : " + deviceId)
    let d = tjCenter.setWifiCon(deviceId, "1")
    if (d == null || d == "") {
        logd("setWifiCon success: " + set)
    } else {
        logd("setWifiCon failed: " + set)
        return
    }
}

main();
```
