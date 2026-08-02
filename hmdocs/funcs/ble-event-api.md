---
title: 蓝牙BLE事件
description: EasyClick 自动化脚本 鸿蒙Next 蓝牙BLE事件 bleEvent
keywords:
  - EasyClick 自动化脚本 鸿蒙Next 蓝牙BLE事件
  - bleEvent
  - BLE
  - ESP32
  - openSerial
  - setScreenSize
  - setSendCmdType
  - systemKey
  - keyPressChar
  - clickPoint
  - EasyClick
  - 手机自动化
  - 自动化测试
  - 脚本开发
  - 鸿蒙Next
  - USB
---

## 说明

- 通过 **PC ↔ ESP32 蓝牙 HID 板 ↔ 手机** 注入触控与按键（手机作 HID Host）
- 脚本对象前缀是 `bleEvent`，例如 `bleEvent.clickPoint(100, 200)`
- 使用前请在中控右键 **「蓝牙HID设置」** 完成 **UDID ↔ bleMac** 绑定，手机已配对该板
- 适配版本：**鸿蒙Next USB 中控 3.2.0+**
- 配置与刷固件步骤见 [蓝牙 BLE 教程](/hmdocs/advance/hm-usb-ble)
- 与 [USB HID事件](/hmdocs/funcs/hid-event-api.md)（`hidEvent`）不同：USB HID 走数据线/AOA；本模块走蓝牙板

:::tip
- 触控为**绝对像素坐标**（与安卓 BleCommander 一致），需先 `setScreenSize(宽, 高)`
- 通信方式：`setSendCmdType(1)` 串口（默认） / `setSendCmdType(2)` 网络（板需已配网）
- 返回值约定：`null` 或空字符串表示成功，其他字符串为错误信息
- 中文长文本请继续用代理输入（如 `inputText`），不要依赖 HID 键码
:::

## 连接与配置

### forceRefreshSerialPort 强制刷新串口与 MAC

* 强制刷新串口和 mac 的对应关系
* 适配鸿蒙Next USB 中控 3.2.0+版本
* @returns `{string}` null或者空字符串，代表成功，其他代表错误信息

```javascript showLineNumbers
function main() {
    let r = bleEvent.forceRefreshSerialPort();
    logd(r == null || r === "" ? "成功" : r);
}
main();
```

### setScreenSize 设置屏幕尺寸

* 设置屏幕尺寸（触控坐标 w/h 上下文）
* 如果不知道屏幕尺寸，就使用截图后的图片的宽度和高度
* 适配鸿蒙Next USB 中控 3.2.0+版本
* @param w 屏幕的宽度
* @param h 屏幕的高度
* @returns `{string}` null或者空字符串，代表成功，其他代表错误信息

```javascript showLineNumbers
function main() {
    let img = image.captureFullScreen();
    let w = img.getWidth();
    let h = img.getHeight();
    img.recycle();
    let r = bleEvent.setScreenSize(w, h);
    logd(r == null || r === "" ? "成功" : r);
}
main();
```

### openSerial 打开串口通信

* 打开串口通信（PC↔板，bleConnectMode=1）
* 适配鸿蒙Next USB 中控 3.2.0+版本
* @param timeout 串口通信超时时间 单位是毫秒 默认是15秒
* @returns `{string}` null或者空字符串，代表成功，其他代表错误信息

```javascript showLineNumbers
function main() {
    bleEvent.setSendCmdType(1);
    let r = bleEvent.openSerial(15000);
    logd(r == null || r === "" ? "成功" : r);
}
main();
```

### closeSerial 关闭串口通信

* 关闭串口通信
* 适配鸿蒙Next USB 中控 3.2.0+版本
* @returns `{string}` null或者空字符串，代表成功，其他代表错误信息

```javascript showLineNumbers
function main() {
    let r = bleEvent.closeSerial();
    logd(r == null || r === "" ? "成功" : r);
}
main();
```

### setSerialTimeout 设置串口超时

* 设置串口超时
* 适配鸿蒙Next USB 中控 3.2.0+版本
* @param out 串口通信超时时间 单位是毫秒 默认是15秒
* @returns `{string}` null或者空字符串，代表成功，其他代表错误信息

```javascript showLineNumbers
function main() {
    let r = bleEvent.setSerialTimeout(15000);
    logd(r == null || r === "" ? "成功" : r);
}
main();
```

### setSendCmdType 设置通信方式

* 用来设置是通过串口还是通过网络和开发板通信，默认是串口
* 适配鸿蒙Next USB 中控 3.2.0+版本
* @param tt 1 串口 2 网络
* @returns `{string}` null或者空字符串，代表成功，其他代表错误信息

```javascript showLineNumbers
function main() {
    // 1 串口  2 网络
    let r = bleEvent.setSendCmdType(1);
    logd(r == null || r === "" ? "成功" : r);
}
main();
```

### setWifiInfo 设置开发板 WiFi

* 设置网络信息，方便开发板联网
* 适配鸿蒙Next USB 中控 3.2.0+版本
* @param name WiFi名称
* @param pwd WiFi 密码
* @returns `{string}` null或者空字符串，代表成功，其他代表错误信息

```javascript showLineNumbers
function main() {
    let r = bleEvent.setWifiInfo("your_ssid", "your_password");
    logd(r == null || r === "" ? "成功" : r);
}
main();
```

### resetBle 重启开发板

* 重启开发板（相当于按板子 RST）
* 适配鸿蒙Next USB 中控 3.2.0+版本
* @returns `{string}` null或者空字符串，代表成功，其他代表错误信息

```javascript showLineNumbers
function main() {
    let r = bleEvent.resetBle();
    logd(r == null || r === "" ? "成功" : r);
}
main();
```

### light 点亮 LED

* 点亮开发板 LED
* 适配鸿蒙Next USB 中控 3.2.0+版本
* @param num 循环点亮次数
* @param lightToOff 从亮到灭过程的时间 单位毫秒
* @param offToLight 从灭再到亮的过程时间 单位毫秒
* @returns `{string}` null或者空字符串，代表成功，其他代表错误信息

```javascript showLineNumbers
function main() {
    let r = bleEvent.light(3, 200, 100);
    logd(r == null || r === "" ? "成功" : r);
}
main();
```

### showBleName 显示蓝牙名称

* 显示蓝牙名称，便于搜索配对
* 适配鸿蒙Next USB 中控 3.2.0+版本
* @returns `{string}` null或者空字符串，代表成功，其他代表错误信息

```javascript showLineNumbers
function main() {
    let r = bleEvent.showBleName();
    logd(r == null || r === "" ? "成功" : r);
}
main();
```

### hideBleName 隐藏蓝牙名称

* 隐藏蓝牙名称
* 适配鸿蒙Next USB 中控 3.2.0+版本
* @returns `{string}` null或者空字符串，代表成功，其他代表错误信息

```javascript showLineNumbers
function main() {
    let r = bleEvent.hideBleName();
    logd(r == null || r === "" ? "成功" : r);
}
main();
```

## 系统键与键盘

### systemKey 系统按键

* 系统按键
* 适配鸿蒙Next USB 中控 3.2.0+版本
* @param key `home` / `recents`（最近任务，鸿蒙为 Win+Tab）/ `back`
* @returns `{string}` null或者空字符串，代表成功，其他代表错误信息

```javascript showLineNumbers
function main() {
    bleEvent.setSendCmdType(1);
    bleEvent.openSerial(15000);
    logd(bleEvent.systemKey("home"));
    sleep(1000);
    logd(bleEvent.systemKey("recents"));
    sleep(1000);
    logd(bleEvent.systemKey("back"));
}
main();
```

### keyPress 按键（ASCII 码）

* 按键
* 适配鸿蒙Next USB 中控 3.2.0+版本
* @param prefix 组合键，可以为空：`alt` / `ctrl` / `gui` / `r_ctrl` / `r_shift` / `shift`
* @param code 整型 ASCII，例如 65 表示 `A`，97 表示 `a`
* @returns `{string}` null或者空字符串，代表成功，其他代表错误信息

```javascript showLineNumbers
function main() {
    // 请先聚焦输入框
    let r = bleEvent.keyPress("", 97);
    logd(r == null || r === "" ? "成功" : r);
}
main();
```

### keyPressChar 字符按键

* 字符按键
* 适配鸿蒙Next USB 中控 3.2.0+版本
* @param prefix 组合键，可以为空：`alt` / `ctrl` / `gui` / `r_ctrl` / `r_shift` / `shift`
* @param code 字符，例如 `a`
* @returns `{string}` null或者空字符串，代表成功，其他代表错误信息

```javascript showLineNumbers
function main() {
    // 请先聚焦输入框
    let r = bleEvent.keyPressChar("", "a");
    logd(r == null || r === "" ? "成功" : r);
}
main();
```

## 触控

### clickPoint 点击坐标

* 点击坐标点（绝对像素）
* 适配鸿蒙Next USB 中控 3.2.0+版本
* @param x X坐标
* @param y Y坐标
* @returns `{string}` null或者空字符串，代表成功，其他代表错误信息

```javascript showLineNumbers
function main() {
    bleEvent.setScreenSize(1080, 2400);
    bleEvent.setSendCmdType(1);
    bleEvent.openSerial(15000);
    let r = bleEvent.clickPoint(540, 1200);
    logd(r == null || r === "" ? "成功" : r);
}
main();
```

### doubleClickPoint 双击坐标

* 双击坐标
* 适配鸿蒙Next USB 中控 3.2.0+版本
* @param x X坐标
* @param y Y坐标
* @returns `{string}` null或者空字符串，代表成功，其他代表错误信息

```javascript showLineNumbers
function main() {
    let r = bleEvent.doubleClickPoint(540, 1200);
    logd(r == null || r === "" ? "成功" : r);
}
main();
```

### press 长按坐标

* 长按坐标
* 适配鸿蒙Next USB 中控 3.2.0+版本
* @param x x坐标
* @param y y坐标
* @param delay 长按时间 毫秒
* @returns `{string}` null或者空字符串，代表成功，其他代表错误信息

```javascript showLineNumbers
function main() {
    let r = bleEvent.press(540, 1200, 1500);
    logd(r == null || r === "" ? "成功" : r);
}
main();
```

### swipeToPoint 滑动

* 从一个坐标到另一个坐标的滑动
* 适配鸿蒙Next USB 中控 3.2.0+版本
* @param startX 起始坐标的X轴值
* @param startY 起始坐标的Y轴值
* @param endX 结束坐标的X轴值
* @param endY 结束坐标的Y轴值
* @param duration 持续时长 单位毫秒
* @returns `{string}` null或者空字符串，代表成功，其他代表错误信息

```javascript showLineNumbers
function main() {
    let r = bleEvent.swipeToPoint(200, 800, 200, 400, 800);
    logd(r == null || r === "" ? "成功" : r);
}
main();
```

### touchDown 按下

* 按下坐标点
* 适配鸿蒙Next USB 中控 3.2.0+版本
* @param x X坐标
* @param y Y坐标
* @returns `{string}` null或者空字符串，代表成功，其他代表错误信息

```javascript showLineNumbers
function main() {
    let r = bleEvent.touchDown(300, 900);
    logd(r == null || r === "" ? "成功" : r);
}
main();
```

### touchMove 移动

* 移动坐标点
* 适配鸿蒙Next USB 中控 3.2.0+版本
* @param x X坐标
* @param y Y坐标
* @returns `{string}` null或者空字符串，代表成功，其他代表错误信息

```javascript showLineNumbers
function main() {
    let r = bleEvent.touchMove(320, 920);
    logd(r == null || r === "" ? "成功" : r);
}
main();
```

### touchUp 抬起

* 抬起坐标点
* 适配鸿蒙Next USB 中控 3.2.0+版本
* @param x X坐标
* @param y Y坐标
* @returns `{string}` null或者空字符串，代表成功，其他代表错误信息

```javascript showLineNumbers
function main() {
    let r = bleEvent.touchUp(320, 920);
    logd(r == null || r === "" ? "成功" : r);
}
main();
```

### multiTouch 多点触摸

* 多点触摸
* 触摸参数: action :一般情况下 按下为0，弹起为1，移动为2
* x: X坐标 / y: Y坐标 / pointer：第几个手指 / delay: 延迟毫秒
* 适配鸿蒙Next USB 中控 3.2.0+版本
* @param touch1 触摸点数组
* @param timeout 多点触摸执行的超时时间，单位是毫秒
* @returns `{string}` null或者空字符串，代表成功，其他代表错误信息

```javascript showLineNumbers
function main() {
    let touch1 = [
        {"action": 0, "x": 500, "y": 1200, "pointer": 1, "delay": 20},
        {"action": 2, "x": 450, "y": 1100, "pointer": 1, "delay": 80},
        {"action": 2, "x": 400, "y": 1000, "pointer": 1, "delay": 80},
        {"action": 1, "x": 400, "y": 1000, "pointer": 1, "delay": 20}
    ];
    let r = bleEvent.multiTouch(touch1, 10000);
    logd(r == null || r === "" ? "成功" : r);
}
main();
```

## 快速连通示例

```javascript showLineNumbers
function main() {
    let img = image.captureFullScreen();
    let w = img.getWidth();
    let h = img.getHeight();
    img.recycle();
    bleEvent.setScreenSize(w, h);
    bleEvent.setSendCmdType(1); // 1串口 2网络
    let r = bleEvent.openSerial(15000);
    if (r != null && r !== "") {
        logw("通信失败: " + r);
        return;
    }
    bleEvent.systemKey("home");
    sleep(800);
    bleEvent.clickPoint(Math.floor(w / 2), Math.floor(h / 2));
}
main();
```
