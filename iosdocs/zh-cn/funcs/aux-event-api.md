---
title: Aux 远程辅助函数
description: EasyClick 自动化脚本 iOS免越狱 auxEvent 远程调用易点云测 Aux 网关
keywords:
  - EasyClick 自动化脚本 iOS免越狱 auxEvent
  - Aux
  - 易点云测
  - ecauto
  - WiFi
  - USB
  - screencap
  - agent
  - ble
  - otg
  - ime
  - EasyClick
  - 手机自动化
  - 自动化测试
  - 脚本开发
  - iOS自动化
---

## 说明

`auxEvent` 是 **USB 中控脚本** 侧模块，调用手机端 **易点云测 App**（脱机主程序 / ecauto）内置的 **服务**。

与脱机 App 内直接调用的 `bleEvent`、`otgEvent`、`imeApi` 等不同：`auxEvent` 让 **PC 中控脚本** 经 USB 或 WiFi 远程操作手机。

:::tip 算力与场景
- **auxEvent（本模块）**：算力在 **电脑侧**，脚本在中控里运行，通过手机上的易点云测 **服务** 远程下发指令。
- **脱机版本 API**：算力在 **手机侧**，脚本在易点云测 App 内运行，直接调用 `bleEvent`、`device` 等本地模块。
- 两者 API 名称相近，但 **应用场景完全不同**，请勿混用。
- auxEvent调用的是易点云测，如果是蓝牙和OTG模块，请进入脱机版本的文档刷入正确的脱机版本的蓝牙和OTG固件
:::

:::tip 适用版本
- 适配 **EC iOS USB 版本 9.39.0+**
- 手机安装 **易点云测**（脱机主程序）并且在 App 设置中选择兼容 USB 模式
:::

:::tip WiFi 方式发现设备（无需 USB 数据线）
若希望使用 **WiFi 扫描** 连接设备、平时 **不插 USB 数据线**，需先完成一次 **设备 ID 注入**（USB 连接手机）：

**方式一：中控界面操作**

1. 将脱机主程序的 **bundleID** 配置到中控的 **代理 bundleID** 设置中
2. 插入 USB，在中控点击 **开启自动化服务**，成功启动一次
3. 打开脱机主程序 App → **设置** → **兼容 USB**，确认页面上已出现 **设备 ID** 的值

**方式二：脚本注入**

在 USB 已连接、且中控 **bridge** 已启动的前提下，脚本中调用以下任一函数完成注入（`bundleId` 为脱机主程序包名）：

- `startAuxBridge(bundleId, opts)` — 启动桥接并拉起易点云测 App
- `launchAppWithAuxEnv(bundleId, opts)` — 仅拉起 App 并注入 Aux 环境变量

注入成功后，同样在 App **设置 → 兼容 USB** 中可看到 **设备 ID**。

出现设备 ID 后，即可拔掉 USB，后续通过 `ensureWifiSession` / `scanWifiDevices` 在局域网发现该设备。手机与电脑需在同一 WiFi 下。

**定向扫描（白名单）**

中控界面点击 **辅助扫描** 按钮，在弹出窗口的 **设备 ID 列表** 输入框中填写允许连接的设备 ID：

- **留空**：扫描并允许局域网内所有已注入的 WiFi 设备连接本中控
- **填写设备 ID**（一行一个）：**仅允许** 列表中的设备 ID 通过 WiFi 连接本中控，其它设备即使扫描到也不会建立会话

设备 ID 即脱机主程序 **设置 → 兼容 USB** 中显示的值。配置保存后，再执行扫描或脚本中的 `scanWifiDevices` / `ensureWifiSession` 时生效。

**不想扫描WiFi设备**
- 默认扫描是开启的，不想扫描就去bridgebin/config/config.toml文件，记事本打开就可以编辑，更改 auxWifiScanEnabled的值设置为2，重启桥接重新就行了 
:::

:::tip 返回值约定
- **字符串类**（会话、录屏、BLE、OTG 等）：`null` 或 `""` 表示成功，非空字符串为错误信息
- **布尔类**（`agent*`、部分 `util*`）：`true` / `false`
:::

### 架构

```
中控脚本 → 手机易点云测 Aux 服务 → Agent/录屏/BLE/OTG/IME/Device/Utils
```

### 八大类一览

| 序号 | 分类 | 前缀 | 函数数 | 对应脱机 jslib |
|------|------|------|--------|----------------|
| 1 | 会话与桥接 | — | 12 | Aux 网关会话 |
| 2 | Agent 自动化 | `agent*` | 24 | agentEvent.js / AgentApi |
| 3 | 系统录屏 | `screencap*` | 5 | image.js 系统录屏 |
| 4 | OTG 远程 | `otg*` | 22 | otgEvent.js |
| 5 | BLE 远程 | `ble*` | 32 | bleEvent.js |
| 6 | 输入法 | `ime*` | 13 | imeApi.js |
| 7 | 设备信息 | `device*` | 15 | device.js |
| 8 | 工具与相册 | `util*` / `auxInsert*` | 8 | utils.js |


## 函数分类目录

### 1、会话与桥接（12 个）

USB/WiFi 会话建立、设备发现、连接偏好与健康检查

`scanWifiDevices`、`startAuxBridge`、`launchAppWithAuxEnv`、`setConnectType`、`getConnectType`、`ensureUsbSession`、`ensureWifiSession`、`ensureSession`、`closeSession`、`getWifiIp`、`sessionBaseUrl`、`health`

### 2、Agent 自动化（24 个）

经 Aux 转发 Agent：环境、手势、应用、节点与截图

**环境与服务**：`agentIsServiceOk`、`agentStartEnv`

**桌面与锁屏**：`agentHome`、`agentHomeScreen`、`agentIsLocked`、`agentLockScreen`、`agentUnlockScreen`

**应用管理**：`agentAppLaunch`、`agentCreateSession`、`agentAppKillByBundleId`

**点击与滑动**：`agentClickPoint`、`agentClickPointPressure`、`agentPress`、`agentLongClickPoint`、`agentDoubleClickPoint`、`agentSwipeToPoint`、`agentSwipeToPointPressure`、`agentMultiTouch`、`agentIoHIDEvent`

**文字输入**：`agentInputText`

**节点与截图**：`agentSetFetchNodeParam`、`agentDumpXml`、`agentCaptureFullScreen`、`agentCaptureFullScreenEx`（`agentDumpXml` 配合 [lockNodeFromXml](./node-api.md#locknodefromxml-从-xml-锁定节点) 可在 PC 侧即可正常使用getNodeInfo等函数）

### 3、系统录屏（5 个）

Aux 系统录屏广播与帧缓存截图，不依赖 自动化服务

`screencapStatus`、`screencapStart`、`screencapStop`、`screencapConfig`、`screencapSnapshot`

### 4、OTG 远程（22 个）

经手机 Aux 转发 OTG HID（对应 otgEvent.js）

**连接与配置**：`otgIsConnect`、`otgGetMacAddress`、`otgRestart`、`otgSetScreenSize`、`otgSetScale`

**鼠标与触摸**：`otgResetZero`、`otgMouseMove`、`otgMouseMoveDistance`、`otgTouchDown`、`otgTouchMove`、`otgTouchUp`、`otgClickPoint`、`otgPress`、`otgDoubleClickPoint`、`otgSwipeToPoint`、`otgMultiTouch`、`otgPressMouseBtn`

**按键与其它**：`otgSystemKey`、`otgToggleSoftKeyboard`、`otgKeyPressChar`、`otgKeyPress`、`otgLight`

### 5、BLE 远程（32 个）

经手机 Aux 转发蓝牙 HID（对应 bleEvent.js）

**连接与配置**：`bleStartConnect`、`bleStopConnect`、`bleIsConnect`、`bleSearchBleIp`、`bleGetConfigBleName`、`bleGetScale`、`bleSetScale`、`bleSetScreenSize`、`bleSetStep`、`bleSetWifiInfo`、`bleSendCmdType`、`bleResetBle`、`bleShowBleName`、`bleHideBleName`

**鼠标与触摸**：`bleResetZero`、`bleMouseMove`、`bleMouseMoveByDistance`、`bleMouseMoveDistance`、`bleTouchDown`、`bleTouchMove`、`bleTouchUp`、`bleClickPoint`、`blePress`、`bleDoubleClickPoint`、`bleSwipeToPoint`、`bleMultiTouch`、`blePressMouseBtn`

**按键与其它**：`bleSystemKey`、`bleToggleSoftKeyboard`、`bleKeyPressChar`、`bleKeyPress`、`bleLight`

### 6、输入法（13 个）

经 Aux 转发自定义输入法（对应 imeApi.js）

`imeIsOk`、`imeInput`、`imePaste`、`imePressDel`、`imePressReturn`、`imeDismiss`、`imeCopyToClipboard`、`imeChangeKeyboard`、`imeRemoveAllContent`、`imeGetClipboard`、`imeSetClipboard`、`imeOpenUrl`、`imeGetText`

### 7、设备信息（15 个）

经 Aux 读取手机设备信息（对应 device.js）

**基础信息**：`deviceGetDeviceIdentifier`、`deviceGetDeviceId`、`deviceGetModel`、`deviceGetOSVersion`、`deviceGetBattery`、`deviceIsCharging`、`deviceGetScale`

**屏幕与方向**：`deviceGetScreenWidth`、`deviceGetScreenHeight`、`deviceGetScreenWidthHeight`、`deviceGetWidthNoAuto`、`deviceGetHeightNoAuto`、`deviceGetOrientation`、`deviceGetOrientationNoAuto`、`deviceSetOrientation`

### 8、工具与相册（8 个）

剪贴板、URL、相册权限与图视频上传

**工具函数**：`utilGetClipboardText`、`utilSetClipboardText`、`utilOpenUrl`、`utilRequestPhotoAuthorization`、`utilDeleteAllPhotos`、`utilDeleteAllVideos`

**相册上传**：`auxInsertImageToAlbum`、`auxInsertVideoToAlbum`

---

## 1、会话与桥接

USB/WiFi 会话建立、设备发现、连接偏好与健康检查

### scanWifiDevices

* 扫描局域网 Aux HTTP 设备（WiFi 模式发现 ecauto，结果缓存在桥接侧）
* 适配EC iOS USB版本9.39.0+
* @param ipRange IP 段，例如 192.168.2.1 - 192.168.2.255
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    let r = auxEvent.scanWifiDevices("");
    if (!auxEvent.isAuxCallOk(r)) logw("scanWifiDevices 失败: {}", r);
    else logd("scanWifiDevices 成功");
}
main();
```

### startAuxBridge

* 启动 Aux 桥接并拉起 易点云测app
* 适配EC iOS USB版本9.39.0+
* @param bundleId 易点云测app 的 bundleId
* @param opts 可选：connectType(usb/wifi)、ipRange/wifiScanRange、startScreencap、screencapDelaySec、killExisting
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    let r = auxEvent.startAuxBridge("com.ieasyclick.ecauto", {});
    if (!auxEvent.isAuxCallOk(r)) logw("startAuxBridge 失败: {}", r);
    else logd("startAuxBridge 成功");
}
main();
```

### launchAppWithAuxEnv

* 仅拉起 易点云测app 并注入 Aux 环境变量
* 适配EC iOS USB版本9.39.0+
* @param bundleId 易点云测app 的 bundleId
* @param opts 可选启动参数，同 startAuxBridge
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    let r = auxEvent.launchAppWithAuxEnv("com.ieasyclick.ecauto", {});
    if (!auxEvent.isAuxCallOk(r)) logw("launchAppWithAuxEnv 失败: {}", r);
    else logd("launchAppWithAuxEnv 成功");
}
main();
```

### setConnectType

* 设置 Aux 连接偏好（usb / wifi）；传空字符串清除偏好，后续 GetSession 自动 USB 优先再 WiFi。
* 适配EC iOS USB版本9.39.0+
* 仅影响路由，不建立或关闭 session。
* @param connectType usb、wifi 或 "" 清除

```javascript showLineNumbers
function main() {
    let r = auxEvent.setConnectType("usb");
    if (!auxEvent.isAuxCallOk(r)) logw("setConnectType 失败: {}", r);
    else logd("setConnectType 成功");
}
main();
```

### getConnectType

* 获取当前设置的 Aux 连接偏好；未设置时返回空字符串。
* 适配EC iOS USB版本9.39.0+
* @returns `{string}`

```javascript showLineNumbers
function main() {
    let r = auxEvent.getConnectType();
    logd("getConnectType: {}", r);
}
main();
```

### ensureUsbSession

* 确保 USB 与易点云测app的链接会话
* 适配EC iOS USB版本9.39.0+
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    let r = auxEvent.ensureUsbSession();
    if (!auxEvent.isAuxCallOk(r)) logw("ensureUsbSession 失败: {}", r);
    else logd("ensureUsbSession 成功");
}
main();
```

### ensureWifiSession

* 确保 USB中控与易点云测app网络会话
* 适配EC iOS USB版本9.39.0+
* @param ipRange IP 段，例如 192.168.2.1 - 192.168.2.255
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    let r = auxEvent.ensureWifiSession("");
    if (!auxEvent.isAuxCallOk(r)) logw("ensureWifiSession 失败: {}", r);
    else logd("ensureWifiSession 成功");
}
main();
```

### ensureSession

* 按连接类型确保 易点云测app 会话
* 适配EC iOS USB版本9.39.0+
* @param connectType 连接类型：usb 或 wifi
* @param ipRange WiFi 扫描 IP 段，connectType 为 wifi 时使用
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    let r = auxEvent.ensureSession("usb", "");
    if (!auxEvent.isAuxCallOk(r)) logw("ensureSession 失败: {}", r);
    else logd("ensureSession 成功");
}
main();
```

### closeSession

* 关闭 易点云测app 会话
* 适配EC iOS USB版本9.39.0+
* @param connectType 可选：usb / wifi；不传或空字符串则关闭 USB 与 WiFi 全部 session
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    let r = auxEvent.closeSession("usb");
    if (!auxEvent.isAuxCallOk(r)) logw("closeSession 失败: {}", r);
    else logd("closeSession 成功");
}
main();
```

### getWifiIp

* 获取设备 易点云测app WiFi IP
* 适配EC iOS USB版本9.39.0+
* @returns `{string|null}` WiFi IP；未找到或无实例时返回 null

```javascript showLineNumbers
function main() {
    let r = auxEvent.getWifiIp();
    logd("getWifiIp: {}", r);
}
main();
```

### sessionBaseUrl

* 获取当前 易点云测app 会话 HTTP baseUrl
* 适配EC iOS USB版本9.39.0+
* @returns `{string}` baseUrl 字符串；失败时返回空字符串

```javascript showLineNumbers
function main() {
    let r = auxEvent.sessionBaseUrl();
    logd("sessionBaseUrl: {}", r);
}
main();
```

### health

* 易点云测app HTTP 网关健康检查
* 适配EC iOS USB版本9.39.0+
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    let r = auxEvent.health();
    if (!auxEvent.isAuxCallOk(r)) logw("health 失败: {}", r);
    else logd("health 成功");
}
main();
```

---

---

## 2、Agent 自动化

经 Aux 转发 Agent：环境、手势、应用、节点与截图

### 环境与服务

### agentIsServiceOk

* 自动化服务是否正常（对应 EcImporter.isServiceOk / basic.isServiceOk）
* 适配EC iOS USB版本9.39.0+
* 经 Aux HTTP 转发至脱机版本 Agent API
* @returns `{boolean}` true 代表正常，false 代表不正常

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

* 启动自动化环境（对应 EcImporter.startEnv / basic.startEnv）
* 适配EC iOS USB版本9.39.0+
* 经 Aux HTTP 转发至脱机版本 Agent API
* @returns `{boolean}` true 代表启动成功，false 代表启动失败

```javascript showLineNumbers
function main() {
    auxEvent.ensureWifiSession("");
    auxEvent.agentStartEnv();
    let ok = auxEvent.agentStartEnv();
    logd("agentStartEnv: {}", ok);
}
main();
```

### 桌面与锁屏

### agentHome

* 返回桌面（对应 agentEvent.home）
* 适配EC iOS USB版本9.39.0+
* @returns `{boolean}` true 成功，false 失败

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

* 强制打开主页，和 home 不同（对应 agentEvent.homeScreen）
* 适配EC iOS USB版本9.39.0+
* @returns `{boolean}` true 成功，false 失败

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

* 屏幕是否是锁定状态（对应 agentEvent.isLocked）
* 适配EC iOS USB版本9.39.0+
* @returns `{boolean}` true 已锁定，false 未锁定

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

* 锁定屏幕（对应 agentEvent.lockScreen）
* 适配EC iOS USB版本9.39.0+
* @returns `{boolean}` true 成功，false 失败

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

* 解锁屏幕（对应 agentEvent.unlockScreen）
* 适配EC iOS USB版本9.39.0+
* @returns `{boolean}` true 成功，false 失败

```javascript showLineNumbers
function main() {
    auxEvent.ensureWifiSession("");
    auxEvent.agentStartEnv();
    let ok = auxEvent.agentUnlockScreen();
    logd("agentUnlockScreen: {}", ok);
}
main();
```

### 应用管理

### agentAppLaunch

* 打开 App（对应 AgentApi.appLaunch / agentEvent.appLaunchEx）
* 适配EC iOS USB版本9.39.0+
* @param bundleId App 的 bundleId
* @param ignoreState 1 忽略之前打开的状态，其他填 ""
* @returns `{boolean}` true 代表成功

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

* 创建 自动化 session（对应 AgentApi.createSession）
* 适配EC iOS USB版本9.39.0+
* @param bundleId 可选，为空则绑定当前前台应用
* @returns `{boolean}` true 成功，false 失败

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

* 使用 bundleId 杀死进程（对应 AgentApi.appKillByBundleId / agentEvent.appKillByBundleIdEx）
* 适配EC iOS USB版本9.39.0+
* @param bundleId App 的 bundleId
* @param ignoreState 1 忽略之前状态，其他填 ""
* @returns `{boolean}` true 成功，false 失败

```javascript showLineNumbers
function main() {
    auxEvent.ensureWifiSession("");
    auxEvent.agentStartEnv();
    let ok = auxEvent.agentAppKillByBundleId("com.ieasyclick.ecauto", "");
    logd("agentAppKillByBundleId: {}", ok);
}
main();
```

### 点击与滑动

### agentClickPoint

* 点击坐标点（对应 agentEvent.clickPoint）
* 适配EC iOS USB版本9.39.0+
* @param x X 坐标
* @param y Y 坐标
* @returns `{boolean}` true 点击成功，false 点击失败

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

* 点击坐标点（带压力）（对应 agentEvent.clickPointPressure）
* 适配EC iOS USB版本9.39.0+
* @param x X 坐标
* @param y Y 坐标
* @param pressure 压力值，0-1 之间
* @returns `{boolean}` true 点击成功，false 点击失败

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

* 按住坐标（对应 agentEvent.press）
* 适配EC iOS USB版本9.39.0+
* @param x X 坐标
* @param y Y 坐标
* @param delay 持续时长，单位毫秒
* @returns `{boolean}` true 成功，false 失败

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

* 长按坐标（对应 agentEvent.longClickPoint）
* 适配EC iOS USB版本9.39.0+
* @param x X 坐标
* @param y Y 坐标
* @param delay 持续时长，单位毫秒
* @returns `{boolean}` true 成功，false 失败

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

* 双击坐标（对应 agentEvent.doubleClickPoint）
* 适配EC iOS USB版本9.39.0+
* @param x X 坐标
* @param y Y 坐标
* @returns `{boolean}` true 成功，false 失败

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

* 坐标滑动（对应 agentEvent.swipeToPoint）
* 适配EC iOS USB版本9.39.0+
* @param startX 起始 X
* @param startY 起始 Y
* @param endX 结束 X
* @param endY 结束 Y
* @param duration 持续时长，单位毫秒
* @returns `{boolean}` true 滑动成功，false 滑动失败

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

* 坐标滑动（带压力）（对应 agentEvent.swipeToPointPressure）
* 适配EC iOS USB版本9.39.0+
* @param startX 起始 X
* @param startY 起始 Y
* @param endX 结束 X
* @param endY 结束 Y
* @param duration 持续时长，单位毫秒
* @param pressure 压力值，0-1 之间
* @returns `{boolean}` true 滑动成功，false 滑动失败

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

* 多点触摸（对应 agentEvent.multiTouch）
* 适配EC iOS USB版本9.39.0+
* @param touch1 触摸点数组或 JSON 字符串
* @param timeout 总超时，单位毫秒
* @returns `{boolean}` true 成功，false 失败

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

* 模拟人机交互 HID 事件（对应 agentEvent.ioHIDEvent）
* 适配EC iOS USB版本9.39.0+
* @param eventPageID 人机交互类型
* @param eventUsageID 人机交互值
* @param delay 时长，一般 0.2 即可
* @returns `{boolean}` true 成功，false 失败

```javascript showLineNumbers
function main() {
    auxEvent.ensureWifiSession("");
    auxEvent.agentStartEnv();
    let ok = auxEvent.agentIoHIDEvent("11", "1", 1000);
    logd("agentIoHIDEvent: {}", ok);
}
main();
```

### 文字输入

### agentInputText

* 输入文字（对应 agentEvent.inputText）
* 适配EC iOS USB版本9.39.0+
* @param content 内容
* @param duration 执行时间，单位毫秒
* @returns `{boolean}` true 成功，false 失败

```javascript showLineNumbers
function main() {
    auxEvent.ensureWifiSession("");
    auxEvent.agentStartEnv();
    let ok = auxEvent.agentInputText("测试内容", 1000);
    logd("agentInputText: {}", ok);
}
main();
```

### 节点与截图

WiFi 场景下若要用 [节点函数](./node-api.md) 的选择器（`label().getNodeInfo()` 等），典型流程为：`agentSetFetchNodeParam` → `agentDumpXml` → `lockNodeFromXml` → 选节点查找。远程在手机端匹配可改用 [nodeAgent](./node-agent-api.md)。

### agentSetFetchNodeParam

* 设置获取节点的基础参数（对应 agentEvent.setFetchNodeParam）
* 适配EC iOS USB版本9.39.0+
*  visibleFilter 1 代表不管visible是true还是false都获取，2 代表只获取 visible=true的节点
*  labelFilter 1 代表不管label是否有值都获取，2 代表只获取label有值的节点
*  maxDepth 代表要获取节点的层级，建议  1 - 500
*  maxChildCount 代表要获取子节点最大数量，越小获取的数量越少，0 代表不限制
*  excludedAttributes 代表要过滤的属性，用英文逗号分割，可以增加抓取速度，例如 visible,selected,enable
* @param ext 配置对象，如 `{visibleFilter, labelFilter, boundsFilter, maxDepth, maxChildCount,excludedAttributes}`
* @returns `{boolean}` true 成功，false 失败

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

* 将元素节点导出为 XML（对应 agentEvent.dumpXml）
* 适配EC iOS USB版本9.39.0+
* 经 Aux HTTP 转发至手机 Agent 拉取 XML，返回字符串；**不会**自动写入 PC 侧节点缓存
* 若要继续用 [节点函数](./node-api.md) 的选择器（`label().getNodeInfo()` 等），需配合 [lockNodeFromXml](./node-api.md#locknodefromxml-从-xml-锁定节点) 注入后再查找
* @returns `{string}` XML 字符串；失败时返回空字符串

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
        logd("agentDumpXml 失败");
        return;
    }

    releaseNode();
    let ok = lockNodeFromXml(xml);
    logd("lockNodeFromXml: {}", ok);
    if (!ok) {
        return;
    }

    let nd = label("设置").getOneNodeInfo(0);
    logd("node: {}", nd ? JSON.stringify(nd) : "null");
}
main();
```

完整说明见 [节点函数 - lockNodeFromXml](./node-api.md#locknodefromxml-从-xml-锁定节点)；对象形式可用 `agentEvent.lockNodeFromXml(xml)`。远程在手机端选节点见 [nodeAgent](./node-agent-api.md)。

### agentCaptureFullScreen

* 抓取全屏 JPG（对应 AgentCall.captureFullScreenData / image.captureFullScreen）
* 适配EC iOS USB版本9.39.0+
* 经 Aux HTTP 转发，直接返回图片二进制对应的 AutoImage
* @returns `{null|AutoImage}`

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let img = auxEvent.agentCaptureFullScreen();
    if (img) { logd("图片 {}x{}", img.getWidth(), img.getHeight()); img.recycle(); }
}
main();
```

### agentCaptureFullScreenEx

* 抓取全屏，可指定格式与质量（对应 AgentCall.captureFullScreenDataEx / image.captureFullScreenEx）
* 适配EC iOS USB版本9.39.0+
* 经 Aux HTTP 转发，直接返回图片二进制对应的 AutoImage
* type: 0/1=jpg方式1, 2=jpg方式2, 3=png
* quality: 1-100
* @param ext 扩展参数，如 `{type: "1", quality: 99}`
* @returns `{null|AutoImage}`

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let img = auxEvent.agentCaptureFullScreenEx({type:"1",quality:90});
    if (img) { logd("图片 {}x{}", img.getWidth(), img.getHeight()); img.recycle(); }
}
main();
```

---

---

## 3、系统录屏

Aux 系统录屏广播与帧缓存截图

### screencapStatus

* 查询录屏广播与帧缓存状态（Aux 专用，类似 image.isSystemScreenCapBroadcasting 配合帧缓存判断）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API image 系统录屏 6.6.0+）
* 一定先开启录屏接收服务才能接收图像，也可从 APP 设置-系统录屏选项中启动
* @returns `{string|null}` null 或空字符串表示录屏就绪，其他为未就绪或错误信息

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.screencapStatus();
    if (!auxEvent.isAuxCallOk(r)) logw("screencapStatus 失败: {}", r);
    else logd("screencapStatus 成功");
}
main();
```

### screencapStart

* 启动系统录屏广播（对应 image.startSystemScreenCapBroadcast）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API image 系统录屏 6.6.0+）
* 需 APP 在前台，或去 APP 设置-系统录屏设置中启动（弹出 Broadcast Picker，需用户确认/点一下）
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.screencapStart();
    if (!auxEvent.isAuxCallOk(r)) logw("screencapStart 失败: {}", r);
    else logd("screencapStart 成功");
}
main();
```

### screencapStop

* 停止系统录屏广播（对应 image.stopSystemScreenCapServer 停止接收）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API image 系统录屏 6.6.0+）
* @returns `{string|null}` null 或空字符串表示录屏已停止，其他为失败或仍在广播

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.screencapStop();
    if (!auxEvent.isAuxCallOk(r)) logw("screencapStop 失败: {}", r);
    else logd("screencapStop 成功");
}
main();
```

### screencapConfig

* 配置录屏帧率与 JPEG 质量（对应 image.setSystemScreenCapFps / setSystemScreenCapJpegQuality）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API image 系统录屏 6.6.0+）
* fps 为每秒张数，例如 10=每秒10张，不要设置太大；扩展每次开始广播时会先读取配置
* jpegQuality 为 JPEG 质量 1-100，越大越清晰，资源消耗越多
* @param config 配置对象，如 `{frameRate, jpegQuality}`
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.screencapConfig({frameRate:10,jpegQuality:80});
    if (!auxEvent.isAuxCallOk(r)) logw("screencapConfig 失败: {}", r);
    else logd("screencapConfig 成功");
}
main();
```

### screencapSnapshot

* 获取当前屏幕 JPEG 快照（对应 image.getSystemScreenCapImage）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API image 系统录屏 6.6.0+）
* @returns `{null|AutoImage}` 截图对象；无帧或无实例时返回 null

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let img = auxEvent.screencapSnapshot();
    if (img) { logd("图片 {}x{}", img.getWidth(), img.getHeight()); img.recycle(); }
}
main();
```

---

---

## 4、OTG 远程

经手机 Aux 转发 OTG HID（对应 otgEvent.js）

### 连接与配置

### otgIsConnect

* 查询 OTG 连接状态（对应 otgEvent.isConnect）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API otgEvent 脱机版6.6.0+）
* @returns `{string|null}` null 或空字符串代表已连接，其他为错误或未连接说明

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgIsConnect();
    if (!auxEvent.isAuxCallOk(r)) logw("otgIsConnect 失败: {}", r);
    else logd("otgIsConnect 成功");
}
main();
```

---

### otgGetMacAddress

* 获取 OTG 开发板 MAC 地址（对应 otgEvent.getMacAddress）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API otgEvent 脱机版6.6.0+）
* @returns `{string}` MAC 字符串；失败时返回错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgGetMacAddress();
    if (!auxEvent.isAuxCallOk(r)) logw("otgGetMacAddress 失败: {}", r);
    else logd("otgGetMacAddress 成功");
}
main();
```

### otgRestart

* 重启 OTG 开发板（相当于按 RST）（对应 otgEvent.restart）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API otgEvent 脱机版6.6.0+）
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgRestart();
    if (!auxEvent.isAuxCallOk(r)) logw("otgRestart 失败: {}", r);
    else logd("otgRestart 成功");
}
main();
```

### otgSetScreenSize

* 设置 OTG 屏幕尺寸（防止鼠标移出屏幕）（对应 otgEvent.setScreenSize）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API otgEvent 脱机版6.6.0+）
* @param w 屏幕宽度
* @param h 屏幕高度
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgSetScreenSize(390, 844);
    if (!auxEvent.isAuxCallOk(r)) logw("otgSetScreenSize 失败: {}", r);
    else logd("otgSetScreenSize 成功");
}
main();
```

### otgSetScale

* 设置 OTG 鼠标补偿比率（对应 otgEvent.setScale）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API otgEvent 脱机版6.6.0+）
* @param x_scale x 轴补偿比率
* @param y_scale y 轴补偿比率
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgSetScale(2.0, 2.0);
    if (!auxEvent.isAuxCallOk(r)) logw("otgSetScale 失败: {}", r);
    else logd("otgSetScale 成功");
}
main();
```

### 鼠标与触摸

### otgResetZero

* OTG 鼠标归零（对应 otgEvent.resetZero）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API otgEvent 脱机版6.6.0+）
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgResetZero();
    if (!auxEvent.isAuxCallOk(r)) logw("otgResetZero 失败: {}", r);
    else logd("otgResetZero 成功");
}
main();
```

### otgMouseMove

* OTG 移动鼠标（相对位移）（对应 otgEvent.mouseMove）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API otgEvent 脱机版6.6.0+）
* @param x X 方向位移
* @param y Y 方向位移
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgMouseMove(300, 400);
    if (!auxEvent.isAuxCallOk(r)) logw("otgMouseMove 失败: {}", r);
    else logd("otgMouseMove 成功");
}
main();
```

### otgMouseMoveDistance

* OTG 按像素距离移动鼠标（对应 otgEvent.mouseMoveDistance）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API otgEvent 脱机版6.6.0+）
* @param x_dis X 方向位移
* @param y_dis Y 方向位移
* @param press 移动时是否按下
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgMouseMoveDistance(10, 10, false);
    if (!auxEvent.isAuxCallOk(r)) logw("otgMouseMoveDistance 失败: {}", r);
    else logd("otgMouseMoveDistance 成功");
}
main();
```

### otgTouchDown

* OTG 按下触摸点（对应 otgEvent.touchDown）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API otgEvent 脱机版6.6.0+）
* @param x X 坐标
* @param y Y 坐标
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgTouchDown(300, 400);
    if (!auxEvent.isAuxCallOk(r)) logw("otgTouchDown 失败: {}", r);
    else logd("otgTouchDown 成功");
}
main();
```

### otgTouchMove

* OTG 移动触摸点（对应 otgEvent.touchMove）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API otgEvent 脱机版6.6.0+）
* @param x X 坐标
* @param y Y 坐标
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgTouchMove(300, 400);
    if (!auxEvent.isAuxCallOk(r)) logw("otgTouchMove 失败: {}", r);
    else logd("otgTouchMove 成功");
}
main();
```

### otgTouchUp

* OTG 抬起触摸点（对应 otgEvent.touchUp）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API otgEvent 脱机版6.6.0+）
* @param x X 坐标
* @param y Y 坐标
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgTouchUp(300, 400);
    if (!auxEvent.isAuxCallOk(r)) logw("otgTouchUp 失败: {}", r);
    else logd("otgTouchUp 成功");
}
main();
```

### otgClickPoint

* OTG 点击坐标（对应 otgEvent.clickPoint）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API otgEvent 脱机版6.6.0+）
* @param x X 坐标
* @param y Y 坐标
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgClickPoint(300, 400);
    if (!auxEvent.isAuxCallOk(r)) logw("otgClickPoint 失败: {}", r);
    else logd("otgClickPoint 成功");
}
main();
```

### otgPress

* OTG 长按坐标（对应 otgEvent.press）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API otgEvent 脱机版6.6.0+）
* @param x X 坐标
* @param y Y 坐标
* @param delay 长按时长，单位毫秒
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgPress(300, 400, 1000);
    if (!auxEvent.isAuxCallOk(r)) logw("otgPress 失败: {}", r);
    else logd("otgPress 成功");
}
main();
```

### otgDoubleClickPoint

* OTG 双击坐标（对应 otgEvent.doubleClickPoint）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API otgEvent 脱机版6.6.0+）
* @param x X 坐标
* @param y Y 坐标
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgDoubleClickPoint(300, 400);
    if (!auxEvent.isAuxCallOk(r)) logw("otgDoubleClickPoint 失败: {}", r);
    else logd("otgDoubleClickPoint 成功");
}
main();
```

### otgSwipeToPoint

* OTG 坐标滑动（对应 otgEvent.swipeToPoint）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API otgEvent 脱机版6.6.0+）
* @param startX 起始 X
* @param startY 起始 Y
* @param endX 结束 X
* @param endY 结束 Y
* @param duration 持续时长，单位毫秒
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgSwipeToPoint(300, 400, 500, 600, 1000);
    if (!auxEvent.isAuxCallOk(r)) logw("otgSwipeToPoint 失败: {}", r);
    else logd("otgSwipeToPoint 成功");
}
main();
```

### otgMultiTouch

* OTG 多点触摸（对应 otgEvent.multiTouch）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API otgEvent 脱机版6.6.0+）
* 触摸参数 action：按下 0，弹起 1，移动 2；pointer 为手指序号；delay 为动作延迟毫秒
* @param touch1 触摸点数组，例如 `[{"action":0,"x":1,"y":1,"pointer":1,"delay":20}]`
* @param timeout 多点触摸总超时，单位毫秒
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let touch1 = [{"action":0,"x":300,"y":400,"pointer":1,"delay":20},{"action":1,"x":300,"y":400,"pointer":1,"delay":20}];
    let r = auxEvent.otgMultiTouch(touch1, 1000);
    if (!auxEvent.isAuxCallOk(r)) logw("otgMultiTouch 失败: {}", r); else logd("otgMultiTouch 成功");
}
main();
```

### otgPressMouseBtn

* OTG 点击鼠标键（辅助触控自定义按钮等）（对应 otgEvent.pressMouseBtn）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API otgEvent 脱机版6.6.0+）
* @param btn 1=左键，2=右键，3=滚轮，4-8=自定义
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgPressMouseBtn(1);
    if (!auxEvent.isAuxCallOk(r)) logw("otgPressMouseBtn 失败: {}", r);
    else logd("otgPressMouseBtn 成功");
}
main();
```

### 按键与其它

### otgSystemKey

* OTG 系统按键（对应 otgEvent.systemKey）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API otgEvent 脱机版6.6.0+）
* @param key 例如 home、recents（最近任务）
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgSystemKey("home");
    if (!auxEvent.isAuxCallOk(r)) logw("otgSystemKey 失败: {}", r);
    else logd("otgSystemKey 成功");
}
main();
```

### otgToggleSoftKeyboard

* OTG 开关软键盘（对应 otgEvent.toggleSoftKeyboard）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API otgEvent 脱机版6.6.0+）
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgToggleSoftKeyboard();
    if (!auxEvent.isAuxCallOk(r)) logw("otgToggleSoftKeyboard 失败: {}", r);
    else logd("otgToggleSoftKeyboard 成功");
}
main();
```

### otgKeyPressChar

* OTG 字符按键（对应 otgEvent.keyPressChar）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API otgEvent 脱机版6.6.0+）
* @param prefix 组合键：alt、ctrl、gui、shift、r_ctrl、r_shift，可为空
* @param code 字符，例如 a、Enter、BS=删除
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgKeyPressChar("", "a");
    if (!auxEvent.isAuxCallOk(r)) logw("otgKeyPressChar 失败: {}", r);
    else logd("otgKeyPressChar 成功");
}
main();
```

### otgKeyPress

* OTG 按键（ASCII 码）（对应 otgEvent.keyPress）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API otgEvent 脱机版6.6.0+）
* @param prefix 组合键，可为空
* @param code 整型 ASCII 码，参考 https://tool.oschina.net/commons?type=4
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgKeyPress("", 65);
    if (!auxEvent.isAuxCallOk(r)) logw("otgKeyPress 失败: {}", r);
    else logd("otgKeyPress 成功");
}
main();
```

### otgLight

* OTG 开发板 LED 闪烁（对应 otgEvent.light）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API otgEvent 脱机版6.6.0+）
* @param num 闪烁次数
* @param lightToOff 亮到灭的间隔，单位毫秒
* @param offToLight 灭到亮的间隔，单位毫秒
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.otgLight(3, 200, 200);
    if (!auxEvent.isAuxCallOk(r)) logw("otgLight 失败: {}", r);
    else logd("otgLight 成功");
}
main();
```

---

## 5、BLE 远程

经手机 Aux 转发蓝牙 HID（对应 bleEvent.js）

### 连接与配置

### bleStartConnect

* 扫描并连接 BLE 开发板（对应 bleEvent.startConnect）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @param bleDeviceName BLE 设备名
* @param save 是否保存连接信息
* @param timeout 超时，单位毫秒
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleStartConnect("", false, 1000);
    if (!auxEvent.isAuxCallOk(r)) logw("bleStartConnect 失败: {}", r);
    else logd("bleStartConnect 成功");
}
main();
```

### bleStopConnect

* 断开 BLE 连接（对应 bleEvent.stopConnect）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleStopConnect();
    if (!auxEvent.isAuxCallOk(r)) logw("bleStopConnect 失败: {}", r);
    else logd("bleStopConnect 成功");
}
main();
```

### bleIsConnect

* 查询 BLE 连接状态（对应 bleEvent.isConnected）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @returns `{boolean}` true 表示已连接

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

* 搜索 BLE 开发板 IP（对应 bleEvent.searchBleIp）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @param force 是否强制重新搜索
* @param timeout 超时，单位毫秒
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleSearchBleIp(false, 1000);
    if (!auxEvent.isAuxCallOk(r)) logw("bleSearchBleIp 失败: {}", r);
    else logd("bleSearchBleIp 成功");
}
main();
```

### bleGetConfigBleName

* 获取 app 配置的蓝牙名称（对应 bleEvent.getConfigBleName）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.5.0+）
* @returns `{string}` 配置的蓝牙名称；无实例或未配置时返回空字符串

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleGetConfigBleName();
    if (!auxEvent.isAuxCallOk(r)) logw("bleGetConfigBleName 失败: {}", r);
    else logd("bleGetConfigBleName 成功");
}
main();
```

### bleGetScale

* 获取当前 BLE 鼠标缩放配置（对应 bleEvent.getScale）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @returns `{string}` 缩放 JSON 字符串或错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleGetScale();
    if (!auxEvent.isAuxCallOk(r)) logw("bleGetScale 失败: {}", r);
    else logd("bleGetScale 成功");
}
main();
```

### bleSetScale

* 设置 BLE 鼠标补偿比率（对应 bleEvent.setScale）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @param x_scale X 轴缩放浮点数，默认约 2.0
* @param y_scale Y 轴缩放浮点数，默认约 2.0
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleSetScale(2.0, 2.0);
    if (!auxEvent.isAuxCallOk(r)) logw("bleSetScale 失败: {}", r);
    else logd("bleSetScale 成功");
}
main();
```

### bleSetScreenSize

* 设置 BLE 屏幕尺寸（防止鼠标移出屏幕）（对应 bleEvent.setScreenSize）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @param w 屏幕宽度
* @param h 屏幕高度
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleSetScreenSize(390, 844);
    if (!auxEvent.isAuxCallOk(r)) logw("bleSetScreenSize 失败: {}", r);
    else logd("bleSetScreenSize 成功");
}
main();
```

### bleSetStep

* BLE 设置鼠标步进（对应 bleEvent.setStep）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @param step 步进值
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleSetStep(1);
    if (!auxEvent.isAuxCallOk(r)) logw("bleSetStep 失败: {}", r);
    else logd("bleSetStep 成功");
}
main();
```

### bleSetWifiInfo

* 配置 BLE 开发板 WiFi（SSID/密码）（对应 bleEvent.setWifiInfo）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @param name WiFi 名称
* @param pwd WiFi 密码
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleSetWifiInfo("WiFi名称", "密码");
    if (!auxEvent.isAuxCallOk(r)) logw("bleSetWifiInfo 失败: {}", r);
    else logd("bleSetWifiInfo 成功");
}
main();
```

### bleSendCmdType

* 设置 BLE 通信方式（串口 / 网络）（对应 bleEvent.sendCmdType）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @param type 1=串口，2=网络
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleSendCmdType(1);
    if (!auxEvent.isAuxCallOk(r)) logw("bleSendCmdType 失败: {}", r);
    else logd("bleSendCmdType 成功");
}
main();
```

### bleResetBle

* 重置 BLE 连接/状态（对应 bleEvent.resetBle）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleResetBle();
    if (!auxEvent.isAuxCallOk(r)) logw("bleResetBle 失败: {}", r);
    else logd("bleResetBle 成功");
}
main();
```

### bleShowBleName

* 显示 BLE 设备名称（校准/配对辅助）（对应 bleEvent.showBleName）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleShowBleName();
    if (!auxEvent.isAuxCallOk(r)) logw("bleShowBleName 失败: {}", r);
    else logd("bleShowBleName 成功");
}
main();
```

### bleHideBleName

* 隐藏 BLE 设备名称（对应 bleEvent.hideBleName）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleHideBleName();
    if (!auxEvent.isAuxCallOk(r)) logw("bleHideBleName 失败: {}", r);
    else logd("bleHideBleName 成功");
}
main();
```

### 鼠标与触摸

### bleResetZero

* BLE 鼠标归零（对应 bleEvent.resetZero）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleResetZero();
    if (!auxEvent.isAuxCallOk(r)) logw("bleResetZero 失败: {}", r);
    else logd("bleResetZero 成功");
}
main();
```

### bleMouseMove

* BLE 移动鼠标（相对位移）（对应 bleEvent.mouseMove）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @param x X 方向位移
* @param y Y 方向位移
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleMouseMove(300, 400);
    if (!auxEvent.isAuxCallOk(r)) logw("bleMouseMove 失败: {}", r);
    else logd("bleMouseMove 成功");
}
main();
```

### bleMouseMoveByDistance

* BLE 按距离移动鼠标（绝对步进）（对应 bleEvent.mouseMoveByDistance）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @param x_dis X 方向位移
* @param y_dis Y 方向位移
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleMouseMoveByDistance(10, 10);
    if (!auxEvent.isAuxCallOk(r)) logw("bleMouseMoveByDistance 失败: {}", r);
    else logd("bleMouseMoveByDistance 成功");
}
main();
```

### bleMouseMoveDistance

* BLE 按距离移动鼠标（可选按下）（对应 bleEvent.mouseMoveDistance）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @param x_dis X 方向位移
* @param y_dis Y 方向位移
* @param press 移动时是否按下
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleMouseMoveDistance(10, 10, false);
    if (!auxEvent.isAuxCallOk(r)) logw("bleMouseMoveDistance 失败: {}", r);
    else logd("bleMouseMoveDistance 成功");
}
main();
```

### bleTouchDown

* BLE 按下触摸点（对应 bleEvent.touchDown）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @param x X 坐标
* @param y Y 坐标
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleTouchDown(300, 400);
    if (!auxEvent.isAuxCallOk(r)) logw("bleTouchDown 失败: {}", r);
    else logd("bleTouchDown 成功");
}
main();
```

### bleTouchMove

* BLE 移动触摸点（对应 bleEvent.touchMove）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @param x X 坐标
* @param y Y 坐标
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleTouchMove(300, 400);
    if (!auxEvent.isAuxCallOk(r)) logw("bleTouchMove 失败: {}", r);
    else logd("bleTouchMove 成功");
}
main();
```

### bleTouchUp

* BLE 抬起触摸点（对应 bleEvent.touchUp）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @param x X 坐标
* @param y Y 坐标
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleTouchUp(300, 400);
    if (!auxEvent.isAuxCallOk(r)) logw("bleTouchUp 失败: {}", r);
    else logd("bleTouchUp 成功");
}
main();
```

### bleClickPoint

* BLE 点击坐标（对应 bleEvent.clickPoint）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @param x X 坐标
* @param y Y 坐标
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleClickPoint(300, 400);
    if (!auxEvent.isAuxCallOk(r)) logw("bleClickPoint 失败: {}", r);
    else logd("bleClickPoint 成功");
}
main();
```

### blePress

* BLE 长按坐标（对应 bleEvent.press）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @param x X 坐标
* @param y Y 坐标
* @param delay 长按时长，单位毫秒
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.blePress(300, 400, 1000);
    if (!auxEvent.isAuxCallOk(r)) logw("blePress 失败: {}", r);
    else logd("blePress 成功");
}
main();
```

### bleDoubleClickPoint

* BLE 双击坐标（对应 bleEvent.doubleClickPoint）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @param x X 坐标
* @param y Y 坐标
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleDoubleClickPoint(300, 400);
    if (!auxEvent.isAuxCallOk(r)) logw("bleDoubleClickPoint 失败: {}", r);
    else logd("bleDoubleClickPoint 成功");
}
main();
```

### bleSwipeToPoint

* BLE 坐标滑动（对应 bleEvent.swipeToPoint）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @param startX 起始 X
* @param startY 起始 Y
* @param endX 结束 X
* @param endY 结束 Y
* @param duration 持续时长，单位毫秒
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleSwipeToPoint(300, 400, 500, 600, 1000);
    if (!auxEvent.isAuxCallOk(r)) logw("bleSwipeToPoint 失败: {}", r);
    else logd("bleSwipeToPoint 成功");
}
main();
```

### bleMultiTouch

* BLE 多点触摸（对应 bleEvent.multiTouch）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @param touch1 触摸点数组，格式同 otgMultiTouch
* @param timeout 总超时，单位毫秒
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let touch1 = [{"action":0,"x":300,"y":400,"pointer":1,"delay":20},{"action":1,"x":300,"y":400,"pointer":1,"delay":20}];
    let r = auxEvent.bleMultiTouch(touch1, 1000);
    if (!auxEvent.isAuxCallOk(r)) logw("bleMultiTouch 失败: {}", r); else logd("bleMultiTouch 成功");
}
main();
```

### blePressMouseBtn

* BLE 点击鼠标键（对应 bleEvent.pressMouseBtn）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @param btn 鼠标键编号，一般 1 左键 2 右键 3 滚轮
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.blePressMouseBtn(1);
    if (!auxEvent.isAuxCallOk(r)) logw("blePressMouseBtn 失败: {}", r);
    else logd("blePressMouseBtn 成功");
}
main();
```

### 按键与其它

### bleSystemKey

* BLE 系统按键（对应 bleEvent.systemKey）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @param key 例如 home、recents
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleSystemKey("home");
    if (!auxEvent.isAuxCallOk(r)) logw("bleSystemKey 失败: {}", r);
    else logd("bleSystemKey 成功");
}
main();
```

### bleToggleSoftKeyboard

* BLE 开关软键盘（对应 bleEvent.toggleSoftKeyboard）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleToggleSoftKeyboard();
    if (!auxEvent.isAuxCallOk(r)) logw("bleToggleSoftKeyboard 失败: {}", r);
    else logd("bleToggleSoftKeyboard 成功");
}
main();
```

### bleKeyPressChar

* BLE 字符按键（对应 bleEvent.keyPressChar）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @param prefix 组合键，可为空
* @param code 字符
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleKeyPressChar("", "a");
    if (!auxEvent.isAuxCallOk(r)) logw("bleKeyPressChar 失败: {}", r);
    else logd("bleKeyPressChar 成功");
}
main();
```

### bleKeyPress

* BLE 按键（ASCII 码）（对应 bleEvent.keyPress）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @param prefix 组合键，可为空
* @param code 整型 ASCII 码
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleKeyPress("", 65);
    if (!auxEvent.isAuxCallOk(r)) logw("bleKeyPress 失败: {}", r);
    else logd("bleKeyPress 成功");
}
main();
```

### bleLight

* BLE 开发板 LED 闪烁（对应 bleEvent.light）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API bleEvent 脱机版6.6.0+）
* @param num 闪烁次数
* @param lightToOff 亮到灭间隔，单位毫秒
* @param offToLight 灭到亮间隔，单位毫秒
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.isAuxCallOk(auxEvent.ensureWifiSession(""))) return;
    let r = auxEvent.bleLight(3, 200, 200);
    if (!auxEvent.isAuxCallOk(r)) logw("bleLight 失败: {}", r);
    else logd("bleLight 成功");
}
main();
```

---

## 6、输入法

经 Aux 转发自定义输入法（对应 imeApi.js）

### imeIsOk

* 输入法状态是否可用（对应 imeApi.isOk）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API imeApi 脱机版3.15.0+）
* 经 Aux HTTP 转发至脱机版本 ImeApi
* @returns `{boolean}` true 代表可用 false 代表不可用

```javascript showLineNumbers
function main() {
    let r = auxEvent.imeIsOk();
    logd("imeIsOk: {}", r);
}
main();
```

### imeInput

* 输入字符串（对应 imeApi.input）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API imeApi 脱机版3.15.0+）
* 经 Aux HTTP 转发至脱机版本 ImeApi
* @param content 字符串
* @returns `{string}` 如果为空，代表输入不成功，如果不为空，代表输入的数据

```javascript showLineNumbers
function main() {
    if (!auxEvent.imeIsOk()) { logw("输入法未就绪"); return; }
    let r = auxEvent.imeInput("测试内容");
    logd("imeInput: {}", r);
}
main();
```

### imePaste

* 粘贴字符串，复制到剪切板后再插入到输入框（对应 imeApi.paste）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API imeApi 脱机版3.15.0+）
* 经 Aux HTTP 转发至脱机版本 ImeApi
* @param content 字符串，如果为空，直接使用剪切板数据
* @returns `{string}` 如果为空，代表不成功，如果不为空，代表输入的数据

```javascript showLineNumbers
function main() {
    if (!auxEvent.imeIsOk()) { logw("输入法未就绪"); return; }
    let r = auxEvent.imePaste("测试内容");
    logd("imePaste: {}", r);
}
main();
```

### imePressDel

* 删除输入框的字符串（对应 imeApi.pressDel）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API imeApi 脱机版3.15.0+）
* 经 Aux HTTP 转发至脱机版本 ImeApi
* @returns `{string}` 如果为空，代表输入框无数据，如果不为空，代表输入框剩余数据

```javascript showLineNumbers
function main() {
    if (!auxEvent.imeIsOk()) { logw("输入法未就绪"); return; }
    let r = auxEvent.imePressDel();
    logd("imePressDel: {}", r);
}
main();
```

### imePressReturn

* 回车键（对应 imeApi.pressEnter / pressReturn）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API imeApi 脱机版3.15.0+）
* 经 Aux HTTP 转发至脱机版本 ImeApi
* @returns `{string}` 服务原始响应

```javascript showLineNumbers
function main() {
    if (!auxEvent.imeIsOk()) { logw("输入法未就绪"); return; }
    let r = auxEvent.imePressReturn();
    logd("imePressReturn: {}", r);
}
main();
```

### imeDismiss

* 隐藏键盘（对应 imeApi.dismiss）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API imeApi 脱机版3.15.0+）
* 经 Aux HTTP 转发至脱机版本 ImeApi
* @returns `{string}` 服务原始响应

```javascript showLineNumbers
function main() {
    if (!auxEvent.imeIsOk()) { logw("输入法未就绪"); return; }
    let r = auxEvent.imeDismiss();
    logd("imeDismiss: {}", r);
}
main();
```

### imeCopyToClipboard

* 复制输入框的数据到剪切板（对应 imeApi.copyToClipboard）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API imeApi 脱机版3.15.0+）
* 经 Aux HTTP 转发至脱机版本 ImeApi
* @returns `{string}` 如果为空，代表输入框无数据，如果不为空，代表输入框剩余数据，并且已经复制到剪切板了

```javascript showLineNumbers
function main() {
    if (!auxEvent.imeIsOk()) { logw("输入法未就绪"); return; }
    let r = auxEvent.imeCopyToClipboard();
    logd("imeCopyToClipboard: {}", r);
}
main();
```

### imeChangeKeyboard

* 切换到其他键盘（对应 imeApi.changeKeyboard）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API imeApi 脱机版3.15.0+）
* 这个是返回结果后，等待2秒切换
* 经 Aux HTTP 转发至脱机版本 ImeApi
* @returns `{string}` 服务原始响应

```javascript showLineNumbers
function main() {
    if (!auxEvent.imeIsOk()) { logw("输入法未就绪"); return; }
    let r = auxEvent.imeChangeKeyboard();
    logd("imeChangeKeyboard: {}", r);
}
main();
```

### imeRemoveAllContent

* 清空输入框的内容（对应 imeApi.removeAllContent）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API imeApi 脱机版3.15.0+）
* 经 Aux HTTP 转发至脱机版本 ImeApi
* @returns `{string}` 服务原始响应

```javascript showLineNumbers
function main() {
    if (!auxEvent.imeIsOk()) { logw("输入法未就绪"); return; }
    let r = auxEvent.imeRemoveAllContent();
    logd("imeRemoveAllContent: {}", r);
}
main();
```

### imeGetClipboard

* 读取剪切板的数据（对应 imeApi.getClipboard）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API imeApi 脱机版3.15.0+）
* 经 Aux HTTP 转发至脱机版本 ImeApi
* @returns `{string}` 剪切板的数据；无数据时返回空字符串

```javascript showLineNumbers
function main() {
    if (!auxEvent.imeIsOk()) { logw("输入法未就绪"); return; }
    let r = auxEvent.imeGetClipboard();
    logd("imeGetClipboard: {}", r);
}
main();
```

### imeSetClipboard

* 设置剪切板数据（对应 imeApi.setClipboard）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API imeApi 脱机版3.15.0+）
* 经 Aux HTTP 转发至脱机版本 ImeApi
* @param content 字符串
* @param type1 1 代表是普通的字符串，2 代表是URL数据
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.imeIsOk()) { logw("输入法未就绪"); return; }
    let r = auxEvent.imeSetClipboard("测试内容", 1);
    logd("imeSetClipboard: {}", r);
}
main();
```

### imeOpenUrl

* 打开 URL 链接（对应 imeApi.openUrl）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API imeApi 脱机版3.15.0+）
* 经 Aux HTTP 转发至脱机版本 ImeApi
* @param url url地址，例如 http://baidu.com 之类的数据
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    if (!auxEvent.imeIsOk()) { logw("输入法未就绪"); return; }
    let r = auxEvent.imeOpenUrl("http://example.com");
    logd("imeOpenUrl: {}", r);
}
main();
```

### imeGetText

* 获取输入框数据（对应 imeApi.getText）
* 适配EC iOS USB版本9.39.0+（对应脱机版本API imeApi 脱机版3.15.0+）
* 经 Aux HTTP 转发至脱机版本 ImeApi
* @returns `{string}` 空代表无数据或者未获取到，有数据代表获取到了

```javascript showLineNumbers
function main() {
    if (!auxEvent.imeIsOk()) { logw("输入法未就绪"); return; }
    let r = auxEvent.imeGetText();
    logd("imeGetText: {}", r);
}
main();
```

---

---

## 7、设备信息

经 Aux 读取手机设备信息（对应 device.js）

### 基础信息

### deviceGetDeviceIdentifier

* 获取设备标识符（对应 device.getDeviceIdentifier）
* 适配EC iOS USB版本9.39.0+
* 经 Aux HTTP 转发至脱机版本 DeviceApi
* @returns `{string}` 设备标识符；失败时返回空字符串

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.deviceGetDeviceIdentifier();
    logd("deviceGetDeviceIdentifier: {}", r);
}
main();
```

### deviceGetDeviceId

* 获取设备号，这里获取的是通过脱机版激活器传递的设备号（对应 device.getDeviceId）
* 适配EC iOS USB版本9.39.0+
* 支持EC iOS脱机版本2.0+
* 经 Aux HTTP 转发至脱机版本 DeviceApi
* @returns `{string}` 设备号；失败时返回空字符串

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.deviceGetDeviceId();
    logd("deviceGetDeviceId: {}", r);
}
main();
```

### deviceGetModel

* 取得手机机型（对应 device.getModel）
* 适配EC iOS USB版本9.39.0+
* 经 Aux HTTP 转发至脱机版本 DeviceApi
* @returns `{string}` 机型；失败时返回空字符串

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.deviceGetModel();
    logd("deviceGetModel: {}", r);
}
main();
```

### deviceGetOSVersion

* 取得手机版本号，例如 6.0 等字符串（对应 device.getOSVersion）
* 适配EC iOS USB版本9.39.0+
* 经 Aux HTTP 转发至脱机版本 DeviceApi
* @returns `{string}` 系统版本；失败时返回空字符串

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.deviceGetOSVersion();
    logd("deviceGetOSVersion: {}", r);
}
main();
```

### deviceGetBattery

* 取得电量（对应 device.getBattery）
* 适配EC iOS USB版本9.39.0+
* 经 Aux HTTP 转发至脱机版本 DeviceApi
* @returns `{number}` 电量 1-100

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.deviceGetBattery();
    logd("deviceGetBattery: {}", r);
}
main();
```

### deviceIsCharging

* 是否正在充电（对应 device.isCharging）
* 适配EC iOS USB版本9.39.0+
* 经 Aux HTTP 转发至脱机版本 DeviceApi
* @returns `{boolean}` true 充电 false 不充电

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.deviceIsCharging();
    logd("deviceIsCharging: {}", r);
}
main();
```

### deviceGetScale

* 获取屏幕缩放比（对应 device.getScale）
* 适配EC iOS USB版本9.39.0+
* 经 Aux HTTP 转发至脱机版本 DeviceApi
* @returns `{number}` 缩放比

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.deviceGetScale();
    logd("deviceGetScale: {}", r);
}
main();
```

### 屏幕与方向

### deviceGetScreenWidth

* [过期]有临界值问题，建议用 deviceGetScreenWidthHeight（对应 device.getScreenWidth）
* 适配EC iOS USB版本9.39.0+
* 经 Aux HTTP 转发至脱机版本 DeviceApi
* @returns `{number}` 屏幕宽度

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.deviceGetScreenWidth();
    logd("deviceGetScreenWidth: {}", r);
}
main();
```

### deviceGetScreenHeight

* [过期]有临界值问题，建议用 deviceGetScreenWidthHeight（对应 device.getScreenHeight）
* 适配EC iOS USB版本9.39.0+
* 经 Aux HTTP 转发至脱机版本 DeviceApi
* @returns `{number}` 屏幕高度

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.deviceGetScreenHeight();
    logd("deviceGetScreenHeight: {}", r);
}
main();
```

### deviceGetScreenWidthHeight

* 取得屏幕宽度、高度字符串，需要自己做分割（对应 device.getScreenWidthHeight）
* 适配EC iOS USB版本9.39.0+
* 经 Aux HTTP 转发至脱机版本 DeviceApi
* @returns `{string}` 宽度,高度 格式；失败时返回空字符串

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.deviceGetScreenWidthHeight();
    logd("deviceGetScreenWidthHeight: {}", r);
}
main();
```

### deviceGetWidthNoAuto

* 获取屏幕宽度(无需自动化服务)（对应 device.getWidthNoAuto）
* 适配EC iOS USB版本9.39.0+
* 经 Aux HTTP 转发至脱机版本 DeviceApi
* @returns `{number}` 宽度

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.deviceGetWidthNoAuto();
    logd("deviceGetWidthNoAuto: {}", r);
}
main();
```

### deviceGetHeightNoAuto

* 获取屏幕高度(无需自动化服务)（对应 device.getHeightNoAuto）
* 适配EC iOS USB版本9.39.0+
* 经 Aux HTTP 转发至脱机版本 DeviceApi
* @returns `{number}` 高度

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.deviceGetHeightNoAuto();
    logd("deviceGetHeightNoAuto: {}", r);
}
main();
```

### deviceGetOrientation

* 获取屏幕方向（对应 device.getOrientation / agentEvent.getOrientation）
* 适配EC iOS USB版本9.39.0+
* 经 Aux HTTP 转发至脱机版本 DeviceApi
* @returns `{string}` 1 竖屏，2 横屏（向右旋转90度(顺时针)），其他未知时返回空字符串

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.deviceGetOrientation();
    logd("deviceGetOrientation: {}", r);
}
main();
```

### deviceGetOrientationNoAuto

* 无自动化获取屏幕方向（对应 device.getOrientationNoAuto）
* 适配EC iOS USB版本9.39.0+
* 无需自动化服务获取屏幕的方向
* 经 Aux HTTP 转发至脱机版本 DeviceApi
* @returns `{string}` 1 竖屏 2 横屏，其他未知时返回空字符串

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.deviceGetOrientationNoAuto();
    logd("deviceGetOrientationNoAuto: {}", r);
}
main();
```

### deviceSetOrientation

* 设置屏幕方向，横屏只支持向右旋转90度（对应 device.setOrientation）
* 适配EC iOS USB版本9.39.0+
* 经 Aux HTTP 转发至脱机版本 DeviceApi
* @param orz orientation 1 正常的竖屏，2 向右旋转90度(顺时针)
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

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

## 8、工具与相册

剪贴板、URL、相册权限与图视频上传

### 工具函数

### utilGetClipboardText

* 读取剪贴板文本（对应 utils.getClipboardText）
* 适配EC iOS USB版本9.39.0+
* 注意：可以开启画中画或者 takeMeToFront 使本程序在前台
* 经 Aux HTTP 转发至脱机版本 UtilsApi
* @returns `{string}` 剪贴板文本；无数据时返回空字符串

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let ok = auxEvent.utilGetClipboardText();
    logd("utilGetClipboardText: {}", ok);
}
main();
```

### utilSetClipboardText

* 设置剪贴板文本（对应 utils.setClipboardText）
* 适配EC iOS USB版本9.39.0+
* 注意：可以开启画中画或者 takeMeToFront 使本程序在前台
* 经 Aux HTTP 转发至脱机版本 UtilsApi
* @param text 文本
* @param type 1 文本 2 链接，默认 1
* @returns `{boolean}` true 代表成功，false 代表失败

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let ok = auxEvent.utilSetClipboardText("测试内容", 1);
    logd("utilSetClipboardText: {}", ok);
}
main();
```

### utilOpenUrl

* 打开 URL（对应 utils.openUrl）
* 适配EC iOS USB版本9.39.0+
* 注意：需要重新在前台运行，先调用 takeMeToFront 函数，将本程序放前台
* 经 Aux HTTP 转发至脱机版本 UtilsApi
* @param url url地址
* @returns `{boolean}` true 代表成功 false 代表失败

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let ok = auxEvent.utilOpenUrl("http://example.com");
    logd("utilOpenUrl: {}", ok);
}
main();
```

### utilRequestPhotoAuthorization

* 请求相册权限（对应 utils.requestPhotoAuthorization）
* 适配EC iOS USB版本9.39.0+
* 第一次请求会有弹窗权限，请允许，或者去手机设置-拉倒最底部，找到 EC app，进入勾选照片权限
* 注意: 这些都是异步的，防止卡住不能模拟点击，请忽略返回值
* 支持版本 EC 脱机4.9.0+
* 经 Aux HTTP 转发至脱机版本 UtilsApi
* @returns `{boolean}` true 代表成功 false 代表失败

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let ok = auxEvent.utilRequestPhotoAuthorization();
    logd("utilRequestPhotoAuthorization: {}", ok);
}
main();
```

### utilDeleteAllPhotos

* 清空相册中的图片（对应 utils.deleteAllPhotos）
* 适配EC iOS USB版本9.39.0+
* 调用时候会有弹窗确定，请模拟点击删除按钮
* 注意: 这些都是异步的，防止卡住不能模拟点击，请忽略返回值
* 支持版本 EC 脱机4.9.0+
* 经 Aux HTTP 转发至脱机版本 UtilsApi
* @returns `{boolean}` true 代表成功 false 代表失败

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let ok = auxEvent.utilDeleteAllPhotos();
    logd("utilDeleteAllPhotos: {}", ok);
}
main();
```

### utilDeleteAllVideos

* 清空相册中的视频（对应 utils.deleteAllVideos）
* 适配EC iOS USB版本9.39.0+
* 调用时候会有弹窗确定，请模拟点击删除按钮
* 注意: 这些都是异步的，防止卡住不能模拟点击，请忽略返回值
* 支持版本 EC 脱机4.9.0+
* 经 Aux HTTP 转发至脱机版本 UtilsApi
* @returns `{boolean}` true 代表成功 false 代表失败

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let ok = auxEvent.utilDeleteAllVideos();
    logd("utilDeleteAllVideos: {}", ok);
}
main();
```

### 相册上传

### auxInsertImageToAlbum

* 通过路径保存图片到相册中去（对应 utils.saveImageToAlbumPath）
* 适配EC iOS USB版本9.39.0+
* PC 本地文件走 multipart 上传，手机路径走 operateAlbum
* 经 Aux HTTP 转发至脱机版本API
* @param path 文件的路径（PC 或手机本地路径）
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.auxInsertImageToAlbum("/path/to/file");
    if (!auxEvent.isAuxCallOk(r)) logw("auxInsertImageToAlbum 失败: {}", r);
    else logd("auxInsertImageToAlbum 成功");
}
main();
```

### auxInsertVideoToAlbum

* 通过路径保存视频到相册中去（对应 utils.saveVideoToAlbumPath）
* 适配EC iOS USB版本9.39.0+
* PC 本地文件走 multipart 上传，手机路径走 operateAlbum
* 经 Aux HTTP 转发至脱机版本API
* @param path 视频文件的路径（PC 或手机本地路径）
* @returns `{string|null}` null 或空字符串代表成功，其他为错误信息

```javascript showLineNumbers
function main() {
    auxEvent.ensureUsbSession();
    let r = auxEvent.auxInsertVideoToAlbum("/path/to/file");
    if (!auxEvent.isAuxCallOk(r)) logw("auxInsertVideoToAlbum 失败: {}", r);
    else logd("auxInsertVideoToAlbum 成功");
}
main();
```

---

---

## 相关文档

- [蓝牙 BLE 函数](/iosdocs/zh-cn/funcs/ble-event-api.md)
- [OTG HID 函数](/iosdocs/zh-cn/funcs/otg-event-api.md)
- [输入法函数](/iosdocs/zh-cn/funcs/ime-api.md)
- [设备函数](/iosdocs/zh-cn/funcs/device-api.md)
- [常用工具函数](/iosdocs/zh-cn/funcs/utils-api.md)
- [iOS USB 投屏教程](/iosdocs/zh-cn/advance/ios-usb-screen.md)
