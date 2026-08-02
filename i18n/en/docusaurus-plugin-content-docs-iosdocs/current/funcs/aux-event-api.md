---
title: Aux Remote Helper Functions
description: EasyClick automation scripts — iOS no jailbreak auxEvent remote EasyClick Cloud Test Aux gateway
keywords:
 - EasyClick automation scripts iOS no jailbreak auxEvent
 - Aux
 - EasyClick Cloud Test
 - ecauto
 - WiFi
 - USB
 - screencap
 - agent
 - ble
 - otg
 - ime
 - EasyClick
 - mobile automation
 - automation testing
 - script development
 - iOS automation
---
## Overview

`auxEvent` is the **USB control center script** module. It sends commands to the **service** built into **EasyClick Cloud Test** (standalone main app / ecauto) on the phone.

Unlike `bleEvent`, `otgEvent`, and `imeApi`, which you call directly from scripts running in EasyClick Cloud Test, `auxEvent` lets **PC control center scripts** operate the phone remotely over USB or WiFi.

:::tip Where Code Runs and Use Cases
- **auxEvent (this module)**: Runs on the **PC**. The script runs in the control center and sends commands through the EasyClick Cloud Test **service** on the phone.
- **Standalone API**: Runs on the **phone**. The script runs in EasyClick Cloud Test and calls local modules such as `bleEvent` and `device`.
- API names are similar, but **use cases differ completely** — do not mix them.
- `auxEvent` targets EasyClick Cloud Test; for BLE/OTG modules, see the standalone docs and flash the correct BLE/OTG firmware
:::

:::tip Supported Versions
- Requires **EC iOS USB 9.39.0+**
- Install **EasyClick Cloud Test** (standalone main app / ecauto) and enable USB compatibility in app settings
:::

:::tip WiFi Device Discovery (No USB Cable Required)
To use **WiFi scan** without a USB cable, complete **device ID injection** once (with USB connected):

**Method 1: Control center UI**

1. Set the EasyClick Cloud Test **bundleID** as the control center **proxy bundleID**
2. Connect USB, then click **Start Automation Service** in the control center once
3. Open EasyClick Cloud Test → **Settings** → **USB Compatible** and confirm **Device ID** appears

**Method 2: Script injection**

With USB connected and the control center **bridge** running, call either function in your script (`bundleId` = EasyClick Cloud Test package):

- `startAuxBridge(bundleId, opts)` — start the bridge and launch EasyClick Cloud Test
- `launchAppWithAuxEnv(bundleId, opts)` — launch the app and inject the Aux environment

After injection, **Device ID** appears under **Settings → USB Compatible**.

Once the device ID appears, disconnect USB and use `ensureWifiSession` / `scanWifiDevices` on the LAN (phone and PC on the same WiFi).

**Directed Scan (Allowlist)**

In control center, click **Aux Scan** and enter allowed device IDs in the **Device ID list** field:

- **Empty**: allow all injected WiFi devices on LAN to connect
- **Device IDs** (one per line): **only** listed IDs may connect; others are ignored even if discovered

Device ID is shown in EasyClick Cloud Test under **Settings → USB Compatible**. Takes effect on the next scan or when calling `scanWifiDevices` / `ensureWifiSession`.

**Disable WiFi Device Scanning**
- WiFi scan is on by default; to disable, edit `bridgebin/config/config.toml`, set `auxWifiScanEnabled` to 2, restart bridge
:::

:::tip Return Value Convention
- **String returns** (session, recording, BLE, OTG, etc.): `null` or `""` = success; non-empty string = error
- **Boolean returns** (`agent*`, some `util*`): `true` / `false`
:::

### Architecture

```
Control center script → EasyClick Cloud Test Aux service → Agent/Recording/BLE/OTG/IME/Device/Utils
```

### Eight Categories Overview

| # | Category | Prefix | Count | Standalone jslib |
|------|------|------|--------|----------------|
| 1 | Session and bridge | — | 12 | Aux gateway session |
| 2 | Agent automation | `agent*` | 24 | agentEvent.js / AgentApi |
| 3 | System screen recording | `screencap*` | 5 | image.js system recording |
| 4 | OTG remote | `otg*` | 22 | otgEvent.js |
| 5 | BLE remote | `ble*` | 32 | bleEvent.js |
| 6 | Input method | `ime*` | 13 | imeApi.js |
| 7 | Device info | `device*` | 15 | device.js |
| 8 | Utilities and album | `util*` / `auxInsert*` | 8 | utils.js |


## Function Category Index

### 1. Session and Bridge (12)

USB/WiFi session setup, device discovery, connection preferences, health checks

`scanWifiDevices`, `startAuxBridge`, `launchAppWithAuxEnv`, `setConnectType`, `getConnectType`, `ensureUsbSession`, `ensureWifiSession`, `ensureSession`, `closeSession`, `getWifiIp`, `sessionBaseUrl`, `health`

### 2. Agent Automation (24)

Agent forwarded via Aux: environment, gestures, apps, nodes, screenshots

**Environment and Service**:`agentIsServiceOk`, `agentStartEnv`

**Home Screen and Lock Screen**:`agentHome`, `agentHomeScreen`, `agentIsLocked`, `agentLockScreen`, `agentUnlockScreen`

**App Management**:`agentAppLaunch`, `agentCreateSession`, `agentAppKillByBundleId`

**Tap and Swipe**:`agentClickPoint`, `agentClickPointPressure`, `agentPress`, `agentLongClickPoint`, `agentDoubleClickPoint`, `agentSwipeToPoint`, `agentSwipeToPointPressure`, `agentMultiTouch`, `agentIoHIDEvent`

**Text Input**:`agentInputText`

**Nodes and Screenshots**: `agentSetFetchNodeParam`, `agentDumpXml`, `agentCaptureFullScreen`, `agentCaptureFullScreenEx` (`agentDumpXml` with [lockNodeFromXml](./node-api#locknodefromxml-从-xml-锁定节点) enables `getNodeInfo` etc. on PC)

### 3. System Screen Recording (5)

Aux system screen recording broadcast and frame-buffer capture; no automation service required

`screencapStatus`, `screencapStart`, `screencapStop`, `screencapConfig`, `screencapSnapshot`

### 4. OTG Remote (22)

OTG HID forwarded via phone Aux (maps to otgEvent.js)

**Connection and Config**:`otgIsConnect`, `otgGetMacAddress`, `otgRestart`, `otgSetScreenSize`, `otgSetScale`

**Mouse and Touch**:`otgResetZero`, `otgMouseMove`, `otgMouseMoveDistance`, `otgTouchDown`, `otgTouchMove`, `otgTouchUp`, `otgClickPoint`, `otgPress`, `otgDoubleClickPoint`, `otgSwipeToPoint`, `otgMultiTouch`, `otgPressMouseBtn`

**Keys and Other**:`otgSystemKey`, `otgToggleSoftKeyboard`, `otgKeyPressChar`, `otgKeyPress`, `otgLight`

### 5. BLE Remote (32)

BLE HID forwarded via phone Aux (maps to bleEvent.js)

**Connection and Config**:`bleStartConnect`, `bleStopConnect`, `bleIsConnect`, `bleSearchBleIp`, `bleGetConfigBleName`, `bleGetScale`, `bleSetScale`, `bleSetScreenSize`, `bleSetStep`, `bleSetWifiInfo`, `bleSendCmdType`, `bleResetBle`, `bleShowBleName`, `bleHideBleName`

**Mouse and Touch**:`bleResetZero`, `bleMouseMove`, `bleMouseMoveByDistance`, `bleMouseMoveDistance`, `bleTouchDown`, `bleTouchMove`, `bleTouchUp`, `bleClickPoint`, `blePress`, `bleDoubleClickPoint`, `bleSwipeToPoint`, `bleMultiTouch`, `blePressMouseBtn`

**Keys and Other**:`bleSystemKey`, `bleToggleSoftKeyboard`, `bleKeyPressChar`, `bleKeyPress`, `bleLight`

### 6. Input Method (13)

Custom input method forwarded via Aux (maps to imeApi.js)

`imeIsOk`, `imeInput`, `imePaste`, `imePressDel`, `imePressReturn`, `imeDismiss`, `imeCopyToClipboard`, `imeChangeKeyboard`, `imeRemoveAllContent`, `imeGetClipboard`, `imeSetClipboard`, `imeOpenUrl`, `imeGetText`

### 7. Device Information (15)

Device info read via Aux from phone (maps to device.js)

**Basic Info**:`deviceGetDeviceIdentifier`, `deviceGetDeviceId`, `deviceGetModel`, `deviceGetOSVersion`, `deviceGetBattery`, `deviceIsCharging`, `deviceGetScale`

**Screen and Orientation**:`deviceGetScreenWidth`, `deviceGetScreenHeight`, `deviceGetScreenWidthHeight`, `deviceGetWidthNoAuto`, `deviceGetHeightNoAuto`, `deviceGetOrientation`, `deviceGetOrientationNoAuto`, `deviceSetOrientation`

### 8. Utilities and Photo Album (8)

Clipboard, URL, album permissions, and image/video upload

**Utility Functions**:`utilGetClipboardText`, `utilSetClipboardText`, `utilOpenUrl`, `utilRequestPhotoAuthorization`, `utilDeleteAllPhotos`, `utilDeleteAllVideos`

**Album Upload**:`auxInsertImageToAlbum`, `auxInsertVideoToAlbum`

---

## 1. Session and Bridge

USB/WiFi session setup, device discovery, connection preferences, health checks

### scanWifiDevices

* Scan LAN for Aux HTTP devices (WiFi ecauto discovery; results cached in bridge)
* Requires EC iOS USB 9.39.0+
* @param ipRange IP range, e.g. 192.168.2.1 - 192.168.2.255
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    let r = auxEvent.scanWifiDevices("");
    if (!auxEvent.isAuxCallOk(r)) logw("scanWifiDevices failed: {}", r);
    else logd("scanWifiDevices succeeded");
}
main();
```

### startAuxBridge

* Start Aux bridge and launch EasyClick Cloud Test
* Requires EC iOS USB 9.39.0+
* @param bundleId EasyClick Cloud Test bundleId
* @param opts Optional: connectType(usb/wifi), ipRange/wifiScanRange, startScreencap, screencapDelaySec, killExisting
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    let r = auxEvent.startAuxBridge("com.ieasyclick.ecauto", {});
    if (!auxEvent.isAuxCallOk(r)) logw("startAuxBridge failed: {}", r);
    else logd("startAuxBridge succeeded");
}
main();
```

### launchAppWithAuxEnv

* Launch EasyClick Cloud Test and inject Aux environment only
* Requires EC iOS USB 9.39.0+
* @param bundleId EasyClick Cloud Test bundleId
* @param opts Optional start params, same as startAuxBridge
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    let r = auxEvent.launchAppWithAuxEnv("com.ieasyclick.ecauto", {});
    if (!auxEvent.isAuxCallOk(r)) logw("launchAppWithAuxEnv failed: {}", r);
    else logd("launchAppWithAuxEnv succeeded");
}
main();
```

### setConnectType

* Set Aux connection preference (usb/wifi); empty clears; GetSession prefers USB then WiFi
* Requires EC iOS USB 9.39.0+
* Affects routing only; does not open/close session.
* @param connectType usb, wifi, or "" to clear

```javascript showLineNumbers
function main() {
    let r = auxEvent.setConnectType("usb");
    if (!auxEvent.isAuxCallOk(r)) logw("setConnectType failed: {}", r);
    else logd("setConnectType succeeded");
}
main();
```

### getConnectType

* Get current Aux preference; empty if unset
* Requires EC iOS USB 9.39.0+
* @returns `{string}`

```javascript showLineNumbers
function main() {
    let r = auxEvent.getConnectType();
    logd("getConnectType: {}", r);
}
main();
```

### ensureUsbSession

* Ensure USB session with EasyClick Cloud Test
* Requires EC iOS USB 9.39.0+
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    let r = auxEvent.ensureUsbSession();
    if (!auxEvent.isAuxCallOk(r)) logw("ensureUsbSession failed: {}", r);
    else logd("ensureUsbSession succeeded");
}
main();
```

### ensureWifiSession

* Ensure USB control center network session with EasyClick Cloud Test
* Requires EC iOS USB 9.39.0+
* @param ipRange IP range, e.g. 192.168.2.1 - 192.168.2.255
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    let r = auxEvent.ensureWifiSession("");
    if (!auxEvent.isAuxCallOk(r)) logw("ensureWifiSession failed: {}", r);
    else logd("ensureWifiSession succeeded");
}
main();
```

### ensureSession

* Ensure EasyClick Cloud Test session by connection type
* Requires EC iOS USB 9.39.0+
* @param connectType Connection type: usb or wifi
* @param ipRange WiFi scan IP range when connectType is wifi
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    let r = auxEvent.ensureSession("usb", "");
    if (!auxEvent.isAuxCallOk(r)) logw("ensureSession failed: {}", r);
    else logd("ensureSession succeeded");
}
main();
```

### closeSession

* Close EasyClick Cloud Test session
* Requires EC iOS USB 9.39.0+
* @param connectType Optional usb/wifi; empty closes all USB and WiFi sessions
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    let r = auxEvent.closeSession("usb");
    if (!auxEvent.isAuxCallOk(r)) logw("closeSession failed: {}", r);
    else logd("closeSession succeeded");
}
main();
```

### getWifiIp

* Get device EasyClick Cloud Test WiFi IP
* Requires EC iOS USB 9.39.0+
* @returns `{string|null}` WiFi IP or null

```javascript showLineNumbers
function main() {
    let r = auxEvent.getWifiIp();
    logd("getWifiIp: {}", r);
}
main();
```

### sessionBaseUrl

* Get current EasyClick Cloud Test session HTTP baseUrl
* Requires EC iOS USB 9.39.0+
* @returns `{string}` baseUrl or empty on failure

```javascript showLineNumbers
function main() {
    let r = auxEvent.sessionBaseUrl();
    logd("sessionBaseUrl: {}", r);
}
main();
```

### health

* EasyClick Cloud Test HTTP gateway health check
* Requires EC iOS USB 9.39.0+
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    let r = auxEvent.health();
    if (!auxEvent.isAuxCallOk(r)) logw("health failed: {}", r);
    else logd("health succeeded");
}
main();
```

---

---

## 2. Agent Automation

Agent forwarded via Aux: environment, gestures, apps, nodes, screenshots

### Environment and Service

### agentIsServiceOk

* Whether automation service is OK (maps to EcImporter.isServiceOk / basic.isServiceOk)
* Requires EC iOS USB 9.39.0+
* Forwarded via Aux HTTP to standalone Agent API
* @returns `{boolean}` true if healthy

```javascript showLineNumbers
function main() {
    auxEvent.ensureWifiSession("");
    auxEvent.agentStartEnv();
    let ok = auxEvent.agentIsServiceOk();
    logd("agentIsServiceOk: {}", ok);
}
main();
```

### agentStartEnv

* Start automation environment (maps to EcImporter.startEnv / basic.startEnv)
* Requires EC iOS USB 9.39.0+
* Forwarded via Aux HTTP to standalone Agent API
* @returns `{boolean}` true if start succeeded

```javascript showLineNumbers
function main() {
    auxEvent.ensureWifiSession("");
    auxEvent.agentStartEnv();
    let ok = auxEvent.agentStartEnv();
    logd("agentStartEnv: {}", ok);
}
main();
```

### Home Screen and Lock Screen

### agentHome

* Go to home screen (maps to agentEvent.home)
* Requires EC iOS USB 9.39.0+
* @returns `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    auxEvent.ensureWifiSession("");
    auxEvent.agentStartEnv();
    let ok = auxEvent.agentHome();
    logd("agentHome: {}", ok);
}
main();
```

### agentHomeScreen

* Force home screen, unlike home (maps to agentEvent.homeScreen)
* Requires EC iOS USB 9.39.0+
* @returns `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    auxEvent.ensureWifiSession("");
    auxEvent.agentStartEnv();
    let ok = auxEvent.agentHomeScreen();
    logd("agentHomeScreen: {}", ok);
}
main();
```

### agentIsLocked

* Whether screen is locked (maps to agentEvent.isLocked)
* Requires EC iOS USB 9.39.0+
* @returns `{boolean}` true if locked

```javascript showLineNumbers
function main() {
    auxEvent.ensureWifiSession("");
    auxEvent.agentStartEnv();
    let ok = auxEvent.agentIsLocked();
    logd("agentIsLocked: {}", ok);
}
main();
```

### agentLockScreen

* Lock screen (maps to agentEvent.lockScreen)
* Requires EC iOS USB 9.39.0+
* @returns `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    auxEvent.ensureWifiSession("");
    auxEvent.agentStartEnv();
    let ok = auxEvent.agentLockScreen();
    logd("agentLockScreen: {}", ok);
}
main();
```

### agentUnlockScreen

* Unlock screen (maps to agentEvent.unlockScreen)
* Requires EC iOS USB 9.39.0+
* @returns `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    auxEvent.ensureWifiSession("");
    auxEvent.agentStartEnv();
    let ok = auxEvent.agentUnlockScreen();
    logd("agentUnlockScreen: {}", ok);
}
main();
```

### App Management

### agentAppLaunch

* Open app (maps to AgentApi.appLaunch / agentEvent.appLaunchEx)
* Requires EC iOS USB 9.39.0+
* @param bundleId App bundleId
* @param ignoreState 1=ignore previous open state; else ""
* @returns `{boolean}` true on success

```javascript showLineNumbers
function main() {
    auxEvent.ensureWifiSession("");
    auxEvent.agentStartEnv();
    let ok = auxEvent.agentAppLaunch("com.ieasyclick.ecauto", "");
    logd("agentAppLaunch: {}", ok);
}
main();
```

### agentCreateSession

* Create automation session (maps to AgentApi.createSession)
* Requires EC iOS USB 9.39.0+
* @param bundleId Optional; empty binds current foreground app
* @returns `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    auxEvent.ensureWifiSession("");
    auxEvent.agentStartEnv();
    let ok = auxEvent.agentCreateSession("com.ieasyclick.ecauto");
    logd("agentCreateSession: {}", ok);
}
main();
```

### agentAppKillByBundleId

* Kill process by bundleId (maps to AgentApi.appKillByBundleId / agentEvent.appKillByBundleIdEx)
* Requires EC iOS USB 9.39.0+
* @param bundleId App bundleId
* @param ignoreState 1=ignore previous state; else ""
* @returns `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    auxEvent.ensureWifiSession("");
    auxEvent.agentStartEnv();
    let ok = auxEvent.agentAppKillByBundleId("com.ieasyclick.ecauto", "");
    logd("agentAppKillByBundleId: {}", ok);
}
main();
```

### Tap and Swipe

### agentClickPoint

* Click point (maps to agentEvent.clickPoint)
* Requires EC iOS USB 9.39.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{boolean}` true if click succeeded

```javascript showLineNumbers
function main() {
    auxEvent.ensureWifiSession("");
    auxEvent.agentStartEnv();
    let ok = auxEvent.agentClickPoint(300, 400);
    logd("agentClickPoint: {}", ok);
}
main();
```

### agentClickPointPressure

* Click point with pressure (maps to agentEvent.clickPointPressure)
* Requires EC iOS USB 9.39.0+
* @param x X coordinate
* @param y Y coordinate
* @param pressure Pressure 0–1
* @returns `{boolean}` true if click succeeded

```javascript showLineNumbers
function main() {
    auxEvent.ensureWifiSession("");
    auxEvent.agentStartEnv();
    let ok = auxEvent.agentClickPointPressure(300, 400, 0.5);
    logd("agentClickPointPressure: {}", ok);
}
main();
```

### agentPress

* Press coordinate (maps to agentEvent.press)
* Requires EC iOS USB 9.39.0+
* @param x X coordinate
* @param y Y coordinate
* @param delay Duration in ms
* @returns `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    auxEvent.ensureWifiSession("");
    auxEvent.agentStartEnv();
    let ok = auxEvent.agentPress(300, 400, 1000);
    logd("agentPress: {}", ok);
}
main();
```

### agentLongClickPoint

* Long-press coordinate (maps to agentEvent.longClickPoint)
* Requires EC iOS USB 9.39.0+
* @param x X coordinate
* @param y Y coordinate
* @param delay Duration in ms
* @returns `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    auxEvent.ensureWifiSession("");
    auxEvent.agentStartEnv();
    let ok = auxEvent.agentLongClickPoint(300, 400, 1000);
    logd("agentLongClickPoint: {}", ok);
}
main();
```

### agentDoubleClickPoint

* Double-click coordinate (maps to agentEvent.doubleClickPoint)
* Requires EC iOS USB 9.39.0+
* @param x X coordinate
* @param y Y coordinate
* @returns `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    auxEvent.ensureWifiSession("");
    auxEvent.agentStartEnv();
    let ok = auxEvent.agentDoubleClickPoint(300, 400);
    logd("agentDoubleClickPoint: {}", ok);
}
main();
```

### agentSwipeToPoint

* Swipe to coordinate (maps to agentEvent.swipeToPoint)
* Requires EC iOS USB 9.39.0+
* @param startX Start X
* @param startY Start Y
* @param endX End X
* @param endY End Y
* @param duration Duration in ms
* @returns `{boolean}` true if swipe succeeded

```javascript showLineNumbers
function main() {
    auxEvent.ensureWifiSession("");
    auxEvent.agentStartEnv();
    let ok = auxEvent.agentSwipeToPoint(300, 400, 500, 600, 1000);
    logd("agentSwipeToPoint: {}", ok);
}
main();
```

### agentSwipeToPointPressure

* Swipe to coordinate with pressure (maps to agentEvent.swipeToPointPressure)
* Requires EC iOS USB 9.39.0+
* @param startX Start X
* @param startY Start Y
* @param endX End X
* @param endY End Y
* @param duration Duration in ms
* @param pressure Pressure 0–1
* @returns `{boolean}` true if swipe succeeded

```javascript showLineNumbers
function main() {
    auxEvent.ensureWifiSession("");
    auxEvent.agentStartEnv();
    let ok = auxEvent.agentSwipeToPointPressure(300, 400, 500, 600, 1000, 0.5);
    logd("agentSwipeToPointPressure: {}", ok);
}
main();
```

### agentMultiTouch

* Multi-touch (maps to agentEvent.multiTouch)
* Requires EC iOS USB 9.39.0+
* @param touch1 Touch point array or JSON string
* @param timeout Total timeout in ms
* @returns `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    auxEvent.ensureWifiSession("");
    auxEvent.agentStartEnv();
    let touch1 = [{"action":0,"x":300,"y":400,"pointer":1,"delay":20},{"action":1,"x":300,"y":400,"pointer":1,"delay":20}];
    let r = auxEvent.agentMultiTouch(touch1, 1000);
    logd("agentMultiTouch: {}", r);
}
main();
```

### agentIoHIDEvent

* Simulate HID event (maps to agentEvent.ioHIDEvent)
* Requires EC iOS USB 9.39.0+
* @param eventPageID HID event page ID
* @param eventUsageID HID event usage ID
* @param delay Duration; 0.2 is usually enough
* @returns `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    auxEvent.ensureWifiSession("");
    auxEvent.agentStartEnv();
    let ok = auxEvent.agentIoHIDEvent("11", "1", 1000);
    logd("agentIoHIDEvent: {}", ok);
}
main();
```

### Text Input

### agentInputText

* Input text (maps to agentEvent.inputText)
* Requires EC iOS USB 9.39.0+
* @param content Content
* @param duration Execution time in ms
* @returns `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    auxEvent.ensureWifiSession("");
    auxEvent.agentStartEnv();
    let ok = auxEvent.agentInputText("test content", 1000);
    logd("agentInputText: {}", ok);
}
main();
```

### Nodes and Screenshots

For WiFi with [Node functions](./node-api) selectors (`label().getNodeInfo()`, etc.): `agentSetFetchNodeParam` → `agentDumpXml` → `lockNodeFromXml` → search. For remote phone-side matching use [nodeAgent](./node-agent-api).

### agentSetFetchNodeParam

* Set node fetch base params (maps to agentEvent.setFetchNodeParam)
* Requires EC iOS USB 9.39.0+
* visibleFilter: 1 = fetch regardless of visible; 2 = only visible=true nodes
* labelFilter: 1 = fetch regardless of label; 2 = only nodes with a label value
* maxDepth: tree depth; recommended 1–500
* maxChildCount: max child nodes; smaller=fewer; 0=unlimited
* excludedAttributes: Comma-separated attributes to exclude for faster fetching, e.g. visible,selected,enable
* @param ext Config map with visibleFilter, labelFilter, etc.
* @returns `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    auxEvent.ensureWifiSession("");
    auxEvent.agentStartEnv();
    let ok = auxEvent.agentSetFetchNodeParam({maxDepth:20,excludedAttributes:"visible,selected,enable,accessible"});
    logd("agentSetFetchNodeParam: {}", ok);
}
main();
```

### agentDumpXml

* Export node tree as XML (maps to agentEvent.dumpXml)
* Requires EC iOS USB 9.39.0+
* Forwarded via Aux HTTP to phone Agent for XML string; does **not** auto-write PC node cache
* To keep using [Node functions](./node-api) selectors (`label().getNodeInfo()`, etc.), use [lockNodeFromXml](./node-api#locknodefromxml-从-xml-锁定节点) then search
* @returns `{string}` XML or empty on failure

```javascript showLineNumbers
function main() {
    auxEvent.ensureWifiSession("");
    auxEvent.agentStartEnv();
    auxEvent.agentSetFetchNodeParam({
        labelFilter: "2",
        maxDepth: "20",
        visibleFilter: "2",
        excludedAttributes:"visible,selected,enable,accessible"
    });

    let xml = auxEvent.agentDumpXml();
    logd("xml length: {}", xml ? xml.length : 0);
    if (!xml) {
        logd("agentDumpXml failed");
        return;
    }

    releaseNode();
    let ok = lockNodeFromXml(xml);
    logd("lockNodeFromXml: {}", ok);
    if (!ok) {
        return;
    }

    let nd = label("Settings").getOneNodeInfo(0);
    logd("node: {}", nd ? JSON.stringify(nd) : "null");
}
main();
```

See [Node functions — lockNodeFromXml](./node-api#locknodefromxml-从-xml-锁定节点); object form: `agentEvent.lockNodeFromXml(xml)`. Remote phone-side selection: [nodeAgent](./node-agent-api).

### agentCaptureFullScreen

* Capture full-screen JPG (maps to AgentCall.captureFullScreenData / image.captureFullScreen)
* Requires EC iOS USB 9.39.0+
* Forwarded via Aux HTTP; returns AutoImage from image binary
* @returns `{null|AutoImage}`

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let img = auxEvent.agentCaptureFullScreen();
    if (img) { logd("image {}x{}", img.getWidth(), img.getHeight()); img.recycle(); }
}
main();
```

### agentCaptureFullScreenEx

* Capture full screen with format/quality (maps to AgentCall.captureFullScreenDataEx / image.captureFullScreenEx)
* Requires EC iOS USB 9.39.0+
* Forwarded via Aux HTTP; returns AutoImage from image binary
* type: 0/1=JPG mode 1, 2=JPG mode 2, 3=PNG
* quality: 1-100
* @param ext Extended params, e.g. `{type: "1", quality: 99}`
* @returns `{null|AutoImage}`

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let img = auxEvent.agentCaptureFullScreenEx({type:"1",quality:90});
    if (img) { logd("image {}x{}", img.getWidth(), img.getHeight()); img.recycle(); }
}
main();
```

---

---

## 3. System Screen Recording

Aux system screen recording broadcast and frame-buffer capture

### screencapStatus

* Query recording broadcast and frame-buffer status (Aux-specific; like image.isSystemScreenCapBroadcasting with buffer)
* Requires EC iOS USB 9.39.0+ (maps to standalone image system recording 6.6.0+)
* Start recording receiver first; or from app Settings → System Screen Recording
* @returns `{string|null}` null/empty=recording ready

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.screencapStatus();
    if (!auxEvent.isAuxCallOk(r)) logw("screencapStatus failed: {}", r);
    else logd("screencapStatus succeeded");
}
main();
```

### screencapStart

* Start system screen recording broadcast (maps to image.startSystemScreenCapBroadcast)
* Requires EC iOS USB 9.39.0+ (maps to standalone image system recording 6.6.0+)
* App must be foreground, or start from Settings → System Screen Recording (Broadcast Picker requires user tap)
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.screencapStart();
    if (!auxEvent.isAuxCallOk(r)) logw("screencapStart failed: {}", r);
    else logd("screencapStart succeeded");
}
main();
```

### screencapStop

* Stop system screen recording broadcast (maps to image.stopSystemScreenCapServer)
* Requires EC iOS USB 9.39.0+ (maps to standalone image system recording 6.6.0+)
* @returns `{string|null}` null/empty=stopped; else error/broadcasting

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.screencapStop();
    if (!auxEvent.isAuxCallOk(r)) logw("screencapStop failed: {}", r);
    else logd("screencapStop succeeded");
}
main();
```

### screencapConfig

* Configure recording FPS and JPEG quality (maps to image.setSystemScreenCapFps / setSystemScreenCapJpegQuality)
* Requires EC iOS USB 9.39.0+ (maps to standalone image system recording 6.6.0+)
* fps = frames per second (e.g. 10/sec); don't set too high; config read on each broadcast start
* jpegQuality is JPEG quality 1–100; higher = clearer, more resources
* @param config Config object, e.g. `{frameRate, jpegQuality}`
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.screencapConfig({frameRate:10,jpegQuality:80});
    if (!auxEvent.isAuxCallOk(r)) logw("screencapConfig failed: {}", r);
    else logd("screencapConfig succeeded");
}
main();
```

### screencapSnapshot

* Get current screen JPEG snapshot (maps to image.getSystemScreenCapImage)
* Requires EC iOS USB 9.39.0+ (maps to standalone image system recording 6.6.0+)
* @returns `{null|AutoImage}` capture; null if no frame/instance

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let img = auxEvent.screencapSnapshot();
    if (img) { logd("image {}x{}", img.getWidth(), img.getHeight()); img.recycle(); }
}
main();
```

---

---

## 4. OTG Remote

OTG HID forwarded via phone Aux (maps to otgEvent.js)

### Connection and Config

### otgIsConnect

* Query OTG connection status (maps to otgEvent.isConnect)
* Requires EC iOS USB 9.39.0+ (maps to standalone otgEvent 6.6.0+)
* @returns `{string|null}` null/empty=connected; else error

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgIsConnect();
    if (!auxEvent.isAuxCallOk(r)) logw("otgIsConnect failed: {}", r);
    else logd("otgIsConnect succeeded");
}
main();
```

---

### otgGetMacAddress

* Get OTG board MAC address (maps to otgEvent.getMacAddress)
* Requires EC iOS USB 9.39.0+ (maps to standalone otgEvent 6.6.0+)
* @returns `{string}` MAC or error

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgGetMacAddress();
    if (!auxEvent.isAuxCallOk(r)) logw("otgGetMacAddress failed: {}", r);
    else logd("otgGetMacAddress succeeded");
}
main();
```

### otgRestart

* Restart OTG board (like RST) (maps to otgEvent.restart)
* Requires EC iOS USB 9.39.0+ (maps to standalone otgEvent 6.6.0+)
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgRestart();
    if (!auxEvent.isAuxCallOk(r)) logw("otgRestart failed: {}", r);
    else logd("otgRestart succeeded");
}
main();
```

### otgSetScreenSize

* Set OTG screen size (prevent cursor leaving screen) (maps to otgEvent.setScreenSize)
* Requires EC iOS USB 9.39.0+ (maps to standalone otgEvent 6.6.0+)
* @param w Screen width
* @param h Screen height
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgSetScreenSize(390, 844);
    if (!auxEvent.isAuxCallOk(r)) logw("otgSetScreenSize failed: {}", r);
    else logd("otgSetScreenSize succeeded");
}
main();
```

### otgSetScale

* Set OTG mouse scale (maps to otgEvent.setScale)
* Requires EC iOS USB 9.39.0+ (maps to standalone otgEvent 6.6.0+)
* @param x_scale X scale compensation
* @param y_scale Y scale compensation
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgSetScale(2.0, 2.0);
    if (!auxEvent.isAuxCallOk(r)) logw("otgSetScale failed: {}", r);
    else logd("otgSetScale succeeded");
}
main();
```

### Mouse and Touch

### otgResetZero

* OTG mouse reset to zero (maps to otgEvent.resetZero)
* Requires EC iOS USB 9.39.0+ (maps to standalone otgEvent 6.6.0+)
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgResetZero();
    if (!auxEvent.isAuxCallOk(r)) logw("otgResetZero failed: {}", r);
    else logd("otgResetZero succeeded");
}
main();
```

### otgMouseMove

* OTG move mouse (relative) (maps to otgEvent.mouseMove)
* Requires EC iOS USB 9.39.0+ (maps to standalone otgEvent 6.6.0+)
* @param x X displacement
* @param y Y displacement
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgMouseMove(300, 400);
    if (!auxEvent.isAuxCallOk(r)) logw("otgMouseMove failed: {}", r);
    else logd("otgMouseMove succeeded");
}
main();
```

### otgMouseMoveDistance

* OTG move mouse by pixel distance (maps to otgEvent.mouseMoveDistance)
* Requires EC iOS USB 9.39.0+ (maps to standalone otgEvent 6.6.0+)
* @param x_dis X displacement
* @param y_dis Y displacement
* @param press Whether pressed while moving
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgMouseMoveDistance(10, 10, false);
    if (!auxEvent.isAuxCallOk(r)) logw("otgMouseMoveDistance failed: {}", r);
    else logd("otgMouseMoveDistance succeeded");
}
main();
```

### otgTouchDown

* OTG touch down (maps to otgEvent.touchDown)
* Requires EC iOS USB 9.39.0+ (maps to standalone otgEvent 6.6.0+)
* @param x X coordinate
* @param y Y coordinate
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgTouchDown(300, 400);
    if (!auxEvent.isAuxCallOk(r)) logw("otgTouchDown failed: {}", r);
    else logd("otgTouchDown succeeded");
}
main();
```

### otgTouchMove

* OTG touch move (maps to otgEvent.touchMove)
* Requires EC iOS USB 9.39.0+ (maps to standalone otgEvent 6.6.0+)
* @param x X coordinate
* @param y Y coordinate
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgTouchMove(300, 400);
    if (!auxEvent.isAuxCallOk(r)) logw("otgTouchMove failed: {}", r);
    else logd("otgTouchMove succeeded");
}
main();
```

### otgTouchUp

* OTG touch up (maps to otgEvent.touchUp)
* Requires EC iOS USB 9.39.0+ (maps to standalone otgEvent 6.6.0+)
* @param x X coordinate
* @param y Y coordinate
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgTouchUp(300, 400);
    if (!auxEvent.isAuxCallOk(r)) logw("otgTouchUp failed: {}", r);
    else logd("otgTouchUp succeeded");
}
main();
```

### otgClickPoint

* OTG click point (maps to otgEvent.clickPoint)
* Requires EC iOS USB 9.39.0+ (maps to standalone otgEvent 6.6.0+)
* @param x X coordinate
* @param y Y coordinate
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgClickPoint(300, 400);
    if (!auxEvent.isAuxCallOk(r)) logw("otgClickPoint failed: {}", r);
    else logd("otgClickPoint succeeded");
}
main();
```

### otgPress

* OTG long-press point (maps to otgEvent.press)
* Requires EC iOS USB 9.39.0+ (maps to standalone otgEvent 6.6.0+)
* @param x X coordinate
* @param y Y coordinate
* @param delay Long-press duration in ms
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgPress(300, 400, 1000);
    if (!auxEvent.isAuxCallOk(r)) logw("otgPress failed: {}", r);
    else logd("otgPress succeeded");
}
main();
```

### otgDoubleClickPoint

* OTG double-click point (maps to otgEvent.doubleClickPoint)
* Requires EC iOS USB 9.39.0+ (maps to standalone otgEvent 6.6.0+)
* @param x X coordinate
* @param y Y coordinate
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgDoubleClickPoint(300, 400);
    if (!auxEvent.isAuxCallOk(r)) logw("otgDoubleClickPoint failed: {}", r);
    else logd("otgDoubleClickPoint succeeded");
}
main();
```

### otgSwipeToPoint

* OTG swipe to point (maps to otgEvent.swipeToPoint)
* Requires EC iOS USB 9.39.0+ (maps to standalone otgEvent 6.6.0+)
* @param startX Start X
* @param startY Start Y
* @param endX End X
* @param endY End Y
* @param duration Duration in ms
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgSwipeToPoint(300, 400, 500, 600, 1000);
    if (!auxEvent.isAuxCallOk(r)) logw("otgSwipeToPoint failed: {}", r);
    else logd("otgSwipeToPoint succeeded");
}
main();
```

### otgMultiTouch

* OTG multi-touch (maps to otgEvent.multiTouch)
* Requires EC iOS USB 9.39.0+ (maps to standalone otgEvent 6.6.0+)
* Touch params: action 0=down, 1=up, 2=move; pointer=finger index; delay=ms
* @param touch1 Touch array, e.g. `[{"action":0,"x":1,"y":1,"pointer":1,"delay":20}]`
* @param timeout Multi-touch total timeout in ms
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let touch1 = [{"action":0,"x":300,"y":400,"pointer":1,"delay":20},{"action":1,"x":300,"y":400,"pointer":1,"delay":20}];
    let r = auxEvent.otgMultiTouch(touch1, 1000);
    if (!auxEvent.isAuxCallOk(r)) logw("otgMultiTouch failed: {}", r); else logd("otgMultiTouch succeeded");
}
main();
```

### otgPressMouseBtn

* OTG mouse button click (AssistiveTouch custom button, etc.) (maps to otgEvent.pressMouseBtn)
* Requires EC iOS USB 9.39.0+ (maps to standalone otgEvent 6.6.0+)
* @param btn 1=left, 2=right, 3=scroll, 4-8=custom
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgPressMouseBtn(1);
    if (!auxEvent.isAuxCallOk(r)) logw("otgPressMouseBtn failed: {}", r);
    else logd("otgPressMouseBtn succeeded");
}
main();
```

### Keys and Other

### otgSystemKey

* OTG system key (maps to otgEvent.systemKey)
* Requires EC iOS USB 9.39.0+ (maps to standalone otgEvent 6.6.0+)
* @param key e.g. home, recents
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgSystemKey("home");
    if (!auxEvent.isAuxCallOk(r)) logw("otgSystemKey failed: {}", r);
    else logd("otgSystemKey succeeded");
}
main();
```

### otgToggleSoftKeyboard

* OTG toggle soft keyboard (maps to otgEvent.toggleSoftKeyboard)
* Requires EC iOS USB 9.39.0+ (maps to standalone otgEvent 6.6.0+)
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgToggleSoftKeyboard();
    if (!auxEvent.isAuxCallOk(r)) logw("otgToggleSoftKeyboard failed: {}", r);
    else logd("otgToggleSoftKeyboard succeeded");
}
main();
```

### otgKeyPressChar

* OTG character key (maps to otgEvent.keyPressChar)
* Requires EC iOS USB 9.39.0+ (maps to standalone otgEvent 6.6.0+)
* @param prefix Modifiers: alt, ctrl, gui, shift, r_ctrl, r_shift; may be empty
* @param code Character, e.g. a, Enter, BS=delete
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgKeyPressChar("", "a");
    if (!auxEvent.isAuxCallOk(r)) logw("otgKeyPressChar failed: {}", r);
    else logd("otgKeyPressChar succeeded");
}
main();
```

### otgKeyPress

* OTG key press (ASCII) (maps to otgEvent.keyPress)
* Requires EC iOS USB 9.39.0+ (maps to standalone otgEvent 6.6.0+)
* @param prefix Modifier key; may be empty
* @param code Integer ASCII code; see https://tool.oschina.net/commons?type=4
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgKeyPress("", 65);
    if (!auxEvent.isAuxCallOk(r)) logw("otgKeyPress failed: {}", r);
    else logd("otgKeyPress succeeded");
}
main();
```

### otgLight

* OTG board LED blink (maps to otgEvent.light)
* Requires EC iOS USB 9.39.0+ (maps to standalone otgEvent 6.6.0+)
* @param num Blink count
* @param lightToOff On-to-off interval in ms
* @param offToLight Off-to-on interval in ms
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgLight(3, 200, 200);
    if (!auxEvent.isAuxCallOk(r)) logw("otgLight failed: {}", r);
    else logd("otgLight succeeded");
}
main();
```

---

## 5. BLE Remote

BLE HID forwarded via phone Aux (maps to bleEvent.js)

### Connection and Config

### bleStartConnect

* Scan and connect BLE board (maps to bleEvent.startConnect)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @param bleDeviceName BLE device name
* @param save Save connection info
* @param timeout Timeout in ms
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleStartConnect("", false, 1000);
    if (!auxEvent.isAuxCallOk(r)) logw("bleStartConnect failed: {}", r);
    else logd("bleStartConnect succeeded");
}
main();
```

### bleStopConnect

* Disconnect BLE (maps to bleEvent.stopConnect)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleStopConnect();
    if (!auxEvent.isAuxCallOk(r)) logw("bleStopConnect failed: {}", r);
    else logd("bleStopConnect succeeded");
}
main();
```

### bleIsConnect

* Query BLE connection status (maps to bleEvent.isConnected)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @returns `{boolean}` true if connected

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let ok = auxEvent.bleIsConnect();
    logd("bleIsConnect: {}", ok);
}
main();
```

---

### bleSearchBleIp

* Search BLE board IP (maps to bleEvent.searchBleIp)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @param force Force re-search
* @param timeout Timeout in ms
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleSearchBleIp(false, 1000);
    if (!auxEvent.isAuxCallOk(r)) logw("bleSearchBleIp failed: {}", r);
    else logd("bleSearchBleIp succeeded");
}
main();
```

### bleGetConfigBleName

* Get app-configured BLE name (maps to bleEvent.getConfigBleName)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.5.0+)
* @returns `{string}` Configured BLE name or empty

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleGetConfigBleName();
    if (!auxEvent.isAuxCallOk(r)) logw("bleGetConfigBleName failed: {}", r);
    else logd("bleGetConfigBleName succeeded");
}
main();
```

### bleGetScale

* Get current BLE mouse scale (maps to bleEvent.getScale)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @returns `{string}` Scale JSON or error

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleGetScale();
    if (!auxEvent.isAuxCallOk(r)) logw("bleGetScale failed: {}", r);
    else logd("bleGetScale succeeded");
}
main();
```

### bleSetScale

* Set BLE mouse scale (maps to bleEvent.setScale)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @param x_scale X scale float; default ~2.0
* @param y_scale Y scale float; default ~2.0
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleSetScale(2.0, 2.0);
    if (!auxEvent.isAuxCallOk(r)) logw("bleSetScale failed: {}", r);
    else logd("bleSetScale succeeded");
}
main();
```

### bleSetScreenSize

* Set BLE screen size (prevent cursor leaving screen) (maps to bleEvent.setScreenSize)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @param w Screen width
* @param h Screen height
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleSetScreenSize(390, 844);
    if (!auxEvent.isAuxCallOk(r)) logw("bleSetScreenSize failed: {}", r);
    else logd("bleSetScreenSize succeeded");
}
main();
```

### bleSetStep

* BLE set mouse step (maps to bleEvent.setStep)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @param step Step value
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleSetStep(1);
    if (!auxEvent.isAuxCallOk(r)) logw("bleSetStep failed: {}", r);
    else logd("bleSetStep succeeded");
}
main();
```

### bleSetWifiInfo

* Configure BLE board WiFi (SSID/password) (maps to bleEvent.setWifiInfo)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @param name WiFi name
* @param pwd WiFi password
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleSetWifiInfo("WiFiName", "password");
    if (!auxEvent.isAuxCallOk(r)) logw("bleSetWifiInfo failed: {}", r);
    else logd("bleSetWifiInfo succeeded");
}
main();
```

### bleSendCmdType

* Set BLE transport (serial/network) (maps to bleEvent.sendCmdType)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @param type 1=serial, 2=network
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleSendCmdType(1);
    if (!auxEvent.isAuxCallOk(r)) logw("bleSendCmdType failed: {}", r);
    else logd("bleSendCmdType succeeded");
}
main();
```

### bleResetBle

* Reset BLE connection/state (maps to bleEvent.resetBle)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleResetBle();
    if (!auxEvent.isAuxCallOk(r)) logw("bleResetBle failed: {}", r);
    else logd("bleResetBle succeeded");
}
main();
```

### bleShowBleName

* Show BLE device name (calibration/pairing) (maps to bleEvent.showBleName)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleShowBleName();
    if (!auxEvent.isAuxCallOk(r)) logw("bleShowBleName failed: {}", r);
    else logd("bleShowBleName succeeded");
}
main();
```

### bleHideBleName

* Hide BLE device name (maps to bleEvent.hideBleName)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleHideBleName();
    if (!auxEvent.isAuxCallOk(r)) logw("bleHideBleName failed: {}", r);
    else logd("bleHideBleName succeeded");
}
main();
```

### Mouse and Touch

### bleResetZero

* BLE mouse reset to zero (maps to bleEvent.resetZero)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleResetZero();
    if (!auxEvent.isAuxCallOk(r)) logw("bleResetZero failed: {}", r);
    else logd("bleResetZero succeeded");
}
main();
```

### bleMouseMove

* BLE move mouse (relative) (maps to bleEvent.mouseMove)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @param x X displacement
* @param y Y displacement
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleMouseMove(300, 400);
    if (!auxEvent.isAuxCallOk(r)) logw("bleMouseMove failed: {}", r);
    else logd("bleMouseMove succeeded");
}
main();
```

### bleMouseMoveByDistance

* BLE move mouse by distance (absolute step) (maps to bleEvent.mouseMoveByDistance)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @param x_dis X displacement
* @param y_dis Y displacement
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleMouseMoveByDistance(10, 10);
    if (!auxEvent.isAuxCallOk(r)) logw("bleMouseMoveByDistance failed: {}", r);
    else logd("bleMouseMoveByDistance succeeded");
}
main();
```

### bleMouseMoveDistance

* BLE move mouse by distance (optional press) (maps to bleEvent.mouseMoveDistance)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @param x_dis X displacement
* @param y_dis Y displacement
* @param press Whether pressed while moving
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleMouseMoveDistance(10, 10, false);
    if (!auxEvent.isAuxCallOk(r)) logw("bleMouseMoveDistance failed: {}", r);
    else logd("bleMouseMoveDistance succeeded");
}
main();
```

### bleTouchDown

* BLE touch down (maps to bleEvent.touchDown)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @param x X coordinate
* @param y Y coordinate
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleTouchDown(300, 400);
    if (!auxEvent.isAuxCallOk(r)) logw("bleTouchDown failed: {}", r);
    else logd("bleTouchDown succeeded");
}
main();
```

### bleTouchMove

* BLE touch move (maps to bleEvent.touchMove)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @param x X coordinate
* @param y Y coordinate
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleTouchMove(300, 400);
    if (!auxEvent.isAuxCallOk(r)) logw("bleTouchMove failed: {}", r);
    else logd("bleTouchMove succeeded");
}
main();
```

### bleTouchUp

* BLE touch up (maps to bleEvent.touchUp)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @param x X coordinate
* @param y Y coordinate
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleTouchUp(300, 400);
    if (!auxEvent.isAuxCallOk(r)) logw("bleTouchUp failed: {}", r);
    else logd("bleTouchUp succeeded");
}
main();
```

### bleClickPoint

* BLE click point (maps to bleEvent.clickPoint)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @param x X coordinate
* @param y Y coordinate
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleClickPoint(300, 400);
    if (!auxEvent.isAuxCallOk(r)) logw("bleClickPoint failed: {}", r);
    else logd("bleClickPoint succeeded");
}
main();
```

### blePress

* BLE long-press point (maps to bleEvent.press)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @param x X coordinate
* @param y Y coordinate
* @param delay Long-press duration in ms
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.blePress(300, 400, 1000);
    if (!auxEvent.isAuxCallOk(r)) logw("blePress failed: {}", r);
    else logd("blePress succeeded");
}
main();
```

### bleDoubleClickPoint

* BLE double-click point (maps to bleEvent.doubleClickPoint)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @param x X coordinate
* @param y Y coordinate
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleDoubleClickPoint(300, 400);
    if (!auxEvent.isAuxCallOk(r)) logw("bleDoubleClickPoint failed: {}", r);
    else logd("bleDoubleClickPoint succeeded");
}
main();
```

### bleSwipeToPoint

* BLE swipe to point (maps to bleEvent.swipeToPoint)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @param startX Start X
* @param startY Start Y
* @param endX End X
* @param endY End Y
* @param duration Duration in ms
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleSwipeToPoint(300, 400, 500, 600, 1000);
    if (!auxEvent.isAuxCallOk(r)) logw("bleSwipeToPoint failed: {}", r);
    else logd("bleSwipeToPoint succeeded");
}
main();
```

### bleMultiTouch

* BLE multi-touch (maps to bleEvent.multiTouch)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @param touch1 Touch array; same format as otgMultiTouch
* @param timeout Total timeout in ms
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let touch1 = [{"action":0,"x":300,"y":400,"pointer":1,"delay":20},{"action":1,"x":300,"y":400,"pointer":1,"delay":20}];
    let r = auxEvent.bleMultiTouch(touch1, 1000);
    if (!auxEvent.isAuxCallOk(r)) logw("bleMultiTouch failed: {}", r); else logd("bleMultiTouch succeeded");
}
main();
```

### blePressMouseBtn

* BLE mouse button click (maps to bleEvent.pressMouseBtn)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @param btn Mouse button: 1=left, 2=right, 3=scroll
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.blePressMouseBtn(1);
    if (!auxEvent.isAuxCallOk(r)) logw("blePressMouseBtn failed: {}", r);
    else logd("blePressMouseBtn succeeded");
}
main();
```

### Keys and Other

### bleSystemKey

* BLE system key (maps to bleEvent.systemKey)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @param key e.g. home, recents
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleSystemKey("home");
    if (!auxEvent.isAuxCallOk(r)) logw("bleSystemKey failed: {}", r);
    else logd("bleSystemKey succeeded");
}
main();
```

### bleToggleSoftKeyboard

* BLE toggle soft keyboard (maps to bleEvent.toggleSoftKeyboard)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleToggleSoftKeyboard();
    if (!auxEvent.isAuxCallOk(r)) logw("bleToggleSoftKeyboard failed: {}", r);
    else logd("bleToggleSoftKeyboard succeeded");
}
main();
```

### bleKeyPressChar

* BLE character key (maps to bleEvent.keyPressChar)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @param prefix Modifier key; may be empty
* @param code Character
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleKeyPressChar("", "a");
    if (!auxEvent.isAuxCallOk(r)) logw("bleKeyPressChar failed: {}", r);
    else logd("bleKeyPressChar succeeded");
}
main();
```

### bleKeyPress

* BLE key press (ASCII) (maps to bleEvent.keyPress)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @param prefix Modifier key; may be empty
* @param code Integer ASCII code
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleKeyPress("", 65);
    if (!auxEvent.isAuxCallOk(r)) logw("bleKeyPress failed: {}", r);
    else logd("bleKeyPress succeeded");
}
main();
```

### bleLight

* BLE board LED blink (maps to bleEvent.light)
* Requires EC iOS USB 9.39.0+ (maps to standalone bleEvent 6.6.0+)
* @param num Blink count
* @param lightToOff On-to-off interval in ms
* @param offToLight Off-to-on interval in ms
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleLight(3, 200, 200);
    if (!auxEvent.isAuxCallOk(r)) logw("bleLight failed: {}", r);
    else logd("bleLight succeeded");
}
main();
```

---

## 6. Input Method

Custom input method forwarded via Aux (maps to imeApi.js)

### imeIsOk

* Whether input method is ready (maps to imeApi.isOk)
* Requires EC iOS USB 9.39.0+ (maps to standalone imeApi 3.15.0+)
* Forwarded via Aux HTTP to standalone ImeApi
* @returns `{boolean}` true if available

```javascript showLineNumbers
function main() {
    let r = auxEvent.imeIsOk();
    logd("imeIsOk: {}", r);
}
main();
```

### imeInput

* Input string (maps to imeApi.input)
* Requires EC iOS USB 9.39.0+ (maps to standalone imeApi 3.15.0+)
* Forwarded via Aux HTTP to standalone ImeApi
* @param content string
* @returns `{string}` empty=input failed; else data

```javascript showLineNumbers
function main() {
    if (!auxEvent.imeIsOk()) { logw("input method not ready"); return; }
    let r = auxEvent.imeInput("test content");
    logd("imeInput: {}", r);
}
main();
```

### imePaste

* Paste string via clipboard into input field (maps to imeApi.paste)
* Requires EC iOS USB 9.39.0+ (maps to standalone imeApi 3.15.0+)
* Forwarded via Aux HTTP to standalone ImeApi
* @param content String; if empty, uses clipboard data
* @returns `{string}` empty=failed; else input data

```javascript showLineNumbers
function main() {
    if (!auxEvent.imeIsOk()) { logw("input method not ready"); return; }
    let r = auxEvent.imePaste("test content");
    logd("imePaste: {}", r);
}
main();
```

### imePressDel

* Delete text in input field (maps to imeApi.pressDel)
* Requires EC iOS USB 9.39.0+ (maps to standalone imeApi 3.15.0+)
* Forwarded via Aux HTTP to standalone ImeApi
* @returns `{string}` empty=no field data; else remaining text

```javascript showLineNumbers
function main() {
    if (!auxEvent.imeIsOk()) { logw("input method not ready"); return; }
    let r = auxEvent.imePressDel();
    logd("imePressDel: {}", r);
}
main();
```

### imePressReturn

* Enter/Return key (maps to imeApi.pressEnter / pressReturn)
* Requires EC iOS USB 9.39.0+ (maps to standalone imeApi 3.15.0+)
* Forwarded via Aux HTTP to standalone ImeApi
* @returns `{string}` Raw service response

```javascript showLineNumbers
function main() {
    if (!auxEvent.imeIsOk()) { logw("input method not ready"); return; }
    let r = auxEvent.imePressReturn();
    logd("imePressReturn: {}", r);
}
main();
```

### imeDismiss

* Hide keyboard (maps to imeApi.dismiss)
* Requires EC iOS USB 9.39.0+ (maps to standalone imeApi 3.15.0+)
* Forwarded via Aux HTTP to standalone ImeApi
* @returns `{string}` Raw service response

```javascript showLineNumbers
function main() {
    if (!auxEvent.imeIsOk()) { logw("input method not ready"); return; }
    let r = auxEvent.imeDismiss();
    logd("imeDismiss: {}", r);
}
main();
```

### imeCopyToClipboard

* Copy input field to clipboard (maps to imeApi.copyToClipboard)
* Requires EC iOS USB 9.39.0+ (maps to standalone imeApi 3.15.0+)
* Forwarded via Aux HTTP to standalone ImeApi
* @returns `{string}` empty=no data; else remaining text copied to clipboard

```javascript showLineNumbers
function main() {
    if (!auxEvent.imeIsOk()) { logw("input method not ready"); return; }
    let r = auxEvent.imeCopyToClipboard();
    logd("imeCopyToClipboard: {}", r);
}
main();
```

### imeChangeKeyboard

* Switch to another keyboard (maps to imeApi.changeKeyboard)
* Requires EC iOS USB 9.39.0+ (maps to standalone imeApi 3.15.0+)
* After result returns, waits 2s then switches
* Forwarded via Aux HTTP to standalone ImeApi
* @returns `{string}` Raw service response

```javascript showLineNumbers
function main() {
    if (!auxEvent.imeIsOk()) { logw("input method not ready"); return; }
    let r = auxEvent.imeChangeKeyboard();
    logd("imeChangeKeyboard: {}", r);
}
main();
```

### imeRemoveAllContent

* Clear input field (maps to imeApi.removeAllContent)
* Requires EC iOS USB 9.39.0+ (maps to standalone imeApi 3.15.0+)
* Forwarded via Aux HTTP to standalone ImeApi
* @returns `{string}` Raw service response

```javascript showLineNumbers
function main() {
    if (!auxEvent.imeIsOk()) { logw("input method not ready"); return; }
    let r = auxEvent.imeRemoveAllContent();
    logd("imeRemoveAllContent: {}", r);
}
main();
```

### imeGetClipboard

* Read clipboard data (maps to imeApi.getClipboard)
* Requires EC iOS USB 9.39.0+ (maps to standalone imeApi 3.15.0+)
* Forwarded via Aux HTTP to standalone ImeApi
* @returns `{string}` Clipboard data or empty

```javascript showLineNumbers
function main() {
    if (!auxEvent.imeIsOk()) { logw("input method not ready"); return; }
    let r = auxEvent.imeGetClipboard();
    logd("imeGetClipboard: {}", r);
}
main();
```

### imeSetClipboard

* Set clipboard data (maps to imeApi.setClipboard)
* Requires EC iOS USB 9.39.0+ (maps to standalone imeApi 3.15.0+)
* Forwarded via Aux HTTP to standalone ImeApi
* @param content string
* @param type1 1=plain string, 2=URL data
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.imeIsOk()) { logw("input method not ready"); return; }
    let r = auxEvent.imeSetClipboard("test content", 1);
    logd("imeSetClipboard: {}", r);
}
main();
```

### imeOpenUrl

* Open URL (maps to imeApi.openUrl)
* Requires EC iOS USB 9.39.0+ (maps to standalone imeApi 3.15.0+)
* Forwarded via Aux HTTP to standalone ImeApi
* @param url URL, e.g. http://baidu.com
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    if (!auxEvent.imeIsOk()) { logw("input method not ready"); return; }
    let r = auxEvent.imeOpenUrl("http://example.com");
    logd("imeOpenUrl: {}", r);
}
main();
```

### imeGetText

* Get input field text (maps to imeApi.getText)
* Requires EC iOS USB 9.39.0+ (maps to standalone imeApi 3.15.0+)
* Forwarded via Aux HTTP to standalone ImeApi
* @returns `{string}` empty=no data; else has data

```javascript showLineNumbers
function main() {
    if (!auxEvent.imeIsOk()) { logw("input method not ready"); return; }
    let r = auxEvent.imeGetText();
    logd("imeGetText: {}", r);
}
main();
```

---

---

## 7. Device Information

Device info read via Aux from phone (maps to device.js)

### Basic Info

### deviceGetDeviceIdentifier

* Get device identifier (maps to device.getDeviceIdentifier)
* Requires EC iOS USB 9.39.0+
* Forwarded via Aux HTTP to standalone DeviceApi
* @returns `{string}` Device identifier or empty on failure

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.deviceGetDeviceIdentifier();
    logd("deviceGetDeviceIdentifier: {}", r);
}
main();
```

### deviceGetDeviceId

* Get device ID from standalone activator (maps to device.getDeviceId)
* Requires EC iOS USB 9.39.0+
* Supports EC iOS standalone 2.0+
* Forwarded via Aux HTTP to standalone DeviceApi
* @returns `{string}` Device ID or empty on failure

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.deviceGetDeviceId();
    logd("deviceGetDeviceId: {}", r);
}
main();
```

### deviceGetModel

* Get phone model (maps to device.getModel)
* Requires EC iOS USB 9.39.0+
* Forwarded via Aux HTTP to standalone DeviceApi
* @returns `{string}` Model or empty on failure

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.deviceGetModel();
    logd("deviceGetModel: {}", r);
}
main();
```

### deviceGetOSVersion

* Get OS version string, e.g. 6.0 (maps to device.getOSVersion)
* Requires EC iOS USB 9.39.0+
* Forwarded via Aux HTTP to standalone DeviceApi
* @returns `{string}` OS version or empty on failure

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.deviceGetOSVersion();
    logd("deviceGetOSVersion: {}", r);
}
main();
```

### deviceGetBattery

* Get battery level (maps to device.getBattery)
* Requires EC iOS USB 9.39.0+
* Forwarded via Aux HTTP to standalone DeviceApi
* @returns `{number}` battery 1–100

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.deviceGetBattery();
    logd("deviceGetBattery: {}", r);
}
main();
```

### deviceIsCharging

* Whether charging (maps to device.isCharging)
* Requires EC iOS USB 9.39.0+
* Forwarded via Aux HTTP to standalone DeviceApi
* @returns `{boolean}` true if charging

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.deviceIsCharging();
    logd("deviceIsCharging: {}", r);
}
main();
```

### deviceGetScale

* Get screen scale ratio (maps to device.getScale)
* Requires EC iOS USB 9.39.0+
* Forwarded via Aux HTTP to standalone DeviceApi
* @returns `{number}` scale ratio

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.deviceGetScale();
    logd("deviceGetScale: {}", r);
}
main();
```

### Screen and Orientation

### deviceGetScreenWidth

* [Deprecated] boundary issues; use deviceGetScreenWidthHeight (maps to device.getScreenWidth)
* Requires EC iOS USB 9.39.0+
* Forwarded via Aux HTTP to standalone DeviceApi
* @returns `{number}` Screen width

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.deviceGetScreenWidth();
    logd("deviceGetScreenWidth: {}", r);
}
main();
```

### deviceGetScreenHeight

* [Deprecated] boundary issues; use deviceGetScreenWidthHeight (maps to device.getScreenHeight)
* Requires EC iOS USB 9.39.0+
* Forwarded via Aux HTTP to standalone DeviceApi
* @returns `{number}` Screen height

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.deviceGetScreenHeight();
    logd("deviceGetScreenHeight: {}", r);
}
main();
```

### deviceGetScreenWidthHeight

* Get width,height string; split yourself (maps to device.getScreenWidthHeight)
* Requires EC iOS USB 9.39.0+
* Forwarded via Aux HTTP to standalone DeviceApi
* @returns `{string}` width,height or empty on failure

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.deviceGetScreenWidthHeight();
    logd("deviceGetScreenWidthHeight: {}", r);
}
main();
```

### deviceGetWidthNoAuto

* Get screen width without automation service (maps to device.getWidthNoAuto)
* Requires EC iOS USB 9.39.0+
* Forwarded via Aux HTTP to standalone DeviceApi
* @returns `{number}` width

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.deviceGetWidthNoAuto();
    logd("deviceGetWidthNoAuto: {}", r);
}
main();
```

### deviceGetHeightNoAuto

* Get screen height without automation service (maps to device.getHeightNoAuto)
* Requires EC iOS USB 9.39.0+
* Forwarded via Aux HTTP to standalone DeviceApi
* @returns `{number}` height

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.deviceGetHeightNoAuto();
    logd("deviceGetHeightNoAuto: {}", r);
}
main();
```

### deviceGetOrientation

* Get screen orientation (maps to device.getOrientation / agentEvent.getOrientation)
* Requires EC iOS USB 9.39.0+
* Forwarded via Aux HTTP to standalone DeviceApi
* @returns `{string}` 1=portrait 2=landscape 90° CW

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.deviceGetOrientation();
    logd("deviceGetOrientation: {}", r);
}
main();
```

### deviceGetOrientationNoAuto

* Get orientation without automation (maps to device.getOrientationNoAuto)
* Requires EC iOS USB 9.39.0+
* Get screen orientation without automation service
* Forwarded via Aux HTTP to standalone DeviceApi
* @returns `{string}` 1=portrait 2=landscape; else empty

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.deviceGetOrientationNoAuto();
    logd("deviceGetOrientationNoAuto: {}", r);
}
main();
```

### deviceSetOrientation

* Set screen orientation; landscape supports 90° CW only (maps to device.setOrientation)
* Requires EC iOS USB 9.39.0+
* Forwarded via Aux HTTP to standalone DeviceApi
* @param orz orientation 1=portrait, 2=landscape (90° clockwise)
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.deviceSetOrientation(1);
    logd("deviceSetOrientation: {}", r);
}
main();
```

---

---

## 8. Utilities and Photo Album

Clipboard, URL, album permissions, and image/video upload

### Utility Functions

### utilGetClipboardText

* Read clipboard text (maps to utils.getClipboardText)
* Requires EC iOS USB 9.39.0+
* Note: use PiP or takeMeToFront to bring app to foreground
* Forwarded via Aux HTTP to standalone UtilsApi
* @returns `{string}` Clipboard text or empty

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let ok = auxEvent.utilGetClipboardText();
    logd("utilGetClipboardText: {}", ok);
}
main();
```

### utilSetClipboardText

* Set clipboard text (maps to utils.setClipboardText)
* Requires EC iOS USB 9.39.0+
* Note: use PiP or takeMeToFront to bring app to foreground
* Forwarded via Aux HTTP to standalone UtilsApi
* @param text Text
* @param type 1=text, 2=link; default 1
* @returns `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let ok = auxEvent.utilSetClipboardText("test content", 1);
    logd("utilSetClipboardText: {}", ok);
}
main();
```

### utilOpenUrl

* Open URL (maps to utils.openUrl)
* Requires EC iOS USB 9.39.0+
* Note: bring app to foreground with takeMeToFront first
* Forwarded via Aux HTTP to standalone UtilsApi
* @param url URL
* @returns `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let ok = auxEvent.utilOpenUrl("http://example.com");
    logd("utilOpenUrl: {}", ok);
}
main();
```

### utilRequestPhotoAuthorization

* Request photo library permission (maps to utils.requestPhotoAuthorization)
* Requires EC iOS USB 9.39.0+
* First request shows permission dialog; allow or enable Photos for EC app in Settings
* Note: async calls; ignore return to avoid blocking simulated taps
* Requires EC standalone 4.9.0+
* Forwarded via Aux HTTP to standalone UtilsApi
* @returns `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let ok = auxEvent.utilRequestPhotoAuthorization();
    logd("utilRequestPhotoAuthorization: {}", ok);
}
main();
```

### utilDeleteAllPhotos

* Delete all photos in album (maps to utils.deleteAllPhotos)
* Requires EC iOS USB 9.39.0+
* Shows confirm dialog; simulate tapping Delete
* Note: async calls; ignore return to avoid blocking simulated taps
* Requires EC standalone 4.9.0+
* Forwarded via Aux HTTP to standalone UtilsApi
* @returns `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let ok = auxEvent.utilDeleteAllPhotos();
    logd("utilDeleteAllPhotos: {}", ok);
}
main();
```

### utilDeleteAllVideos

* Delete all videos in album (maps to utils.deleteAllVideos)
* Requires EC iOS USB 9.39.0+
* Shows confirm dialog; simulate tapping Delete
* Note: async calls; ignore return to avoid blocking simulated taps
* Requires EC standalone 4.9.0+
* Forwarded via Aux HTTP to standalone UtilsApi
* @returns `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let ok = auxEvent.utilDeleteAllVideos();
    logd("utilDeleteAllVideos: {}", ok);
}
main();
```

### Album Upload

### auxInsertImageToAlbum

* Save image to album by path (maps to utils.saveImageToAlbumPath)
* Requires EC iOS USB 9.39.0+
* PC local file uses multipart upload; phone path uses operateAlbum
* Forwarded via Aux HTTP to standalone API
* @param path File path (PC or phone local)
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.auxInsertImageToAlbum("/path/to/file");
    if (!auxEvent.isAuxCallOk(r)) logw("auxInsertImageToAlbum failed: {}", r);
    else logd("auxInsertImageToAlbum succeeded");
}
main();
```

### auxInsertVideoToAlbum

* Save video to album by path (maps to utils.saveVideoToAlbumPath)
* Requires EC iOS USB 9.39.0+
* PC local file uses multipart upload; phone path uses operateAlbum
* Forwarded via Aux HTTP to standalone API
* @param path Video file path (PC or phone local)
* @returns `{string|null}` null or empty string means success; else error message

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.auxInsertVideoToAlbum("/path/to/file");
    if (!auxEvent.isAuxCallOk(r)) logw("auxInsertVideoToAlbum failed: {}", r);
    else logd("auxInsertVideoToAlbum succeeded");
}
main();
```

---

---

## Related Documentation

- [BLE functions](/iosdocs/funcs/ble-event-api)
- [OTG HID functions](/iosdocs/funcs/otg-event-api)
- [IME functions](/iosdocs/funcs/ime-api)
- [Device functions](/iosdocs/funcs/device-api)
- [Utility functions](/iosdocs/funcs/utils-api)
- [iOS USB screen mirroring tutorial](/iosdocs/advance/ios-usb-screen)
