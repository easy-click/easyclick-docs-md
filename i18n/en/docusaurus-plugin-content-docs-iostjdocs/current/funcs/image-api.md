---
title: Image & Color Functions
description: EasyClick automation scripts — iOS standalone image and color functions
keywords:
 - EasyClick automation scripts iOS no jailbreak image and color functions resource download
 - image
 - jpg
 - Image
 - param
 - setInitParam
 - useOpencvMat
 - captureFullScreenEx
 - mat
 - png
 - captureFullScreen
 - EasyClick
 - mobile automation
 - automation testing
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---
## Settings

### image.setInitParam Initialize Parameters

* Set image module initialization parameters
* @param params Parameters TBD

```javascript showLineNumbers
function main() {
    image.setInitParam({});
}

main();
```

### image.useOpencvMat Initialize Parameters

* Switch image storage to OpenCV mat format
* Requires EC iOS 4.6.0+
* After switching, capture, read, find-image, and find-color use mat format — faster and lower memory
* Measured: 50–80% less memory, 20–30% less CPU, 100–200% faster
* To convert formats, see `imageToMatFormat` and `matToImageFormat`
* @param use 1 = yes, 0 = no
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    let r = image.useOpencvMat(1);
    logd(r)
    // Remaining code unchanged — find-color, find-image, etc.
}

main();
```

## Proxy Mode Standard Capture (JPG)

### image.captureFullScreenEx Capture Full Screen as Image Object

* Capture full screen as JPG
* @param ext Extended params to adjust capture mode and quality
* fetchImageMode: 1=JPG mode 1, 2=JPG mode 2, 3=PNG (no quality param); choose per device
* fetchImageQuality: when fetchImageMode=1, supports 1, 50, 100
* When fetchImageMode=2, quality 1–100
* @return `{null|AutoImage}`

```javascript showLineNumbers
function main() {
    logd("isServiceOk " + isServiceOk());
    startEnv()
    logd("isServiceOk " + isServiceOk());
    for (let i = 0; i < 10; i++) {
        console.time(1)
        let cap = image.captureFullScreenEx({"fetchImageMode": "1", "fetchImageQuality": 50})
        logd("capture data: " + cap + " elapsed: " + console.timeEnd(1))
        image.saveTo(cap, "b.jpg");
        sleep(1000)
        // Recycle image to free memory
        image.recycle(cap)
    }
}
```

### image.captureFullScreen Capture Full Screen as Image Object

* Capture current screen as Image (JPG format).
* @return AutoImage object or null

```javascript showLineNumbers
function main() {
    logd("isServiceOk " + isServiceOk());
    startEnv()
    logd("isServiceOk " + isServiceOk());
    for (let i = 0; i < 10; i++) {
        let cap = image.captureFullScreen()
        logd("capture data: " + cap)
        sleep(1000)
        // Recycle image to free memory
        image.recycle(cap)
    }
}

main();
```

### image.captureFullScreenUIImage Capture Screen as UIImage

* Capture screen as UIImage
* Requires EC 4.2.0+
* @param ext Extended params to adjust capture mode and quality
* type: 1 = JPG capture mode 1
* 2 = JPG capture mode 2
* 3 = PNG (no quality param); choose per device
* quality: Image quality; type=1 supports 1, 50, 100
* When type=2, quality 1–100
* @return Swift UIImage object or null

```javascript showLineNumbers
function main() {
    logd("isServiceOk " + isServiceOk());
    startEnv()
    logd("isServiceOk " + isServiceOk());
    setComputeMode(1)
    let img1 = image.captureFullScreen();
    logd(img1)

    // Test 1
    let uiimage = image.autoImageToUIImage(img1)
    logd("autoImageToUIImage uiimage " + uiimage)
    let x = pluginLoader.callMethodAny(name, clzName, "testMethod", uiimage)
    logd(x)
    image.recycle(img1)


    let img2 = image.captureFullScreenUIImage({})
    let xX = pluginLoader.callMethodAny(name, clzName, "testMethod", img2)
    logd(xX)


    let au = image.uiimageToAutoImage(uiimage)
    logd("au " + au)

    Save to file
    image.saveTo(au, file.getSandBoxFilePath("a.jpg"))
    image.recycle(au)
    image.recycle(img2)
}

main();
```


## Shortcuts Screenshot
:::tip
- Shortcuts screenshot requires standalone service; see [OTG-HID screenshot setup](/iostjdocs/advance/tj-otg-starter#快捷指令截图)
- You can configure Shortcuts screenshot without OTG HID; setup is the same
:::

### image.getShortcutImage Get Shortcuts Screenshot
* Get Shortcuts screenshot
* Internally loops to fetch frames
* If phone is landscape but capture is not, use rotate functions
* Requires EC standalone 6.6.0+
* @param timeout Timeout in ms
* @return `{AutoImage|null}` AutoImage object

```javascript
function test_shortcut_screen() {
    // Release all OCR resources at start to avoid leaks
    ocrMut.releaseAll();
    logd("Starting script...")
    let ocrtest = ocrMut.newOcr()
    let modelPath = file.getSandBoxFilePath("")
    let detName = "det"
    let recName = "rec"
    // Built-in OCR model
    let paddleNcnnOcrMap1 = {"type": "paddleNcnnOcrV5", "numThread": 2, "padding": 32, "maxSideLen": 640}

    // Use external model
    let paddleOnnxOcrMap2 = {
        "type": "paddleNcnnOcrV5", "numThread": 2, "padding": 32, "maxSideLen": 640,
        "modelsDir": modelPath,
        "keysName": "keys.txt",
        "detName": detName,
        "recName": recName
    }
    let inited = ocrtest.initOcr(paddleNcnnOcrMap1)
    if (!inited) {
        loge("inited ocr error : " + ocrtest.getErrorMsg())
        return
    } else {
        logd("ocr inited ok")
    }
    // May stop first or not
    // image.stopShortcutHttpServer();
    // Start once per run, or from app settings
    let b = image.startShortcutHttpServer(0)
    if (!b) {
        logw("failed to start Shortcuts receiver {}", image.shortcutHttpServerLastError())
        return;
    }
    image.useOpencvMat(1)
    logd("Start capture; bind Shortcuts hotkey first — see docs")
    for (let i = 0; i < 100; i++) {
        sleep(1000)
        logd("capture #" + (i + 1))
        // Test: Shortcuts bound to gui+A, send then fetch
        //logd(otgEvent.keyPressChar("gui", "A"))
        let img = image.getShortcutImage(10 * 1000);
        if (img) {
            logd("Shortcuts capture succeeded " + img)
            logd("image width and height {},{}", img.getWidth(), img.getHeight())
            let ss = file.getSandBoxFilePath("a.png");
            img.saveTo(ss)
            logd("image saved to: " + ss)
        } else {
            continue
        }


        console.time(1)

        // Dynamic params can be set here
        let ocrResult = ocrtest.ocrImage(img, 20000, {"numThread": 2, "padding": 32})

        logd("ocrResult " + JSON.stringify(ocrResult));
        if (ocrResult) {
            logd("OCR result-》 " + JSON.stringify(ocrResult));
            for (var j = 0; j < ocrResult.length; j++) {
                var value = ocrResult[j];
                logd("text : " + value.label + " " + value.x + "," + value.y + "," + (value.x + value.width) + "," + (value.y + value.height));
            }
        } else {
            logw("no result recognized");
        }
        logd("elapsed: {} ms", console.timeEnd(1))
        sleep(100)
        image.recycle(img)
        sleep(200)
    }
    // Release when script ends; not needed after every use
    ocrtest.releaseAll()
}


test_shortcut_screen();

```

### image.startShortcutHttpServer Start Shortcuts Receiver Service
* Start Shortcuts receiver service
* Receives image data sent from Shortcuts
* Can start from app settings or via this script function
* Requires EC standalone 6.6.0+
* @param port Listen port 1–65535; 0=default
* @return `{boolean}` true if start succeeded
```javascript
// Example: see image.getShortcutImage
```


### image.stopShortcutHttpServer Stop Shortcuts Receiver Service

* Stop Shortcuts screenshot receiver service
* Requires EC standalone 6.6.0+
* @return `{boolean}` true if start succeeded
```javascript
// Example: see image.getShortcutImage
```


### image.shortcutHttpServerBaseUrl Get Receiver Service URL

* Get receiver service URL
* Usually not needed
* Requires EC standalone 6.6.0+
* @return `{string}` Service URL
```javascript
// Example: see image.getShortcutImage
```


### image.shortcutHttpServerLastError Get Capture Service Error

* Get capture service error message
* Latest error from Shortcuts capture receiver service
* (e.g. start failure reason)
* Requires EC standalone 6.6.0+
* @return `{string}` null or "" = no error
```javascript
// Example: see image.getShortcutImage
```


## Screen Recording Capture
:::tip
- Screen recording capture requires standalone service; see [OTG-HID recording setup](/iostjdocs/advance/tj-otg-starter#系统录屏截图)
- You can configure screen recording capture without OTG HID; setup is the same
- In IDE, enable capture via **screen recording mode (phone recording)** before use
:::
### image.getSystemScreenCapImage Get Screen Recording Capture
* Get system screen recording capture
* If phone is landscape but capture is not, use rotate functions
* Requires EC standalone 6.6.0+
* @param timeout Timeout in ms
* @return `{AutoImage|null}` null means capture failed
```javascript
function check_record_img_ok() {
    let picNum = 0;
    let ok = false;
    for (let i = 0; i < 5; i++) {
        let img = image.getSystemScreenCapImage(1000);
        if (img) {
            picNum++;
            logd("received recording data {},{}", img.getWidth(), img.getHeight())
            img.recycle();
            // Success if more than 2 frames received
            if (picNum > 2) {
                ok = true;
                break
            }
        }
    }
    if (ok) {
        return true;
    }
    return false;
}

function check_record_ok() {
    // Start once per run, or from app settings
    let b = image.startSystemScreenCapServer()
    if (!b) {
        logw("failed to start recording receiver")
        return;
    }
    // Set frame rate and quality before starting recording
    // Or start system recording from app settings
    image.setSystemScreenCapFps(10)
    image.setSystemScreenCapJpegQuality(80)
    
    if (image.isSystemScreenCapBroadcasting()) {
        logd("recording service started, checking for frames")
        if (check_record_img_ok()) {
            return true
        }
        if (!image.startSystemScreenCapBroadcast()) {
            logw("failed to start recording broadcast: " + image.systemScreenCapLastError())
            return false;
        }
        logd("waiting for user confirmation")
        sleep(5000)
        return check_record_img_ok();
    } else {
        if (!image.startSystemScreenCapBroadcast()) {
            logw("failed to start recording broadcast: " + image.systemScreenCapLastError())
            return false;
        }
        logd("waiting for user confirmation")
        sleep(5000)
        return check_record_img_ok();

    }
}

function test_system_screenrecord() {

    if (!check_record_ok()) {
        logw("cannot get recording data; check app Settings → System Screen Recording")
        return;
    }

    logd("start fetching data")
    for (let i = 0; i < 10; i++) {
        sleep(1000)
        logd("capture #" + (i + 1))
        let img = image.getSystemScreenCapImage(10 * 1000);
        if (img) {
            logd("recording data " + img)
            logd("image width and height {},{}", img.getWidth(), img.getHeight())
            let ss = file.getSandBoxFilePath("a.png");
            img.saveTo(ss)
            logd("image saved to: " + ss)
            img.recycle();
        }

    }
}

test_system_screenrecord();
```


### image.startSystemScreenCapServer Start Screen Recording Receiver

* Start screen recording receiver service
* Start this first to receive frames; or from app Settings → System Screen Recording
* Requires EC standalone 6.6.0+
* @return `{boolean}` true on success, false on failure
```javascript
// Example: see image.getSystemScreenCapImage
```



### image.stopSystemScreenCapServer Start Screen Recording Receiver

* Stop screen recording receiver service
* Requires EC standalone 6.6.0+
* @return `{boolean}` true on success, false on failure
```javascript
// Example: see image.getSystemScreenCapImage
```


### image.startSystemScreenCapBroadcast Start System Screen Recording
* Start system screen recording
* Requires EC standalone 6.6.0+
* App must be foreground, or start from Settings → System Screen Recording (pick ScreenCapture, tap Start Broadcast)
* (Triggers system Broadcast Picker; user must confirm/tap)
* @return `{boolean}` true on success, false on failure
```javascript
// Example: see image.getSystemScreenCapImage
```


### image.isSystemScreenCapBroadcasting Is Screen Recording Running
* Whether screen recording is running
* Requires EC standalone 6.6.0+
* May be inaccurate; demo code also checks image data
* @return `{boolean}` true if recording starting, false if not
```javascript
// Example: see image.getSystemScreenCapImage
```


### image.setSystemScreenCapFps Set System Screen Recording Frame Rate
* Set system screen recording frame rate
* (fps, frames per second), e.g. 10 = 10 frames/sec.
* Do not set too high
* Note: reads config on each broadcast start
* Requires EC standalone 6.6.0+
* @param fps Frame rate 1–60
* @return `{boolean}` true on success, false on failure
```javascript
// Example: see image.getSystemScreenCapImage
```

### image.setSystemScreenCapJpegQuality Set Screen Recording Image Quality
* Set screen recording image quality
* JPEG quality (1–100)
* Requires EC standalone 6.6.0+
* @param q Quality 1–100; higher=clearer, more resources
* @return `{boolean}` true on success, false on failure
```javascript
// Example: see image.getSystemScreenCapImage
```


### image.systemScreenCapLastError Get Screen Recording Error
* Get screen recording error message
* Requires EC standalone 6.6.0+
* @return `{string}` null/empty = no error; else error message
```javascript
// Example: see image.getSystemScreenCapImage
```


## Color Comparison

### image.cmpColor Multi-Point Color Comparison

* Single- or multi-point color comparison; returns true if all points match, else false
* @param image1 Image
* @param points String like 6|1|0x969696-0x000010,1|12|0x969696,-4|0|0x969696
* @param threshold Color similarity 0.0–1.0 for find-color
* @param x Region start X; 0 = full screen
* @param y Region start Y; 0 = full screen
* @param ex End X; 0 = full screen
* @param ey End Y; 0 = full screen
* @return boolean true if found, false if not

```javascript showLineNumbers
function main() {
    let req = startEnv();
    if (!req) {
        logd("permission request failed");
        return;
    }
    let aimage = image.captureFullScreen();
    if (aimage != null) {
        let points3 = "205|1130|0xff944b-0x101010,211|1158|0xff8e42,191|1175|0xfcfbf7";
        let points = image.cmpColor(aimage, points3, 0.9, 0, 0, 0, 0);
        logd("points " + points);
        // Recycle image to free memory
        image.recycle(aimage)
    }

}

main();
```

### image.cmpColorJ Multi-Point Color Comparison (JSON)

* Single- or multi-point color comparison; returns true if all points match, else false
* @param image1 Image
* @param jsonFileName JSON from color tool in res folder, e.g. a.json
* @return `{boolean}` true if found

```javascript showLineNumbers
function main() {
    let req = startEnv();
    if (!req) {
        logd("permission request failed");
        return;
    }
    let aimage = image.captureFullScreen();
    if (aimage != null) {
        let points = image.cmpColorJ(aimage, "a.json");
        logd("points " + points);
        // Recycle image to free memory
        image.recycle(aimage)
    }
}

main();
```

### image.cmpColorEx Multi-Point Color Comparison (Extended)

* Single- or multi-point color comparison with auto screenshot; returns true if all match, else false
* @param points String like 6|1|0x969696-0x000010,1|12|0x969696,-4|0|0x969696
* @param threshold Color similarity 0.0–1.0 for find-color
* @param x Region start X; 0 = full screen
* @param y Region start Y; 0 = full screen
* @param ex End X; 0 = full screen
* @param ey End Y; 0 = full screen
* @return boolean true if found, false if not

```javascript showLineNumbers
function main() {
    let req = startEnv();
    if (!req) {
        logd("permission request failed");
        return;
    }
    let points3 = "205|1130|0xff944b-0x101010,211|1158|0xff8e42,191|1175|0xfcfbf7";
    let points = image.cmpColorEx(points3, 0.9, 0, 0, 0, 0);
    logd("points " + points);

}

main();
```

### image.cmpMultiColor Multi-Group Color Comparison

* Multi-point or multi-group color comparison; returns index of first matching group, or -1 if none
* @param image1 Image
* @param points Array like ```["6|1|0x969696-0x000010,1|12|0x969696,-4|0|0x969696","6|1|0x969696"]```
* @param threshold Color similarity 0.0–1.0 for find-color
* @param x Region start X; 0 = full screen
* @param y Region start Y; 0 = full screen
* @param ex End X; 0 = full screen
* @param ey End Y; 0 = full screen
* @return integer index of matched points, or -1 if none

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 let aimage = image.captureFullScreen();
 if (aimage != null) {
 let points1 = "205|112230|0xff944b,211|1158|0xff8e42,191|1175|0xfcfbf7";
 let points2 = "205|113022|0xff944b,211|1158|0xff8e42,191|1175|0xfcfbf7";
 let points3 = "205|1130|0xff944b,211|1158|0xff8e42,191|1175|0xfcfbf7";
 let points = image.cmpMultiColor(aimage, [points1, points2, points3], 0.9, 0, 0, 0, 0);
 logd("points " + points);
 // Recycle image to free memory
 image.recycle(aimage)
 }
}

main();
```

### image.cmpMultiColorJ Multi-Group Color Comparison (JSON)

* Multi-point or multi-group color comparison; returns index of first matching group, or -1 if none
* Runtime: unrestricted
* @param image1 Image
* @param jsonFileName JSON from color tool in res folder, e.g. a.json
* @return `{boolean}` true if found

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 let aimage = image.captureFullScreen();
 if (aimage != null) {
 let points = image.cmpMultiColorJ(aimage, "a.json");
 logd("points " + points);
 // Recycle image to free memory
 image.recycle(aimage)
 }
}

main();
```

### image.cmpMultiColorEx Multi-Group Color Comparison (Extended)

* Multi-point color comparison with auto screenshot; returns index of first match, or -1 if none
* @param points Array like ```["6|1|0x969696-0x000010,1|12|0x969696,-4|0|0x969696","6|1|0x969696"]```
* @param threshold Color similarity 0.0–1.0 for find-color
* @param x Region start X; 0 = full screen
* @param y Region start Y; 0 = full screen
* @param ex End X; 0 = full screen
* @param ey End Y; 0 = full screen
* @return integer index of matched points, or -1 if none

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 let points1 = "205|112230|0xff944b,211|1158|0xff8e42,191|1175|0xfcfbf7";
 let points2 = "205|113022|0xff944b,211|1158|0xff8e42,191|1175|0xfcfbf7";
 let points3 = "205|1130|0xff944b,211|1158|0xff8e42,191|1175|0xfcfbf7";
 let points = image.cmpMultiColorEx([points1, points2, points3], 0.9, 0, 0, 0, 0);
 logd("points " + points);
}

main();
```

## Find Color

### image.findColor Single-Point Find Color

* Find a pixel in the image whose color exactly equals color; returns coordinates or null
* @param image Image
* @param color Target colors like 0xCDD7E9-0x101010
* @param threshold Color similarity 0.0–1.0 for find-color
* @param x Region start X
* @param y Region start Y
* @param ex End X
* @param ey End Y
* @param limit Result limit
* @param orz Direction 1–8
* @return Array of Point coordinates or null

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 let aimage = image.captureFullScreen();
 if (aimage != null) {
 let points = image.findColor(aimage, "0xCDD7E9-0x101010,0xCDD7E9-0x101010", 0.9, 0, 0, 0, 0, 10, 1);
 logd("points " + JSON.stringify(points));
 // This is an array
 if (points) {
 for (let i = 0; i < points.length; i++) {
 logd(JSON.stringify(points[i]), points[i].x, points[i].y)
 // Click coordinates
 clickPoint(points[i].x, points[i].y)
 }
 }
 // Recycle image to free memory
 image.recycle(aimage)
 }

}

main();
```

### image.findColorJ Single-Point Find Color (JSON)

* Find pixel with exact color match; params from JSON; returns coordinates or null
* @param image Image
* @param jsonFileName JSON from color tool in res folder, e.g. a.json
* @return `{null|PointIndex[]}` point array or null

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 let aimage = image.captureFullScreen();
 if (aimage != null) {
 let points = image.findColorJ(aimage, "coin.json");
 logd("points " + JSON.stringify(points));
 // This is an array
 if (points && points.length > 0) {
 for (let i = 0; i < points.length; i++) {
 logd(JSON.stringify(points[i]), points[i].x, points[i].y)
 // Click coordinates
 clickPoint(points[i].x, points[i].y)
 }
 }
 // Recycle image to free memory
 image.recycle(aimage)
 }

}

main();
```

### image.findColorEx Auto Screenshot Single-Point Find Color

* Find pixel with exact color on current screen; returns coordinates or null
* @param color Color to find
* @param threshold Color similarity 0.0–1.0 for find-color
* @param x Region start X
* @param y Region start Y
* @param ex End X
* @param ey End Y
* @param limit Result limit
* @param orz Direction 1–8
* @return Array of Point coordinates or null

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 let points = image.findColorEx("0xCDD7E9-0x101010,0xCDD7E9-0x101010", 0.9, 0, 0, 0, 0, 10, 1);
 logd("points " + JSON.stringify(points));
 This is an array
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

### image.findMultiColor Multi-Point Find Color

* Multi-point find-color; returns all matching points (similar to Auto.js multi-color find)
* @param image Image to find color in
* @param firstColor First point color
* @param points String like 6|1|0x969696-0x000010,1|12|0x969696,-4|0|0x969696
* @param threshold Color similarity 0.0–1.0 for find-color
* @param x Region start X
* @param y Region start Y
* @param ex End X
* @param ey End Y
* @param limit Result limit
* @param orz Direction 1–8
* @return Array of Point coordinates or null

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }

 let aimage = image.captureFullScreen();
 if (aimage != null) {
 let points = image.findMultiColor(aimage, "0xDD7A5F-0x101010", "29|25|0xBB454B-0x101010,58|44|0xA6363A-0x101010", 0.9, 0, 0, 0, 0, 10, 1);
 logd("points " + JSON.stringify(points));
 // This is an array
 if (points && points.length > 0) {
 for (let i = 0; i < points.length; i++) {
 logd(points[i], points[i].x, points[i].y)
 // Click coordinates
 clickPoint(points[i].x, points[i].y)
 }
 }
 // Recycle image to free memory
 image.recycle(aimage)
 }

}

main();
```



### image.findMultiColorJ Multi-Point Find Color (JSON)

* Multi-point find-color; params read from JSON file (similar to Auto.js multi-color find)
* Returns null if no match in the entire image
* @param image1 Image to find color in
* @param jsonFileName JSON from color tool in res folder, e.g. a.json
* @return `{null|Point[]}` point array or null

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }

 let aimage = image.captureFullScreen();
 if (aimage != null) {
 let points = image.findMultiColorJ(aimage, "coin.json");
 logd("points " + JSON.stringify(points));
 // This is an array
 if (points && points.length > 0) {
 for (let i = 0; i < points.length; i++) {
 logd(points[i], points[i].x, points[i].y)
 // Click coordinates
 clickPoint(points[i].x, points[i].y)
 }
 }
 // Recycle image to free memory
 image.recycle(aimage)
 }

}

main();
```




### image.findMultiColorEx Auto Screenshot Multi-Point Find Color

* Multi-point find-color; returns all matching points (similar to Auto.js multi-color find)
* @param firstColor First point color
* @param points String like 6|1|0x969696-0x000010,1|12|0x969696,-4|0|0x969696
* @param threshold Color similarity 0.0–1.0 for find-color
* @param x Region start X
* @param y Region start Y
* @param ex End X
* @param ey End Y
* @param limit Result limit
* @param orz Direction 1–8
* @return Array of Point coordinates or null

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }

 let points = image.findMultiColorEx("0xDD7A5F-0x101010", "29|25|0xBB454B-0x101010,58|44|0xA6363A-0x101010", 0.9, 0, 0, 0, 0, 10, 1);
 logd("points " + JSON.stringify(points));
 This is an array
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

## Find Non-Color

### image.findNotColor Find Non-Color

* Find a pixel whose color is not equal to color; returns coordinates or null
* Requires EC standalone 3.10.0+
* @param image Image
* @param color Target colors like 0xCDD7E9-0x101010; generated by EC color tool
* @param threshold Color similarity 0.0–1.0 for find-color
* @param x Region start X
* @param y Region start Y
* @param ex End X
* @param ey End Y
* @param limit Result limit
* @param orz Direction 1–8
* @return PointIndex array or null

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }

 let aimage = image.captureFullScreen();
 if (aimage != null) {
 let points = image.findNotColor(aimage, "0xCDD7E9-0x101010,0xCDD7E9-0x101010", 0.9, 0, 0, 0, 0, 10, 1);
 logd("points " + JSON.stringify(points));
 // This is an array
 if (points) {
 for (let i = 0; i < points.length; i++) {
 logd(JSON.stringify(points[i]), points[i].x, points[i].y)
 // Click coordinates
 clickPoint(points[i].x, points[i].y)
 }
 }
 // Recycle image to free memory
 image.recycle(aimage)
 }

}

main();
```

### image.findNotColorJ Find Non-Color (JSON)

* Find a pixel whose color is not equal to color; returns coordinates or null
* @param image1 Image
* @param jsonFileName JSON from color tool in res folder, e.g. a.json
* @return `{null|PointIndex[]}` PointIndex array or null

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }

 let aimage = image.captureFullScreen();
 if (aimage != null) {
 let points = image.findNotColorJ(aimage, "a.json");
 logd("points " + JSON.stringify(points));
 // This is an array
 if (points) {
 for (let i = 0; i < points.length; i++) {
 logd(JSON.stringify(points[i]), points[i].x, points[i].y)
 // Click coordinates
 clickPoint(points[i].x, points[i].y)
 }
 }
 // Recycle image to free memory
 image.recycle(aimage)
 }

}

main();
```

## Find Image

### image.findImageByColor Transparent Find Image

* Transparent find-image (no OpenCV init required)
* @param image Large image
* @param template Template (small) image
* @param x Find-image region start X
* @param y Find-image region start Y
* @param ex End X
* @param ey End Y
* @param threshold Image similarity 0–1; default 0.9
* @param limit Result limit; 1 for single match
* @return Array of Point coordinates or null

```javascript showLineNumbers
function main() {
 Read sms.png from project res folder
 let sms = readResAutoImage("sms.png");
 Capture screen
 let aimage = image.captureFullScreen();
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
 // Recycle image to free memory
 image.recycle(aimage)
 }
 // Recycle image to free memory
 image.recycle(sms)
}

main();
```




### image.findImageByColorJ Transparent Find Image (JSON)

* Find image by color; supports transparent images; no OpenCV required
* Returns null if no match in the entire image
* @param image1 Large image
* @param jsonFileName JSON from color tool in res, e.g. a.json; configure template path in JSON
* @return `{null|Point[]}` point array or null

```javascript showLineNumbers
function main() {
 Capture screen
 let aimage = image.captureFullScreen();
 if (aimage != null) {
 // Search in image
 let points = image.findImageByColorJ(aimage, "a.json");
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
 image.recycle(aimage)
 }
}

main();
```


### image.findImageByColorEx Transparent Find Image (Extended)

* Find image by color; supports transparent images; no OpenCV required
* Returns null if no match in the entire image
* @param image1 Large image
* @param template Template (small) image
* @param x Find-image region start X
* @param y Find-image region start Y
* @param ex End X
* @param ey End Y
* @param limit Result limit; 1 for single match
* @param extra Extended function; map e.g.
*

```{"firstColorOffset":"#101010","firstColorThreshold":1.0,"otherColorOffset":"#101010","otherColorThreshold":0.9,"cmpColorSucThreshold":1.0}```

* firstColorOffset: Color offset for first match, e.g. #101010
* firstColorThreshold: Threshold for first color offset, e.g. 0.9
* otherColorOffset: Color offset for remaining colors, e.g. #101010
* otherColorThreshold: Threshold for remaining color offsets, e.g. 0.9
* cmpColorSucThreshold: Fraction of colors that must match, e.g. 0.9 = 90% of points
* startX: Start X for first search point
* startY: Start Y for first search point
* @return Array of Point coordinates or null

```javascript showLineNumbers
function main() {

 let d = startEnv();
 logd("start service--{}", d)
 let smallTmplate = readResAutoImage("tmp4.png");

 for (let i = 0; i < 100; i++) {
 sleep(1000)
 let img = image.captureFullScreen();
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
 let points = image.findImageByColorEx(img, smallTmplate, 0, 0, 0, 0, 100, extra);
 logd("time-{}", console.timeEnd(1))
 // This is an array
 if (points) {
 logd("points " + JSON.stringify(points));
 }

 image.recycle(img)

 }

 image.recycle(smallTmplate)
}
main()
```


### image.findImageByColorExJ Transparent Find Image (Extended) (JSON)
* Find image by color; supports transparent images; no OpenCV required
* Returns null if no match in the entire image
* @param image1 Large image
* @param jsonFileName JSON from color tool in res, e.g. a.json; configure template path in JSON
* @return `{null|Point[]}` point array or null

```javascript showLineNumbers
function main() {
 Capture screen
 let aimage = image.captureFullScreen();
 logd("aimage " + aimage);
 if (aimage != null) {
 // Search in image
 let points = image.findImageByColorExJ(aimage, "a.json");
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
 image.recycle(aimage)
 }
}

main();
```


### image.findImage OpenCV Find Image

* Find image: locate template in large image (template matching); returns Rect region or null if not found.
* EC standalone 4.5.0+
* @param image1 Large image
* @param template Template (small) image
* @param x Find-image region start X
* @param y Find-image region start Y
* @param ex End X
* @param ey End Y
* @param weakThreshold Image similarity 0–1; default 0.9
* @param threshold Image similarity 0–1; default 0.9
* @param limit Result limit; 1 for single match
* @param method 0: TM_SQDIFF, 1: TM_SQDIFF_NORMED, 2: TM_CCORR, 3:
  TM_CCORR_NORMED, 4: TM_CCOEFF, 5: TM_CCOEFF_NORMED
* @return `Rect` region array or null



```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 Read sms.png from project res folder
 let sms = readResAutoImage("sms.png");
 Capture screen
 let aimage = image.captureFullScreen();
 logd("aimage " + aimage);
 if (aimage != null) {
 // Search in image
 let points = image.findImage(aimage, sms, 0, 0, 0, 0, 0.7, 0.9, 1, 5);
 logd("points " + JSON.stringify(points));
 // This is an array
 if (points && points.length > 0) {
 for (let i = 0; i < points.length; i++) {
 logd(points[i])
 let x = parseInt((points[i].left + points[i].right) / 2)
 let y = parseInt((points[i].top + points[i].bottom) / 2)
 // Click coordinates
 clickPoint(x, y)
 }
 }
 // Recycle image to free memory
 image.recycle(aimage)
 }
 // Recycle image to free memory
 image.recycle(sms)
}

main();
```





### image.findImageJ Find Image (JSON)

* Find image: locate template in large image (template matching); returns Rect region or null if not found.
* @param image1 Large image
* @param jsonFileName JSON from color tool in res, e.g. a.json; configure template path in JSON
* @return `{null|Rect[]}` region array or null

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 Capture screen
 let aimage = image.captureFullScreen();
 logd("aimage " + aimage);
 if (aimage != null) {
 // Search in image
 let points = image.findImageJ(aimage, "a.json");
 logd("points " + JSON.stringify(points));
 // This is an array
 if (points && points.length > 0) {
 for (let i = 0; i < points.length; i++) {
 logd(points[i])
 logd("similarity: " + points[i]['similarity'])
 let x = parseInt((points[i].left + points[i].right) / 2)
 let y = parseInt((points[i].top + points[i].bottom) / 2)
 // Click coordinates
 clickPoint(x, y)
 }
 }
 // Recycle image to free memory
 image.recycle(aimage)
 }
}

main();
```



### image.findImageEx OpenCV Auto Screenshot Find Image

* Find image: locate template on current screen (template matching); returns Rect region or null if not found.
* EC standalone 4.5.0+
* @param template Template (small) image
* @param x Find-image region start X
* @param y Find-image region start Y
* @param ex End X
* @param ey End Y
* @param weakThreshold Image similarity 0–1; default 0.9
* @param threshold Image similarity 0–1; default 0.9
* @param limit Result limit; 1 for single match
* @param method 0: TM_SQDIFF, 1: TM_SQDIFF_NORMED, 2: TM_CCORR, 3:
  TM_CCORR_NORMED, 4: TM_CCOEFF, 5: TM_CCOEFF_NORMED
* @return `Rect` region array or null

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 Read sms.png from project res folder
 let sms = readResAutoImage("sms.png");
 Search current screen, limit to one match
 let points = image.findImageEx(sms, 0, 0, 0, 0, 0.7, 0.9, 1, 5);
 logd("points " + JSON.stringify(points));
 This is an array
 if (points && points.length > 0) {
 for (let i = 0; i < points.length; i++) {
 logd(points[i])
 let x = parseInt((points[i].left + points[i].right) / 2)
 let y = parseInt((points[i].top + points[i].bottom) / 2)
 // Click coordinates
 clickPoint(x, y)
 }
 }
 // Recycle image to free memory
 image.recycle(sms)
}

main();
```

### image.matchTemplate OpenCV Image Template Matching

* OpenCV template matching wrapper
* EC standalone 4.5.0+
* @param image1 Large image
* @param template Template (small) image
* @param weakThreshold Image similarity 0–1; default 0.9
* @param threshold Image similarity 0–1; default 0.9
* @param rect Find-image region; see findColor rect docs
* @param maxLevel Default -1; usually unchanged. Auto-adjusts by image size. Uses image pyramid.
  level = pyramid depth,
* Higher level may improve speed but can cause misses (over-shrunk image) or wrong positions. Tune only if you understand it.
* @param limit Result limit; 1 for single match
* @param method 0: TM_SQDIFF, 1: TM_SQDIFF_NORMED, 2: TM_CCORR, 3:
  TM_CCORR_NORMED, 4: TM_CCOEFF, 5: TM_CCOEFF_NORMED
* @return `Match` collection of matches

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 let aimage = image.captureFullScreen();
 if (aimage != null) {
 let temp = readResAutoImage("tmp.png");
 let rectp = new Rect();
 rectp.left = 0;
 rectp.top = 0;
 rectp.right = 0
 rectp.bottom = 0
 let matchs = image.matchTemplate(aimage, temp, 0.7, 0.9, rectp, -1, 10, 5);
 // This is an array
 logd(JSON.stringify(matchs));
 // This is an array
 if (matchs) {
 for (let i = 0; i < matchs.length; i++) {
 logd(JSON.stringify(matchs[i].point));
 }
 }
 // Recycle image to free memory
 image.recycle(aimage)
 // Recycle image to free memory
 image.recycle(temp)
 }
}

main();
```




### image.matchTemplateJ Image Template Matching (JSON)

* OpenCV template matching wrapper
* @param image1 Large image
* @param jsonFileName JSON from color tool in res, e.g. a.json; configure template path in JSON
* @return `{null|Match[]}` matches

```javascript showLineNumbers
function main() {
 let aimage = image.captureFullScreen();
 if (aimage != null) {
 let matchs = image.matchTemplateJ(aimage, "a.json");
 // This is an array
 logd(JSON.stringify(matchs));
 // This is an array
 if (matchs && matchs.length > 0) {
 for (let i = 0; i < matchs.length; i++) {
 logd(JSON.stringify(matchs[i].point));
 clickPoint(matchs[i].point.x, matchs[i].point.y)
 }
 }
 // Recycle image to free memory
 image.recycle(aimage)
 }
}

main();
```



### image.matchTemplateEx OpenCV Image Template Matching

* OpenCV template matching on current screen capture
* EC standalone 4.5.0+
* @param template Template (small) image
* @param weakThreshold Image similarity 0–1; default 0.9
* @param threshold Image similarity 0–1; default 0.9
* @param rect Find-image region; see findColor rect docs
* @param maxLevel Default -1; usually unchanged. Auto-adjusts by image size. Uses image pyramid.
  level = pyramid depth,
* Higher level may improve speed but can cause misses (over-shrunk image) or wrong positions. Tune only if you understand it.
* @param limit Result limit; 1 for single match
* @param method 0: TM_SQDIFF, 1: TM_SQDIFF_NORMED, 2: TM_CCORR, 3:
  TM_CCORR_NORMED, 4: TM_CCOEFF, 5: TM_CCOEFF_NORMED
* @return `Match` collection of matches

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 let temp = readResAutoImage("tmp.png");
 let rectp = new Rect();
 rectp.left = 0;
 rectp.top = 0;
 rectp.right = 0;
 rectp.bottom = 0;
 let matchs = image.matchTemplateEx(temp, 0.7, 0.9, rectp, -1, 1, 5);
 logd(JSON.stringify(matchs));
 This is an array
 if (matchs) {
 for (let i = 0; i < matchs.length; i++) {
 logd(JSON.stringify(matchs[i].point));
 }
 }
 // Recycle image to free memory
 image.recycle(aimage)
 // Recycle image to free memory
 image.recycle(temp)
}

main();
```

## Grayscale
### image.gray Grayscale Image

* Grayscale image via OpenCV
* EC standalone 5.11.0+
* @param img AutoImage object
* @return `{null|AutoImage}`

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 for (let i = 0; i < 1; i++) {
 sleep(1000);
 let s = new Date().getTime();
 let d = image.captureFullScreenEx();
 if (d) {
 let p = file.getSandBoxFilePath("test1.png")
 let saved = image.saveTo(d, p);
 let s = new Date().getTime();
 let bd = image.gray(d);
 logd("time " + (new Date().getTime() - s))
 logd(bd.uuid);
 if (bd) {
 let p2 = file.getSandBoxFilePath("test2.png")
 let saved = image.saveTo(bd, p2);
 logd("saved " + saved)
 exit()
 }
 // Recycle image to free memory
 image.recycle(d)
 }
 }

}

main();
```

## Binarization

### image.binaryzation Binarize Image

* Binarize AutoImage
* @param img AutoImage object
* @param threshold Binarization coefficient, 0–255
* @return AutoImage object or null

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 for (let i = 0; i < 1000; i++) {
 sleep(1000);
 let s = new Date().getTime();
 let d = image.captureFullScreen();
 if (d) {
 let s = new Date().getTime();
 let bd = image.binaryzation(d, 200);
 logd("time " + (new Date().getTime() - s))
 logd(bd.uuid);
 // Recycle image to free memory
 image.recycle(d)
 }
 }

}

main();
```

### image.binaryzationEx Binarize Image

* Adaptive binarization via OpenCV adaptiveThreshold
* EC standalone 4.5.0+
* @param img AutoImage object
* @param map Map parameters
    * diameter: Denoise diameter; see OpenCV bilateralFilter
    * adaptiveMethod: 0=ADAPTIVE_THRESH_MEAN_C, 1=ADAPTIVE_THRESH_GAUSSIAN_C
    * blockSize: Neighborhood block size in pixels; use odd values like 3, 5, 7
    * c: Constant offset adjustment
    * ```{"diameter":0,"adaptiveMethod":1,"c":0,"blockSize":5}```
* @return `{null|AutoImage}`

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 for (let i = 0; i < 1; i++) {
 sleep(1000);
 let s = new Date().getTime();
 let d = image.captureFullScreenEx();
 if (d) {
 let p = file.getSandBoxFilePath("test1.png")
 let saved = image.saveTo(d, p);
 let s = new Date().getTime();
 let bd = image.binaryzationEx(d, {
 "diameter": 0,
 "adaptiveMethod": 1,
 "c": 10, "blockSize": 7
 });
 logd("time " + (new Date().getTime() - s))
 logd(bd.uuid);
 if (bd) {
 let p2 = file.getSandBoxFilePath("test2.png")
 let saved = image.saveTo(bd, p2);
 logd("saved " + saved)
 exit()
 }
 // Recycle image to free memory
 image.recycle(d)
 }
 }

}

main();
```

## Other

### image.rotateImage Rotate Image

* Rotate image
* Requires EC standalone 1.6.0+
* @param img Image object
* @param degree Degrees: 0=portrait (home bottom), -90/90 as above
* @return `{null|AutoImage}`

```javascript showLineNumbers
function main() {
 let img = image.captureFullScreen()
 logd(" img width " + image.getWidth(img))
 let img2 = image.rotateImage(img, -90);
 image.recycle(img)
 logd(" img2 width " + image.getWidth(img2))
 image.recycle(img2)
}

main();
```

### image.readImage Read File as Image

* Read image at path and return ```{@link AutoImage}```, or null if missing or undecodable
* @param path Image path
* @return AutoImage object or null

```javascript showLineNumbers
function main() {
 let path = file.getSandBoxFilePath("a.png")
 let autoimg = image.readImage(path);
 // Recycle image to free memory
 image.recycle(autoimg)
}

main();
```

### image.argb Color to Hex String

* Convert integer color value to hex RGB string
* @param color Integer value
* @return `{string}` Color string

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 let aimage = image.captureFullScreen();
 if (aimage != null) {
 let points3 = "765|22|0x1296DB";
 logd("==" + image.argb(image.pixel(aimage, 765, 22)));
 let points = image.cmpColor(aimage, points3, 0.5, 0, 0, 0, 0);
 logd("points " + points);
 // Recycle image to free memory
 image.recycle(aimage)
 }
}

main();
```

## Image Conversion

### image.saveTo Save to File

* Save to file
* @param img Image object
* @param path Path
* @return bool true on success, false on failure

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }

 let imageX = image.captureFullScreen();
 let path = file.getSandBoxFilePath("a.png")
 let r = image.saveTo(imageX, path);
 logd("result " + r);
 // Recycle image to free memory
 image.recycle(imageX)
}

main();
```


### image.base64ToImage Base64 to Image

* Convert base64 data to AutoImage
* @param base64data
* @returns `{AutoImage|null}`

```javascript showLineNumbers
function main() {
 let data = "iVBORw0KGgoAAAANSUhEUgAAAAwAAAAMCAYAAABWdVznAAAAAXNSR0IArs4c6QAAATFJREFUKFOVUrFKA1EQnAn5CMFGW8HKykYMqI0I7z3RKlrYCkGLYKMo2sVK8wEWdkJu76Wz8sRK8A8k2PkXL2s23B0ELHSrhd2Z2RmW+GfR9p1zKyTPRGS/wnvv9wCcAlgVkeme1bTx3r8AWFfVVp7nhXNugeQXyZ6qLonIzgygVLkieWlsRjABv05ICiMiuQjg0Ga1VKlkZzxZX50RQtgAsK2qJ78BngFsATgSkYeSpAPgDsCuiGS1gnOuTfIWwLENStOmVjQaje5gMPioTVsTQvhMKbVjjO/VsoVA0vz08zw3pTolBbDcbDZHKaVNVY0kH1X1oAxBSXayLOvXsYpIy3vfBdBLKc0Ph8PvEMKNqp5XoNp0ecI9gLnxeLwWY3yrcnfOXZO8MC9GOhPrX77kB5AYkg38B4vzAAAAAElFTkSuQmCC"
 let img = image.base64ToImage(data)
 logd("img "+img+" "+image.getHeight(img))
 let p = file.getSandBoxFilePath("test1.png")
 let saved = image.saveTo(img, p);
 logd("saved "+saved)
 image.recycle(img)
}

main();
```


### image.toBase64Format Image to Base64

* Convert to base64 string; JPG is smaller and uses less memory
* @param img Image object
* @param format Format: jpg or png
* @param q Quality 1–100; higher=clearer; png ignores
* @return string

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 let imageX = image.captureFullScreen();
 let r = image.toBase64Format(imageX, "jpg", 50);
 logd("result " + r);
 // Recycle image to free memory
 image.recycle(imageX)
}

main();
```

### image.clip Clip Image

* Clip image
* @param img Image object
* @param x Start X
* @param y Start Y
* @param ex End X
* @param ey End Y
* @return AutoImage object or null

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }

 let imageX = image.captureFullScreen();
 let r = image.clip(imageX, 100, 100, 300, 400);
 logd("result " + r);
 // Recycle image to free memory
 image.recycle(imageX)
 image.recycle(r)
}

main();
```

### image.pixel Pixel Color Value

* Get pixel color at a point in the image
* @param img Image object
* @param x X coordinate
* @param y Y coordinate
* @return int color value

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 let imageX = image.captureFullScreen();
 let r = image.pixel(imageX, 100, 100);
 logd("result " + r);
 // Recycle image to free memory
 image.recycle(imageX)
}

main();
```

### image.isRecycled Image Recycle Check

* Whether already recycled
* @param img Image object
* @return bool true if already recycled

```javascript showLineNumbers
function main() {
 let imageX = image.captureFullScreen();
 let r = image.isRecycled(imageX);
 logd("result " + r);
 // Recycle image to free memory
 image.recycle(imageX)
}

main();
```

### image.recycle Recycle Image

* Recycle image
* @param img Image object

```javascript showLineNumbers
function main() {
 let imageX = image.captureFullScreen();
 // Recycle image to free memory
 image.recycle(imageX)
}

main();
```

### image.recycleAllImage Recycle All Images

* Recycle all images
* Requires EC standalone 4.8.0+
* @return `bool` true on success

```javascript showLineNumbers
function main() {
 let imageX = image.captureFullScreen();
 image.recycleAllImage()
}

main();
```

### image.autoImageToUIImage Convert to UIImage

* Convert to UIImage
* Requires EC 4.2.0+
* @param img AutoImage
* @return Swift UIImage object or null

```javascript showLineNumbers
function main() {
 logd("isServiceOk " + isServiceOk());
 startEnv()
 logd("isServiceOk " + isServiceOk());
 setComputeMode(1)
 let img1 = image.captureFullScreen();
 logd(img1)

 // Test 1
 let uiimage = image.autoImageToUIImage(img1)
 logd("autoImageToUIImage uiimage " + uiimage)
 let x = pluginLoader.callMethodAny(name, clzName, "testMethod", uiimage)
 logd(x)
 image.recycle(img1)


 let img2 = image.captureFullScreenUIImage({})
 let xX = pluginLoader.callMethodAny(name, clzName, "testMethod", img2)
 logd(xX)


 let au = image.uiimageToAutoImage(uiimage)
 logd("au " + au)

 Save to file
 image.saveTo(au, file.getSandBoxFilePath("a.jpg"))
 image.recycle(au)
 image.recycle(img2)
}

main();
```

### image.uiimageToAutoImage Convert UIImage to AutoImage

* Convert UIImage to AutoImage
* Requires EC 4.2.0+
* @param uiimage Swift UIImage object
* @return AutoImage object or null

```javascript showLineNumbers
function main() {
 logd("isServiceOk " + isServiceOk());
 startEnv()
 logd("isServiceOk " + isServiceOk());
 setComputeMode(1)
 let img1 = image.captureFullScreen();
 logd(img1)

 // Test 1
 let uiimage = image.autoImageToUIImage(img1)
 logd("autoImageToUIImage uiimage " + uiimage)
 let x = pluginLoader.callMethodAny(name, clzName, "testMethod", uiimage)
 logd(x)
 image.recycle(img1)


 let img2 = image.captureFullScreenUIImage({})
 let xX = pluginLoader.callMethodAny(name, clzName, "testMethod", img2)
 logd(xX)


 let au = image.uiimageToAutoImage(uiimage)
 logd("au " + au)

 Save to file
 image.saveTo(au, file.getSandBoxFilePath("a.jpg"))
 image.recycle(au)
 image.recycle(img2)
}

main();
```

### image.imageToMatFormat (UIImage AutoImage to Mat AutoImage)

* Convert to Mat storage format
* Requires EC iOS 4.6.0+
* @param img `{AutoImage}` object
* @return AutoImage in MAT storage format or null

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 return;
 }
 image.useOpencvMat(0)
 for (let i = 0; i < 100; i++) {
 let d = image.captureFullScreen();
 logd(d)
 sleep(1000);
 if (d) {
 let ds = image.imageToMatFormat(d);
 logd(ds)
 image.recyle(d);
 }
 }
}

main();
```

### image.matToImageFormat (Mat AutoImage to UIImage AutoImage)

* Convert to normal image storage format
* Requires EC iOS 4.6.0+
* @param img `{AutoImage}` object
* @return AutoImage in normal storage format or null

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 return;
 }
 image.useOpencvMat(1)
 for (let i = 0; i < 100; i++) {
 let d = image.captureFullScreen();
 logd(d)
 sleep(1000);
 if (d) {
 let ds = image.matToImageFormat(d);
 logd(ds)
 image.recyle(d);
 }
 }
}

main();
```
