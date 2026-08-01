---
title: 蓝牙BLE教程
description: EasyClick 自动化脚本 鸿蒙Next USB 蓝牙BLE教程
keywords:
  - EasyClick 自动化脚本 鸿蒙Next USB 蓝牙BLE教程
  - 鸿蒙Next
  - USB
  - ble
  - BLE
  - ESP32C3
  - ESP32S3
  - img
  - src
  - hmimg
  - c3
  - EasyClick
  - 手机自动化
  - 自动化测试
  - 脚本开发
  - 安卓自动化
  - iOS自动化
---

## 说明

- 适配版本：**鸿蒙Next USB 中控 3.2.0+**
- 形态与 [iOS USB 蓝牙 BLE](/iosdocs/zh-cn/advance/ios-usb-ble) 一致：手机只当 **HID Host**；电脑/中控经串口或 WiFi 连接 ESP32 开发板，板再对手机发送蓝牙 HID
- 支持 **ESP32-C3 / ESP32-S3**，分别有**焊接引脚**与**不焊引脚**固件，刷写时务必选对芯片型号与板型
- 蓝牙 HID 与 HDC 自动化、投屏、USB HID 等能力不冲突，可组合使用，也可单独使用 BLE 做点击/按键
- 固件免费，开发板自行在淘宝、拼多多、1688 购买；示意：
  <br/><img src="/hmimg/ble/ble-c3.jpg" alt="" style={{zoom:'10%'}} />
- 脚本 API 见 [蓝牙 BLE 事件](/hmdocs/zh-cn/funcs/ble-event-api)

## 下载固件

- 请到网盘 **鸿蒙Next资源文件夹** → **USB版本** → **蓝牙固件** 下载对应开发板固件
- **不要**刷入 iOS USB 专用蓝牙固件
- 注意区分：
  - 芯片：**C3** / **S3**
  - 板型：**焊接** / **不焊**
  - 功能：请使用**含键盘**版本（HOME、按键、快捷键等依赖键盘 HID）

## 刷入固件

- 刷入步骤与安卓一致，参考 [安卓蓝牙刷入固件](/docs/zh-cn/advance/blehid#刷入固件)
- 刷入时选择鸿蒙/安卓 USB 蓝牙固件，不要选错平台
- 获取蓝牙 MAC（名称一般为 MAC 后 8 位）同样参考安卓文档「读取 mac 地址」；中控绑定串口时也会自动读出后 8 位

## 设备与蓝牙绑定

- 建议在开发板上标注蓝牙 MAC 后 8 位，手机侧也可贴对应标签，方便多机管理
- 打开**鸿蒙Next USB 中控**，选中一台已连接设备，右键菜单 → **蓝牙HID设置** → **绑定蓝牙BLE**
  <br/><img src="/hmimg/ble/ble-1.png" alt="" style={{zoom:'30%'}} />
- 选择已连接的串口；若列表为空，可取消「只显示未绑定」类筛选后点**强制刷新**；仍找不到时，可直接在 MAC 输入框填写蓝牙 MAC **后 8 位**后绑定
  <br/><img src="/hmimg/ble/ble-2.png" alt="" style={{zoom:'30%'}} />
- 绑定成功后，中控设备列表 **蓝牙 MAC** 一栏应显示刚绑定的硬件地址
- 身份映射为 **UDID ↔ bleMac**

## 测试蓝牙功能

- 绑定完成后，在手机 **设置 → 蓝牙** 中搜索并配对该开发板（名称一般为 MAC 后 8 位，图标可能显示为键盘/鼠标）
- 中控右键 → **蓝牙HID设置** → **测试蓝牙BLE**
  <br/><img src="/hmimg/ble/ble-3.png" alt="" style={{zoom:'30%'}} />
- 选择通信方式（默认串口），点击 **触控长按** 或 **HOME 按键** 等；手机有对应反应即表示通路正常
  <br/><img src="/hmimg/ble/ble-4.png" alt="" style={{zoom:'30%'}} />
- 无反应时：手机忽略该设备后重新配对，开发板按 RST 重启，再测一次

## 通信功能

- 与开发板通信支持 **串口** 与 **WiFi** 两种方式
  - 串口：开发板 USB/排针串口接到电脑，无需额外配网（`bleEvent.setSendCmdType(1)`）
  - WiFi：需先写入开发板 WiFi 账号密码，再使用网络模式（`setSendCmdType(2)`）
- 中控右键 → **蓝牙HID设置** → **设置WiFi信息**
  <br/><img src="/hmimg/ble/ble-5.png" alt="" style={{zoom:'30%'}} />
- 设置后重启开发板；中控会扫描开发板 IP，成功后设备列表 **硬件 IP / 蓝牙 WiFi IP** 一栏可见
- 中控启动且绑定关系正确时会自动扫描，一般无需手工干预

## 键盘快捷键

- 用于向手机发送组合键（如系统键、自定义快捷键），脚本侧对应 `bleEvent.keyPressChar`
- 中控右键 → **蓝牙HID设置** → **新增键盘快捷键**
  <br/><img src="/hmimg/ble/ble-6.png" alt="" style={{zoom:'30%'}} />
- 选择组合键（如 `gui`）并输入字符，点**发送**即可在已配对手机上触发对应按键
- 系统键也可在测试面板或脚本中调用：`home` / `back` / `recents`（最近任务）等，详见 [蓝牙 BLE 事件](/hmdocs/zh-cn/funcs/ble-event-api)

## 输入功能

### 已开启自动化服务（推荐）

- 中文与长文本请继续用代理输入，例如全局函数 `inputText`

### 仅 BLE、未开自动化

- 可通过 `bleEvent.keyPressChar` 输入英数与快捷键
- 中文仍建议开启自动化后使用 `inputText`

## 截图与图色

- 已开自动化时：正常使用 `image.captureFullScreen`、OCR、YOLO、图色等
- 也可与 USB HID、BLE 点击组合：先截图/找图，再 `bleEvent.clickPoint` 点击
- 触控为**绝对像素坐标**，脚本里需先 `bleEvent.setScreenSize(宽, 高)`（宽高可用截图像素尺寸）

## 常见问题

### 蓝牙连不上

- 按住开发板 **RST** 约 5 秒后松开，手机蓝牙里忽略该设备后再搜一次
- 一块开发板同时只连一台手机；连接成功后蓝牙名称可能隐藏；换绑前请先在手机上断开并忽略，再按 RST
- 刷固件后给开发板重新上电；手机开关一次蓝牙再搜索

### 手机侧需要注意什么

- **设置 → 蓝牙** 中完成配对，并保持蓝牙开启
- 配对后若系统弹出「键盘 / 鼠标 / 输入设备」类权限或连接确认，请允许
- 部分机型需在开发者选项中保持 USB 调试（用于自动化与截图），与 BLE 配对互不替代

### 开发板灯语提示

- 蓝牙配对：常亮约三秒后熄灭
- 蓝牙断开：慢闪约 10 次后熄灭
- 查找蓝牙：快闪约 15 次后熄灭

### 点击偏移或不准确

- 未调用或错误调用 `bleEvent.setScreenSize`（横竖屏切换后需重新设置）
- 通信方式选错（串口占用、或未配网却选了网络模式）
- 绑定的 MAC 与当前 USB 串口不是同一块板

### 中控列表蓝牙 MAC 不更新

- 确认中控版本 ≥ **3.2.0**，且中控已正常启动
- 绑定成功后可在「蓝牙HID设置」中再次打开绑定对话框核对当前 MAC；必要时解绑后重新强制绑定
- 串口被刷写工具或其他程序占用时，先关闭占用再开中控

### 中控提示无线串口 / 读不到 MAC

- 重启中控
- 刷完固件后先关闭刷写工具，再启动中控，避免串口冲突

### 如何配合脚本使用

- 绑定 UDID 与 bleMac，手机已配对开发板
- 脚本示例流程：`setSendCmdType` → `setScreenSize` → `openSerial`（串口模式）→ `clickPoint` / `systemKey` / `keyPressChar`
- 完整函数说明见 [蓝牙 BLE 事件](/hmdocs/zh-cn/funcs/ble-event-api)
- 对比 USB 有线 HID 方案见 [USB HID 事件](/hmdocs/zh-cn/funcs/hid-event-api)
