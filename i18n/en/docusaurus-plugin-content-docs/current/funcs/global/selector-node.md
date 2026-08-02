---
title: Selectors & Nodes
description: EasyClick automation scripts — Android no root — selectors & nodes
keywords:
 - EasyClick automation scripts Android no root selectors nodes
 - xpath
 - text
 - java
 - https
 - www
 - com
 - id
 - clz
 - runoob
 - html
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview {#说明}

This chapter covers selector usage and the node info class.

## Selector Objects {#选择器对象}

- Selector objects support cascading selection. When an element cannot be selected directly, select a parent first, then a child.
- Selectors partially support regex matching. See Java regex syntax in this [tutorial](https://www.runoob.com/java/java-regular-expressions.html).

## XPath Selection {#xpath-选择}

* XPath selector; see : https://www.jianshu.com/p/c205334122b3 and https://www.runoob.com/xpath/xpath-syntax.html
* Use the IDE node panel to inspect xpath attributes and test xpath
* Requires EC 10.12.0+
* @param value e.g. //node[@text='Sample'] selects nodes with that text

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = xpath("//node[@clz='android.widget.TextView']");
    let n = selector.getNodeInfo(1000);
    logd(JSON.stringify(n))
}

main();
```

## text Attribute Selection {#text属性选择}

### Full Text Match {#全文本匹配}

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = text("Settings");
    click(selector);
}

main();
```

### Regex Match {#正则匹配}

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = textMatch(".*Settings.*");
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

## id Attribute Selection {#id-属性选择}

### Full Match {#全量匹配}

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = id("com.xx:id/a1");
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

### Regex Match {#正则匹配}

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = idMatch(".*id8.*");
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

## clz Attribute Selection {#clz-属性选择}

### Full Match {#全量匹配}

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = clz("android.widget.TextView");
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

### Regex Match {#正则匹配}

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = clzMatch(".*TextView.*");
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

## pkg Attribute Selection {#pkg-属性选择}

### Full Match {#全量匹配}

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = pkg("com.xx");
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

### Regex Match {#正则匹配}

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = pkgMatch(".*tencent.*");
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

## desc Text Attribute Selection {#desc-文本属性选择}

### Full Match {#全量匹配}

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = desc("My description");
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

### Regex Match {#正则匹配}

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = descMatch(".*Description.*");
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main()
```

## Depth and Drawing Order Matching {#深度和绘制顺序匹配}

### drawingOrder

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = drawingOrder(1);
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

### depth

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = depth(1);
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

## other Matching Rules {#其他匹配规则}

### visible Visible Attribute Match {#visible-可视化属性匹配}

* Match on visible attribute
* @param value string
* @return `{Selector}` node selector

```javascript showLineNumbers
function main() {
    var node = visible(true).getOneNodeInfo(1000);
    logd("node " + node);
}

main();
```

### bounds Bounds Match {#bounds-范围匹配}

* Match on bounds region
*
* @param left Left bound
* @param top Top bound
* @param right Right bound
* @param bottom Bottom bound
* @return `{Selector}` node selector

```javascript showLineNumbers
function main() {
    // Get selector object controls within bounds 0–800
    var selector = bounds(0, 0, 800, 800);
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

### checkable

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = checkable(true);
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

### checked

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = checked(true);
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

### clickable

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = clickable(true);
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

### longClickable

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = longClickable(true);
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

### scrollable

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = scrollable(true);
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

### focusable

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = focusable(true);
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

### enabled

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = enabled(true);
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

### focused

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = focused(true);
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

### selected

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = selected(true);
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

### index

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = index(1);
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

### childCount

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = childCount(1);
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

### row

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = row(1);
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

### column

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = column(1);
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

### rowSpan

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = rowSpan(1);
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

### columnSpan

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = columnSpan(1);
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

### rowCount

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = rowCount(1);
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

### columnCount

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = columnCount(1);
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

## Cascading Matching {#级联匹配}

```javascript showLineNumbers
function main() {
    // Get selector object
    // Select children under ScrollView parentclz=android.widget.CheckBox all nodes
    var selector = clz("android.widget.ScrollView")
        .child()
        .clz("android.widget.CheckBox");
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

## Multi-attribute Matching {#多属性匹配}

```javascript showLineNumbers
function main() {
    // Get selector object,
    // Select CheckBox elements containing "Selector" with checked=true
    var selector = textMatch(".*Selector.*")
        .checked(true)
        .clz("android.widget.CheckBox");
    var result = click(selector);
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

## Node Info Class {#节点信息类}

### Overview {#说明}

NodeInfo objects are returned as arrays from getNodeInfo; node fields include:

- id: string — resource ID
- clz: string — view class name, e.g. android.widget.TextView
- pkg: string — package name, e.g. com.xx
- desc: string — content description
- text: string — text content
- checkable: Boolean — whether checkable
- checked: Boolean — whether checked
- clickable: Boolean — whether clickable
- enabled: Boolean — whether enabled
- focusable: Boolean — whether focusable
- focused: Boolean — whether focused
- longClickable: Boolean — whether long-clickable
- scrollable: Boolean — whether scrollable
- selected: Boolean — whether selected
- childCount: int — number of child nodes
- index: int — node index
- depth: int — node depth
- drawingOrder: int — drawing order
- bounds: Rect — bounds object
 - top: int — top position
 - bottom: int — bottom position
 - left: int — left position
 - right: int — right position
- visibleBounds: Rect — visible bounds object
 - top: int — top position
 - bottom: int — bottom position
 - left: int — left position
 - right: int — right position

### Get one node with selector getOneNodeInfo {#选择器获取一个节点-getonenodeinfo}

* Get the first node via selector
* @param timeout Wait time in ms; 0 = no wait
* @return `{NodeInfo}` object or null

```javascript showLineNumbers
function main() {
    // Get selector object
    // Select all CheckBox nodes: clz=android.widget.CheckBox
    var node = clz("android.widget.CheckBox").getOneNodeInfo(10000);

    if (node) {
        var x = node.click();
        logd(x);
    } else {
        toast("No node");
    }
}

main();
```

### Get multiple nodes with selector getNodeInfo {#选择器获取多个节点-getnodeinfo}

* Get node information
* @param timeout Wait time in ms; 0 = no wait
* @return `{array}` Collection of NodeInfo nodes

```javascript showLineNumbers
function main() {
    // Get selector object
    // Select all CheckBox nodes: clz=android.widget.CheckBox
    var node = clz("android.widget.CheckBox").getNodeInfo(10000);
    // This is an array
    for (let i = 0; i < node.length; i++) {
        logd(JSON.stringify(node[i]))
    }
}

main();
```

### Cascade get one child node getOneNodeInfo {#级联获取一个子节点操作-getonenodeinfo}

* Get the first node via selector
* @param timeout Wait time in ms; 0 = no wait
* @return `{NodeInfo}` object or null

```javascript showLineNumbers
function main() {
    // Get selector object
    // Select all ViewGroup nodes: clz=android.widget.ViewGroup
    var node = clz("android.widget.ViewGroup").getOneNodeInfo(10000);
    if (node) {
        // Get child nodes
        node = node.getOneNodeInfo(text("Ad"), 10000);
        if (!node) {
            toast("No child nodes");
            return;
        }
        var x = node.click();
        logd(x);
    } else {
        toast("No node");
    }
}

main();
```

### Cascade get multiple child nodes getNodeInfo {#级联获取多个子节点操作-getnodeinfo}

* Get node information
* @param timeout Wait time in ms; 0 = no wait
* @return `{array}` Collection of NodeInfo nodes

```javascript showLineNumbers
function main() {
    // Get selector object
    // Select all ViewGroup nodes: clz=android.widget.ViewGroup
    var node = clz("android.widget.ViewGroup").getNodeInfo(10000);
    if (node) {
        // Get child nodes
        node = node.getNodeInfo(text("Ad").clz("android.widget.TextView"), 10000);
        if (!node) {
            toast("No child nodes");
            return;
        }
        var x = node.click();
        logd(x);
    } else {
        toast("No node");
    }
}

main();
```

### Get parent node {#获取父节点-parent}

* Parent node of this node
* @return `{NodeInfo}` object or null

```javascript showLineNumbers
function main() {
    // Get selector object
    // Select all CheckBox nodes: clz=android.widget.CheckBox
    var node = clz("android.widget.CheckBox").getOneNodeInfo(10000);
    if (node) {
        var x = node.parent();
        logd(x);
    } else {
        toast("No node");
    }
}

main();
```

### Get child node {#获取子节点-child}

* Get a single child node
* @param index child node index
* @return `{NodeInfo}` object or null

```javascript showLineNumbers
function main() {
    // Select all ViewGroup nodes: clz=android.widget.ViewGroup
    var node = clz("android.widget.ViewGroup").getOneNodeInfo(10000);
    if (node) {
        var x = node.child(0);
        logd(x);
    } else {
        toast("No node");
    }
}

main();
```

### Get all child nodes {#获取所有子节点-allchildren}

* Get all child nodes
* @return `{NodeInfo}` node collection

```javascript showLineNumbers
function main() {
    // Select all ViewGroup nodes: clz=android.widget.ViewGroup
    var node = clz("android.widget.ViewGroup").getOneNodeInfo(10000);
    if (node) {
        var x = node.allChildren();
        // This is an array
        for (let i = 0; i < x.length; i++) {
            logd(x[i])
        }
    } else {
        toast("No node");
    }
}

main();
```

### All sibling nodes {#所有兄弟节点-siblings}

* All sibling nodes of the current node
* @return `{NodeInfo}` node collection

```javascript showLineNumbers
function main() {
    // Select all ViewGroup nodes: clz=android.widget.ViewGroup
    var node = clz("android.widget.ViewGroup").getOneNodeInfo(10000);
    if (node) {
        var x = node.siblings();
        // This is an array
        for (let i = 0; i < x.length; i++) {
            logd(x[i])
        }
    } else {
        toast("No node");
    }
}

main();
```

### Previous sibling nodes previousSiblings {#前面的兄弟节点-previoussiblings}

* Sibling nodes before the current node
* @return `{NodeInfo}` node collection

```javascript showLineNumbers
function main() {
    // Select all ViewGroup nodes: clz=android.widget.ViewGroup
    var node = clz("android.widget.ViewGroup").getOneNodeInfo(10000);
    if (node != null) {
        let x = node.previousSiblings();
        // This is an array
        for (let i = 0; i < x.length; i++) {
            logd(x[i])
        }
    } else {
        toast("No node");
    }
}

main();
```

### Next sibling nodes nextSiblings {#后面的兄弟节点-nextsiblings}

* Sibling nodes after the current node
* @return `{NodeInfo}` node collection

```javascript showLineNumbers
function main() {
    // Select all ViewGroup nodes: clz=android.widget.ViewGroup
    var node = clz("android.widget.ViewGroup").getOneNodeInfo(10000);
    if (node) {
        let x = node.nextSiblings();
        // This is an array
        for (let i = 0; i < x.length; i++) {
            logd(x[i])
        }
    } else {
        toast("No node");
    }
}

main();
```

### Random click in node region {#节点区域随机点击-click}

* Requires accessibility 7.0+ or gestures via agent service
* Click node
* @return bool; true on success, false on failure

```javascript showLineNumbers
function main() {
    // Get selector object
    // Select all CheckBox nodes: clz=android.widget.CheckBox
    var node = clz("android.widget.CheckBox").getOneNodeInfo(10000);
    if (node) {
        node.click()
    } else {
        toast("No node");
    }
}

main();
```

### Pointerless click clickEx {#无指针点击-clickex}

* Requires accessibility 5.0+ or gestures via agent service
* Pointerless click on selector; node must be clickable
* @param selectors Selector object
* @return `{boolean}`

```javascript showLineNumbers
function main() {
    var node = text("Sample text").getOneNodeInfo(10000);
    var result = node.clickEx();
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

### Pointerless long click longClickEx {#无指针长点击-longclickex}

* Requires accessibility 5.0+ or gestures via agent service
* Pointerless long click on selector; node must be clickable
* @param selectors Selector object
* @return `{boolean}`

```javascript showLineNumbers
function main() {
    var node = text("Sample text").getOneNodeInfo(10000);
    var result = node.longClickEx();
    if (result) {
        toast("Click succeeded");
    } else {
        toast("Click failed");
    }
}

main();
```

### Click node center {#节点点击中心点-clickcenter}

* Requires accessibility 7.0+ or gestures via agent service
* Click node center point
* @return bool; true on success, false on failure

```javascript showLineNumbers
function main() {
    // Get selector object
    // Select all CheckBox nodes: clz=android.widget.CheckBox
    var node = clz("android.widget.CheckBox").getOneNodeInfo(10000);
    if (node) {
        node.clickCenter();
    } else {
        toast("No node");
    }
}

main();
```

### Long click node {#节点长点击-longclick}

* Requires accessibility 7.0+ or gestures via agent service
* Long click node
* @return bool; true on success, false on failure

```javascript showLineNumbers
function main() {
    // Get selector object
    // Select all CheckBox nodes: clz=android.widget.CheckBox
    var node = clz("android.widget.CheckBox").getOneNodeInfo(10000);
    if (node) {
        node.longClick()
    } else {
        toast("No node");
    }
}

main();
```

### Node input inputText {#节点输入-inputtext}

* Requires accessibility 5.0+ or gestures via agent service
* Input data on a node
* @param content content to enter
* @return bool; true on success, false on failure

```javascript showLineNumbers
function main() {
    // Get selector object
    // Select all EditText nodes: clz=android.widget.EditText
    var node = clz("android.widget.EditText").getOneNodeInfo(10000);
    if (node) {
        node.inputText("content")
    } else {
        toast("No node");
    }
}

main();
```

### Node IME input imeInputText {#节点输入法输入-imeinputtext}

* Requires accessibility 5.0+ or gestures via agent service
* IME input on a node; requires this app's IME as default
* @param content content to enter
* @return bool; true on success, false on failure

```javascript showLineNumbers
function main() {
    // Get selector object
    // Select all EditText nodes: clz=android.widget.EditText
    var node = clz("android.widget.EditText").getOneNodeInfo(10000);
    if (node) {
        node.imeInputText("content")
    } else {
        toast("No node");
    }
}

main();
```

### imeInputKeyCode

* IME input; requires this app's IME as default
* For scenarios without nodes, e.g. games
* @param selectors Selector; may be empty if input field is focused
* @param content See KeyEvent.KEYCODE_* values, e.g. 66 = enter, 67 = del, 84 = SEARCH
* @return `{boolean}`

```javascript showLineNumbers
function main() {
    var result = clz("android.widget.EditText").getOneNodeInfo(10000);
    if (result) {
        result.imeInputKeyCode(66);
        toast("Yes");
    } else {
        toast("No");
    }
}

main();
```

### Clear node text {#节点数据清除-cleartext}

* Requires accessibility 5.0+ or gestures via agent service
* Clear node text

```javascript showLineNumbers
function main() {
    // Get selector object
    // Select all EditText nodes: clz=android.widget.EditText
    var node = clz("android.widget.EditText").getOneNodeInfo(10000);
    if (node) {
        var r = node.clearText();
        logd("r -=" + r);
    } else {
        toast("No node");
    }
}

main();
```

### Refresh node {#节点刷新-refresh}

* This method refreshes the node cache

```javascript showLineNumbers
function main() {
    // Get selector object
    // Select all EditText nodes: clz=android.widget.EditText
    var node = clz("android.widget.EditText").getOneNodeInfo(10000);
    if (node) {
        node.refresh();
    } else {
        toast("No node");
    }
}

main();
```

### Check node valid {#节点有效判断-isvalid}

* Whether node info is valid @return bool|Boolean true if present

```javascript showLineNumbers
function main() {
    // Get selector object
    // Select all EditText nodes: clz=android.widget.EditText
    var node = clz("android.widget.EditText").getOneNodeInfo(10000);
    if (node) {
        var x = node.isValid();
        toast("Node valid:" + x);
    } else {
        toast("No node");
    }
}

main();
```

### Scroll forward scrollForward {#向前滚动-scrollforward}

* Requires accessibility 5.0+ or gestures via agent service
* Scroll forward
* @param selectors Selector object
* @return `{boolean}`

```javascript showLineNumbers
function main() {
    var node = scrollable(true).getOneNodeInfo(10000);
    var result = node.scrollForward();
    if (result) {
        toast("Scroll succeeded");
    } else {
        toast("Scroll failed");
    }
}

main();
```

### Scroll backward scrollBackward {#向后滚动-scrollbackward}

* Requires accessibility 5.0+ or gestures via agent service
* Scroll backward
* @param selectors Selector object
* @return `{boolean}`

```javascript showLineNumbers
function main() {
    var node = scrollable(true).getOneNodeInfo(10000);
    var result = node.scrollBackward();
    if (result) {
        toast("Scroll succeeded");
    } else {
        toast("Scroll failed");
    }
}

main();
```
