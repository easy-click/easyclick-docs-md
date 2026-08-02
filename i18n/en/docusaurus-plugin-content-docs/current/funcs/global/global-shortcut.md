---
title: Global Shortcut Events
description: EasyClick automation scripts — Android no root — global shortcut events
keywords:
 - EasyClick automation scripts Android no root global shortcut events
 - dump
 - setAgentSupportNode
 - param
 - shell
 - 'true'
 - setGestureActionMode
 - clickPoint
 - click
 - clickEx
 - longClickEx
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview {#说明}

Shortcut events encapsulated in the global module; you do not need to distinguish accessibility mode from agent mode.

## Agent Node Settings {#代理节点设置}

### setAgentSupportNode Set node fetch mode in agent mode {#setagentsupportnode-设置代理模式下获取节点方式}

* Set node fetch mode in agent mode
* Applies to agent mode only
* EC Android 11.2.0+
* Call before starting agent service; options 2 and 3 reduce detection signatures
* Option 1 may be detected by ruru as AccessibilityManager.isEnabled; options 2 and others are not
* Option 1 has stronger node capabilities; option 2 is weaker; option 3 disables node service
* @param support 1 = accessibility-like; 2 = shell dump; 3 = no node service
* @return `{boolean}` true = success, false = failure

```javascript showLineNumbers
function main() {
  setAgentSupportNode("2");
}

main();
```

### setShellDumpCompressed Agent mode dump compression {#setshelldumpcompressed-代理模式dump压缩模式}

* Call after agent service has started
* Set whether agent-mode shell dump uses compression
* Compressed mode returns fewer nodes but is faster
* Applies to agent mode only
* @param compressed 1 = compressed, 2 = uncompressed
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
  setAgentSupportNode("2")
  startEnv()
  let a = setShellDumpCompressed(1);
  logd("setShellDumpCompressed " + a)
}

main();
```

## Gesture Event Mode {#手势事件模式}

### setGestureActionMode Set gesture event mode {#setgestureactionmode-设置手势模式事件}

* Set gesture event mode; default is async; accessibility mode only
* @param mode 1 = async, 2 = sync
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
  setGestureActionMode(1);
  // setGestureActionMode(2);
}

main();
```

## Click Functions {#点击函数}

### clickPoint Click by coordinates {#clickpoint-坐标点击}

* Requires accessibility 7.0+ or gestures via agent service
* Click at coordinates
* @param x X coordinate
* @param y Y coordinate
* @return `{boolean}`

```javascript showLineNumbers
function main() {
  var result = clickPoint(100, 100);
  if (result) {
    toast("Click succeeded");
  } else {
    toast("Click failed");
  }
}

main();
```

### click Click selector {#click-选择器点击}

* Requires accessibility 7.0+ or gestures via agent service
* Click selector
* @param selectors Selector object
* @return `{boolean}`

```javascript showLineNumbers
function main() {
  var selector = text("Sample text");
  var result = click(selector);
  if (result) {
    toast("Click succeeded");
  } else {
    toast("Click failed");
  }
}

main();
```

### clickEx Pointerless click {#clickex-无指针点击}

* Requires accessibility 5.0+ or gestures via agent service
* Pointerless click on selector; node must be clickable (clickable = true)
* @param selectors Selector object
* @return `{boolean}`

```javascript showLineNumbers
function main() {
  var selector = text("Sample text");
  var result = clickEx(selector);
  if (result) {
    toast("Click succeeded");
  } else {
    toast("Click failed");
  }
}

main();
```

### longClickEx Pointerless long click {#longclickex-无指针长点击}

* Requires accessibility 5.0+ or gestures via agent service
* Pointerless long click on selector; node must be clickable
* @param selectors Selector object
* @return `{boolean}`

```javascript showLineNumbers
function main() {
  var selector = text("Sample text");
  var result = longClickEx(selector);
  if (result) {
    toast("Click succeeded");
  } else {
    toast("Click failed");
  }
}

main();
```

### clickRandom Random click on selector {#clickrandom-选择器随机点击}

* Requires accessibility 7.0+ or gestures via agent service
* Random click on any matching selector element
* @param selectors Selector object
* @return `{boolean}`

```javascript showLineNumbers
function main() {
  var selector = text("Sample text");
  var result = clickRandom(selector);
  if (result) {
    toast("Click succeeded");
  } else {
    toast("Click failed");
  }
}

main();
```

### clickRandomEx Pointerless random click {#clickrandomex-无指针随机点击}

* Requires accessibility 5.0+ or gestures via agent service
* Random click on any matching selector element
* @param selectors Selector object
* @return `{boolean}`

```javascript showLineNumbers
function main() {
  var selector = text("Sample text");
  var result = clickRandomEx(selector);
  if (result) {
    toast("Click succeeded");
  } else {
    toast("Click failed");
  }
}

main();
```

### clickRandomRect Random click in region {#clickrandomrect-区域随机点击}

* Requires accessibility 7.0+ or gestures via agent service
* Random click within region coordinates
* @param rect Region object
* @return `{boolean}`

```javascript showLineNumbers
function main() {
  var rect = new Rect();
  rect.left = 10;
  rect.right = 200;
  rect.top = 10;
  rect.bottom = 400;
  var result = clickRandomRect(rect);
  if (result) {
    toast("Click succeeded");
  } else {
    toast("Click failed");
  }
}

main();
```

### clickCenter Click center point {#clickcenter-点击中心点}

* Requires accessibility 7.0+ or gestures via agent service
* Click center coordinates of region
* @param rect Region object
* @return `{boolean}`

```javascript showLineNumbers
function main() {
  var rect = new Rect();
  rect.left = 10;
  rect.right = 200;
  rect.top = 10;
  rect.bottom = 400;
  var result = clickCenter(rect);
  if (result) {
    toast("Click succeeded");
  } else {
    toast("Click failed");
  }
}

main();
```

### clickText Click text {#clicktext-点击文本}

* Requires accessibility 7.0+ or gestures via agent service
* Click text
* @param text Text
* @return `{boolean}`

```javascript showLineNumbers
function main() {
  var result = clickText("Settings");
  if (result) {
    toast("Click succeeded");
  } else {
    toast("Click failed");
  }
}

main();
```

### longClick Long click selector {#longclick-选择器长点击}

* Requires accessibility 7.0+ or gestures via agent service
* Long click selector
* @param selectors Selector object
* @return `{boolean}`

```javascript showLineNumbers
function main() {
  var selector = text("Sample text");
  var result = longClick(selector);
  if (result) {
    toast("Click succeeded");
  } else {
    toast("Click failed");
  }
}

main();
```

### longClickPoint Long click by coordinates {#longclickpoint-坐标长点击}

* Requires accessibility 7.0+ or gestures via agent service
* Long-click at coordinates
* @param x X coordinate
* @param y Y coordinate
* @return `{boolean}`

```javascript showLineNumbers
function main() {
  var result = longClickPoint(100, 100);
  if (result) {
    toast("Click succeeded");
  } else {
    toast("Click failed");
  }
}

main();
```

### press Long-press by coordinates {#press-坐标长按}

* Long-press event
* Requires EC 5.15.0+
* @param x X coordinate
* @param y Y coordinate
* @param delay Long-press duration in milliseconds
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
  var result = press(100, 100, 5000);
  if (result) {
    toast("Long press succeeded");
  } else {
    toast("Long press failed");
  }
}

main();
```

## Multi-touch {#多点触摸}

### multiTouch Multi-touch {#multitouch-多点触摸}

* Requires accessibility 7.0+ or gestures via agent service
* Multi-touch<br/>
* Touch params: action — 0 = down, 1 = up, 2 = move
* x: X coordinate
* y: Y coordinate
* pointer: finger index (1, 2, 3, … for nth finger)
* delay: delay in ms before this action runs
* @param touch1
 First finger touch point array, e.g.:
 ```[{"action":0,"x":1,"y":1,"pointer":1,"delay":20},{"action":2,"x":1,"y":1,"pointer":1,"delay":20}]```
* @param touch2 Second finger touch point array
* @param touch3 Third finger touch point array
* @param timeout Multi-touch total timeout in milliseconds
* @return boolean

```javascript showLineNumbers
function main() {
 utils.openAppByName("Video");
 sleep(3000);
 // First style: array-based
 var touch1 = [
 {"action": 0, "x": 500, "y": 1200, "pointer": 1, "delay": 1},
 {"action": 2, "x": 500, "y": 1100, "pointer": 1, "delay": 20},
 {"action": 2, "x": 500, "y": 1000, "pointer": 1, "delay": 20},
 {"action": 1, "x": 1, "y": 1, "pointer": 1, "delay": 20}
 ]
 // Second style: chained calls
 var touch1 = MultiPoint
 .get()
 .action(0).x(500).y(1200).pointer(1).delay(1)
 .next()
 .action(2).x(500).y(1100).pointer(1).delay(1)
 .next()
 .action(2).x(500).y(1000).pointer(1).delay(1)
 .next()
 .action(2).x(500).y(900).pointer(1).delay(1)
 .next()
 .action(1).x(500).y(800).pointer(1).delay(1);
 var touch2 = MultiPoint
 .get()
 .action(0).x(300).y(1200).pointer(2).delay(1)
 .next()
 .action(2).x(300).y(1100).pointer(2).delay(1)
 .next()
 .action(2).x(300).y(1000).pointer(2).delay(1)
 .next()
 .action(2).x(300).y(900).pointer(2).delay(1)
 .next()
 .action(1).x(300).y(800).pointer(2).delay(1);
 var x = multiTouch(touch1, touch2, null, 30000);
 logd("xxs " + x);
}

main();
```

### touchDown Touch down {#touchdown-执行按下}

* Touch down
* Requires EC 7.4.0+, Android 8+
* @param x X coordinate
* @param y Y coordinate
* @return Boolean true = success, false = failure

```javascript showLineNumbers
function main() {
 //Full gesture sequence required; single action alone has no effect
 // Touch down
 touchDown(100, 100)
 sleep(50)
 // Touch move
 touchMove(100, 150)
 sleep(50)
 // Touch move
 touchMove(100, 200)
 sleep(50)
 // Touch up
 touchUp(100, 200)
 sleep(200)
}

main();
```

### touchMove Touch move {#touchmove-执行移动}

* Touch move
* Requires EC 7.4.0+, Android 8+
* @param x X coordinate
* @param y Y coordinate
* @return Boolean true = success, false = failure

```javascript showLineNumbers
function main() {
 //Full gesture sequence required; single action alone has no effect
 // Touch down
 touchDown(100, 100)
 sleep(50)
 // Touch move
 touchMove(100, 150)
 sleep(50)
 // Touch move
 touchMove(100, 200)
 sleep(50)
 // Touch up
 touchUp(100, 200)
 sleep(200)
}

main();
```

### touchUp Touch up {#touchup-执行弹起}

* Touch up event
* Requires EC 7.4.0+, Android 8+
* @param x X coordinate
* @param y Y coordinate
* @return Boolean true = success, false = failure

```javascript showLineNumbers
function main() {
 //Full gesture sequence required; single action alone has no effect
 // Touch down
 touchDown(100, 100)
 sleep(50)
 // Touch move
 touchMove(100, 150)
 sleep(50)
 // Touch move
 touchMove(100, 200)
 sleep(50)
 // Touch up
 touchUp(100, 200)
 sleep(200)
}

main();
```


### resetAllTouchState Reset touch state {#resetalltouchstate-重置touch状态}
* Reset touch state
* Prevents touch from failing to run again after external interruption during touch execution
* Not effective in accessibility mode
* Requires EC 11.16.0+
* @return `{boolean}` true = success, false = failure

```javascript showLineNumbers
function main() {
 resetAllTouchState();
 logd(touchDownEx(300,500,1));
 logd(touchMoveEx(300,500,1));
 sleep(2000)
 while(true){
 sleep(10)
 logd(touchDownEx(610,510,2));
 sleep(1000)
 logd(touchMoveEx(660,660,2));
 sleep(2000)
 logd(touchUpEx(710,710,2));
 sleep(2000)
 }
}

main();
```

### touchDownEx Touch up {#touchdownex-执行弹起}

* Touch down event with finger parameter
* Finger params not effective in accessibility mode
* Requires EC 11.16.0+
* @param x X coordinate
* @param y Y coordinate
* @param pointerId Finger index starting at 1
* @return `{boolean}` true = success, false = failure

```javascript showLineNumbers
function main() {
 resetAllTouchState();
 logd(touchDownEx(300,500,1));
 logd(touchMoveEx(300,500,1));
 sleep(2000)
 while(true){
 sleep(10)
 logd(touchDownEx(610,510,2));
 sleep(1000)
 logd(touchMoveEx(660,660,2));
 sleep(2000)
 logd(touchUpEx(710,710,2));
 sleep(2000)
 }
}

main();
```


### touchMoveEx Touch up {#touchmoveex-执行弹起}

* Touch move event with finger parameter
* Finger params not effective in accessibility mode
* Requires EC 11.16.0+
* @param x X coordinate
* @param y Y coordinate
* @param pointerId Finger index starting at 1
* @return `{boolean}` true = success, false = failure

```javascript showLineNumbers
function main() {
 resetAllTouchState();
 logd(touchDownEx(300,500,1));
 logd(touchMoveEx(300,500,1));
 sleep(2000)
 while(true){
 sleep(10)
 logd(touchDownEx(610,510,2));
 sleep(1000)
 logd(touchMoveEx(660,660,2));
 sleep(2000)
 logd(touchUpEx(710,710,2));
 sleep(2000)
 }
}

main();
```



### touchUpEx Touch up {#touchupex-执行弹起}

* Touch up event with finger parameter
* Finger params not effective in accessibility mode
* Requires EC 11.16.0+
* @param x X coordinate
* @param y Y coordinate
* @param pointerId Finger index starting at 1
* @return `{boolean}` true = success, false = failure

```javascript showLineNumbers
function main() {
 resetAllTouchState();
 logd(touchDownEx(300,500,1));
 logd(touchMoveEx(300,500,1));
 sleep(2000)
 while(true){
 sleep(10)
 logd(touchDownEx(610,510,2));
 sleep(1000)
 logd(touchMoveEx(660,660,2));
 sleep(2000)
 logd(touchUpEx(710,710,2));
 sleep(2000)
 }
}

main();
```

## Scroll Functions {#滚动函数}

### scrollForward Pointerless scroll forward {#scrollforward-无指针向前滚动}

* Requires accessibility 5.0+ or gestures via agent service
* Scroll forward
* @param selectors Selector object
* @return `{boolean}`

```javascript showLineNumbers
function main() {
 var selector = scrollable(true);
 var result = scrollForward(selector);
 if (result) {
 toast("Scroll succeeded");
 } else {
 toast("Scroll failed");
 }
}

main();
```

### scrollBackward Pointerless scroll backward {#scrollbackward-无指针向后滚动}

* Requires accessibility 5.0+ or gestures via agent service
* Scroll backward
* @param selectors Selector object
* @return `{boolean}`

```javascript showLineNumbers
function main() {
 var selector = scrollable(true);
 var result = scrollBackward(selector);
 if (result) {
 toast("Scroll succeeded");
 } else {
 toast("Scroll failed");
 }
}

main();
```

## Swipe Functions {#滑动函数}

### swipe Swipe {#swipe-滑动}

* Requires accessibility 7.0+ or gestures via agent service
* Swipe node via selector
* @param selectors Node selector
* @param endX End X coordinate
* @param endY End Y coordinate
* @param duration Duration in milliseconds
* @return Boolean; true on success, false on failure

```javascript showLineNumbers
function main() {
 var selectors = text("Sample text");
 var result = swipe(selectors, 100, 100, 200);
 if (result) {
 toast("Swipe succeeded");
 } else {
 toast("Swipe failed");
 }
}

main();
```

### swipeToPoint Swipe between coordinate points {#swipetopoint-坐标点滑动}

* Requires accessibility 7.0+ or gestures via agent service
* Swipe from one coordinate to another
* @param startX Start X coordinate
* @param startY Start Y coordinate
* @param endX End X coordinate
* @param endY End Y coordinate
* @param duration Duration in milliseconds
* @return Boolean; true on success, false on failure

```javascript showLineNumbers
function main() {
 var result = swipeToPoint(10, 10, 100, 100, 200);
 if (result) {
 toast("Swipe succeeded");
 } else {
 toast("Swipe failed");
 }
}

main();
```

### isScrollEnd Check if scrolled to bottom {#isscrollend-滚到底部判断}

* Requires accessibility 5.0+ or gestures via agent service
* Whether scrolled to bottom; returns false if element not found
* @param distance Scroll direction UP, DOWN, LEFT, RIGHT
* @param selectors Selector
* @return false = not at target; true = scroll complete

```javascript showLineNumbers
function main() {
 var selectors = clz("android.widget.ListView");
 var result = isScrollEnd("UP", selectors);
 if (result) {
 toast("Scroll complete");
 } else {
 toast("Scroll incomplete");
 }
}

main();
```

## Drag Functions {#拖动函数}

### drag Drag coordinates {#drag-拖动坐标}

* Requires accessibility 7.0+ or gestures via agent service
* Drag from one coordinate to another
* @param startX Start X coordinate
* @param startY Start Y coordinate
* @param endX End X coordinate
* @param endY End Y coordinate
* @param duration Duration in milliseconds
* @return Boolean; true on success, false on failure

```javascript showLineNumbers
function main() {
 var result = drag(10, 10, 100, 100, 200);
 if (result) {
 toast("Drag succeeded");
 } else {
 toast("Drag failed");
 }
}

main();
```

### dragTo Drag selector {#dragto-拖动选择器}

* Requires accessibility 7.0+ or gestures via agent service
* Drag element to target element via selector
* @param selectors Selector
* @param destObj Target element selector
* @param duration Duration in milliseconds
* @return Boolean; true on success, false on failure

```javascript showLineNumbers
function main() {
 var selectors = text("Settings");
 var destObj = text("Calendar");
 var result = dragTo(selectors, destObj, 200);
 if (result) {
 toast("Drag succeeded");
 } else {
 toast("Drag failed");
 }
}

main();
```

### dragToPoint Drag to target selector {#dragtopoint-拖动到目标选择器}

* Requires accessibility 7.0+ or gestures via agent service
* Drag element to target X Y coordinates via selector
* @param selectors Source element selector
* @param endX Target X coordinate
* @param endY Target Y coordinate
* @param duration Duration in milliseconds
* @return Boolean; true on success, false on failure

```javascript showLineNumbers
function main() {
 var selectors = text("Settings");
 var result = dragToPoint(selectors, 100, 100, 200);
 if (result) {
 toast("Drag succeeded");
 } else {
 toast("Drag failed");
 }
}

main();
```

## Input Data {#输入数据}

### imeInputViewShown Whether IME keyboard is shown {#imeinputviewshown-输入法键盘是否展示}

* Whether IME keyboard view is shown when entering text via IME
* Prerequisite: enable Show IME keyboard in EC System Settings
* Requires EC 9.18.0+
* @return `{boolean}` true if view shown, false if not

```javascript showLineNumbers
function main() {
 var result = imeInputViewShown();
 if (result) {
 toast("Yes");
 } else {
 toast("No");
 }
}

main();
```

### currentIsOurIme Whether using built-in IME {#currentisourime-是否是自带输入法}

* Whether current IME is ours
* @return `{boolean}`

```javascript showLineNumbers
function main() {
 var result = currentIsOurIme();
 if (result) {
 toast("Yes");
 } else {
 toast("No");
 }
}

main();
```

### inputText Input text {#inputtext-输入数据}

* Requires accessibility 5.0+
* Input data via selector
* @param selectors Selector
* @param content data string
* @return `{boolean}`

```javascript showLineNumbers
function main() {
 var selectors = clz("android.widget.EditText");
 var result = inputText(selectors, "My content");
 if (result) {
 toast("Yes");
 } else {
 toast("No");
 }
}

main();
```

### imeInputText IME input text {#imeinputtext-输入法输入数据}

* IME input; requires this app's IME as default
* For scenarios without nodes, e.g. games
* @param selectors Selector; may be empty if input field is focused
* @param content data string, Special string values: --ec_close_input-- close soft keyboard,--ec_open_input-- open soft keyboard
  ; --ec_show_input_view--
  Show keyboard view; --ec_hide_input_view-- hides keyboard view
* @return `{boolean}`

```javascript showLineNumbers
function main() {

 // open soft keyboard
 //imeInputText(null, "--ec_open_input--");

 var selectors = clz("android.widget.EditText");
 var result = imeInputText(selectors, "My content");

 if (result) {
 toast("Yes");
 } else {
 toast("No");
 }
}

main();
```

### imeInputKeyCode IME input key code {#imeinputkeycode-输入法输入code}

* IME input; requires this app's IME as default
* For scenarios without nodes, e.g. games
* @param selectors Selector; may be empty if input field is focused
* @param content See KeyEvent.KEYCODE_* values, e.g. 66 = enter, 67 = del, 84 = SEARCH
* Special codes: 1000 = search, 1001 = done, 1002 = go, 1003 = next, 1004 = previous, 1005 = send
* @return `{boolean}`

```javascript showLineNumbers
function main() {
 // Simulate search key
 // imeInputKeyCode(null,1000);
 var selectors = clz("android.widget.EditText");
 var result = imeInputKeyCode(selectors, 66);
 if (result) {
 toast("Yes");
 } else {
 toast("No");
 }
}

main();
```

### pasteText Paste text {#pastetext-粘贴数据}

* Requires accessibility 5.0+
* Paste text via selector
* @param selectors Selector
* @param content data string
* @return `{boolean}`

```javascript showLineNumbers
function main() {
 var selectors = clz("android.widget.EditText");
 var result = pasteText(selectors, "My content");
 if (result) {
 toast("Yes");
 } else {
 toast("No");
 }
}

main();
```

### clearTextField Clear text {#cleartextfield-清除数据}

* Requires accessibility 5.0+
* @param selectors Node selector
* @return `{boolean}`

```javascript showLineNumbers
function main() {
 var selectors = clz("android.widget.EditText");
 var result = clearTextField(selectors);
 if (result) {
 toast("Yes");
 } else {
 toast("No");
 }
}

main();
```

## Node Operations {#节点操作}

### lastNodeEventTime Last node event time {#lastnodeeventtime-最近节点事件发生时间}

* Requires EC 5.14.0+
* Last node event time; use to check whether node service is available
* @return `{long}` timestamp in milliseconds

```javascript showLineNumbers
function main() {
 startEnv();
 logd("Start listening");

 while (true) {
 let d = lastNodeEventTime();
 logd("time-" + d);
 sleep(1000)
 }
}

main();
```

### setFocus Set node focus {#setfocus-设置节点聚焦}

* Set node focus
* Requires EC 10.21.0+
* @param selectors Selector object
* @return `boolean`

```javascript showLineNumbers
function main() {
 var selectors = text("Settings");
 var result = setFocus(selectors);
 if (result) {
 toast("ok");
 } else {
 toast("false");
 }
}

main();
```

### has Check node exists {#has-节点存在判断}

* Check element exists via selector
* @param selectors Selector
* @return `{null|Boolean}`

```javascript showLineNumbers
function main() {
 var selectors = text("Settings");
 var result = has(selectors);
 if (result) {
 toast("Node exists");
 } else {
 toast("Node does not exist");
 }
}

main();
```

### waitExistActivity Wait for activity {#waitexistactivity-等界面出现}

* Wait for activity to appear
* @param activity Activity name
* @param timeout Timeout in milliseconds
* @return `{null|Boolean}`

```javascript showLineNumbers
function main() {
 var ac = "com.xxx.MainActivity";
 var result = waitExistActivity(ac, 10000);
 if (result) {
 toast("Activity exists");
 } else {
 toast("Activity does not exist");
 }
}

main();
```

### waitExistNode Wait for node {#waitexistnode-等节点出现}

* Wait for element via selector
* @param selectors Selector
* @param timeout Timeout in milliseconds
* @return `{null|Boolean}`

```javascript showLineNumbers
function main() {
 var selectors = text("Settings");
 var result = waitExistNode(selectors, 10000);
 if (result) {
 toast("Node exists");
 } else {
 toast("Node does not exist");
 }
}

main();
```

### getText Get text {#gettext-获取文本}

* Get text data from selector
* @param selectors Selector
* @return `{string[]|null|string collection}`

```javascript showLineNumbers
function main() {
 var selectors = clz("android.widget.TextView");
 var result = getText(selectors);
 toast("result:" + result);
}

main();
```

### getNodeInfo Get node array {#getnodeinfo-获取节点数组}

* Get node information
* @param selectors Selector
* @param timeout Wait time in milliseconds
* @return `{null|NodeInfo array|node info collection}`

```javascript showLineNumbers
function main() {
 var result = getNodeInfo(clz("android.widget.TextView"), 10 * 1000);
 toast("result:" + JSON.stringify(result));
}

main();
```

### getNodeAttrs Node attribute info {#getnodeattrs-节点属性信息}

* Get node attribute info
* @param selectors Selector
* @param attr Attribute value,e.g. text,className,see NodeInfo for more attributes
* @return `{null|string[]|Rect[]}`

```javascript showLineNumbers
function main() {
 var selectors = clz("android.widget.TextView");
 // Get all text attributes
 var result = getNodeAttrs(selectors, "text");
 toast("result:" + result);
 // Get all bounds attributes
 result = getNodeAttrs(selectors, "bounds");
 toast("result:" + result);
}

main();
```

### getOneNodeInfo Get single node {#getonenodeinfo-获取单个节点}

* Get the first node via selector
* @param selectors Selector
* @param timeout Wait time in milliseconds
* @return `{NodeInfo}` object or null

```javascript showLineNumbers
function main() {
 var result = getOneNodeInfo(clz("android.widget.TextView"), 10 * 1000);
 toast("result:" + JSON.stringify(result));
 if (result) {
 result.click();
 }
}

main();
```

### setFetchNodeMode Set node fetch mode {#setfetchnodemode-获取节点的模式}

* Set node fetch mode
* @param mode 1 = enhanced, 2 = fast; default enhanced
* @param fetchInvisibleNode Whether to fetch invisible nodes; default false
* @param fetchNotImportantNode Whether to fetch unimportant nodes; default true
* @param algorithm Node search algorithm; default nsf; nsf = static, bsf = breadth-first, dsf = depth-first
* @return `{boolean}`

```javascript showLineNumbers
function main() {
 // Run once at the start of the script
 var result = setFetchNodeMode(1, false, true, "nsf");
 toast("result:" + result);
}

main();
```

### setNodeDumpParam Set NSF node dump parameters {#setnodedumpparam-设置nsf抓节点参数}

* Set NSF node dump parameters
* NSF node fetch may cause StackOverflow on some devices; set dumpMethod to 2 or 3
* maxDepth controls node depth and can improve speed
* Requires EC 10.9.0+
* Depth and fetch mode affect speed and help avoid OOM with too many nodes
* @param data map format ```{"dumpMethod":1,"maxDepth":50}```
* dumpMethod: NSF fetch modes 1, 2, 3; default 1
* maxDepth: node fetch depth
* @return boolean

```javascript showLineNumbers
function main() {
 // Run once at the start of the script
 var result = setNodeDumpParam(1, false, true, "nsf");
 toast("result:" + result);
 logd(dumpXml())
}

main();
```

### setBlockNodeAttr Block node attributes {#setblocknodeattr-屏蔽节点属性}

* Set blocked node attributes
* After setting, these attributes are not fetched
* Requires EC 9.23.0+
* @param blockNode string,comma-separated, e.g. clz,index,bounds,get attribute values; see IDE node panel
* Set blockNode to "" to restore defaults
* @return `{boolean}`

```javascript showLineNumbers
function main() {
 var result = setBlockNodeAttr("row,bounds,index");
 toast("result:" + result);
 let data = dumpXml()
 logd(data)

 // Restore defaults
 setBlockNodeAttr("")
}

main();
```

### addNodeFlag Add node fetch flag {#addnodeflag-加上节点获取的某个标志位}

* Add node fetch flag
* @param flag See AccessibilityServiceInfo.FLAG_*; 0 = force refresh
* @return `{null|boolean}`

```javascript showLineNumbers
function main() {
 addNodeFlag(0);
}

main();
```

### removeNodeFlag Remove node fetch flag [force refresh nodes] {#removenodeflag-移除节点获取的某个标志位强制刷新节点}

* Remove a node fetch flag
* @param flag See AccessibilityServiceInfo.FLAG_*; 0 = force refresh

```javascript showLineNumbers
function main() {
 removeNodeFlag(0);
}

main();
```

### dumpXml Dump elements to XML {#dumpxml-元素变xml}

* Convert element nodes to XML
* @return string string|null

```javascript showLineNumbers
function main() {
 var result = dumpXml();
 if (result) {
 toast("ok");
 } else {
 toast("no");
 }
}

main();
```

### lockNode Lock current node {#locknode-锁定当前节点}

* Lock current node; after lock, node info stays stale on UI refresh until releaseNode

```javascript showLineNumbers
function main() {
 logd("Lock node...")
 // Lock node; UI refresh does not change it
 console.time("1")
 lockNode()
 for (let i = 0; i < 10; i++) {
 let n = text("Settings").getOneNodeInfo(1000)
 logd("lock " + n)
 }
 logd("Release node lock...")
 // Release node lock
 releaseNode()
 logd(console.timeEnd("1"))

 console.time("1")
 for (var i = 0; i < 10; i++) {
 let n = text("Settings").getOneNodeInfo(1000)
 logd("unlocked " + n)
 }
 logd(console.timeEnd("1"))
 // Locked fetch is noticeably faster
}

main();
```

### releaseNode Release node lock {#releasenode-释放节点的锁}

* Release node lock; node info updates on next UI refresh

```javascript showLineNumbers
function main() {
 logd("Lock node...")
 // Lock node; UI refresh does not change it
 console.time("1")
 lockNode()
 for (let i = 0; i < 10; i++) {
 let n = text("Settings").getOneNodeInfo(1000)
 logd("lock " + n)
 }
 logd("Release node lock...")
 // Release node lock
 releaseNode()
 logd(console.timeEnd("1"))

 console.time("1")
 for (var i = 0; i < 10; i++) {
 let n = text("Settings").getOneNodeInfo(1000)
 logd("unlocked " + n)
 }
 logd(console.timeEnd("1"))
 // Locked fetch is noticeably faster
}

main();
```

## System Key Functions {#系统按键相关}

### home Go to home screen {#home-返回主页}

* Requires accessibility 5.0+ or gestures via agent service
* Go to home screen
* @return `{null|Boolean}`

```javascript showLineNumbers
function main() {
 var result = home();
 if (result) {
 toast("Success");
 } else {
 toast("Failed");
 }
}

main();
```

### home2 Go to home screen 2 {#home2-返回主页2}

* Go to home screen; no permission or automation required
* Requires EC Android9.16.0+
* @return `{null|Boolean}`

```javascript showLineNumbers
function main() {
 var result = home2();
 if (result) {
 toast("Success");
 } else {
 toast("Failed");
 }
}

main();
```

### splitScreen Split screen {#splitscreen-分割屏幕}

* Requires accessibility 5.0+ or gestures via agent service
* Requires EC 5.15.0+
* Go to home screen
* @return `{null|Boolean}`

```javascript showLineNumbers
function main() {
 var result = splitScreen();
 if (result) {
 toast("Success");
 } else {
 toast("Failed");
 }
}

main();
```

### power Simulate power button {#power-模拟电源按键}

* Requires accessibility 5.0+ or gestures via agent service
* Simulate power button; accessibility shows power dialog, agent mode sends power key
* @return `{null|Boolean}`

```javascript showLineNumbers
function main() {
 var result = power();
 if (result) {
 toast("Success");
 } else {
 toast("Failed");
 }
}

main();
```

### back Back button {#back-返回键}

* Requires accessibility 5.0+ or gestures via agent service
* Back button
* @return `{null|Boolean}`

```javascript showLineNumbers
function main() {
 var result = back();
 if (result) {
 toast("Success");
 } else {
 toast("Failed");
 }
}

main();
```

### openNotification Open notification bar {#opennotification-打开通知栏}

* Requires accessibility 5.0+ or gestures via agent service
* Open notification bar
* @return `{null|Boolean}`

```javascript showLineNumbers
function main() {
 var result = openNotification();
 if (result) {
 toast("Success");
 } else {
 toast("Failed");
 }
}

main();
```

### openQuickSettings Open quick settings {#openquicksettings-打开快速设置}

* Requires accessibility 5.0+ or gestures via agent service
* Open quick settings
* @return `{null|Boolean}`

```javascript showLineNumbers
function main() {
 var result = openQuickSettings();
 if (result) {
 toast("Success");
 } else {
 toast("Failed");
 }
}

main();
```

### recentApps Recent apps button {#recentapps-最近app任务按键}

* Requires accessibility 5.0+ or gestures via agent service
* Recent apps button
* @return `{null|Boolean}`

```javascript showLineNumbers
function main() {
 var result = recentApps();
 if (result) {
 toast("Success");
 } else {
 toast("Failed");
 }
}

main();
```

### getCurrentRunningPkg Current running app package name {#getcurrentrunningpkg-当前运行的app包名}

* Get current running app package name
* Fetched via nodes; requires automation service
* Requires EC 10.6.0+
* @return string

```javascript showLineNumbers
function main() {
 var result = getCurrentRunningPkg();
 logd(result)
}

main();
```

### getRunningPkg Current running app package name {#getrunningpkg-当前运行的app包名}

* Deprecated; result is inaccurate
* Get current running app package name
* @return `{string|null}`

```javascript showLineNumbers
function main() {
 var result = getRunningPkg();
}

main();
```

### getRunningActivity Current running Activity class name {#getrunningactivity-当前运行的activity类名}

* Get current running Activity class name
* @return `{string|null}`

```javascript showLineNumbers
function main() {
 var result = getRunningActivity();
}

main();
```

### lockScreen Lock screen {#lockscreen-锁屏}

* See performGlobalAction function
* Requires EC Android 11.12.0+
* @return `{boolean}` Boolean | true on success

```javascript showLineNumbers
function main() {
 var result = lockScreen();
 if (result) {
 toast("Success");
 } else {
 toast("Failed");
 }
}

main();
```

### performGlobalAction Perform global action {#performglobalaction-执行全局的action动作}

* Perform global action
* Requires EC Android 11.12.0+
* @param actionType See AccessibilityService global_* constants, e.g.:
* public static final int GLOBAL_ACTION_BACK = 1;
* public static final int GLOBAL_ACTION_HOME = 2;
* public static final int GLOBAL_ACTION_LOCK_SCREEN = 8;
* public static final int GLOBAL_ACTION_NOTIFICATIONS = 4;
* public static final int GLOBAL_ACTION_POWER_DIALOG = 6;
* public static final int GLOBAL_ACTION_QUICK_SETTINGS = 5;
* public static final int GLOBAL_ACTION_RECENTS = 3;
* public static final int GLOBAL_ACTION_TAKE_SCREENSHOT = 9;
* public static final int GLOBAL_ACTION_TOGGLE_SPLIT_SCREEN = 7;
* @return `{boolean}` Boolean | true on success

```javascript showLineNumbers
function main() {
 var result = performGlobalAction(8);
 if (result) {
 toast("Success");
 } else {
 toast("Failed");
 }
}

main();
```

## Notification Bar {#通知栏}

### requestNotificationPermission Request notification listener permission {#requestnotificationpermission-请求监听状态栏的权限}

* Request notification listener permission
* @param timeout Permission request timeout in seconds
* @return true = permission request succeeded, false = failure

```javascript showLineNumbers
function main() {
 var result = requestNotificationPermission(10);
 toast("Has permission: " + result);
}

main();
```

### hasNotificationPermission Whether notification listener permission granted {#hasnotificationpermission-是否有状态栏监听权限}

* Check whether notification listener permission is granted
* @return true = permission request succeeded, false = failure

```javascript showLineNumbers
function main() {
 var result = hasNotificationPermission();
 toast("Has permission: " + result);
}

main();
```

### getLastNotification Get latest notification object {#getlastnotification-获取最近通知栏对象}

* Get latest notification object
* @param pkg Package name
* @param size Number of items to fetch
* @return `{NotificationInfo array}`

```javascript showLineNumbers
function main() {
 var result = getLastNotification("com.x", 100);
 toast("Result:" + result);
}

main();
```

### shotNotification Emit notification {#shotnotification-通知发射处理}

* Emit notification (equivalent to tapping notification bar)
* @param seqId
* @return `{boolean}`

```javascript showLineNumbers
function main() {
 var result = getLastNotification("com.x", 1);
 if (result && result.length > 0) {
 var s = shotNotification(result[0].seqId);
 toast("Result: " + s);
 }
}

main();
```

### ignoreNotification Ignore notification {#ignorenotification-忽略通知}

* Ignore notification; remove from cache queue
* @param seqId
* @return `{boolean}`

```javascript showLineNumbers
function main() {
 var result = getLastNotification("com.x", 1);
 if (result != null && result.length > 0) {
 var s = ignoreNotification(result[0].seqId);
 toast("Result: " + s);
 }
}

main();
```

### cancelNotification Cancel notification {#cancelnotification-取消通知}

* Cancel notification
* @param seqId
* @return `{boolean}`

```javascript showLineNumbers
function main() {
 var result = getLastNotification("com.x", 1);
 if (result != null && result.length > 0) {
 var s = cancelNotification(result[0].seqId);
 toast("Result: " + s);
 }
}

main();
```

### getLastToast Get toast data {#getlasttoast-获取toast数据}

* Get toast data
* @param pkg Package name
* @param size Number of items to fetch
* @return `{null|ToastInfo array}`

```javascript showLineNumbers
function main() {
 let result = getLastToast("com.xx", 100);
 toast("Result:" + JSON.stringify(result));
 if (result && result.length > 0) {
 for (let i = 0; i < result.length; i++) {
 logd(result[i])
 }
 }
}

main();
```

## Floating Window Logging {#悬浮窗日志}

### requestFloatViewPermission Request floating window permission {#requestfloatviewpermission-请求浮窗权限}

* Request floating window permission
* @param timeout Permission request timeout in seconds
* @return true = permission request succeeded, false = failure

```javascript showLineNumbers
function main() {
 var result = requestFloatViewPermission(10);
 toast("Has permission: " + result);
}

main();
```

### hasFloatViewPermission Check floating window permission {#hasfloatviewpermission-检查浮窗权限}

* Check whether floating window permission is granted
* @return true = permission request succeeded, false = failure

```javascript showLineNumbers
function main() {
 var result = hasFloatViewPermission();
 toast("Has permission: " + result);
}

main();
```

### showFloatView Show floating window {#showfloatview-展示浮窗}

* Show floating window (when debugging in IDE,preview project first,to avoid missing path content)
* @param params JS map object containing
* ```var map = {"path":"main.html","tag":"test"}```; parameters in this format
* Parameters:
* tag: string — unique floating window identifier
* path: string — layout file in IEC package
* title: string — floating window title
* titleBg: string — floating window background hex, e.g. #888888 or #88000000
* x: int — floating window start X
* y: int — floating window start Y
* w: int — floating window start width
* h: int — floating window start height
* @return true = permission request succeeded, false = failure

```javascript showLineNumbers
function main() {
 var m = {
 "path": "main.html",
 "tag": "tag",
 "title": "sss",
 "titleBg": "#888888",
 "x": 10,
 "y": 10,
 "w": 100,
 "h": 200
 };
 var xd = showFloatView(m);
 logd("showFloatView " + xd);
}

main();
```

### closeFloatView Close floating window {#closefloatview-关闭浮窗}

* Close floating window
* @param tag Tag for showFloatView; uniquely identifies the floating window
* @return true on success, false on failure

```javascript showLineNumbers
function main() {
 var m = {
 "path": "main.html",
 "tag": "tag",
 "title": "sss",
 "titleBg": "#888888",
 "x": 10,
 "y": 10,
 "w": 100,
 "h": 200
 };
 var xd = showFloatView(m);
 logd("showFloatView " + xd);
 sleep(3000);
 closeFloatView("tag");
}

main();
```

### closeAllFloatView Close all floating windows {#closeallfloatview-关闭所有悬浮窗}

* Close all floating windows except log floating window
* @return true on success, false on failure

```javascript showLineNumbers
function main() {
 var m = {
 "path": "main.html",
 "tag": "tag",
 "title": "sss",
 "titleBg": "#888888",
 "x": 10,
 "y": 10,
 "w": 100,
 "h": 200
 };
 var xd = showFloatView(m);
 logd("showFloatView " + xd);
 sleep(3000);
 closeAllFloatView();
}

main();
```

### showScriptCtrlFloatView Show script pause control floating window {#showscriptctrlfloatview-显示脚本暂停控制悬浮窗}

* Show script pause control floating window
* Requires EC 10.0.0+

```javascript showLineNumbers
function main() {
 showScriptCtrlFloatView();
}

main();
```

### closeScriptCtrlFloatView Close script pause control floating window {#closescriptctrlfloatview-关闭脚本暂停控制悬浮窗}

* Close script pause control floating window
* Requires EC 10.0.0+

```javascript showLineNumbers
function main() {
 closeScriptCtrlFloatView();
}

main();
```

### showCtrlWindow Show start/stop floating window {#showctrlwindow-展示启停浮窗}

* Show start/stop floating window
* @return true = permission request succeeded, false = failure

```javascript showLineNumbers
function main() {
 var result = showCtrlWindow();
 toast("Is shown: " + result);
}

main();
```

### closeCtrlWindow Close start/stop floating window {#closectrlwindow-关闭启停浮窗}

* Close start/stop floating window
* @return true = permission request succeeded, false = failure

```javascript showLineNumbers
function main() {
 var result = closeCtrlWindow();
 toast("Is shown: " + result);
}

main();
```

### setCtrlViewSizeEx Set start/stop control window {#setctrlviewsizeex-设置启停控制窗口}

* Set start/stop control window parameters
* Requires EC 6.5.0+
* @param map e.g.

```json
{
 "x": 100,
 "y": 100,
 "backgroundColor": "#ffffff"
}
// Parameters:
// x: start X position
// y: start Y position
```

* @return bool; true = success, false = failure

```javascript showLineNumbers
function main() {
 requestFloatViewPermission(1000);
 var m = {
 "x": 100,
 "y": 200,
 "backgroundColor": "#ffffff"
 }
 showCtrlWindow();
 setCtrlViewSizeEx(m);
 sleep(5000);
}

main();
```

### showLogWindow Show log floating window {#showlogwindow-展示日志浮窗}

* Show log floating window
* @return true = permission request succeeded, false = failure

```javascript showLineNumbers
function main() {
 var result = showLogWindow();
 toast("Is shown: " + result);
}

main();
```

### closeLogWindow Close log floating window {#closelogwindow-关闭日志浮窗}

* Close log floating window
* @return true = permission request succeeded, false = failure

```javascript showLineNumbers
function main() {
 closeLogWindow();
}

main();
```

### setLogViewSizeEx Set log window {#setlogviewsizeex-设置日志窗口}

* Set log window size (extended)
* backgroundImgFor EC 6.0.0+)
* @param map e.g.

:::tip

```json showLineNumbers
{
 "x": 100,
 "y": 100,
 "w": 100,
 "h": 200,
 "textSize": 12,
 "backgroundColor": "#ffffff",
 "backgroundImg": "res/a.png",
 "title": "My log",
 "backgroundAlpha": 255,
 "showTitle": true,
 "canTouch": false,
 "gravity": "center",
 "showCloseBtn": true
}
```

- x: start X position
- y: start Y position
- w: width
- h: height
- textSize: log font size
- backgroundColor: background color, e.g. #336699
- title: log window title
- showTitle: whether to show title
- gravity: title position — start = left, center = center, end = right
- backgroundImg: background image; GIF supported; place in res/, enter as res/a.png
- backgroundAlpha: background opacity 255–0
- 9.33.0+ new parameters:
    - canTouch: Whether touchable; includes move, click, etc.; false = not touchable during script (default false, click-through); true = accepts events during script
    - showCloseBtn: whether to show close button; false = hide, true = show

:::

* @return bool; true = success, false = failure

```javascript showLineNumbers
function main() {
 requestFloatViewPermission(1000);
 var m = {
 "x": 100,
 "y": 200,
 "w": 600,
 "h": 600,
 "gravity": "center",
 "textSize": 12,
 "backgroundColor": "#ffffff",
 "title": "My log",
 "showTitle": false
 }

 showLogWindow();
 sleep(2000);
 setLogViewSizeEx(m);
 sleep(5000);
}

main();
```

### setLogFixedViewEx Set log fixed column properties {#setlogfixedviewex-设置日志固定栏目属性}

* Set fixed top log window properties
* Requires EC 6.17.0+
* @param param map parameters

:::tip Parameters:

- show: whether to show
- h: height; must be > 0
- textSize: log font size
- backgroundColor: background color, e.g. #336699
  :::

```javascript showLineNumbers
function main() {
 requestFloatViewPermission(1000);
 var m = {
 "show": true,
 "h": 150,
 "textSize": 12,
 "textColor": "#000000",
 "backgroundColor": "#ffffff"
 }
 showLogWindow();
 sleep(1000)
 setLogFixedViewEx(m);
 setFixedViewText("TEST\nall good")

 sleep(5000);


}

main();
```

### setFixedViewText Show fixed message {#setfixedviewtext-展示固定消息}

* Show message in fixed floating window view
* @param msg Message

```javascript showLineNumbers
function main() {
 requestFloatViewPermission(1000);
 var m = {
 "show": true,
 "h": -1,
 "textSize": 12,
 "backgroundColor": "#ffffff"
 }
 showLogWindow();
 sleep(1000)
 setLogFixedViewEx(m);
 sleep(1000);
 setFixedViewText("TEST")
}

main();
```

### setLogText Show message {#setlogtext-展示消息}

* Show message in floating window log
* @param msg Message
* @param color Color valuee.g. #ffffff
* @param size font size

```javascript showLineNumbers
function main() {
 var result = setLogText("Starting...", "#ffffff", 18);
}

main();
```

### expandLogView Expand log floating window {#expandlogview-展开日志悬浮窗}

* Expand log floating window
* Requires EC 9.32.0+
* @return true on success, false on failure

```javascript showLineNumbers
function main() {
 showLogWindow();
 sleep(1000)
 var result = expandLogView();
}

main();
```

### collapseLogView Collapse log floating window {#collapselogview-折叠日志悬浮窗}

* Collapse log floating window; title only
* Requires EC 9.32.0+
* @return true on success, false on failure

```javascript showLineNumbers
function main() {
 showLogWindow();
 sleep(1000)
 var result = collapseLogView();
}

main();
```

## Scheduled Tasks {#定时任务}

### startJob Start scheduled task {#startjob-开启定时}

* Start a scheduled script task
* @param tag Unique task tag (required). Use readConfigString("jobTaskTag") in script to identify invoking task
* @param execTime scheduled time format: 2020-04-17 19:20:00,or seconds as number,e.g. 3,means 3 seconds later
* @param cancelBeforeRunning
* @return int jobid
  :::tip
* Runs entire script only; cannot target a single function
  :::

```javascript showLineNumbers
function main() {
 var time = "2020-04-17 09:00:00";
 // Schedule task by date
 var id = startJob("task1", time, true);
 logd("job id " + id);
 // Schedule task in 60 seconds
 var id2 = startJob("task2", "60", true);
 logd("job id " + id2);
}

main();
```

### cancelAllJob Cancel all scheduled tasks {#cancelalljob-取消所有定时}

* Cancel all scheduled tasks
* @return bool; true if a task was cancelled

```javascript showLineNumbers
function main() {
 var result = cancelAllJob();
 logd(result);
}

main();
```

### cancelJob Cancel scheduled task by tag {#canceljob-取消指定tag定时}

* Cancel scheduled task by tag
* @param tag tag name from startJob
* @return bool; true if a task was cancelled

```javascript showLineNumbers
function main() {

 var result = cancelJob("task1");
 logd(result);
}

main();
```

### getAllJobTag Get all scheduled task tags {#getalljobtag-获取所有定时标签}

* Get all scheduled task tags
* @return `{string[]}` or null

```javascript showLineNumbers
function main() {
 var result = getAllJobTag();
 logd(result);
}

main();
```

## Runtime Permission Requests {#动态权限申请}

### tryGetProjectionPermission Get screenshot auto-allow permission {#trygetprojectionpermission-获取截图自允许权限}

* Get screenshot auto-allow permission
* Ignored in agent mode; for floating-window screenshot permission mode
* Requires EC 10.25.0+
* Run with shell or root permission; can disable shell/root after completion
* Try to get auto-allow screenshot permission without dialog
* @return bool true = success, false = failure

```javascript showLineNumbers
function main() {
 let result = tryGetProjectionPermission()
 logd(result);
 // Then request screenshot permission
}

main();
```

### tryGetAccStartupPermission Get accessibility auto-start permission {#trygetaccstartuppermission-获取无障碍自允许权限}

* Get accessibility auto-start permission
* With permission, accessibility can auto-start
* Requires EC 10.25.0+
* Request with shell or root permission; can disable shell/root after request
* Try to get accessibility auto-allow permission
* @return bool true = success, false = failure

```javascript showLineNumbers
function main() {
 let result = tryGetAccStartupPermission()
 logd(result);
 // Then auto-start accessibility
 startEnv();
}

main();
```

### requestRuntimePermission Request runtime permission {#requestruntimepermission-申请动态权限}

* Request runtime permission
* Requires EC 7.9.0+
* @param permissionArray Runtime permission array; may contain multiple entries
* @timeout Request timeout in milliseconds
* @return `{bool}` true = has permission, false = no permission or request failed

```javascript showLineNumbers
function main() {
 let per = [
 "android.permission.READ_CALENDAR",
 "android.permission.READ_SMS"
 ]
 let result = requestRuntimePermission(per, 10000)
 logd(result);
}

main();
```

## other Functions {#其他函数}

### random Random function {#random-随机函数}

* Random value in range
* @param min Minimum
* @param max Maximum
* @return int between min and max inclusive

```javascript showLineNumbers
function main() {
 var result = random(100, 1000);
 sleep(result);
}

main();
```

### isReleaseIec Whether script is release version {#isreleaseiec-脚本是否是release版本}

* Check whether script is release version
* Requires EC 11.12.0+
* @return `{boolean}` true = release, false = debug

```javascript showLineNumbers
function main() {
 var result = isReleaseIec();
 logd(result)
}

main();
```
