---
title: Overlay Functions
description: EasyClick automation scripts — Android no root overlay functions
keywords:
 - EasyClick automation scripts Android no root overlay functions
 - floaty
 - param
 - View
 - XML
 - return
 - android
 - showFloatXml
 - showFloatView
 - updateX
 - tag
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview

- Overlay module functions manage floating overlays
- The overlay module uses the `floaty` prefix, e.g. `floaty.requestFloatViewPermission()`

## floaty.requestFloatViewPermission Request Overlay Permission

* Request overlay permission
* @return bool true on success

```javascript showLineNumbers
function main() {
    let tag = "123";
    // Close overlay with tag=123
    floaty.close(tag)
    // Request permission
    let p = floaty.requestFloatViewPermission(1000)
    logd("Has overlay permission: " + p);
    if (!p) {
        loge("No overlay permission, aborting");
        return;
    }

    sleep(1000);
    // Show main.xml view and return the native Android object
    let view = floaty.showFloatXml(tag, "main.xml", 100, 100);

    logd(view);
    if (view) {
        // Find view with tag=web in overlay (example)
        //let web = view.findViewWithTag("web")
    }

    sleep(2000)
    // Update size
    floaty.updateSize(tag, 800, 1800)

    sleep(2000)
    // Update X position
    floaty.updateX(tag, 100)

    sleep(2000)
    // Update Y position
    floaty.updateY(tag, 100)
    sleep(2000)
    // Set non-touchable
    floaty.touchable(tag, false)
    sleep(10000)
    // Set touchable
    floaty.touchable(123, true)
}

main();
```

## floaty.showFloatXml Show an XML Overlay

* Show an XML overlay
* @param tag Overlay tag
* @param xml XML path or content
* @param x Starting X position
* @param y Starting Y position
* @return `{View}` Android View object

```javascript showLineNumbers
function main() {
 let tag = "123";
 floaty.close(tag)
 let p = floaty.requestFloatViewPermission(1000)
 logd("Has overlay permission: " + p);
 if (!p) {
 loge("No overlay permission, aborting");
 return;
 }
 sleep(1000);
 let view = floaty.showFloatXml(tag, "main.xml", 100, 100);
 logd(view);
 if (view) {
 //let web = view.findViewWithTag("web")
 }
}

main();
```

## floaty.showFloatView Show a View Overlay

* Show a View overlay
* @param tag Overlay tag
* @param view Android View object
* @param x Starting X position
* @param y Starting Y position
* @return `{View}` Android View object

```javascript showLineNumbers
function main() {
 let tag = "123";
 floaty.close(tag)
 let p = floaty.requestFloatViewPermission(1000)
 logd("Has overlay permission: " + p);
 if (!p) {
 loge("No overlay permission, aborting");
 return;
 }
 sleep(1000);
 importPackage(android.widget)
 importPackage(android.graphics)
 let tv = new TextView(context);
 tv.setText("TEST");
 tv.setBackgroundColor(Color.parseColor("#336699"))
 let view = floaty.showFloatView(tag, tv, 100, 100);
 logd(view);
}

main();
```

## floaty.updateX Set Overlay X Position

* Set overlay X position
* @param tag Overlay tag
* @param x X position
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 let tag = "123";
 floaty.close(tag)
 let p = floaty.requestFloatViewPermission(1000)
 logd("Has overlay permission: " + p);
 if (!p) {
 loge("No overlay permission, aborting");
 return;
 }
 sleep(1000);
 let view = floaty.showFloatXml(tag, "main.xml", 100, 100);
 logd(view);
 sleep(2000)
 floaty.updateSize(tag, 800, 1800)
 sleep(2000)
 floaty.updateX(tag, 100)
 sleep(2000)
 floaty.updateY(tag, 100)
 sleep(2000)
 floaty.touchable(tag, false)
 sleep(10000)
 floaty.touchable(123, true)
}

main();
```

## floaty.updateY Set Overlay Y Position

* Set overlay Y position
* @param tag Overlay tag
* @param y Y position
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 let tag = "123";
 floaty.close(tag)
 let p = floaty.requestFloatViewPermission(1000)
 logd("Has overlay permission: " + p);
 if (!p) {
 loge("No overlay permission, aborting");
 return;
 }
 sleep(1000);
 let view = floaty.showFloatXml(tag, "main.xml", 100, 100);
 logd(view);
 sleep(2000)
 floaty.updateSize(tag, 800, 1800)
 sleep(2000)
 floaty.updateX(tag, 100)
 sleep(2000)
 floaty.updateY(tag, 100)
 sleep(2000)
 floaty.touchable(tag, false)
 sleep(10000)
 floaty.touchable(123, true)
}

main();
```

## floaty.updateSize Set Overlay Size

* Set overlay size
* @param tag Overlay tag
* @param w width
* @param h height
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 let tag = "123";
 floaty.close(tag)
 let p = floaty.requestFloatViewPermission(1000)
 logd("Has overlay permission: " + p);
 if (!p) {
 loge("No overlay permission, aborting");
 return;
 }
 sleep(1000);
 let view = floaty.showFloatXml(tag, "main.xml", 100, 100);
 logd(view);
 sleep(2000)
 floaty.updateSize(tag, 800, 1800)
 sleep(2000)
 floaty.updateX(tag, 100)
 sleep(2000)
 floaty.updateY(tag, 100)
 sleep(2000)
 floaty.touchable(tag, false)
 sleep(10000)
 floaty.touchable(123, true)
}

main();
```

## floaty.close Close Overlay

* Close overlay
* @param tag Overlay tag
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 let tag = "123";
 floaty.close(tag)
 let p = floaty.requestFloatViewPermission(1000)
 logd("Has overlay permission: " + p);
 if (!p) {
 loge("No overlay permission, aborting");
 return;
 }
 sleep(1000);
 let view = floaty.showFloatXml(tag, "main.xml", 100, 100);
 logd(view);
 sleep(2000)
 floaty.updateSize(tag, 800, 1800)
 sleep(2000)
 floaty.updateX(tag, 100)
 sleep(2000)
 floaty.updateY(tag, 100)
 sleep(2000)
 floaty.touchable(tag, false)
 sleep(10000)
 floaty.touchable(123, true)
}

main();
```

## floaty.touchable Set Overlay Touch State

* Set overlay touchable
* @param touchable Touchable: true = touchable, false = not touchable
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 let tag = "123";
 floaty.close(tag)
 let p = floaty.requestFloatViewPermission(1000)
 logd("Has overlay permission: " + p);
 if (!p) {
 loge("No overlay permission, aborting");
 return;
 }
 sleep(1000);
 let view = floaty.showFloatXml(tag, "main.xml", 100, 100);
 logd(view);
 sleep(2000)
 floaty.updateSize(tag, 800, 1800)
 sleep(2000)
 floaty.updateX(tag, 100)
 sleep(2000)
 floaty.updateY(tag, 100)
 sleep(2000)
 floaty.touchable(tag, false)
 sleep(10000)
 floaty.touchable(123, true)
}

main();
```

## floaty.focusable Set Overlay Focus State

* Set overlay focusable (required for input fields to receive focus)
* @param focusable Focusable: true = focusable, false = not focusable
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 let tag = "123";
 floaty.close(tag)
 let p = floaty.requestFloatViewPermission(1000)
 logd("Has overlay permission: " + p);
 if (!p) {
 loge("No overlay permission, aborting");
 return;
 }
 sleep(1000);
 let view = floaty.showFloatXml(tag, "main.xml", 100, 100);
 floaty.focusable(tag, true)
 logd(view);
 sleep(2000)
 floaty.updateSize(tag, 800, 1800)
 sleep(2000)
 floaty.updateX(tag, 100)
 sleep(2000)
 floaty.updateY(tag, 100)
 sleep(2000)
 floaty.touchable(tag, false)
 sleep(10000)
 floaty.touchable(123, true)
}

main();
```

## floaty.getX Get Overlay X Position

- Get overlay X position
- Supported EC 6.7.0+
- @param tag Overlay tag
- @return `{int}` -1 on failure; otherwise coordinate value

```javascript showLineNumbers
function main() {
    let tag = "123";
    floaty.close(tag)
    let p = floaty.requestFloatViewPermission(1000)
    logd("Has overlay permission: " + p);
    if (!p) {
    loge("No overlay permission, aborting");
    return;
        }
        sleep(1000);
    let view = floaty.showFloatXml(tag, "main.xml", 100, 100);
    floaty.focusable(tag, true)
    logd(view);
    view.measure(0, 0);
    sleep(2000)
    logd(floaty.getX(tag))
    logd(floaty.getY(tag))
    logd(floaty.getWidth(tag))
    logd(floaty.getHeight(tag))
}

main();
```

## floaty.getY Get Overlay Y Position

- Get overlay Y position
- Supported EC 6.7.0+
- @param tag Overlay tag
- @return `{int}` -1 on failure; otherwise coordinate value

```javascript showLineNumbers
function main() {
 let tag = "123";
 floaty.close(tag)
 let p = floaty.requestFloatViewPermission(1000)
 logd("Has overlay permission: " + p);
 if (!p) {
 loge("No overlay permission, aborting");
 return;
 }
 sleep(1000);
 let view = floaty.showFloatXml(tag, "main.xml", 100, 100);
 floaty.focusable(tag, true)
 logd(view);
 view.measure(0, 0);
 sleep(2000)
 logd(floaty.getX(tag))
 logd(floaty.getY(tag))
 logd(floaty.getWidth(tag))
 logd(floaty.getHeight(tag))
}

main();
```

## floaty.getWidth Get Overlay Width

- Get overlay width
- Supported EC 6.7.0+
- @param tag Overlay tag
- @return `{int}` -1 on failure; otherwise dimension value

```javascript showLineNumbers
function main() {
 let tag = "123";
 floaty.close(tag)
 let p = floaty.requestFloatViewPermission(1000)
 logd("Has overlay permission: " + p);
 if (!p) {
 loge("No overlay permission, aborting");
 return;
 }
 sleep(1000);
 let view = floaty.showFloatXml(tag, "main.xml", 100, 100);
 floaty.focusable(tag, true)
 logd(view);
 view.measure(0, 0);
 sleep(2000)
 logd(floaty.getX(tag))
 logd(floaty.getY(tag))
 logd(floaty.getWidth(tag))
 logd(floaty.getHeight(tag))
}

main();
```

## floaty.getHeight Get Overlay Height

- Get overlay height
- Supported EC 6.7.0+
- @param tag Overlay tag
- @return `{int}` -1 on failure; otherwise dimension value

```javascript showLineNumbers
function main() {
 let tag = "123";
 floaty.close(tag)
 let p = floaty.requestFloatViewPermission(1000)
 logd("Has overlay permission: " + p);
 if (!p) {
 loge("No overlay permission, aborting");
 return;
 }
 sleep(1000);
 let view = floaty.showFloatXml(tag, "main.xml", 100, 100);
 floaty.focusable(tag, true)
 logd(view);
 view.measure(0, 0);
 sleep(2000)
 logd(floaty.getX(tag))
 logd(floaty.getY(tag))
 logd(floaty.getWidth(tag))
 logd(floaty.getHeight(tag))
}

main();
```
