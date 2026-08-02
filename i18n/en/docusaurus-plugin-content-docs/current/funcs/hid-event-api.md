---
title: HID Events
description: EasyClick automation scripts — Android no root HID events
keywords:
 - EasyClick automation scripts Android no root HID events
 - HID
 - USB
 - docs
 - zh
 - cn
 - advance
 - hid
 - hidEvent
 - setHidCenter
 - initUsbDevice
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview

- All functions in the HID events module require activating HID via the EasyClick HID control host before they can be called
- For HID control host usage, see Advanced — HID control host: [HID control host](/docs/advance/hid)
 or use the [HID mini host](/docs/centerscreen/hidlinux)
- The HID events module uses the `hidEvent` prefix, e.g. `hidEvent.click()`

:::tip

- EC 10.6.0+ adds USB-mode direct communication functions — useful when the phone and HID control host are not on the same network. Requires HID control host v2.2.0+. See [HID USB communication](/docs/advance/hid#usb通信模式v220)
- Network mode does not require confirming accessory mode permission on the phone, but devices must be on the same LAN, use a public IP, or use frp forwarding
- USB mode is not limited by network, but requires manually confirming accessory mode permission
- USB-mode functions end with `ByUsb`
- Both network and USB modes support [Advanced networking](/docs/advance/hid#高级组网v220), which forwards data to slave hosts automatically
- Choose the mode that fits your scenario
 :::

:::tip

- HID is one click mode; accessibility, agent, and root modes also provide click and node access. If nodes are unavailable, use image/color matching
- For image/color capture permission, use `image.requestScreenCapture` with `type=1` (capture with permission)
- HID cannot use nodes; other features work the same with no special handling
 :::

:::tip
- HID control host 4.0+ supports the WinUSB driver on Windows — no libusb install required
- HID control host 4.0 does not support USB communication yet
:::

## HID Network Mode Functions {#hid网络模式函数}

### setHidCenter Set HID Control Host Address {#sethidcenter-设置hid主控地址}

* Used in network mode
* Set the HID control host address
* Requires EC Android 9.15.0+
* @param hidCenterUrl URL where the HID control host is running
* @return `{string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let i = initHid()
    if (!i) {
        return
    }
}

function initHid() {
    // First set the LAN address of the HID program; you can also map it to the public network with FRP and use the public address
    hidEvent.setHidCenter("http://192.168.2.14:8988")
    hidEvent.closeUsbDevice();
    sleep(3000)
    let init = hidEvent.initUsbDevice()
    if (init == null) {
        logd("HID device initialized successfully")
    } else {
        loge("Initialization failed:" + init);
        return false
    }
   
    return true;
}

main();
```

### initUsbDevice Initialize HID Device

* Used in network mode
* Initialize the HID device
* Requires EC Android 9.15.0+
* @return `{string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let i = initHid()
    if (!i) {
        return
    }
}

function initHid() {
    // First set the LAN address of the HID program; you can also map it to the public network with FRP and use the public address
    hidEvent.setHidCenter("http://192.168.2.14:8988")
    hidEvent.closeUsbDevice();
    sleep(3000)
    let init = hidEvent.initUsbDevice()
    if (init == null) {
        logd("HID device initialized successfully")
    } else {
        loge("Initialization failed:" + init);
        return false
    }
   
    return true;
}

main();
```

### checkFirstPoint Calibrate HID Coordinates

* Used in network mode; deprecated — HID v2.0.0+ does not require coordinate calibration
* Call after `initUsbDevice`
* If the HID control host is v2.0+, do not call this function — click and other actions work without calibration
* Requires EC Android 9.15.0+
* @return `{string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let i = initHid()
    if (!i) {
        return
    }
}

function initHid() {
    // First set the LAN address of the HID program; you can also map it to the public network with FRP and use the public address
    hidEvent.setHidCenter("http://192.168.2.14:8988")
    hidEvent.closeUsbDevice();
    let init = hidEvent.initUsbDevice()
    if (init == null) {
        logd("HID device initialized successfully")
    } else {
        loge("Initialization failed:" + init);
        return false
    }
    // Start calibrating screen coordinates
    // If the HID control host is v2.0+, do not call this function
    let po = hidEvent.checkFirstPoint()
    if (po == null) {
        logd("Coordinate calibration succeeded")
    } else {
        loge("Coordinate calibration failed:" + init);
        return false
    }
    return true;
}

main();
```

### closeUsbDevice Close HID Device

* Used in network mode
* Close the HID device
* This stops communication between the HID control host and the device — call when appropriate
* Requires EC Android 9.15.0+
* @return `{string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let i = initHid()
    if (!i) {
        return
    }
}

function initHid() {
    // First set the LAN address of the HID program; you can also map it to the public network with FRP and use the public address
    hidEvent.setHidCenter("http://192.168.2.14:8988")
    hidEvent.closeUsbDevice();
    sleep(3000)
    let init = hidEvent.initUsbDevice()
    if (init == null) {
        logd("HID device initialized successfully")
    } else {
        loge("Initialization failed:" + init);
        return false
    }
   
    return true;
}

main();
```

### clickPoint Click Coordinates

* Used in network mode
* Requires EC Android 9.15.0+
* @param x x coordinate
* @param y y coordinate
* @return `{string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let i = initHid()
    if (!i) {
        return
    }
    // Click at screen corner coordinates
    let click = hidEvent.clickPoint(device.getScreenWidth(), device.getScreenHeight())
    if (click == null) {
        logd("Click succeeded")
    } else {
        console.log("Click failed" + click)
    }
    sleep(1000)
    let doub = hidEvent.doubleClickPoint(300, 400)
    if (doub == null) {
        logd("Double-click succeeded")
    } else {
        console.log("Double-click failed" + doub)
    }

    sleep(1000)
    let ps = hidEvent.press(600, 800, 2000)
    if (ps == null) {
        logd("Long press succeeded")
    } else {
        console.log("Long press failed" + ps)
    }

    sleep(1000)
    let ps1 = hidEvent.press(device.getScreenWidth(), device.getScreenHeight(), 2000)
    if (ps1 == null) {
        logd("Long press succeeded")
    } else {
        console.log("Long press failed" + ps1)
    }


    mtouch()


}

function initHid() {
    // First set the LAN address of the HID program; you can also map it to the public network with FRP and use the public address
    hidEvent.setHidCenter("http://192.168.2.14:8988")
    hidEvent.closeUsbDevice();
    sleep(3000)
    let init = hidEvent.initUsbDevice()
    if (init == null) {
        logd("HID device initialized successfully")
    } else {
        loge("Initialization failed:" + init);
        return false
    }
  
    return true;
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


main();


```

### doubleClickPoint Double-Click Coordinates

* Requires EC Android 9.15.0+
* @param x x coordinate
* @param y y coordinate
* @return `{string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let i = initHid()
    if (!i) {
        return
    }
    // Click at screen corner coordinates
    let click = hidEvent.clickPoint(device.getScreenWidth(), device.getScreenHeight())
    if (click == null) {
        logd("Click succeeded")
    } else {
        console.log("Click failed" + click)
    }
    sleep(1000)
    let doub = hidEvent.doubleClickPoint(300, 400)
    if (doub == null) {
        logd("Double-click succeeded")
    } else {
        console.log("Double-click failed" + doub)
    }

    sleep(1000)
    let ps = hidEvent.press(600, 800, 2000)
    if (ps == null) {
        logd("Long press succeeded")
    } else {
        console.log("Long press failed" + ps)
    }

    sleep(1000)
    let ps1 = hidEvent.press(device.getScreenWidth(), device.getScreenHeight(), 2000)
    if (ps1 == null) {
        logd("Long press succeeded")
    } else {
        console.log("Long press failed" + ps1)
    }


    mtouch()


}

function initHid() {
    // First set the LAN address of the HID program; you can also map it to the public network with FRP and use the public address
    hidEvent.setHidCenter("http://192.168.2.14:8988")
    hidEvent.closeUsbDevice();
    sleep(3000)
    let init = hidEvent.initUsbDevice()
    if (init == null) {
        logd("HID device initialized successfully")
    } else {
        loge("Initialization failed:" + init);
        return false
    }
   
    return true;
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


main();


```

### press Long Press Coordinates

* Used in network mode
* Long press at coordinates
* Requires EC Android 9.15.0+
* @param x x coordinate
* @param y y coordinate
* @param delay Hold duration in milliseconds
* @return `{string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {
    let i = initHid()
    if (!i) {
        return
    }
    // Click at screen corner coordinates
    let click = hidEvent.clickPoint(device.getScreenWidth(), device.getScreenHeight())
    if (click == null) {
        logd("Click succeeded")
    } else {
        console.log("Click failed" + click)
    }
    sleep(1000)
    let doub = hidEvent.doubleClickPoint(300, 400)
    if (doub == null) {
        logd("Double-click succeeded")
    } else {
        console.log("Double-click failed" + doub)
    }

    sleep(1000)
    let ps = hidEvent.press(600, 800, 2000)
    if (ps == null) {
        logd("Long press succeeded")
    } else {
        console.log("Long press failed" + ps)
    }

    sleep(1000)
    let ps1 = hidEvent.press(device.getScreenWidth(), device.getScreenHeight(), 2000)
    if (ps1 == null) {
        logd("Long press succeeded")
    } else {
        console.log("Long press failed" + ps1)
    }


    mtouch()


}

function initHid() {
    // First set the LAN address of the HID program; you can also map it to the public network with FRP and use the public address
    hidEvent.setHidCenter("http://192.168.2.14:8988")
    hidEvent.closeUsbDevice();
    sleep(3000)
    let init = hidEvent.initUsbDevice()
    if (init == null) {
        logd("HID device initialized successfully")
    } else {
        loge("Initialization failed:" + init);
        return false
    }
  
    return true;
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


main();


```

### multiTouch Multi-Touch

* Used in network mode
* Multi-touch
* Requires EC Android 9.15.0+
* Touch parameters: action — typically 0 = down, 1 = up, 2 = move
* x: X coordinate
* y: Y coordinate
* pointer: finger index — 1, 2, 3, etc.
* delay: delay before this action in milliseconds
* @param touch1
 Touch point array for the first finger, e.g.:
 ```[{"action":0,"x":1,"y":1,"pointer":1,"delay":20},{"action":2,"x":1,"y":1,"pointer":1,"delay":20}]```
* @param timeout Multi-touch timeout in milliseconds
* @return `{boolean}`

```javascript showLineNumbers
function main() {
 let i = initHid()
 if (!i) {
 return
 }
 // Click at screen corner coordinates
 let click = hidEvent.clickPoint(device.getScreenWidth(), device.getScreenHeight())
 if (click == null) {
 logd("Click succeeded")
 } else {
 console.log("Click failed" + click)
 }
 sleep(1000)
 let doub = hidEvent.doubleClickPoint(300, 400)
 if (doub == null) {
 logd("Double-click succeeded")
 } else {
 console.log("Double-click failed" + doub)
 }

 sleep(1000)
 let ps = hidEvent.press(600, 800, 2000)
 if (ps == null) {
 logd("Long press succeeded")
 } else {
 console.log("Long press failed" + ps)
 }

 sleep(1000)
 let ps1 = hidEvent.press(device.getScreenWidth(), device.getScreenHeight(), 2000)
 if (ps1 == null) {
 logd("Long press succeeded")
 } else {
 console.log("Long press failed" + ps1)
 }


 mtouch()


}

function initHid() {
 // First set the LAN address of the HID program; you can also map it to the public network with FRP and use the public address
 hidEvent.setHidCenter("http://192.168.2.14:8988")
 sleep(3000)
 hidEvent.closeUsbDevice();
 let init = hidEvent.initUsbDevice()
 if (init == null) {
 logd("HID device initialized successfully")
 } else {
 loge("Initialization failed:" + init);
 return false
 }

 return true;
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


main();


```

### swipe Swipe

* Used in network mode
* Swipe with built-in randomization
* Requires EC Android 9.36.0+
* @param x Start x
* @param y Start y
* @param ex End x
* @param ey End y
* @param delay Hold duration in milliseconds
* @return `{string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {
 let i = initHid()
 if (!i) {
 return
 }
}

function initHid() {
 // First set the LAN address of the HID program; you can also map it to the public network with FRP and use the public address
 hidEvent.setHidCenter("http://192.168.2.14:8988")
 hidEvent.closeUsbDevice();
 sleep(3000)
 let init = hidEvent.initUsbDevice()
 if (init == null) {
 logd("HID device initialized successfully")
 } else {
 loge("Initialization failed:" + init);
 return false
 }
 hidEvent.swipe(100, 200, 600, 800, 2000)
 return true;
}

main();
```

### home Home

* Used in network mode
* Requires EC Android 10.21.0+
* @return `{string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {
 let i = initHid()
 if (!i) {
 return
 }
}

function initHid() {
 // First set the LAN address of the HID program; you can also map it to the public network with FRP and use the public address
 hidEvent.setHidCenter("http://192.168.2.14:8988")
 hidEvent.closeUsbDevice();
 sleep(3000)
 let init = hidEvent.initUsbDevice()
 if (init == null) {
 logd("HID device initialized successfully")
 } else {
 loge("Initialization failed:" + init);
 return false
 }
 let r = hidEvent.home()
 logd(r)
 return true;
}

main();
```

### back Back

* Used in network mode
* Requires EC Android 10.21.0+
* @return `{string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {
 let i = initHid()
 if (!i) {
 return
 }
}

function initHid() {
 // First set the LAN address of the HID program; you can also map it to the public network with FRP and use the public address
 hidEvent.setHidCenter("http://192.168.2.14:8988")
 hidEvent.closeUsbDevice();
 sleep(3000)
 let init = hidEvent.initUsbDevice()
 if (init == null) {
 logd("HID device initialized successfully")
 } else {
 loge("Initialization failed:" + init);
 return false
 }
 let r = hidEvent.back()
 logd(r)
 return true;
}

main();
```

### openNotification Open Notification Shade

* Used in network mode
* Requires EC Android 10.21.0+
* @return `{string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {
 let i = initHid()
 if (!i) {
 return
 }
}

function initHid() {
 // First set the LAN address of the HID program; you can also map it to the public network with FRP and use the public address
 hidEvent.setHidCenter("http://192.168.2.14:8988")
 hidEvent.closeUsbDevice();
 sleep(3000)
 let init = hidEvent.initUsbDevice()
 if (init == null) {
 logd("HID device initialized successfully")
 } else {
 loge("Initialization failed:" + init);
 return false
 }
 let r = hidEvent.openNotification()
 logd(r)
 return true;
}

main();
```

### recentApps Recent Apps

* Used in network mode
* Requires EC Android 10.21.0+
* @return `{string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {
 let i = initHid()
 if (!i) {
 return
 }
}

function initHid() {
 // First set the LAN address of the HID program; you can also map it to the public network with FRP and use the public address
 hidEvent.setHidCenter("http://192.168.2.14:8988")
 hidEvent.closeUsbDevice();
 sleep(3000)
 let init = hidEvent.initUsbDevice()
 if (init == null) {
 logd("HID device initialized successfully")
 } else {
 loge("Initialization failed:" + init);
 return false
 }
 let r = hidEvent.recentApps()
 logd(r)
 return true;
}

main();
```

## HID USB Mode Functions {#hid-usb模式函数}

### initUsbDeviceByUsb Initialize HID Device

* [USB mode] Initialize the HID device
* Requires EC Android 10.6.0+
* @return `{string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {
 let i = initHid()
 if (!i) {
 return
 }
}

function initHid() {
 let init = hidEvent.initUsbDeviceByUsb()
 if (init == null) {
 logd("HID device initialized successfully")
 } else {
 loge("Initialization failed:" + init);
 return false
 }
 let swipe = hidEvent.swipeByUsb(100, 200, 600, 800, 2000)
 logd("swipe result " + swipe)
 return true;
}

main();
```

### clickPointByUsb Click Coordinates

* [USB mode] Click coordinates
* Requires EC Android 10.6.0+
* @param x x coordinate
* @param y y coordinate
* @return `{string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {
 let i = initHid()
 if (!i) {
 return
 }
}

function initHid() {
 let init = hidEvent.initUsbDeviceByUsb()
 if (init == null) {
 logd("HID device initialized successfully")
 } else {
 loge("Initialization failed:" + init);
 return false
 }
 let result = hidEvent.clickPointByUsb(600, 800)
 logd(" result " + result)
 return true;
}

main();
```

### doubleClickPointByUsb Double-Click Coordinates

* [USB mode] Double-click coordinates
* Requires EC Android 10.6.0+
* @param x x coordinate
* @param y y coordinate
* @return `{string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {
 let i = initHid()
 if (!i) {
 return
 }
}

function initHid() {
 let init = hidEvent.initUsbDeviceByUsb()
 if (init == null) {
 logd("HID device initialized successfully")
 } else {
 loge("Initialization failed:" + init);
 return false
 }
 let result = hidEvent.doubleClickPointByUsb(600, 800)
 logd(" result " + result)
 return true;
}

main();
```

### pressByUsb Long Press Coordinates

* [USB mode] Long press coordinates
* Requires EC Android 10.6.0+
* @param x x coordinate
* @param y y coordinate
* @param delay Hold duration in milliseconds
* @return `{string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {
 let i = initHid()
 if (!i) {
 return
 }
}

function initHid() {
 let init = hidEvent.initUsbDeviceByUsb()
 if (init == null) {
 logd("HID device initialized successfully")
 } else {
 loge("Initialization failed:" + init);
 return false
 }
 let result = hidEvent.pressByUsb(600, 800, 5000)
 logd(" result " + result)
 return true;
}

main();
```

### multiTouchByUsb Multi-Touch

* [USB mode] Multi-touch
* Requires EC Android 10.6.0+
* Touch parameters: action — typically 0 = down, 1 = up, 2 = move
* x: X coordinate
* y: Y coordinate
* pointer: finger index — 1, 2, 3, etc.
* delay: delay before this action in milliseconds; use more than 40 ms to avoid coordinate drift
* @param touch1 Touch point array for the first finger, e.g.:
  ```[{"action":0,"x":1,"y":1,"pointer":1,"delay":30},{"action":2,"x":1,"y":1,"pointer":1,"delay":30}]```
* @param timeout Multi-touch timeout in milliseconds
* @return `{boolean}`

```javascript showLineNumbers
function main() {
 let i = initHid()
 if (!i) {
 return
 }
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

 let tou = hidEvent.multiTouchByUsb(data, 1000)
 if (tou == null) {
 logd("Multi-touch succeeded")
 } else {
 loge("Multi-touch failed:" + tou);
 return false
 }
}

function initHid() {
 let init = hidEvent.initUsbDeviceByUsb()
 if (init == null) {
 logd("HID device initialized successfully")
 } else {
 loge("Initialization failed:" + init);
 return false
 }
 mtouch();
 return true;
}

main();
```

### swipeByUsb Swipe

* [USB mode] Swipe
* Requires EC Android 10.6.0+
* @param x Start x
* @param y Start y
* @param ex End x
* @param ey End y
* @param delay Hold duration in milliseconds
* @return `{string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {
 let i = initHid()
 if (!i) {
 return
 }
}

function initHid() {
 let init = hidEvent.initUsbDeviceByUsb()
 if (init == null) {
 logd("HID device initialized successfully")
 } else {
 loge("Initialization failed:" + init);
 return false
 }
 let swipe = hidEvent.swipeByUsb(100, 200, 600, 800, 2000)
 logd("swipe result " + swipe)
 return true;
}

main();
```

### homeByUsb Home

* Used in USB mode
* Requires EC Android 10.21.0+
* @return `{string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {
 let i = initHid()
 if (!i) {
 return
 }
}

function initHid() {
 let init = hidEvent.initUsbDeviceByUsb()
 if (init == null) {
 logd("HID device initialized successfully")
 } else {
 loge("Initialization failed:" + init);
 return false
 }
 let r = hidEvent.homeByUsb()
 logd(r)
 return true;
}

main();
```

### backByUsb Back

* Used in USB mode
* Requires EC Android 10.21.0+
* @return `{string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {
 let i = initHid()
 if (!i) {
 return
 }
}

function initHid() {
 let init = hidEvent.initUsbDeviceByUsb()
 if (init == null) {
 logd("HID device initialized successfully")
 } else {
 loge("Initialization failed:" + init);
 return false
 }
 let r = hidEvent.backByUsb()
 logd(r)
 return true;
}

main();
```

### openNotificationByUsb Open Notification Shade

* Used in USB mode
* Requires EC Android 10.21.0+
* @return `{string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {
 let i = initHid()
 if (!i) {
 return
 }
}

function initHid() {
 let init = hidEvent.initUsbDeviceByUsb()
 if (init == null) {
 logd("HID device initialized successfully")
 } else {
 loge("Initialization failed:" + init);
 return false
 }
 let r = hidEvent.openNotificationByUsb()
 logd(r)
 return true;
}

main();
```

### recentAppsByUsb Recent Apps

* Used in USB mode
* Requires EC Android 10.21.0+
* @return `{string}` null on success; otherwise an error message

```javascript showLineNumbers
function main() {
 let i = initHid()
 if (!i) {
 return
 }
}

function initHid() {
 let init = hidEvent.initUsbDeviceByUsb()
 if (init == null) {
 logd("HID device initialized successfully")
 } else {
 loge("Initialization failed:" + init);
 return false
 }
 let r = hidEvent.recentAppsByUsb()
 logd(r)
 return true;
}

main();
```
