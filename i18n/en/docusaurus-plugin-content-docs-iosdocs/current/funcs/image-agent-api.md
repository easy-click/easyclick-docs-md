---
title: Image Functions — On-Device Execution
description: EasyClick automation scripts — iOS no jailbreak image functions on-device execution
keywords:
 - EasyClick automation scripts — iOS no jailbreak image functions on-device execution
 - imageAgent
 - AutoImage
 - useOpencvMat
 - setInitParam
 - jpg
 - captureFullScreenEx
 - mat
 - param
 - captureFullScreen
 - cmpColor
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview {#说明}

:::tip

- This module runs computations on the phone; data is stored on the phone as well, so no data transfer to the control center is needed
- You can also exchange images with the control center via functions
- This greatly improves USB performance and saves transfer time
 :::

## Settings {#设置}

### imageAgent.useOpencvMat Initialize Parameters {#imageagentuseopencvmat-初始化参数}

* Switch image storage mode to OpenCV mat format
* Compatible with EC iOS 7.16.0+
* After switching, capture, read, find image, find color, etc. all use mat format — faster and less memory
* Measured: 50%–80% memory reduction, 20%–30% CPU reduction, 100%–200% speed improvement
* To switch image formats, see the `imageToMatFormat` and `matToImageFormat` functions
* @param use 1 = yes, 0 = no
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    let r = imageAgent.useOpencvMat(1);
    logd(r)
    // Remaining code same as before — find color, find image, etc.
}

main();
```

### imageAgent.setInitParam Initialize Parameters {#imageagentsetinitparam-初始化参数}

* @param params Parameters TBD

```javascript showLineNumbers
function main() {
    imageAgent.setInitParam({});
}

main();
```

## Standard Screenshot (JPG) {#普通截图-jpg}

### imageAgent.captureFullScreenEx Capture Full Screen as AutoImage {#imageagentcapturefullscreenex-截取全屏-autoimage-对象}

* Capture full screen in JPG format
* Compatible with EC USB iOS 5.0.0+
* @param ext Extended parameters to adjust capture method and quality:
* type: 1 = JPG capture method 1
* 2 = JPG capture method 2
* 3 = PNG format; PNG does not support quality parameter — choose based on your device
* quality: Image quality; when type=1, supports 1, 50, 100 (three quality levels)
* When type=2, supports quality 1–100
* @return `{null|AutoImage}`

```javascript showLineNumbers
function main() {
    logd("isServiceOk " + isServiceOk());
    startEnv()
    logd("isServiceOk " + isServiceOk());
    for (let i = 0; i < 10; i++) {
        console.time(1)
        let cap = imageAgent.captureFullScreenEx({"type": "1", "quality": 50})
        logd("Screenshot data: " + cap + " elapsed: " + console.timeEnd(1))
        sleep(1000)
        // Recycle image
        imageAgent.recycle(cap)
    }
}
```

### imageAgent.captureFullScreen Capture Full Screen as AutoImage {#imageagentcapturefullscreen-截取全屏-autoimage-对象}

* Capture current screen and return an AutoImage object in JPG format.
* @return AutoImage object or null

```javascript showLineNumbers
function main() {
    logd("isServiceOk " + isServiceOk());
    startEnv()
    logd("isServiceOk " + isServiceOk());
    for (let i = 0; i < 10; i++) {
        let cap = imageAgent.captureFullScreen()
        logd("Screenshot data: " + cap)
        sleep(1000)
        // Recycle image
        imageAgent.recycle(cap)
    }
}

main();
```

## Color Comparison {#比色}

### imageAgent.cmpColor Multi-Point Color Comparison {#imageagentcmpcolor-多点比色}

* Single or multi-point color comparison; returns true if all points match, false otherwise
* @param image1 Image
* @param points String like `6|1|0x969696-0x000010,1|12|0x969696,-4|0|0x969696`
* @param threshold Color similarity when finding color; range 0.0 ~ 1.0
* @param x Region start X coordinate; 0 for full-screen search
* @param y Region start Y coordinate; 0 for full-screen search
* @param ex End X coordinate; 0 for full-screen search
* @param ey End Y coordinate; 0 for full-screen search
* @return Boolean; true if found, false if not found

```javascript showLineNumbers
function main() {
    let req = startEnv();
    if (!req) {
        logd("Failed to request permissions");
        return;
    }
    // Wait at least 1s after permissions (more on slow devices) before screenshot, or capture may fail
    sleep(1000)
    let aimage = imageAgent.captureFullScreen();
    if (aimage != null) {
        let points3 = "205|1130|0xff944b-0x101010,211|1158|0xff8e42,191|1175|0xfcfbf7";
        let points = imageAgent.cmpColor(aimage, points3, 0.9, 0, 0, 0, 0);
        logd("points " + points);
        // Recycle image
        imageAgent.recycle(aimage)
    }

}

main();
```

### imageAgent.cmpColorEx Multi-Point Color Comparison (Extended) {#imageagentcmpcolorex-多点比色扩展}

* Single or multi-point color comparison with auto-screenshot; returns true if all match, false otherwise
* @param points String like `6|1|0x969696-0x000010,1|12|0x969696,-4|0|0x969696`
* @param threshold Color similarity when finding color; range 0.0 ~ 1.0
* @param x Region start X coordinate; 0 for full-screen search
* @param y Region start Y coordinate; 0 for full-screen search
* @param ex End X coordinate; 0 for full-screen search
* @param ey End Y coordinate; 0 for full-screen search
* @return Boolean; true if found, false if not found

```javascript showLineNumbers
function main() {
    let req = startEnv();
    if (!req) {
        logd("Failed to request permissions");
        return;
    }

    // Wait at least 1s after permissions (more on slow devices) before screenshot, or capture may fail
    sleep(1000)
    let points3 = "205|1130|0xff944b-0x101010,211|1158|0xff8e42,191|1175|0xfcfbf7";
    let points = imageAgent.cmpColorEx(points3, 0.9, 0, 0, 0, 0);
    logd("points " + points);

}

main();
```

### imageAgent.cmpMultiColor Multi-Group Color Comparison {#imageagentcmpmulticolor-多组比色}

* Multi-point or multi-point array color comparison; searches sequentially. Returns index of matching points, or -1 if none found
* @param image1 Image
* @param points Array like `["6|1|0x969696-0x000010,1|12|0x969696,-4|0|0x969696","6|1|0x969696"]`
* @param threshold Color similarity when finding color; range 0.0 ~ 1.0
* @param x Region start X coordinate; 0 for full-screen search
* @param y Region start Y coordinate; 0 for full-screen search
* @param ex End X coordinate; 0 for full-screen search
* @param ey End Y coordinate; 0 for full-screen search
* @return Integer; index of matching points, or -1 if none found

```javascript showLineNumbers
function main() {
    let req = startEnv();
    if (!req) {
        logd("Failed to request permissions");
        return;
    }
    // Wait at least 1s after permissions (more on slow devices) before screenshot, or capture may fail
    sleep(1000)
    let aimage = imageAgent.captureFullScreen();
    if (aimage != null) {
        let points1 = "205|112230|0xff944b,211|1158|0xff8e42,191|1175|0xfcfbf7";
        let points2 = "205|113022|0xff944b,211|1158|0xff8e42,191|1175|0xfcfbf7";
        let points3 = "205|1130|0xff944b,211|1158|0xff8e42,191|1175|0xfcfbf7";
        let points = imageAgent.cmpMultiColor(aimage, [points1, points2, points3], 0.9, 0, 0, 0, 0);
        logd("points " + points);
        // Recycle image
        imageAgent.recycle(aimage)
    }
}

main();
```

### imageAgent.cmpMultiColorEx Multi-Group Color Comparison (Extended) {#imageagentcmpmulticoloex-多组比色扩展}

* Multi-point or array color comparison with auto-screenshot; returns index or -1
* @param points Array like `["6|1|0x969696-0x000010,1|12|0x969696,-4|0|0x969696","6|1|0x969696"]`
* @param threshold Color similarity when finding color; range 0.0 ~ 1.0
* @param x Region start X coordinate; 0 for full-screen search
* @param y Region start Y coordinate; 0 for full-screen search
* @param ex End X coordinate; 0 for full-screen search
* @param ey End Y coordinate; 0 for full-screen search
* @return Integer; index of matching points, or -1 if none found

```javascript showLineNumbers
function main() {
    let req = startEnv();
    if (!req) {
        logd("Failed to request permissions");
        return;
    }
    // Wait at least 1s after permissions (more on slow devices) before screenshot, or capture may fail
    sleep(1000)
    let points1 = "205|112230|0xff944b,211|1158|0xff8e42,191|1175|0xfcfbf7";
    let points2 = "205|113022|0xff944b,211|1158|0xff8e42,191|1175|0xfcfbf7";
    let points3 = "205|1130|0xff944b,211|1158|0xff8e42,191|1175|0xfcfbf7";
    let points = imageAgent.cmpMultiColorEx([points1, points2, points3], 0.9, 0, 0, 0, 0);
    logd("points " + points);
}

main();
```

## Find Color {#找色}

### imageAgent.findColor Single-Point Find Color {#imageagentfindcolor-单点找色}

* Find a point in the image whose color exactly matches color and return its coordinates; returns null if not found.
* @param image Image
* @param color Target color, e.g. `0xCDD7E9-0x101010,0xCDD7E9-0x101010`
* @param threshold Color similarity when finding color; range 0.0 ~ 1.0
* @param x Region start X coordinate
* @param y Region start Y coordinate
* @param ex End X coordinate
* @param ey End Y coordinate
* @param limit Result count limit
* @param orz Direction; values 1–8
* @return Array of Point coordinates or null

```javascript showLineNumbers
function main() {
    let req = startEnv();
    if (!req) {
        logd("Failed to request permissions");
        return;
    }
    // Wait at least 1s after permissions (more on slow devices) before screenshot, or capture may fail
    sleep(1000)
    let aimage = imageAgent.captureFullScreen();
    if (aimage != null) {
        let points = imageAgent.findColor(aimage, "0xCDD7E9-0x101010,0xCDD7E9-0x101010", 0.9, 0, 0, 0, 0, 10, 1);
        logd("points " + JSON.stringify(points));
        // This is an array
        if (points) {
            for (let i = 0; i < points.length; i++) {
                logd(JSON.stringify(points[i]), points[i].x, points[i].y)
                // Click coordinates
                clickPoint(points[i].x, points[i].y)
            }
        }
        // Recycle image
        imageAgent.recycle(aimage)
    }

}

main();
```

### imageAgent.findColorEx Auto-Screenshot Single-Point Find Color {#imageagentfindcolorex-自动截屏单点找色}

* Find a point on the current screen whose color exactly matches and return coordinates; null if not found.
* @param color Target color
* @param threshold Color similarity when finding color; range 0.0 ~ 1.0
* @param x Region start X coordinate
* @param y Region start Y coordinate
* @param ex End X coordinate
* @param ey End Y coordinate
* @param limit Result count limit
* @param orz Direction; values 1–8
* @return Array of Point coordinates or null

```javascript showLineNumbers
function main() {
    let req = startEnv();
    if (!req) {
        logd("Failed to request permissions");
        return;
    }
    // Wait at least 1s after permissions (more on slow devices) before screenshot, or capture may fail
    sleep(1000)
    let points = image.findColorEx("0xCDD7E9-0x101010,0xCDD7E9-0x101010", 0.9, 0, 0, 0, 0, 10, 1);
    logd("points " + JSON.stringify(points));
    // This is an array
    if (points && points.length > 0) {
        for (let i = 0; i < points.length; i++) {
            logd(JSON.stringify(points[i]), points[i].x, points[i].y)
            // Click coordinates
            clickPoint(points[i].x, points[i].y)
        }
    }

}

main();
```

### imageAgent.findMultiColor Multi-Point Find Color {#imageagentfindmulticolo-多点找色}

* Multi-point find color; finds all matching points, similar to KeyWizard multi-point find color.
* @param image Image to search
* @param firstColor First point color
* @param points String like `6|1|0x969696-0x000010,1|12|0x969696,-4|0|0x969696`
* @param threshold Color similarity when finding color; range 0.0 ~ 1.0
* @param x Region start X coordinate
* @param y Region start Y coordinate
* @param ex End X coordinate
* @param ey End Y coordinate
* @param limit Result count limit
* @param orz Direction; values 1–8
* @return Array of Point coordinates or null

```javascript showLineNumbers
function main() {
    let req = startEnv();
    if (!req) {
        logd("Failed to request permissions");
        return;
    }
    // Wait at least 1s after permissions (more on slow devices) before screenshot, or capture may fail
    sleep(1000)
    let aimage = imageAgent.captureFullScreen();
    if (aimage != null) {
        let points = imageAgent.findMultiColor(aimage, "0xDD7A5F-0x101010", "29|25|0xBB454B-0x101010,58|44|0xA6363A-0x101010", 0.9, 0, 0, 0, 0, 10, 1);
        logd("points " + JSON.stringify(points));
        // This is an array
        if (points && points.length > 0) {
            for (let i = 0; i < points.length; i++) {
                logd(points[i], points[i].x, points[i].y)
                // Click coordinates
                clickPoint(points[i].x, points[i].y)
            }
        }
        // Recycle image
        imageAgent.recycle(aimage)
    }

}

main();
```

### imageAgent.findMultiColorEx Auto-Screenshot Multi-Point Find Color {#imageagentfindmulticoloex-自动截屏多点找色}

* Multi-point find color; finds all matching points, similar to KeyWizard multi-point find color.
* @param firstColor First point color
* @param points String like `6|1|0x969696-0x000010,1|12|0x969696,-4|0|0x969696`
* @param threshold Color similarity when finding color; range 0.0 ~ 1.0
* @param x Region start X coordinate
* @param y Region start Y coordinate
* @param ex End X coordinate
* @param ey End Y coordinate
* @param limit Result count limit
* @param orz Direction; values 1–8
* @return Array of Point coordinates or null

```javascript showLineNumbers
function main() {
    let req = startEnv();
    if (!req) {
        logd("Failed to request permissions");
        return;
    }

    // Wait at least 1s after permissions (more on slow devices) before screenshot, or capture may fail
    sleep(1000)
    let points = imageAgent.findMultiColorEx("0xDD7A5F-0x101010", "29|25|0xBB454B-0x101010,58|44|0xA6363A-0x101010", 0.9, 0, 0, 0, 0, 10, 1);
    logd("points " + JSON.stringify(points));
    // This is an array
    if (points && points.length > 0) {
        for (let i = 0; i < points.length; i++) {
            logd(points[i], points[i].x, points[i].y)
            // Click coordinates
            clickPoint(points[i].x, points[i].y)
        }
    }


}

main();
```

## Find Non-Color {#找非色}

### imageAgent.findNotColor Find Non-Color {#imageagentfindnotcolor-找非色}

* Find points in the image whose color does not match color; null if not found.
* Compatible with EC USB 6.30.0+
* @param image Image
* @param color Target color, e.g. `0xCDD7E9-0x101010,0xCDD7E9-0x101010` (generated by EC tool)
* @param threshold Color similarity when finding color; range 0.0 ~ 1.0
* @param x Region start X coordinate
* @param y Region start Y coordinate
* @param ex End X coordinate
* @param ey End Y coordinate
* @param limit Result count limit
* @param orz Direction; values 1–8
* @return Array of Point coordinates or null

```javascript showLineNumbers
function main() {
    let aimage = imageAgent.captureFullScreen();
    if (aimage != null) {
        let points = imageAgent.findNotColor(aimage, "0xCDD7E9-0x101010,0xCDD7E9-0x101010", 0.9, 0, 0, 0, 0, 10, 1);
        logd("points " + JSON.stringify(points));
        // This is an array
        if (points) {
            for (let i = 0; i < points.length; i++) {
                logd(JSON.stringify(points[i]), points[i].x, points[i].y)
                // Click coordinates
                clickPoint(points[i].x, points[i].y)
            }
        }
        // Recycle image
        imageAgent.recycle(aimage)
    }

}

main();
```

## Find Image {#找图}

### imageAgent.findImageByColor Transparent Find Image {#imageagentfindimagebycolor-透明找图}

* Transparent find image (this function does not require OpenCV initialization)
* @param image Large image
* @param template Small image (template)
* @param x Find region start X coordinate
* @param y Find region start Y coordinate
* @param ex End X coordinate
* @param ey End Y coordinate
* @param threshold Image similarity; float 0~1. Default 0.9.
* @param limit Result count limit; use 1 for one result, or more for multiple
* @return Array of Point coordinates or null

```javascript showLineNumbers
function main() {
    // Wait at least 1s after permissions (more on slow devices) before screenshot, or capture may fail
    sleep(1000)
    // Read sms.png from project res folder
    let sms = imageAgent.readResAutoImage("sms.png");
    // Capture screen
    let aimage = imageAgent.captureFullScreen();
    logd("aimage " + aimage);
    if (aimage != null) {
        // Search in image
        let points = image.findImageByColor(aimage, sms, 0, 0, 0, 0, 0.8, 5);
        logd("points " + JSON.stringify(points));
        // This is an array
        if (points && points.length > 0) {
            for (let i = 0; i < points.length; i++) {
                logd(points[i])
                let x = points[i].x
                let y = points[i].y
                // Click coordinates
                clickPoint(x, y)
            }
        }
        // Recycle image
        imageAgent.recycle(aimage)
    }
    // Recycle image
    imageAgent.recycle(sms)
}

main();
```

### imageAgent.findImageByColorEx Transparent Find Image (Extended) {#imageagentfindimagebycolorex-透明找图扩展}

* Find image by color; supports transparent images; no OpenCV processing needed
* Returns null when not found anywhere in the image
* @param image1 Large image
* @param template Small image (template)
* @param x Find region start X coordinate
* @param y Find region start Y coordinate
* @param ex End X coordinate
* @param ey End Y coordinate
* @param limit Result count limit; use 1 for one result, or more for multiple
* @param extra Extended parameters as a map, e.g.
*
```{"firstColorOffset":"#101010","firstColorThreshold":1.0,"otherColorOffset":"#101010","otherColorThreshold":0.9,"cmpColorSucThreshold":1.0}```
* firstColorOffset: Color offset for first matched color, e.g. #101010
* firstColorThreshold: Color offset coefficient for first match, e.g. 0.9
* otherColorOffset: Color offset for remaining colors, e.g. #101010
* otherColorThreshold: Color offset coefficient for remaining colors, e.g. 0.9
* cmpColorSucThreshold: Fraction of colors that must match for success, e.g. 0.9 = 90% of points
* startX: X coordinate to start searching from for the first point
* startY: Y coordinate to start searching from for the first point
* @return Array of Point coordinates or null

```javascript showLineNumbers
function main() {

 let d = startEnv();
 logd("Starting service--{}", d)
 let smallTmplate = readResAutoImage("tmp4.png");

 for (let i = 0; i < 100; i++) {
 sleep(1000)
 let img = imageAgent.captureFullScreen();
 logd("img = {}", img)
 if (img == null) {
 continue
 }
 console.time(1)
 let extra = {
 "firstColorOffset": "#202020",
 "otherColorOffset": "#000000",
 "cmpColorSucThreshold": 1,
 "firstColorThreshold": "1",
 "otherColorThreshold": "1",
 "startX": 0,
 "startY": 0
 }
 let points = imageAgent.findImageByColorEx(img, smallTmplate, 0, 0, 0, 0, 100, extra);
 logd("time-{}", console.timeEnd(1))
 // This is an array
 if (points) {
 logd("points " + JSON.stringify(points));
 }
 imageAgent.recycle(img)
 }
 imageAgent.recycle(smallTmplate)
}

main()
```

### imageAgent.findImage Find Image {#imageagentfindimage-找图}

* Find image. Locates template in image via template matching; returns Rect region array or null.
* Compatible with EC USB 7.16.0+
* @param image Large image
* @param template Small image (template)
* @param x Find region start X coordinate
* @param y Find region start Y coordinate
* @param ex End X coordinate
* @param ey End Y coordinate
* @param weakThreshold Weak threshold. Used each matching round to decide whether to continue. If similarity is below this, stop matching. Float 0~1. Default 0.7.
* @param threshold Image similarity; float 0~1. Default 0.9.
* @param limit Result count limit; use 1 for one result, or more for multiple
* @param method 0: TM_SQDIFF squared difference, 1: TM_SQDIFF_NORMED normalized squared difference, 2: TM_CCORR correlation, 3: TM_CCORR_NORMED normalized correlation, 4: TM_CCOEFF coefficient, 5: TM_CCOEFF_NORMED normalized coefficient
* @return Rect region object array or null

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("Failed to request permissions");
 return;
 }
 // Read sms.png from project res folder
 let sms = imageAgent.readResAutoImage("sms.png");
 // Capture screen
 let aimage = imageAgent.captureFullScreen();
 logd("aimage " + aimage);
 if (aimage != null) {
 // Search in image
 let points = imageAgent.findImage(aimage, sms, 0, 0, 0, 0, 0.7, 0.9, 21, 5);
 logd("points " + JSON.stringify(points));
 // This is an array
 if (points && points.length > 0) {
 for (let i = 0; i < points.length; i++) {
 logd(points[i])
 logd("Similarity: " + points[i]['similarity'])
 let x = parseInt((points[i].left + points[i].right) / 2)
 let y = parseInt((points[i].top + points[i].bottom) / 2)
 // Click coordinates
 clickPoint(x, y)
 }
 }
 // Recycle image
 imageAgent.recycle(aimage)
 }
 // Recycle image
 imageAgent.recycle(sms)
}

main();
```

### imageAgent.matchTemplate Image Template Matching {#imageagentmatchtemplate-图片模板匹配}

* OpenCV template matching wrapper
* Compatible with EC iOS 7.16.0+
* @param image Large image
* @param template Small image (template)
* @param weakThreshold Weak threshold. Used each matching round to decide whether to continue. Float 0~1. Default 0.9.
* @param threshold Strong threshold. Validates final match result; if similarity exceeds this in any round, returns immediately. Float 0~1. Default 0.9.
* @param rect Find region. See findColor for rect details.
* @param maxLevel Default -1; generally you do not need to change. Auto-adjusts by image size when omitted. Uses image pyramid; level = pyramid depth. Higher level may improve speed but can cause failures (over-shrunk image) or wrong positions. Only tune if you understand this parameter.
* @param limit Result count limit; use 1 for one result, or more for multiple
* @param method 0: TM_SQDIFF squared difference, 1: TM_SQDIFF_NORMED normalized squared difference, 2: TM_CCORR correlation, 3: TM_CCORR_NORMED normalized correlation, 4: TM_CCOEFF coefficient, 5: TM_CCOEFF_NORMED normalized coefficient
* @return Match collection or null

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("Failed to request permissions");
 return;
 }
 // Wait at least 1s after permissions (more on slow devices) before screenshot, or capture may fail
 sleep(1000)
 let aimage = imageAgent.captureFullScreen();
 if (aimage != null) {
 let temp = imageAgent.readResAutoImage("tmp.png");
 let rectp = new Rect();
 rectp.left = 0;
 rectp.top = 0;
 rectp.right = 0;
 rectp.bottom = 0;
 let matchs = imageAgent.matchTemplate(aimage, temp, 0.7, 0.9, rectp, -1, 10, 5);
 // This is an array
 logd(JSON.stringify(matchs));
 // This is an array
 if (matchs && matchs.length > 0) {
 for (let i = 0; i < matchs.length; i++) {
 logd(JSON.stringify(matchs[i].point));
 clickPoint(matchs[i].point.x, matchs[i].point.y)
 }
 }
 // Recycle image
 imageAgent.recycle(aimage)
 // Recycle image
 imageAgent.recycle(temp)
 }
}

main();
```

## Grayscale {#灰度}
### imageAgent.gray Grayscale Image {#imageagentgray-灰度图片}

* Grayscale image
* Compatible with EC iOS USB version 8.18.0+
* @param img
* @return `{null|AutoImage}`

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("Failed to request permissions");
 return;
 }
 let screenImage = imageAgent.captureFullScreenEx({"type": "1", "quality": 100});
 logd("start...")
 if (screenImage != null) {
 // Binarize
 let newimage = imageAgent.gray(screenImage);
 if (newimage) {
 logd("newimage " + newimage.uuid);
 // Pull image data to control center
 let imginCenter = imageAgent.pullImageToCenter(newimage)
 logd("imginCenter " + imginCenter);
 // Save to PC using control center function
 image.saveTo(imginCenter, "aaa.jpg")
 }
 // Recycle image
 imageAgent.recycle(screenImage)
 }
}

main();
```

## Binarization {#二值化}

### imageAgent.binaryzation Binarize Image {#imageagentbinaryzation-二值化image}

* Binarize AutoImage
* @param img AutoImage object
* @param threshold Binarization coefficient, 0 ~ 255
* @return AutoImage object or null

```javascript showLineNumbers
function main() {
 for (let i = 0; i < 1; i++) {
// Capture screen
 let screenImage = imageAgent.captureFullScreenEx({"type": "1", "quality": 100});
 logd("start...")
 if (screenImage != null) {
 // Binarize
 let newimage = imageAgent.binaryzation(screenImage, 200);
 if (newimage) {
 logd("newimage " + newimage.uuid);
 // Pull image data to control center
 let imginCenter = imageAgent.pullImageToCenter(newimage)
 logd("imginCenter " + imginCenter);
 // Save to PC using control center function
 image.saveTo(imginCenter, "aaa.jpg")
 }
 // Recycle image
 imageAgent.recycle(screenImage)
 }
 }

}

main();
```

### imageAgent.binaryzationEx Adaptive Binarization {#imageagentbinaryzationex-自适应二值化}

* Adaptive binarization using OpenCV adaptiveThreshold
* Compatible with EC USB 7.16.0+
* @param img AutoImage object
* @param map MAP parameters
    * diameter: Denoising diameter; see OpenCV bilateralFilter
    * adaptiveMethod: Adaptive binarization method 0 or 1 — ADAPTIVE_THRESH_MEAN_C=0, ADAPTIVE_THRESH_GAUSSIAN_C=1
    * blockSize: Neighborhood block size in pixels; odd values like 3, 5, 7
    * c: Offset adjustment value
    * ```{"diameter":20,"adaptiveMethod":1,"c":9,"blockSize":51}```
* @return `{null|AutoImage}`

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("Failed to request permissions");
 return;
 }
 let screenImage = imageAgent.captureFullScreenEx({"type": "1", "quality": 100});
 logd("start...")
 if (screenImage != null) {
 // Binarize
 let newimage = imageAgent.binaryzationEx(d, {
 "diameter": 20,
 "adaptiveMethod": 1,
 "c": 9, "blockSize": 51
 });
 if (newimage) {
 logd("newimage " + newimage.uuid);
 // Pull image data to control center
 let imginCenter = imageAgent.pullImageToCenter(newimage)
 logd("imginCenter " + imginCenter);
 // Save to PC using control center function
 image.saveTo(imginCenter, "aaa.jpg")
 }
 // Recycle image
 imageAgent.recycle(screenImage)
 }
}

main();
```

## Other {#其他}

### imageAgent.rotateImage Rotate Image {#imageagentrotateimage-旋转图像}

* Rotate image
* Compatible with EC iOS control center 6.0+
* @param img Image object
* @param degree Degrees: 0 = portrait (Home button at bottom), -90 = counter-clockwise 90° (Home on right), 90 = clockwise 90° (Home on left)
* @return AutoImage object or null

```javascript showLineNumbers
function main() {
 let img = imageAgent.captureFullScreen()
 logd(" img width " + imageAgent.getWidth(img2))
 let img2 = imageAgent.rotateImage(img, -90);
 imageAgent.recycle(img)
 logd(" img2 width " + imageAgent.getWidth(img2))
 imageAgent.recycle(img2)
}

main();
```

### imageAgent.readImage Read File as Image {#imageagentreadimage-读取文件为image}

* Read image file at path and return an `{@link AutoImage}` object. Returns null if file missing or undecodable.
* @param path Image path
* @return AutoImage object or null

```javascript showLineNumbers
function main() {
 let autoimg = imageAgent.readImage("F:/a.png");
 // Recycle image
 imageAgent.recycle(autoimg)
}

main();
```

### imageAgent.readResAutoImage Read res File as Image {#imageagentreadresautoimage-读取res文件为image}

* Read image from IEC res folder and return an `{@link AutoImage}` object. Returns null if missing or undecodable.
* @param res Image path
* @return AutoImage object or null

```javascript showLineNumbers
function main() {
 let autoimg = imageAgent.readResAutoImage("a.png");
 // Recycle image
 imageAgent.recycle(autoimg)
}

main();
```

### imageAgent.argb Color to Hex String {#imageagentargb-颜色转16进制字符串}

* Convert integer color value to hex RGB string
* @param color Integer value
* @return `{string}` Color string

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("Failed to request permissions");
 return;
 }
 // Wait at least 1s after permissions (more on slow devices) before screenshot, or capture may fail
 sleep(1000)
 let aimage = imageAgent.captureFullScreen();
 if (aimage != null) {
 let points3 = "765|22|0x1296DB";
 logd("==" + imageAgent.argb(imageAgent.pixel(aimage, 765, 22)));
 let points = imageAgent.cmpColor(aimage, points3, 0.5, 0, 0, 0, 0);
 logd("points " + points);
 // Recycle image
 imageAgent.recycle(aimage)
 }
}

main();
```

### imageAgent.pixel Get Pixel Color Value {#imageagentpixel-取得图片的某个点的颜色值}

* Get color value of a pixel in the image
* @param img Image object
* @param x X coordinate
* @param y Y coordinate
* @return int Color value

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("Failed to request permissions");
 return;
 }
 // Wait at least 1s after permissions (more on slow devices) before screenshot, or capture may fail
 sleep(1000)
 let aimage = imageAgent.captureFullScreen();
 if (aimage != null) {
 let points3 = "765|22|0x1296DB";
 logd("==" + imageAgent.argb(imageAgent.pixel(aimage, 765, 22)));
 let points = imageAgent.cmpColor(aimage, points3, 0.5, 0, 0, 0, 0);
 logd("points " + points);
 // Recycle image
 imageAgent.recycle(aimage)
 }
}

main();
```

### imageAgent.getWidth Get Width {#imageagentgetwidth-取得宽度}

* Get width
* @param img Image object
* @return int

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("Failed to request permissions");
 return;
 }
 // Wait at least 1s after permissions (more on slow devices) before screenshot, or capture may fail
 sleep(1000)
 let aimage = imageAgent.captureFullScreen();
 if (aimage != null) {
 let w = imageAgent.getWidth(aimage);
 logd("w " + w);
 // Recycle image
 imageAgent.recycle(aimage)
 }
}

main();
```

### imageAgent.getHeight Get Height {#imageagentgetheight-取得高度}

* Get height
* @param img Image object
* @return int

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("Failed to request permissions");
 return;
 }
 // Wait at least 1s after permissions (more on slow devices) before screenshot, or capture may fail
 sleep(1000)
 let aimage = imageAgent.captureFullScreen();
 if (aimage != null) {
 let h = imageAgent.getHeight(aimage);
 logd("h " + h);
 // Recycle image
 imageAgent.recycle(aimage)
 }
}

main();
```

## Image Conversion {#图片转换}

### imageAgent.imageToMatFormat {#imageagentimagetomatformat}

* Convert to Mat storage format
* Compatible with EC iOS 7.16.0+
* @param img `{AutoImage}` Image object
* @return AutoImage in MAT storage format, or null

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("Failed to request permissions");
 return;
 }
 logd("Screenshot permission result... " + request)
 // Wait at least 1s after permissions (more on slow devices) before screenshot, or capture may fail
 sleep(1000)
 imageAgent.useOpencvMat(0)
 for (let i = 0; i < 100; i++) {
 let d = imageAgent.captureFullScreen();
 logd(d)
 sleep(1000);
 if (d) {
 let ds = imageAgent.imageToMatFormat(d);
 logd(ds)
 imageAgent.recyle(d);
 }

 }
}

main();
```

### imageAgent.matToImageFormat {#imageagentmattoimageformat}

* Convert to standard image storage format
* Compatible with EC iOS 7.16.0+
* @param img `{AutoImage}` Image object
* @return AutoImage in standard storage format, or null

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("Failed to request permissions");
 return;
 }
 logd("Screenshot permission result... " + request)
 sleep(1000)
 imageAgent.useOpencvMat(1)
 for (let i = 0; i < 100; i++) {
 let d = imageAgent.captureFullScreen();
 logd(d)
 sleep(1000);
 if (d) {
 let ds = imageAgent.matToImageFormat(d);
 logd(ds)
 imageAgent.recyle(d);
 }

 }
}

main();
```

### imageAgent.clip Clip Image {#imageagentclip-剪切图片}

* Clip image
* @param img Image object
* @param x Start X coordinate
* @param y Start Y coordinate
* @param ex End X coordinate
* @param ey End Y coordinate
* @return AutoImage object or null

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("Failed to request permissions");
 return;
 }
 logd("Screenshot permission result... " + request)
 // Wait at least 1s after permissions (more on slow devices) before screenshot, or capture may fail
 sleep(1000)
 let imageX = imageAgent.captureFullScreen();
 let r = imageAgent.clip(imageX, 100, 100, 300, 400);
 logd("result " + r);
 // Recycle image
 imageAgent.recycle(imageX)
 imageAgent.recycle(r)
}

main();
```

### imageAgent.toBase64 Convert to Base64 {#imageagenttobase64-转base64}

* Get image base64 string
* Compatible with EC iOS USB version 6.26.0+
* @param img Image object
* @return `{string}` Base64 data

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("Failed to request permissions");
 return;
 }
 sleep(1000)
 let aimage = imageAgent.captureFullScreen();
 if (aimage != null) {
 let h = imageAgent.toBase64(aimage);
 logd("h " + h);
 // Recycle image
 imageAgent.recycle(aimage)
 }
}

main();
```

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("Failed to request permissions");
 return;
 }
 logd("Screenshot permission result... " + request)
 // Wait at least 1s after permissions (more on slow devices) before screenshot, or capture may fail
 sleep(1000)
 let imageX = imageAgent.captureFullScreen();
 let r = imageAgent.clip(imageX, 100, 100, 300, 400);
 logd("result " + r);
 // Recycle image
 imageAgent.recycle(imageX)
 imageAgent.recycle(r)
}

main();
```

## Image Exchange {#图片交换}

### imageAgent.pushImageToAgent Push Image to Phone {#imageagentpushimagetoagent-推送图像到手机}

* Push control center image data to the phone environment
* @param img AutoImage object on the control center
* @return AutoImage object or null

```javascript showLineNumbers
function main() {
 let screenImage = image.captureFullScreenEx({"type": "1", "quality": 100});
 logd("==>Control center image " + screenImage.uuid)
 let age = imageAgent.pushImageToAgent(screenImage)
 logd("--Image on phone " + age.uuid)
 logd("--Image on phone getWidth " + imageAgent.getWidth(age))
}

main();
```

### imageAgent.pullImageToCenter Pull Image to Control Center {#imageagentpullimagetocenter-拉取图像到中控}

* Pull image data to the control center environment
* @param img AutoImage object on the phone
* @return AutoImage object or null

```javascript showLineNumbers
function main() {
 for (let i = 0; i < 1; i++) {
// Capture screen
 let screenImage = imageAgent.captureFullScreenEx({"type": "1", "quality": 100});
 logd("start...")
 if (screenImage != null) {
 // clip
 imageAgent
 let newimage = imageAgent.clip(screenImage, 200, 200, 400, 500);
 // This is an array
 if (newimage) {
 logd("newimage " + newimage.uuid);
 let imginCenter = imageAgent.pullImageToCenter(newimage)
 logd("imginCenter " + imginCenter);
 image.saveTo(imginCenter, "aaa.jpg")
 }
 // Recycle image
 imageAgent.recycle(screenImage)
 }
 }
}

main();
```

## Image Recycling {#图片回收}

### imageAgent.isRecycled Check If Image Is Recycled {#imageagentisrecycled-图片回收判断}

* Whether the image has been recycled
* @param img Image object
* @return bool true if already recycled

```javascript showLineNumbers
function main() {
 let imageX = imageAgent.captureFullScreen();
 let r = imageAgent.isRecycled(imageX);
 logd("result " + r);
 // Recycle image
 imageAgent.recycle(imageX)
}

main();
```

### imageAgent.recycle Recycle Image {#imageagentrecycle-回收图片}

* Recycle image
* @param img Image object

```javascript showLineNumbers
function main() {
 let imageX = imageAgent.captureFullScreen();
 // Recycle image
 imageAgent.recycle(imageX)

}

main();
```

### imageAgent.recycleAllImage Recycle All Images {#imageagentrecycleallimage-回收图片}

* Recycle all images
* Compatible with EC iOS 7.16.0+
* @return `boolean` true on success

```javascript showLineNumbers
function main() {
 let imageX = imageAgent.captureFullScreen();
 // Recycle image
 imageAgent.recycleAllImage()
}

main();
```
