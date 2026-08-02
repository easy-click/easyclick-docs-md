---
title: Proxy Events
description: EasyClick automation scripts — Android no root proxy events
keywords:
 - EasyClick automation scripts Android no root proxy events
 - return
 - setAgentCallParam
 - setCurrentIme
 - restoreIme
 - fastScreenshot
 - agentEvent
 - param
 - inputEvent
 - touchDown
 - root
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview {#说明}

- All proxy event module functions require PC activation; no root needed
- The proxy event module uses the `agentEvent` prefix, e.g. `agentEvent.clickPoint()`
- Functions listed here are specific to proxy mode; use global functions for other calls

## Settings {#设置}

### setAgentCallParam Set Proxy Global Communication Timeout {#setagentcallparam-代理全局通信设置超时}

* Set proxy mode parameters
* Requires EC 7.0.0+
* @param data Parameter map
* Example: ```{"remoteCallTimeout":10000}```
* remoteCallTimeout: Remote call timeout in milliseconds; default 10 seconds
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 var result = agentEvent.setAgentCallParam({"remoteCallTimeout": 10000});
 if (result) {
 toast("yes");
 } else {
 toast("no");
 }
}

main();
```

## Input {#输入数据}

### setCurrentIme Set Current Input Method {#setcurrentime-设置当前的输入法}

* Set the current IME for text input
* @return `{boolean}`

```javascript showLineNumbers
function main() {
 var result = agentEvent.setCurrentIme();
 if (result) {
 toast("yes");
 } else {
 toast("no");
 }
}

main();
```

### restoreIme Restore Previous Input Method {#restoreime-恢复到之前的输入法}

* Restore the previous IME
* @return `{boolean}`

```javascript showLineNumbers
function main() {
 var result = agentEvent.restoreIme();
 if (result) {
 toast("yes");
 } else {
 toast("no");
 }
}

main();
```

## Screenshot {#截图}

### fastScreenshot Fast Full-Screen Screenshot {#fastscreenshot-快速截屏幕的截图}

* Capture a fast full-screen screenshot
* @param filename File path
* @return string Path to the screenshot

```javascript showLineNumbers
function main() {
 var result = agentEvent.fastScreenshot("/sdcard/a.jpg");
 toast("result:" + result);
}

main();
```

## Gestures and Input Events {#手势及输入事件}

### inputEvent Execute Input Event {#inputevent-执行输入事件}

* Execute an input event
* @param action Action; see MotionEvent.ACTION_*
* @param x X coordinate
* @param y Y coordinate
* @param metaState Modifier keys such as shift, alt, ctrl; 0 or 1
* @return boolean true on success, false on failure

```javascript showLineNumbers
function main() {
 var result = agentEvent.inputEvent(1, 10, 10, 0);
 if (result) {
 toast("success");
 } else {
 toast("failed");
 }
}

main();
```

### touchDown Touch Down {#touchdown-执行按下}

* Execute touch-down input event
* @param x X coordinate
* @param y Y coordinate
* @return boolean true on success, false on failure

```javascript showLineNumbers
function main() {
 // Must be used together; alone it has no effect
 // Touch down
 agentEvent.touchDown(100, 100)
 sleep(50)
 // Move
 agentEvent.touchMove(100, 150)
 sleep(50)
 // Move
 agentEvent.touchMove(100, 200)
 sleep(50)
 // Touch up
 agentEvent.touchUp(100, 200)
 sleep(200)
}

main();
```

### touchMove Touch Move {#touchmove-执行移动}

* Execute touch-move input event
* @param x X coordinate
* @param y Y coordinate
* @return boolean true on success, false on failure

```javascript showLineNumbers
function main() {
 // Must be used together; alone it has no effect
 // Touch down
 agentEvent.touchDown(100, 100)
 sleep(50)
 // Move
 agentEvent.touchMove(100, 150)
 sleep(50)
 // Move
 agentEvent.touchMove(100, 200)
 sleep(50)
 // Touch up
 agentEvent.touchUp(100, 200)
 sleep(200)
}

main();
```

### touchUp Touch Up {#touchup-执行弹起输入}

* Execute touch-up input event
* @param x X coordinate
* @param y Y coordinate
* @return boolean true on success, false on failure

```javascript showLineNumbers
function main() {
 // Must be used together; alone it has no effect
 // Touch down
 agentEvent.touchDown(100, 100)
 sleep(50)
 // Move
 agentEvent.touchMove(100, 150)
 sleep(50)
 // Move
 agentEvent.touchMove(100, 200)
 sleep(50)
 // Touch up
 agentEvent.touchUp(100, 200)
 sleep(200)
}

main();
```

### pressKey Simulate Key Press {#presskey-模拟按键}

* Simulate a key press, e.g. home, back
* @param key Values: home, back, left, right, up, down, center, menu, search, enter, delete (or del), recent (recent apps), volume_up, volume_down, volume_mute, camera, power
* @return boolean true on success, false on failure

```javascript showLineNumbers
function main() {
 var result = agentEvent.pressKey("home");
 if (result) {
 toast("success");
 } else {
 toast("failed");
 }
}

main();
```

### pressKeyCode Simulate Keyboard Input {#presskeycode-模拟键盘输入}

* Simulate keyboard input
* @param keyCode Key code; see KeyEvent.KEYCODE_*
* @return boolean true on success, false on failure

```javascript showLineNumbers
function main() {
 var result = agentEvent.pressKeyCode(65);
 if (result) {
 toast("success");
 } else {
 toast("failed");
 }
}

main();
```

### pressKeyCodeWithMetaState Simulate Keyboard Input with Meta State {#presskeycodewithmetastate-模拟键盘输入}

* Simulate keyboard input with modifier keys
* @param keyCode Key code; see KeyEvent.KEYCODE_*
* @param metaState Modifier keys such as shift, alt, ctrl; 0 or 1
* @return boolean true on success, false on failure

```javascript showLineNumbers
function main() {
 var result = agentEvent.pressKeyCodeWithMetaState(65, 1);
 if (result) {
 toast("success");
 } else {
 toast("failed");
 }
}

main();
```

## System Keys {#系统按键相关}

### menu Open Menu {#menu-打开菜单}

* Open the menu
* @return `{null|boolean}`

```javascript showLineNumbers
function main() {
 var result = agentEvent.menu();
 if (result) {
 toast("success");
 } else {
 toast("failed");
 }
}

main();
```

### enter Enter Key {#enter-enter键}

* Enter key
* @return `{null|boolean}`

```javascript showLineNumbers
function main() {
 var result = agentEvent.enter();
 if (result) {
 toast("success");
 } else {
 toast("failed");
 }
}

main();
```

### delete Delete Key {#delete-删除键}

* Delete key
* @return `{null|boolean}`

```javascript showLineNumbers
function main() {
 var result = agentEvent.delete();
 if (result) {
 toast("success");
 } else {
 toast("failed");
 }
}

main();
```

### search Search {#search-搜索}

* Search key
* @return `{null|boolean}`

```javascript showLineNumbers
function main() {
 var result = agentEvent.search();
 if (result) {
 toast("success");
 } else {
 toast("failed");
 }
}

main();
```

## Screen Control {#屏幕控制}

### closeScreen Turn Screen Off {#closescreen-关闭屏幕}

* Turn off the screen (display off but automation still works; unlike sleepScreen)
* @return boolean true on success, false on failure

```javascript showLineNumbers
function main() {
 // If this function does not work, download another plugin from the forum
 // https://bbs.ieasyclick.com/thread-617-1-1.html
 var x = agentEvent.closeScreen();
}

main();
```

### lightScreen Turn Screen On {#lightscreen-点亮屏幕}

* Turn on the screen (opposite of closeScreen)
* @return boolean true on success, false on failure

```javascript showLineNumbers
function main() {
 var x = agentEvent.lightScreen();
 // If this function has no effect, force wake the screen with:
 // agentEvent.execShellCommandEx("input keyevent 26 && input keyevent 82 && input keyevent 82")
}

main();
```
