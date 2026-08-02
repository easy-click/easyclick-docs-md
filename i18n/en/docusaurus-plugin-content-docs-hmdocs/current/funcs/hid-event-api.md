---
title: USB HID Events
description: EasyClick automation scripts — HarmonyOS Next USB HID events
keywords:
 - EasyClick automation scripts HarmonyOS Next USB HID events
 - HID
 - USB
 - hid
 - winusb
 - EasyClick
 - zadig
 - Next
 - hidEvent
 - setHidCenter
 - isUsbConnected
 - mobile automation
 - automation testing
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
 - remote screen mirroring
---

## Overview {#说明}

- All functions in the USB HID event module require activating USB HID with the EasyClick USB HID host program before they can be called
- You can also activate USB HID with one click from the HarmonyOS Next control center (right-click menu "USB HID Control")
- The module object prefix is `hidEvent`, e.g. `hidEvent.clickPoint`
- Different from Bluetooth BLE HID (`bleEvent`): USB HID uses a data cable/AOA; Bluetooth HID uses an ESP32 board

:::info Driver note
- HarmonyOS Next phones and Android phones use the same USB HID program. Download **EasyClick USB HID Host Program** from the cloud drive and install the winusb driver on Windows
- Driver installation reference: [Android HID driver installation](/docs/advance/hid)
- The driver must be installed three times
 - When plugging in the device, use Zadig to select the device and install the winusb driver
 - In the phone Developer options, disable USB debugging, then use Zadig to select the device and install the winusb driver again
 - Activate USB HID mode with the USB HID host; when VID becomes 18D1, use Zadig to select the device and install the winusb driver again
:::
:::tip
- Be sure to download **EasyClick USB HID Host Program** from the cloud drive and run the host program before use
- USB HID is only a tap mode; the functions here are adapted for the HarmonyOS Next USB edition
- On the HarmonyOS Next USB edition, you can obtain nodes and screenshots without enabling the automation service, then combine that with USB HID taps
:::

## USB HID Functions {#usb-hid函数}

### setHidCenter — Set USB HID Host Address {#sethidcenter-设置usb-hid主控地址}

* Set the USB HID host address
* Supported: HarmonyOS Next USB edition 2.15.0+
* @param hidCenterUrl URL where the USB HID host program is running
* @return `{string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {
    // Usually not needed
    logd(hidEvent.setHidCenter("http://127.0.0.1:8988"))
}

main();
```

### isUsbConnected — Check USB Connection {#isusbconnected-是否是usb链接}

* Check whether the device is connected via USB
* Supported: HarmonyOS Next USB edition 2.15.0+
* @return `{boolean}` true if the device is connected via USB

```javascript showLineNumbers
function main() {
    logd(hidEvent.isUsbConnected())
}

main();
```

### simCloseUsbDebug — Simulate Disabling USB Debugging {#simcloseusbdebug-模拟关闭usb调试}

* Simulate disabling USB debugging
* Supported: HarmonyOS Next USB edition 2.15.0+
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    // See activeHid — Initialize USB HID device
}

main();
```

### simOpenUsbDebug — Simulate Enabling USB Debugging {#simopenusbdebug-模拟开启usb调试}

* Simulate enabling USB debugging
* Supported: HarmonyOS Next USB edition 2.15.0+
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    // See activeHid — Initialize USB HID device
}

main();
```

### connectByTcp — Switch to TCP Connection {#connectbytcp-转成tcp链接}

* Switch to a TCP connection
* Supported: HarmonyOS Next USB edition 2.15.0+
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    // See activeHid — Initialize USB HID device
}

main();
```

### clearHidStatus — Clear USB HID Connection State {#clearhidstatus-清除usb-hid链接状态}

* Clear USB HID connection state
* This function does not reset PID and VID
* Supported: HarmonyOS Next USB edition 2.15.0+
* @return `{null|string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {
    // See activeHid — Initialize USB HID device
}

main();
```

### isHidStatus — Check USB HID Status {#ishidstatus-usb-hid状态判断}

* Check USB HID status
* Supported: HarmonyOS Next USB edition 2.15.0+
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    // See activeHid — Initialize USB HID device
}

main();
```

### activeHid — Initialize USB HID Device {#activehid-初始化usb-hid设备}

* Initialize the USB HID device
* Supported: HarmonyOS Next USB edition 2.15.0+
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {

 let r = one_key_active_hid();
 console.log("--- one_key_active_hid {}", r)
 if (!r) {
 return;
 }


 let click = hidEvent.clickPoint(469, 1306)
 if (click == null) {
 console.log("Tap succeeded")
 } else {
 console.log("Tap failed")
 }


 // let openNotification = hidEvent.openNotification();
 // logd("Result: {}", openNotification)


}


function one_key_active_hid() {
 if (hidEvent.isHidStatus()) {
 logd("Already in USB HID mode, no need to activate")
 return true;
 }
 logd("Switching to TCP connection...")
 let toTcpResult = hidEvent.connectByTcp()
 if (toTcpResult) {
 logd("TCP switch succeeded, continuing USB HID activation...")
 } else {
 logw("TCP switch failed; USB HID activation may not work")
 }
 sleep(3000)
 if (hidEvent.isUsbConnected()) {
 console.log("Simulating USB debugging off... please wait")
 let close_debug_result = hidEvent.simCloseUsbDebug();
 console.log("close_debug_result {}", close_debug_result)
 if (close_debug_result) {
 console.log("USB debugging disabled successfully")
 sleep(3000)
 } else {
 logw("Failed to disable USB debugging")
 return;
 }
 }
 logd("Clearing previous USB HID records...")
 let clearResult = hidEvent.clearHidStatus()
 if (clearResult != null) {
 clearResult = hidEvent.clearHidStatus()
 if (clearResult != null) {
 logw("Clear failed --> USB HID host call failed: " + clearResult)
 return false;
 }
 }

 logd("USB HID records cleared; activating USB HID... connection may drop and reconnect automatically")


 let activeResult = hidEvent.activeHid();
 if (activeResult != null) {
 activeResult = hidEvent.activeHid()
 if (hidEvent.isHidStatus()) {
 return true
 } else {
 logw("Activation failed --> USB HID host call failed: " + activeResult)
 return false;
 }
 }
 sleep(2000)
 logd("USB HID activated successfully")
 let rs = hidEvent.isHidStatus();
 if (rs) {
 logd("Device is in USB HID mode")
 } else {
 logw("Device is not in USB HID mode")
 return false;
 }

 return true;


}

main()
```

### closeDevice — Close USB HID Device {#closedevice-关闭usb-hid设备}

* Close and reset the USB HID device
* Resets PID and VID
* Supported: HarmonyOS Next USB edition 2.15.0+
* @return `{null|string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let rr = hidEvent.closeDevice();
    if (rr == null) {
        logd("USB reset succeeded")
    } else {
        logd("USB reset failed: " + rr)
    }
}

main();
```

### clickPoint — Tap Coordinates {#clickpoint-点击坐标}

* Tap coordinates
* Supported: HarmonyOS Next USB edition 2.15.0+
* @param x x coordinate
* @param y y coordinate
* @return `{null|string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {

    let r = one_key_active_hid();
    console.log("--- one_key_active_hid {}", r)
    if (!r) {
        return;
    }
    let click = hidEvent.clickPoint(469, 1306)
    if (click == null) {
        console.log("Tap succeeded")
    } else {
        console.log("Tap failed {}", click)
    }
}


function one_key_active_hid() {
    if (hidEvent.isHidStatus()) {
        logd("Already in USB HID mode, no need to activate")
        return true;
    }
    logd("Switching to TCP connection...")
    let toTcpResult = hidEvent.connectByTcp()
    if (toTcpResult) {
        logd("TCP switch succeeded, continuing USB HID activation...")
    } else {
        logw("TCP switch failed; USB HID activation may not work")
    }
    sleep(3000)
    if (hidEvent.isUsbConnected()) {
        console.log("Simulating USB debugging off... please wait")
        let close_debug_result = hidEvent.simCloseUsbDebug();
        console.log("close_debug_result {}", close_debug_result)
        if (close_debug_result) {
            console.log("USB debugging disabled successfully")
            sleep(3000)
        } else {
            logw("Failed to disable USB debugging")
            return;
        }
    }
    logd("Clearing previous USB HID records...")
    let clearResult = hidEvent.clearHidStatus()
    if (clearResult != null) {
        clearResult = hidEvent.clearHidStatus()
        if (clearResult != null) {
            logw("Clear failed --> USB HID host call failed: " + clearResult)
            return false;
        }
    }

    logd("USB HID records cleared; activating USB HID... connection may drop and reconnect automatically")


    let activeResult = hidEvent.activeHid();
    if (activeResult != null) {
        activeResult = hidEvent.activeHid()
        if (hidEvent.isHidStatus()) {
            return true
        } else {
            logw("Activation failed --> USB HID host call failed: " + activeResult)
            return false;
        }
    }
    sleep(2000)
    logd("USB HID activated successfully")
    let rs = hidEvent.isHidStatus();
    if (rs) {
        logd("Device is in USB HID mode")
    } else {
        logw("Device is not in USB HID mode")
        return false;
    }

    return true;


}

main()
```
### doubleClickPoint — Double-Tap Coordinates {#doubleclickpoint-双击坐标}

* Double-tap coordinates
* Supported: HarmonyOS Next USB edition 2.15.0+
* @param x x coordinate
* @param y y coordinate
* @return `{null|string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {

    let r = one_key_active_hid();
    console.log("--- one_key_active_hid {}", r)
    if (!r) {
        return;
    }
    let rse = hidEvent.doubleClickPoint(469, 1306)
    if (rse == null) {
        console.log("Succeeded")
    } else {
        console.log("Failed {}", rse)
    }
}


function one_key_active_hid() {
    if (hidEvent.isHidStatus()) {
        logd("Already in USB HID mode, no need to activate")
        return true;
    }
    logd("Switching to TCP connection...")
    let toTcpResult = hidEvent.connectByTcp()
    if (toTcpResult) {
        logd("TCP switch succeeded, continuing USB HID activation...")
    } else {
        logw("TCP switch failed; USB HID activation may not work")
    }
    sleep(3000)
    if (hidEvent.isUsbConnected()) {
        console.log("Simulating USB debugging off... please wait")
        let close_debug_result = hidEvent.simCloseUsbDebug();
        console.log("close_debug_result {}", close_debug_result)
        if (close_debug_result) {
            console.log("USB debugging disabled successfully")
            sleep(3000)
        } else {
            logw("Failed to disable USB debugging")
            return;
        }
    }
    logd("Clearing previous USB HID records...")
    let clearResult = hidEvent.clearHidStatus()
    if (clearResult != null) {
        clearResult = hidEvent.clearHidStatus()
        if (clearResult != null) {
            logw("Clear failed --> USB HID host call failed: " + clearResult)
            return false;
        }
    }

    logd("USB HID records cleared; activating USB HID... connection may drop and reconnect automatically")


    let activeResult = hidEvent.activeHid();
    if (activeResult != null) {
        activeResult = hidEvent.activeHid()
        if (hidEvent.isHidStatus()) {
            return true
        } else {
            logw("Activation failed --> USB HID host call failed: " + activeResult)
            return false;
        }
    }
    sleep(2000)
    logd("USB HID activated successfully")
    let rs = hidEvent.isHidStatus();
    if (rs) {
        logd("Device is in USB HID mode")
    } else {
        logw("Device is not in USB HID mode")
        return false;
    }

    return true;


}

main()
```
### press — Long-Press Coordinates {#press-长按坐标}

* Long-press coordinates
* Supported: HarmonyOS Next USB edition 2.15.0+
* @param x x coordinate
* @param y y coordinate
* @param delay hold duration in milliseconds
* @return `{null|string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {

    let r = one_key_active_hid();
    console.log("--- one_key_active_hid {}", r)
    if (!r) {
        return;
    }
    let rse = hidEvent.press(469, 1306, 5000)
    if (rse == null) {
        console.log("Succeeded")
    } else {
        console.log("Failed {}", rse)
    }
}


function one_key_active_hid() {
    if (hidEvent.isHidStatus()) {
        logd("Already in USB HID mode, no need to activate")
        return true;
    }
    logd("Switching to TCP connection...")
    let toTcpResult = hidEvent.connectByTcp()
    if (toTcpResult) {
        logd("TCP switch succeeded, continuing USB HID activation...")
    } else {
        logw("TCP switch failed; USB HID activation may not work")
    }
    sleep(3000)
    if (hidEvent.isUsbConnected()) {
        console.log("Simulating USB debugging off... please wait")
        let close_debug_result = hidEvent.simCloseUsbDebug();
        console.log("close_debug_result {}", close_debug_result)
        if (close_debug_result) {
            console.log("USB debugging disabled successfully")
            sleep(3000)
        } else {
            logw("Failed to disable USB debugging")
            return;
        }
    }
    logd("Clearing previous USB HID records...")
    let clearResult = hidEvent.clearHidStatus()
    if (clearResult != null) {
        clearResult = hidEvent.clearHidStatus()
        if (clearResult != null) {
            logw("Clear failed --> USB HID host call failed: " + clearResult)
            return false;
        }
    }

    logd("USB HID records cleared; activating USB HID... connection may drop and reconnect automatically")


    let activeResult = hidEvent.activeHid();
    if (activeResult != null) {
        activeResult = hidEvent.activeHid()
        if (hidEvent.isHidStatus()) {
            return true
        } else {
            logw("Activation failed --> USB HID host call failed: " + activeResult)
            return false;
        }
    }
    sleep(2000)
    logd("USB HID activated successfully")
    let rs = hidEvent.isHidStatus();
    if (rs) {
        logd("Device is in USB HID mode")
    } else {
        logw("Device is not in USB HID mode")
        return false;
    }

    return true;


}

main()
```
### swipe — Swipe {#swipe-滑动}

* Swipe
* Supported: HarmonyOS Next USB edition 2.15.0+
* @param x start x coordinate
* @param y start y coordinate
* @param ex end x coordinate
* @param ey end y coordinate
* @param delay hold duration in milliseconds
* @return `{null|string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {

    let r = one_key_active_hid();
    console.log("--- one_key_active_hid {}", r)
    if (!r) {
        return;
    }
    let rse = hidEvent.swipe(469, 1306, 600, 1000, 1000)
    if (rse == null) {
        console.log("Succeeded")
    } else {
        console.log("Failed {}", rse)
    }
}


function one_key_active_hid() {
    if (hidEvent.isHidStatus()) {
        logd("Already in USB HID mode, no need to activate")
        return true;
    }
    logd("Switching to TCP connection...")
    let toTcpResult = hidEvent.connectByTcp()
    if (toTcpResult) {
        logd("TCP switch succeeded, continuing USB HID activation...")
    } else {
        logw("TCP switch failed; USB HID activation may not work")
    }
    sleep(3000)
    if (hidEvent.isUsbConnected()) {
        console.log("Simulating USB debugging off... please wait")
        let close_debug_result = hidEvent.simCloseUsbDebug();
        console.log("close_debug_result {}", close_debug_result)
        if (close_debug_result) {
            console.log("USB debugging disabled successfully")
            sleep(3000)
        } else {
            logw("Failed to disable USB debugging")
            return;
        }
    }
    logd("Clearing previous USB HID records...")
    let clearResult = hidEvent.clearHidStatus()
    if (clearResult != null) {
        clearResult = hidEvent.clearHidStatus()
        if (clearResult != null) {
            logw("Clear failed --> USB HID host call failed: " + clearResult)
            return false;
        }
    }

    logd("USB HID records cleared; activating USB HID... connection may drop and reconnect automatically")


    let activeResult = hidEvent.activeHid();
    if (activeResult != null) {
        activeResult = hidEvent.activeHid()
        if (hidEvent.isHidStatus()) {
            return true
        } else {
            logw("Activation failed --> USB HID host call failed: " + activeResult)
            return false;
        }
    }
    sleep(2000)
    logd("USB HID activated successfully")
    let rs = hidEvent.isHidStatus();
    if (rs) {
        logd("Device is in USB HID mode")
    } else {
        logw("Device is not in USB HID mode")
        return false;
    }

    return true;


}

main()
```
### touchDown — Touch Down {#touchdown-按下}

* Touch down
* Supported: HarmonyOS Next USB edition 2.15.0+
* @param x x coordinate
* @param y y coordinate
* @return `{null|string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {

    let r = one_key_active_hid();
    console.log("--- one_key_active_hid {}", r)
    if (!r) {
        return;
    }
    let rse = hidEvent.touchDown(469, 1306)
    if (rse == null) {
        console.log("Succeeded")
    } else {
        console.log("Failed {}", rse)
    }
}


function one_key_active_hid() {
    if (hidEvent.isHidStatus()) {
        logd("Already in USB HID mode, no need to activate")
        return true;
    }
    logd("Switching to TCP connection...")
    let toTcpResult = hidEvent.connectByTcp()
    if (toTcpResult) {
        logd("TCP switch succeeded, continuing USB HID activation...")
    } else {
        logw("TCP switch failed; USB HID activation may not work")
    }
    sleep(3000)
    if (hidEvent.isUsbConnected()) {
        console.log("Simulating USB debugging off... please wait")
        let close_debug_result = hidEvent.simCloseUsbDebug();
        console.log("close_debug_result {}", close_debug_result)
        if (close_debug_result) {
            console.log("USB debugging disabled successfully")
            sleep(3000)
        } else {
            logw("Failed to disable USB debugging")
            return;
        }
    }
    logd("Clearing previous USB HID records...")
    let clearResult = hidEvent.clearHidStatus()
    if (clearResult != null) {
        clearResult = hidEvent.clearHidStatus()
        if (clearResult != null) {
            logw("Clear failed --> USB HID host call failed: " + clearResult)
            return false;
        }
    }

    logd("USB HID records cleared; activating USB HID... connection may drop and reconnect automatically")


    let activeResult = hidEvent.activeHid();
    if (activeResult != null) {
        activeResult = hidEvent.activeHid()
        if (hidEvent.isHidStatus()) {
            return true
        } else {
            logw("Activation failed --> USB HID host call failed: " + activeResult)
            return false;
        }
    }
    sleep(2000)
    logd("USB HID activated successfully")
    let rs = hidEvent.isHidStatus();
    if (rs) {
        logd("Device is in USB HID mode")
    } else {
        logw("Device is not in USB HID mode")
        return false;
    }

    return true;


}

main()
```
### touchMove — Touch Move {#touchmove-移动}

* Touch move
* Supported: HarmonyOS Next USB edition 2.15.0+
* @param x x coordinate
* @param y y coordinate
* @return `{null|string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {

    let r = one_key_active_hid();
    console.log("--- one_key_active_hid {}", r)
    if (!r) {
        return;
    }
    let rse = hidEvent.touchMove(480, 1310)
    if (rse == null) {
        console.log("Succeeded")
    } else {
        console.log("Failed {}", rse)
    }
}


function one_key_active_hid() {
    if (hidEvent.isHidStatus()) {
        logd("Already in USB HID mode, no need to activate")
        return true;
    }
    logd("Switching to TCP connection...")
    let toTcpResult = hidEvent.connectByTcp()
    if (toTcpResult) {
        logd("TCP switch succeeded, continuing USB HID activation...")
    } else {
        logw("TCP switch failed; USB HID activation may not work")
    }
    sleep(3000)
    if (hidEvent.isUsbConnected()) {
        console.log("Simulating USB debugging off... please wait")
        let close_debug_result = hidEvent.simCloseUsbDebug();
        console.log("close_debug_result {}", close_debug_result)
        if (close_debug_result) {
            console.log("USB debugging disabled successfully")
            sleep(3000)
        } else {
            logw("Failed to disable USB debugging")
            return;
        }
    }
    logd("Clearing previous USB HID records...")
    let clearResult = hidEvent.clearHidStatus()
    if (clearResult != null) {
        clearResult = hidEvent.clearHidStatus()
        if (clearResult != null) {
            logw("Clear failed --> USB HID host call failed: " + clearResult)
            return false;
        }
    }

    logd("USB HID records cleared; activating USB HID... connection may drop and reconnect automatically")


    let activeResult = hidEvent.activeHid();
    if (activeResult != null) {
        activeResult = hidEvent.activeHid()
        if (hidEvent.isHidStatus()) {
            return true
        } else {
            logw("Activation failed --> USB HID host call failed: " + activeResult)
            return false;
        }
    }
    sleep(2000)
    logd("USB HID activated successfully")
    let rs = hidEvent.isHidStatus();
    if (rs) {
        logd("Device is in USB HID mode")
    } else {
        logw("Device is not in USB HID mode")
        return false;
    }

    return true;


}

main()
```
### touchUp — Touch Up {#touchup-抬起}

* Touch up
* Supported: HarmonyOS Next USB edition 2.15.0+
* @param x x coordinate
* @param y y coordinate
* @return `{null|string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {

    let r = one_key_active_hid();
    console.log("--- one_key_active_hid {}", r)
    if (!r) {
        return;
    }
    let rse = hidEvent.touchUp(480, 1310)
    if (rse == null) {
        console.log("Succeeded")
    } else {
        console.log("Failed {}", rse)
    }
}


function one_key_active_hid() {
    if (hidEvent.isHidStatus()) {
        logd("Already in USB HID mode, no need to activate")
        return true;
    }
    logd("Switching to TCP connection...")
    let toTcpResult = hidEvent.connectByTcp()
    if (toTcpResult) {
        logd("TCP switch succeeded, continuing USB HID activation...")
    } else {
        logw("TCP switch failed; USB HID activation may not work")
    }
    sleep(3000)
    if (hidEvent.isUsbConnected()) {
        console.log("Simulating USB debugging off... please wait")
        let close_debug_result = hidEvent.simCloseUsbDebug();
        console.log("close_debug_result {}", close_debug_result)
        if (close_debug_result) {
            console.log("USB debugging disabled successfully")
            sleep(3000)
        } else {
            logw("Failed to disable USB debugging")
            return;
        }
    }
    logd("Clearing previous USB HID records...")
    let clearResult = hidEvent.clearHidStatus()
    if (clearResult != null) {
        clearResult = hidEvent.clearHidStatus()
        if (clearResult != null) {
            logw("Clear failed --> USB HID host call failed: " + clearResult)
            return false;
        }
    }

    logd("USB HID records cleared; activating USB HID... connection may drop and reconnect automatically")


    let activeResult = hidEvent.activeHid();
    if (activeResult != null) {
        activeResult = hidEvent.activeHid()
        if (hidEvent.isHidStatus()) {
            return true
        } else {
            logw("Activation failed --> USB HID host call failed: " + activeResult)
            return false;
        }
    }
    sleep(2000)
    logd("USB HID activated successfully")
    let rs = hidEvent.isHidStatus();
    if (rs) {
        logd("Device is in USB HID mode")
    } else {
        logw("Device is not in USB HID mode")
        return false;
    }

    return true;


}

main()
```
### openNotification — Open Notification Shade {#opennotification-打开通知栏}

* Open the notification shade
* Supported: HarmonyOS Next USB edition 2.15.0+
* @return `{null|string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {

    let r = one_key_active_hid();
    console.log("--- one_key_active_hid {}", r)
    if (!r) {
        return;
    }
    let rse = hidEvent.openNotification()
    if (rse == null) {
        console.log("Succeeded")
    } else {
        console.log("Failed {}", rse)
    }
}


function one_key_active_hid() {
    if (hidEvent.isHidStatus()) {
        logd("Already in USB HID mode, no need to activate")
        return true;
    }
    logd("Switching to TCP connection...")
    let toTcpResult = hidEvent.connectByTcp()
    if (toTcpResult) {
        logd("TCP switch succeeded, continuing USB HID activation...")
    } else {
        logw("TCP switch failed; USB HID activation may not work")
    }
    sleep(3000)
    if (hidEvent.isUsbConnected()) {
        console.log("Simulating USB debugging off... please wait")
        let close_debug_result = hidEvent.simCloseUsbDebug();
        console.log("close_debug_result {}", close_debug_result)
        if (close_debug_result) {
            console.log("USB debugging disabled successfully")
            sleep(3000)
        } else {
            logw("Failed to disable USB debugging")
            return;
        }
    }
    logd("Clearing previous USB HID records...")
    let clearResult = hidEvent.clearHidStatus()
    if (clearResult != null) {
        clearResult = hidEvent.clearHidStatus()
        if (clearResult != null) {
            logw("Clear failed --> USB HID host call failed: " + clearResult)
            return false;
        }
    }

    logd("USB HID records cleared; activating USB HID... connection may drop and reconnect automatically")


    let activeResult = hidEvent.activeHid();
    if (activeResult != null) {
        activeResult = hidEvent.activeHid()
        if (hidEvent.isHidStatus()) {
            return true
        } else {
            logw("Activation failed --> USB HID host call failed: " + activeResult)
            return false;
        }
    }
    sleep(2000)
    logd("USB HID activated successfully")
    let rs = hidEvent.isHidStatus();
    if (rs) {
        logd("Device is in USB HID mode")
    } else {
        logw("Device is not in USB HID mode")
        return false;
    }

    return true;


}

main()
```
### sendKey — USB HID Keyboard Input {#sendkey-usb-hid键盘输入}

* USB HID keyboard input
* Supported: HarmonyOS Next USB edition 2.15.0+
* @param modifiers int modifier keys: 306 Left Ctrl, 304 Left Shift, 308 Left Alt, 305 Right Ctrl, 303 Right Shift, 307 Right Alt, 309 left Windows key, 310 Right Windows key
* @param code int key code; see https://max.book118.com/html/2018/0108/147954370.shtm or https://wenku.csdn.net/answer/f525e3adc4034414899a2d53fe143c3e
* or search for "HID keyboard key code values"
* @return `{null|string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {

    let r = one_key_active_hid();
    console.log("--- one_key_active_hid {}", r)
    if (!r) {
        return;
    }
    // See documentation links for code values
    // 0x04 = a, 0x05 = b
    let rse = hidEvent.sendKey(0, 0x05)
    if (rse == null) {
        console.log("Succeeded")
    } else {
        console.log("Failed {}", rse)
    }
}


function one_key_active_hid() {
    if (hidEvent.isHidStatus()) {
        logd("Already in USB HID mode, no need to activate")
        return true;
    }
    logd("Switching to TCP connection...")
    let toTcpResult = hidEvent.connectByTcp()
    if (toTcpResult) {
        logd("TCP switch succeeded, continuing USB HID activation...")
    } else {
        logw("TCP switch failed; USB HID activation may not work")
    }
    sleep(3000)
    if (hidEvent.isUsbConnected()) {
        console.log("Simulating USB debugging off... please wait")
        let close_debug_result = hidEvent.simCloseUsbDebug();
        console.log("close_debug_result {}", close_debug_result)
        if (close_debug_result) {
            console.log("USB debugging disabled successfully")
            sleep(3000)
        } else {
            logw("Failed to disable USB debugging")
            return;
        }
    }
    logd("Clearing previous USB HID records...")
    let clearResult = hidEvent.clearHidStatus()
    if (clearResult != null) {
        clearResult = hidEvent.clearHidStatus()
        if (clearResult != null) {
            logw("Clear failed --> USB HID host call failed: " + clearResult)
            return false;
        }
    }

    logd("USB HID records cleared; activating USB HID... connection may drop and reconnect automatically")


    let activeResult = hidEvent.activeHid();
    if (activeResult != null) {
        activeResult = hidEvent.activeHid()
        if (hidEvent.isHidStatus()) {
            return true
        } else {
            logw("Activation failed --> USB HID host call failed: " + activeResult)
            return false;
        }
    }
    sleep(2000)
    logd("USB HID activated successfully")
    let rs = hidEvent.isHidStatus();
    if (rs) {
        logd("Device is in USB HID mode")
    } else {
        logw("Device is not in USB HID mode")
        return false;
    }

    return true;


}

main()
```
### multiTouch — Multi-Touch {#multitouch-多点触摸}

* Multi-touch
* Supported: HarmonyOS Next USB edition 2.15.0+
* Touch parameters: action — usually 0 for down, 1 for up, 2 for move
* x: X coordinate
* y: Y coordinate
* pointer: finger index — 1, 2, 3, etc.
* delay: delay before this action in milliseconds; use more than 40 ms to avoid coordinate drift
* @param touch1 touch point array for the first finger, e.g. `[{"action":0,"x":1,"y":1,"pointer":1,"delay":30},{"action":2,"x":1,"y":1,"pointer":1,"delay":30}]`
* @param timeout multi-touch execution timeout in milliseconds
* @return `{null|string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {

    let r = one_key_active_hid();
    console.log("--- one_key_active_hid {}", r)
    if (!r) {
        return;
    }
    mtouch();
}
function mtouch() {
    let data = [
        {"action": 0, "x": 250, "y": 1800, "pointer": 1, "delay": 100},
        {"action": 2, "x": 250, "y": 1700, "pointer": 1, "delay": 100},
        {"action": 2, "x": 300, "y": 1700, "pointer": 1, "delay": 100},
        {"action": 2, "x": 330, "y": 1650, "pointer": 1, "delay": 200},
        {"action": 2, "x": 400, "y": 1650, "pointer": 1, "delay": 100},
        {"action": 1, "x": 600, "y": 400, "pointer": 1, "delay": 100}
    ]

    let tou = hidEvent.multiTouch(data, 1000)
    if (tou == null) {
        logd("Multi-touch succeeded")
    } else {
        loge("Multi-touch failed:" + tou);
        return false
    }
}

function one_key_active_hid() {
    if (hidEvent.isHidStatus()) {
        logd("Already in USB HID mode, no need to activate")
        return true;
    }
    logd("Switching to TCP connection...")
    let toTcpResult = hidEvent.connectByTcp()
    if (toTcpResult) {
        logd("TCP switch succeeded, continuing USB HID activation...")
    } else {
        logw("TCP switch failed; USB HID activation may not work")
    }
    sleep(3000)
    if (hidEvent.isUsbConnected()) {
        console.log("Simulating USB debugging off... please wait")
        let close_debug_result = hidEvent.simCloseUsbDebug();
        console.log("close_debug_result {}", close_debug_result)
        if (close_debug_result) {
            console.log("USB debugging disabled successfully")
            sleep(3000)
        } else {
            logw("Failed to disable USB debugging")
            return;
        }
    }
    logd("Clearing previous USB HID records...")
    let clearResult = hidEvent.clearHidStatus()
    if (clearResult != null) {
        clearResult = hidEvent.clearHidStatus()
        if (clearResult != null) {
            logw("Clear failed --> USB HID host call failed: " + clearResult)
            return false;
        }
    }

    logd("USB HID records cleared; activating USB HID... connection may drop and reconnect automatically")


    let activeResult = hidEvent.activeHid();
    if (activeResult != null) {
        activeResult = hidEvent.activeHid()
        if (hidEvent.isHidStatus()) {
            return true
        } else {
            logw("Activation failed --> USB HID host call failed: " + activeResult)
            return false;
        }
    }
    sleep(2000)
    logd("USB HID activated successfully")
    let rs = hidEvent.isHidStatus();
    if (rs) {
        logd("Device is in USB HID mode")
    } else {
        logw("Device is not in USB HID mode")
        return false;
    }

    return true;


}

main()
```
