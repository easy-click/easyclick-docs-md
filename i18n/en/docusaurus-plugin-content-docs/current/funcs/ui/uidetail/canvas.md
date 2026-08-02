---
title: Canvas
description: EasyClick automation scripts — Android no root — Canvas
keywords:
 - EasyClick automation scripts Android no root Canvas
 - ui
 - Canvas
 - UI
 - js
 - EC
 - 6.8.0
 - en
 - funcs
 - native
 - EasyClick
 - mobile automation
 - automation testing
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---


## Overview
Canvas drawing component — draw custom visual effects

Requires EC 6.8.0+

## Example
```xml showLineNumbers
<?xml version="1.0" encoding="utf-8"?>

    <LinearLayout
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns:android="http://schemas.android.com/apk/res/android" xsi:noNamespaceSchemaLocation="layout.xsd"
        android:layout_height="match_parent" android:layout_width="match_parent"
                  android:background="#336699"
                  android:orientation="vertical" android:padding="20dp">



            <Canvas android:tag="ca" android:cornerRadius="24dp"
                    android:background="#88000000" android:textColor="#ffffff" android:layout_width="match_parent"/>


    </LinearLayout>



```

## Usage in UI (ui.js)

```javascript showLineNumbers

// Drawing range
var minX = -5;
var maxX = 5;
var minY = -10;

// Paint brush
var paint = new android.graphics.Paint();
// Function to draw — here a quadratic function
var f = function (x) {
    return x * x + 3 * x - 4;
}

function main() {
    ui.layout("ddd", "f.xml")
    logd(ui.ca);

    ui.ca.onDraw(function (canvas) {
        logd("==> " + canvas);
        var w = canvas.getWidth();
        var h = canvas.getHeight();
        // Calculate Y-axis upper bound
        var maxY = minY + (maxX - minX) * h / w;
        // Set paint color to black
        paint.setColor(android.graphics.Color.parseColor("#000000"));
        // Draw two axes
        canvas.drawLine(w / 2, 0, w / 2, h, paint);
        canvas.drawLine(0, h / 2, w, h / 2, paint);
        // Set paint color to red
        paint.setColor(android.graphics.Color.parseColor("#ff0000"));
        // Draw the graph
        for (var i = 0; i < w; i++) {
            var x = minX + i / w * (maxX - minX);
            var y = f(x);
            var j = h - (y - minY) / (maxY - minY) * h;
            canvas.drawPoint(i, j, paint);
        }
    })
}

main();



```



## Properties

### Common Properties
See [Common Properties](/docs/funcs/ui/ui-native-view#common-properties)

### Private Properties

| Property | Description | Values |
| :------: | :------: | :------: |
| layout_weight | Child weight | Number<br/>When the parent is LinearLayout, child controls can set weight|
| fillViewport | Fill entire viewport | true: yes false: no |

##




