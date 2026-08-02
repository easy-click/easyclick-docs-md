---
title: Image & Color Functions
description: EasyClick automation scripts — iOS no jailbreak image and color functions
keywords:
 - EasyClick automation scripts iOS no jailbreak image and color functions resource download
 - image
 - param
 - useOpencvMat
 - setInitParam
 - mat
 - EC
 - iOS
 - return
 - startPreCapScreen
 - startScreenStream
 - EasyClick
 - mobile automation
 - automation testing
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---
## Overview

## Settings

### image.useOpencvMat Initialize Parameters

* Switch image storage to OpenCV mat format
* Requires EC iOS 7.11.0+
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

### image.setInitParam Initialize Parameters

* Set image module init parameters, including find-image/find-color timeout
* @param params Timeout in ms

```javascript showLineNumbers
function main() {
  // action_timeout: max find-image/find-color time in ms
  image.setInitParam({"action_timeout": 1000});
  image.setInitParam(
    {
      "action_timeout": 1000,
      "auto_click_request_dialog": false
    }
  );
}

main();
```

### image.setFindColorImageMode Set Find-Color Algorithm Mode

* Set find-color/find-image algorithm mode
* Requires EC iOS 6.11.0+
* @param type 1 = legacy algorithm, 2 = new algorithm
* @return boolean

```javascript showLineNumbers
function main() {

  image.setFindColorImageMode(1);

}

main();
```

## Stream Capture

### image.startPreCapScreen Pre-Capture Function

* Pre-capture function; background thread loops screenshots
* Can improve capture efficiency
* Requires EC iOS USB 6.18.0+
* With BLE events, automation without proxy IPA; set types to 10
* @param types:
* 10 = PNG format, no compression; works without automation
* 1 = JPG capture mode 1
* 2 = JPG capture mode 2
* 3 = PNG (no quality param); choose per device
* @param quality Image quality; type=1 supports 1, 50, 100
* When type=2, quality 1–100
* @param delay Interval between pre-capture loops in ms
* @return boolean true on success, false on failure

```javascript showLineNumbers
function main() {
  logd("isServiceOk " + isServiceOk());
  startEnv()
  logd("isServiceOk " + isServiceOk());

  let start = image.startPreCapScreen("1", 50, 20);
  logd("startPreCapScreen {}", start)
  for (let i = 0; i < 10; i++) {
    sleep(1000);
    console.time("1")
    let img = image.captureScreenStream()
    logd("img {} time: {}", img, console.timeEnd("1"))
    if (img) {
      image.saveTo(img, "a.png");
    }
    image.recycle(img)
  }
  image.stopScreenStream()

}

main();
```

### image.startScreenStream Start Stream Capture

* Init stream capture mode; faster than other methods
* @return boolean true on success, false on failure

```javascript showLineNumbers
function main() {
  logd("isServiceOk " + isServiceOk());
  startEnv()
  logd("isServiceOk " + isServiceOk());

  let cap = image.startScreenStream()
  logd("capture: " + cap)

}

main();
```

### image.stopScreenStream Stop Stream Capture

* Stop image stream
* @return boolean true on success, false on failure

```javascript showLineNumbers
function main() {
  logd("isServiceOk " + isServiceOk());
  startEnv()
  logd("isServiceOk " + isServiceOk());

  let cap = image.stopScreenStream()
  logd("capture: " + cap)

}

main();
```

### image.isScreenStreamOk Is Stream Capture Healthy

* Whether stream capture is healthy
* @return boolean true on success, false on failure

```javascript showLineNumbers
function testScreen() {
  let start = image.startScreenStream();
  logd("start {}", start)
  setAgentSetting({"screenStreamQuality": 100})

  thread.execAsync(function () {
    while (true) {
      sleep(2000)
      // When service is not running
      let serviceOk = isServiceOk();
      if (!serviceOk) {
        serviceOk = startEnv();
      }
      if (!serviceOk) {
        // Optionally reset USB connection
        resetUsbConn();
        continue
      }
      if (image.isScreenStreamOk()) {
        continue
      }
      // Check if capture succeeded
      logd("restart stream capture state")

      let start = image.startScreenStream()
      logd("startScreenStream :{}", start)
    }
  })


  while (true) {
    sleep(1000);
    console.time("1")
    let img = image.captureScreenStream()
    logd("img {} time: {}", img, console.timeEnd("1"))
    if (img) {
      image.saveTo(img, "a.png");
    }
    image.recycle(img)
  }

}


function main() {
  logd("isServiceOk " + isServiceOk());
  startEnv()
  // Guarded stream capture; can set mirroring quality
  testScreen()

}

main();
```

### image.captureScreenStream Image Stream

* Get one frame from image stream
* @return AutoImage object or null

```javascript showLineNumbers
function main() {
  logd("isServiceOk " + isServiceOk());
  startEnv()
  logd("isServiceOk " + isServiceOk());
  for (let i = 0; i < 10; i++) {
    let cap = image.captureScreenStream()
    logd("capture data: " + cap)
    sleep(1000)
    // Recycle image to free memory
    image.recycle(cap)
  }
}

main();
```

## Standard Capture (JPG)

### image.captureFullScreenEx Capture Full Screen as Image Object

* Capture full screen as JPG
* Requires EC iOS 3.1.0+
* @param ext Extended params to adjust capture mode and quality
* type: 1=JPG mode 1, 2=JPG mode 2, 3=PNG (no quality); choose per device
* quality: Image quality; type=1 supports 1, 50, 100
* When type=2, quality 1–100
* @return AutoImage object or null

```javascript showLineNumbers
function main() {
  logd("isServiceOk " + isServiceOk());
  startEnv()
  logd("isServiceOk " + isServiceOk());
  for (let i = 0; i < 10; i++) {
    console.time(1)
    let cap = image.captureFullScreenEx({"type": "1", "quality": 50})
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

## Standard Capture (PNG)

### image.captureFullScreenPng Capture Full Screen as Image Object

* Capture current screen as Image (PNG format).
* @return AutoImage object or null

```javascript showLineNumbers
function main() {
  logd("isServiceOk " + isServiceOk());
  startEnv()
  logd("isServiceOk " + isServiceOk());
  for (let i = 0; i < 10; i++) {
    let cap = image.captureFullScreenPng()
    logd("capture data: " + cap)
    sleep(1000)
    // Recycle image to free memory
    image.recycle(cap)
  }
}

main();
```


### image.resetCaptureScreenNoAutoEnv Reset No-Permission Capture Environment

* Reset no-permission capture environment
* On iOS 17+, call this first to start the tunnel
* Closes prior tunnel and opens new one; no-op on iOS &lt;17
* Requires EC iOS USB 9.21.0+
* @returns `{boolean}` true on success, false on failure

```javascript showLineNumbers
// See image.captureFullScreenNoAuto example
```

### image.captureFullScreenNoAuto Screen Capture Without Automation

* Screen capture without automation
* Slower; for speed see [startPreCapScreen]
* With BLE event module, automation without proxy IPA install
* Requires EC iOS USB 6.25.0+
* @return AutoImage object or null

```javascript showLineNumbers
let tocr = null;

function ocrimage(img) {
    if (tocr == null) {
        let paddleOcrOn = {
            "type": "paddleOcrNcnnV5",
            "numThread": 1,
            "single": 0,
            "matMode": 1
        }
        // Release first to avoid prior resource leaks
        ocr.releaseAll()
        tocr = ocr.newOcr()
        let inited = tocr.initOcr(paddleOcrOn)
        logd("init result -" + inited);
        if (!inited) {
            loge("error : " + tocr.getErrorMsg());
            return;
        }
    }
    let result = tocr.ocrImage(img, 30 * 1000, {"padding": 32, "numThread": 1});
    logd("ocr result: " + JSON.stringify(result))

}


function testCap() {
    image.useOpencvMat(1)
    let tt = 0;
    let timex = 0;
    logd("start resetCaptureScreenNoAutoEnv")
    image.resetCaptureScreenNoAutoEnv();
    logd("end resetCaptureScreenNoAutoEnv")
    while (!isScriptExit()) {
        let sp = utils.getRangeInt(200, 500)
        timex = timex + sp
        sleep(sp)
        tt++;
        logd("call captureFullScreenNoAuto " )
        console.time(1)
        let img = image.captureFullScreenNoAuto();
        logd("img {}", img +" elapsed: "+console.timeEnd(1))
        if (img != null) {
            let cl2 = image.clip(img, 10, 10, 600, 600)
            logd("cl2 " + cl2)
            ocrimage(cl2)
            logd("capture succeeded, count: " + tt + " total time: " + timex + " ms")
            image.recycle(cl2)
            image.recycle(img)
        } else {
            logd("capture failed, success count: " + tt + " total time: " + timex + " ms")
            logd("call resetCaptureScreenNoAutoEnv and wait for recovery")
            // Wait for recovery
            image.resetCaptureScreenNoAutoEnv();
            sleep(10000)
        }
    }

}

testCap();
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
  // Wait ≥1s after permission (more on slow devices) before capture
  sleep(1000)
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
  // Wait ≥1s after permission (more on slow devices) before capture
  sleep(1000)
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

  // Wait ≥1s after permission (more on slow devices) before capture
  sleep(1000)
  let points3 = "205|1130|0xff944b-0x101010,211|1158|0xff8e42,191|1175|0xfcfbf7";
  let points = image.cmpColorEx(points3, 0.9, 0, 0, 0, 0);
  logd("points " + points);

}

main();
```

### image.cmpMultiColor Multi-Group Color Comparison

* Multi-point or multi-group color comparison; returns index of first matching group, or -1 if none
* @param image1 Image
* @param points Array like ["6|1|0x969696-0x000010,1|12|0x969696,-4|0|0x969696","6|1|0x969696"]
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
  // Wait ≥1s after permission (more on slow devices) before capture
  sleep(1000)
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
  // Wait ≥1s after permission (more on slow devices) before capture
  sleep(1000)
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
* @param points Array like ["6|1|0x969696-0x000010,1|12|0x969696,-4|0|0x969696","6|1|0x969696"]
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
  // Wait ≥1s after permission (more on slow devices) before capture
  sleep(1000)
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
  // Wait ≥1s after permission (more on slow devices) before capture
  sleep(1000)
  let aimage = image.captureFullScreen();
  if (aimage != null) {
    let points = image.findColor(aimage, "0xCDD7E9-0x101010,0xCDD7E9-0x101010", 0.9, 0, 0, 0, 0, 10, 1);
    logd("points " + JSON.stringify(points));
    This is an array
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
  // Wait ≥1s after permission (more on slow devices) before capture
  sleep(1000)
  let aimage = image.captureFullScreen();
  if (aimage != null) {
    let points = image.findColorJ(aimage, "coin.json");
    logd("points " + JSON.stringify(points));
    This is an array
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
  // Wait ≥1s after permission (more on slow devices) before capture
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

### image.findColorExJ Auto Screenshot Single-Point Find Color (JSON)

* Find pixel with exact color on current screen; params from JSON; returns coordinates or null
* @param jsonFileName JSON from color tool in res folder
* @return Array of Point coordinates or null

```javascript showLineNumbers
function main() {
  let req = startEnv();
  if (!req) {
    logd("permission request failed");
    return;
  }
  // Wait ≥1s after permission (more on slow devices) before capture
  sleep(1000)
  let points = image.findColorExJ("coin.json");
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

### image.findMultiColor Multi-Point Find Color

* Multi-point find-color; returns all matching points (similar to Auto.js multi-color find)
* @param image Image to find color in
* @param firstColor First point color
* @param threshold Color similarity 0.0–1.0 for find-color
* @param points String like 6|1|0x969696-0x000010,1|12|0x969696,-4|0|0x969696
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
  // Wait ≥1s after permission (more on slow devices) before capture
  sleep(1000)
  let aimage = image.captureFullScreen();
  if (aimage != null) {
    let points = image.findMultiColor(aimage, "0xDD7A5F-0x101010", "29|25|0xBB454B-0x101010,58|44|0xA6363A-0x101010", 0.9, 0, 0, 0, 0, 10, 1);
    logd("points " + JSON.stringify(points));
    This is an array
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
  // Wait ≥1s after permission (more on slow devices) before capture
  sleep(1000)
  let aimage = image.captureFullScreen();
  if (aimage != null) {
    let points = image.findMultiColorJ(aimage, "coin.json");
    logd("points " + JSON.stringify(points));
    This is an array
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
* @param threshold Color similarity 0.0–1.0 for find-color
* @param points String like 6|1|0x969696-0x000010,1|12|0x969696,-4|0|0x969696
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

  // Wait ≥1s after permission (more on slow devices) before capture
  sleep(1000)
  let points = image.findMultiColorEx("0xDD7A5F-0x101010", "29|25|0xBB454B-0x101010,58|44|0xA6363A-0x101010", 0.9, 0, 0, 0, 0, 10, 1);
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

### image.findMultiColorExJ Auto Screenshot Multi-Point Find Color (JSON)

* Multi-point find-color on auto-captured screen; params from JSON (similar to Auto.js)
* @param jsonFileName JSON from color tool in res folder, e.g. a.json
* @return Array of Point coordinates or null

```javascript showLineNumbers
function main() {
  let req = startEnv();
  if (!req) {
    logd("permission request failed");
    return;
  }
  // Wait ≥1s after permission (more on slow devices) before capture
  sleep(1000)
  let points = image.findMultiColorExJ("coin.json");
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

## Find Non-Color

### image.findNotColor Find Non-Color

* Find a pixel whose color is not equal to color; returns coordinates or null
* Requires EC USB 6.30.0+
* @param image Image
* @param color Target colors like 0xCDD7E9-0x101010; generated by EC color tool
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
  // Wait ≥1s after permission (more on slow devices) before capture
  sleep(1000)
  let aimage = image.captureFullScreen();
  if (aimage != null) {
    let points = image.findNotColor(aimage, "0xCDD7E9-0x101010,0xCDD7E9-0x101010", 0.9, 0, 0, 0, 0, 10, 1);
    logd("points " + JSON.stringify(points));
    This is an array
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
  // Wait ≥1s after permission (more on slow devices) before capture
  sleep(1000)
  let aimage = image.captureFullScreen();
  if (aimage != null) {
    let points = image.findNotColorJ(aimage, "a.json");
    logd("points " + JSON.stringify(points));
    This is an array
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
  // Wait ≥1s after permission (more on slow devices) before capture
  sleep(1000)
  // Read sms.png from project res folder
  let sms = readResAutoImage("sms.png");
  // Capture screen
  let aimage = image.captureFullScreen();
  logd("aimage " + aimage);
  if (aimage != null) {
    // Search in image
    let points = image.findImageByColor(aimage, sms, 0, 0, 0, 0, 0.8, 5);
    logd("points " + JSON.stringify(points));
    This is an array
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

  // Capture screen
  let aimage = image.captureFullScreen();
  logd("aimage " + aimage);
  if (aimage != null) {
    // Search in image
    let points = image.findImageByColorJ(aimage, "a.json");
    logd("points " + JSON.stringify(points));
    This is an array
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
 This is an array
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
 sleep(1000)
 // Capture screen
 let aimage = image.captureFullScreen();
 logd("aimage " + aimage);
 if (aimage != null) {
 // Search in image
 let points = image.findImageByColorExJ(aimage, "a.json");
 logd("points " + JSON.stringify(points));
 This is an array
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

### image.initOpenCV Initialize OpenCV

* Initialize OpenCV library
* Call before find-image; first init copies libraries (may take time); later runs are fast
* @return boolean true on success, false on failure

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 sleep(1000)
 let d = image.initOpenCV();
 logd(d)
}

main();
```

### image.findImage Find Image

* Find image: locate template in large image (template matching); returns Rect region or null if not found.
* @param image Large image
* @param template Template (small) image
* @param x Find-image region start X
* @param y Find-image region start Y
* @param ex End X
* @param ey End Y
* @param weakThreshold Weak threshold per template-matching round; stop if similarity below this (0~
  float; default 0.7.
* @param threshold Image similarity 0–1; default 0.9
* @param limit Result limit; 1 for single match
* @param method 0: TM_SQDIFF, 1: TM_SQDIFF_NORMED, 2: TM_CCORR, 3:
  TM_CCORR_NORMED, 4:
  TM_CCOEFF, 5: TM_CCOEFF_NORMED
* @return Rect region array or null

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 let d = image.initOpenCV();
 logd(d)
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
 // Read sms.png from project res folder
 let sms = readResAutoImage("sms.png");
 // Capture screen
 let aimage = image.captureFullScreen();
 logd("aimage " + aimage);
 if (aimage != null) {
 // Search in image
 let points = image.findImage(aimage, sms, 0, 0, 0, 0, 0.7, 0.9, 21, 5);
 logd("points " + JSON.stringify(points));
 This is an array
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
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
 let d = image.initOpenCV();
 logd(d)
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
 // Capture screen
 let aimage = image.captureFullScreen();
 logd("aimage " + aimage);
 if (aimage != null) {
 // Search in image
 let points = image.findImageJ(aimage, "a.json");
 logd("points " + JSON.stringify(points));
 This is an array
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

### image.findImage2 Find Image

* Find image: locate template in large image (template matching); returns Rect region or null if not found.
* Note: includes scaling
* EC iOS 6.45.0+
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
  TM_CCORR_NORMED, 4:
  TM_CCOEFF, 5: TM_CCOEFF_NORMED
* @return Rect region array or null

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 let d = image.initOpenCV();
 logd(d)
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
 // Read sms.png from project res folder
 let sms = readResAutoImage("sms.png");
 // Capture screen
 let aimage = image.captureFullScreen();
 logd("aimage " + aimage);
 if (aimage != null) {
 // Search in image
 let points = image.findImage2(aimage, sms, 0, 0, 0, 0, 0.7, 0.9, 21, 5);
 logd("points " + JSON.stringify(points));
 This is an array
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
 // Recycle image to free memory
 image.recycle(sms)
}

main();
```

### image.findImage2J Find Image (JSON)

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
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
 // Capture screen
 let aimage = image.captureFullScreen();
 logd("aimage " + aimage);
 if (aimage != null) {
 // Search in image
 let points = image.findImage2J(aimage, "a.json");
 logd("points " + JSON.stringify(points));
 This is an array
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
 image.recycle(aimage)
 }
}

main();
```

### image.findImageEx Auto Screenshot Find Image

* Find image: locate template on current screen (template matching); returns Rect region or null if not found.
* @param template Template (small) image
* @param x Find-image region start X
* @param y Find-image region start Y
* @param ex End X
* @param ey End Y
* @param weakThreshold Weak threshold per template-matching round; stop if similarity below this (0~
  float; default 0.7.
* @param threshold Image similarity 0–1; default 0.9
* @param limit Result limit; 1 for single match
* @param method 0: TM_SQDIFF, 1: TM_SQDIFF_NORMED, 2: TM_CCORR, 3:
  TM_CCORR_NORMED, 4:
  TM_CCOEFF, 5: TM_CCOEFF_NORMED
* @return Rect region array or null

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 let d = image.initOpenCV();
 logd(d)
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
 // Read sms.png from project res folder
 let sms = readResAutoImage("sms.png");
 // Search current screen, limit to one match
 let points = image.findImageEx(sms, 0, 0, 0, 0, 0.7, 0.9, 21, 5);
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
 image.recycle(sms)
}

main();
```

### image.matchTemplate Image Template Matching

* OpenCV template matching wrapper
* @param image Large image
* @param template Template (small) image
* @param weakThreshold Weak threshold per template-matching round; stop if similarity below this (0~
  float; default 0.9.
* @param threshold Strong threshold for final match; early return if similarity exceeds this (0~
  float; default 0.9.
* @param rect Find-image region; see findColor rect docs.
* @param maxLevel Default -1; usually unchanged. Auto-adjusts by image size. Uses image pyramid.
  level = pyramid depth,
  Higher level may improve speed but can cause misses (over-shrunk image) or wrong positions. Tune only if you understand it.
* @param limit Result limit; 1 for single match
* @param method 0: TM_SQDIFF, 1: TM_SQDIFF_NORMED, 2: TM_CCORR, 3:
  TM_CCORR_NORMED, 4:
  TM_CCOEFF, 5: TM_CCOEFF_NORMED
* @return Match collection or null

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 let d = image.initOpenCV();
 logd(d)
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
 let aimage = image.captureFullScreen();
 if (aimage != null) {
 let temp = readResAutoImage("tmp.png");
 let rectp = new Rect();
 rectp.left = 0;
 rectp.top = 0;
 rectp.right = device.getScreenWidth();
 rectp.bottom = device.getScreenHeight();
 let matchs = image.matchTemplate(aimage, temp, 0.7, 0.9, rectp, -1, 10, 5);
 This is an array
 logd(JSON.stringify(matchs));
 This is an array
 if (matchs && matchs.length > 0) {
 for (let i = 0; i < matchs.length; i++) {
 logd(JSON.stringify(matchs[i].point));
 clickPoint(matchs[i].point.x, matchs[i].point.y)
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
 let d = image.initOpenCV();
 logd(d)
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
 let aimage = image.captureFullScreen();
 if (aimage != null) {
 let matchs = image.matchTemplateJ(aimage, "a.json");
 This is an array
 logd(JSON.stringify(matchs));
 This is an array
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

### image.matchTemplate2 Image Template Matching

* OpenCV template matching wrapper
* Note: includes scaled find-image
* EC iOS 6.45.0+
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
  TM_CCORR_NORMED, 4:
  TM_CCOEFF, 5: TM_CCOEFF_NORMED
* @return Match collection of matches

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 let d = image.initOpenCV();
 logd(d)
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
 let aimage = image.captureFullScreen();
 if (aimage != null) {
 let temp = readResAutoImage("tmp.png");
 let rectp = new Rect();
 rectp.left = 0;
 rectp.top = 0;
 rectp.right = device.getScreenWidth();
 rectp.bottom = device.getScreenHeight();
 let matchs = image.matchTemplate2(aimage, temp, 0.7, 0.9, rectp, -1, 10, 5);
 This is an array
 logd(JSON.stringify(matchs));
 This is an array
 if (matchs && matchs.length > 0) {
 for (let i = 0; i < matchs.length; i++) {
 logd(JSON.stringify(matchs[i].point));
 clickPoint(matchs[i].point.x, matchs[i].point.y)
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



### image.matchTemplate2J Image Template Matching (JSON)

* OpenCV template matching wrapper
* @param image1 Large image
* @param jsonFileName JSON from color tool in res, e.g. a.json; configure template path in JSON
* @return `{null|Match[]}` matches

```javascript showLineNumbers
function main() {
 let d = image.initOpenCV();
 logd(d)
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
 let aimage = image.captureFullScreen();
 if (aimage != null) {
 let matchs = image.matchTemplate2J(aimage, "a.json");
 This is an array
 logd(JSON.stringify(matchs));
 This is an array
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

### image.matchTemplateEx Image Template Matching

* OpenCV template matching wrapper on current screen capture
* @param template Template (small) image
* @param weakThreshold Weak threshold per template-matching round; stop if similarity below this (0~
  float; default 0.9.
* @param threshold Strong threshold for final match; early return if similarity exceeds this (0~
  float; default 0.9.
* @param rect Find-image region; see findColor rect docs.
* @param maxLevel Default -1; usually unchanged. Auto-adjusts by image size. Uses image pyramid.
  level = pyramid depth,
  Higher level may improve speed but can cause misses (over-shrunk image) or wrong positions. Tune only if you understand it.
* @param limit Result limit; 1 for single match
* @param method 0: TM_SQDIFF, 1: TM_SQDIFF_NORMED, 2: TM_CCORR, 3:
  TM_CCORR_NORMED, 4:
  TM_CCOEFF, 5: TM_CCOEFF_NORMED
* @return Match collection or null

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 let d = image.initOpenCV();
 logd(d)
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
 let temp = readResAutoImage("tmp.png");
 let rectp = new Rect();
 rectp.left = 0;
 rectp.top = 0;
 rectp.right = device.getScreenWidth();
 rectp.bottom = device.getScreenHeight();
 let matchs = image.matchTemplateEx(temp, 0.7, 0.9, rectp, -1, 1, 5);
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
 // Recycle image to free memory
 image.recycle(temp)
}

main();
```


## Grayscale


### image.gray Grayscale Image

* Grayscale image via OpenCV
* Requires EC iOS USB 8.18.0+
* @param img AutoImage object
* @return `{null|AutoImage}`

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
 for (let i = 0; i < 1000; i++) {
 sleep(1000);
 let s = new Date().getTime();
 let d = image.captureFullScreenEx();
 if (d) {
 let saved = image.saveTo(d, "D:/testb.png");
 let s = new Date().getTime();
 let bd = image.gray(d);
 logd("time " + (new Date().getTime() - s))
 logd(bd.uuid);
 if (bd) {
 let saved = image.saveTo(bd, "D:/testb2.png");
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
* @param type Binarization type; usually 1
* 0: grayscale above threshold = max, else 0
* 1: grayscale above threshold = 0, else max
* 2: above threshold = threshold, else unchanged
* 3: above threshold unchanged, else 0
* 4: above threshold = 0, else unchanged
* 7: not supported
* 8: Otsu automatic global threshold
* 16: Triangle automatic global threshold
* @param threshold Binarization coefficient, 0–255
* @return AutoImage object or null

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
 for (let i = 0; i < 1000; i++) {
 sleep(1000);
 let s = new Date().getTime();
 let d = image.captureFullScreenEx();
 if (d) {
 let saved = image.saveTo(d, "D:/testb.png");
 let s = new Date().getTime();
 let bd = image.binaryzation(d, 1, 200);
 logd("time " + (new Date().getTime() - s))
 logd(bd.uuid);
 if (bd) {
 let saved = image.saveTo(bd, "D:/testb2.png");
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

### image.binaryzationBitmap Binarize BufferedImage

* Binarize Java BufferedImage
* @param bitmap BufferedImage object
* @param type Binarization type; usually 1
* 0: grayscale above threshold = max, else 0
* 1: grayscale above threshold = 0, else max
* 2: above threshold = threshold, else unchanged
* 3: above threshold unchanged, else 0
* 4: above threshold = 0, else unchanged
* 7: not supported
* 8: Otsu automatic global threshold
* 16: Triangle automatic global threshold
* @param threshold Binarization coefficient, 0–255
* @return BufferedImage object or null

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
 for (let i = 0; i < 1000; i++) {
 sleep(1000);
 let s = new Date().getTime();
 let d = image.captureFullScreen();
 if (d) {
 let s = new Date().getTime();
 let bd = image.binaryzationBitmap(image.imageToBitmap(d), 1, 200);
 logd("time " + (new Date().getTime() - s))
 logd(bd);
 if (bd) {
 exit()
 }
 // Recycle image to free memory
 image.recycle(d)
 }
 }

}

main();
```

### image.binaryzationBitmapEx Binarize BufferedImage

* Adaptive binarization via OpenCV adaptiveThreshold
* @param bitmap BufferedImage object
* @param map Map parameters
    * diameter: Denoise diameter; see OpenCV bilateralFilter
    * adaptiveMethod: 0=ADAPTIVE_THRESH_MEAN_C, 1=ADAPTIVE_THRESH_GAUSSIAN_C
    * blockSize: Neighborhood block size in pixels; use odd values like 3, 5, 7
    * c: Constant offset adjustment
    * ```{"diameter":20,"adaptiveMethod":1,"c":9,"blockSize":51}```
* @return BufferedImage object or null

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
 for (let i = 0; i < 1000; i++) {
 sleep(1000);
 let d = image.captureFullScreen();
 if (d) {
 let s = new Date().getTime();
 let bd = image.binaryzationBitmapEx(image.imageToBitmap(d),
 {
 "diameter": 20,
 "adaptiveMethod": 1,
 "c": 9, "blockSize": 51
 });
 logd("time " + (new Date().getTime() - s))
 logd(bd);
 if (bd) {
 exit()
 }
 // Recycle image to free memory
 image.recycle(d)
 }
 }
}

main();
```

### image.binaryzationEx Binarize Image

* Adaptive binarization via OpenCV adaptiveThreshold
* @param img AutoImage object
* @param map Map parameters
    * diameter: Denoise diameter; see OpenCV bilateralFilter
    * adaptiveMethod: 0=ADAPTIVE_THRESH_MEAN_C, 1=ADAPTIVE_THRESH_GAUSSIAN_C
    * blockSize: Neighborhood block size in pixels; use odd values like 3, 5, 7
    * c: Constant offset adjustment
    * ```{"diameter":20,"adaptiveMethod":1,"c":9,"blockSize":51}```
* @return `{null|AutoImage}`

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
 for (let i = 0; i < 1000; i++) {
 sleep(1000);
 let s = new Date().getTime();
 let d = image.captureFullScreenEx();
 if (d) {
 let saved = image.saveTo(d, "D:/testb.png");
 let s = new Date().getTime();
 let bd = image.binaryzationEx(d, {
 "diameter": 20,
 "adaptiveMethod": 1,
 "c": 9, "blockSize": 51
 });
 logd("time " + (new Date().getTime() - s))
 logd(bd.uuid);
 if (bd) {
 let saved = image.saveTo(bd, "D:/testb2.png");
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
* Supports EC iOS control center 6.0+
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

### image.scaleImage Scale Image

* Scale image
* Requires EC iOS 6.45.0+
* @param img `{AutoImage}` object
* @param w Width
* @param h Height
* @return AutoImage object or null

```javascript showLineNumbers
function main() {
 let img = image.captureFullScreen()
 logd(" img width " + image.getWidth(img2))
 let img2 = image.scaleImage(img, 100, 200);
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
 let autoimg = image.readImage("F:/a.png");
 // Recycle image to free memory
 image.recycle(autoimg)
}

main();
```

### image.readBitmap Read File as Bitmap

* Read image at path and return ```{@link AutoImage}```, or null if missing or undecodable
* @param path Image path
* @return Android Bitmap or null

```javascript showLineNumbers
function main() {
 let autoimg = image.readBitmap("F:/a.png");
 // Recycle image to free memory
 image.recycle(autoimg)
}

main();
```

### image.scaleBitmap Scale Image

* Scale image
* Requires EC iOS 6.45.0+
* @param img `{BufferedImage}` object
* @param w Width
* @param h Height
* @return BufferedImage object or null

```javascript showLineNumbers
function main() {
 let autoimg = image.readBitmap("F:/a.png");
 let bbs = image.scaleBitmap(autoimg, 100, 200)
 logd("bb " + bbs)
 // Save to file for inspection
}

main();
```

### image.pixelInImage Pixel Color at Image Coordinates

* Returns ARGB of pixel at (x, y) in image.
* Format is 0xAARRGGBB (32-bit integer)
* Origin at top-left; x axis along top, y axis along left
* @param image Image
* @param x Pixel X to read
* @param y Pixel Y to read
* @return integer

```javascript showLineNumbers
function main() {
 let request = image.requestScreenCapture(10000, 0);
 if (!request) {
 request = image.requestScreenCapture(10000, 0);
 }
 logd("Screen capture request result... " + request)
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
 let imageX = image.captureFullScreen();
 let color = image.pixelInImage(imageX, 100, 100);
 // Recycle image to free memory
 image.recycle(imageX)
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
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
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

### image.getWidth Get Width

* Get width
* @param img Image object
* @return int

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
 let aimage = image.captureFullScreen();
 if (aimage != null) {
 let w = image.getWidth(aimage);
 logd("w " + w);
 // Recycle image to free memory
 image.recycle(aimage)
 }
}

main();
```

### image.getHeight Get Height

* Get height
* @param img Image object
* @return int

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
 let aimage = image.captureFullScreen();
 if (aimage != null) {
 let h = image.getHeight(aimage);
 logd("h " + h);
 // Recycle image to free memory
 image.recycle(aimage)
 }
}

main();
```

### image.argb RGB String

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
 logd("Screen capture request result... " + request)
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
 let imageX = image.captureFullScreen();
 let color = image.pixel(imageX, 100, 100);
 logd(image.argb(color))
 // Recycle image to free memory
 image.recycle(imageX)
}

main();
```

### image.getPixelBitmap Get Bitmap Single-Point Color

* Get pixel color in Bitmap
* @param bitmap Image object
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
 logd("Screen capture request result... " + request)
 /// Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
 let bitmap = image.captureFullScreen("jpg", 800, 800, 100, 100, 100);
 let color = image.getPixelBitmap(image.imageToBitmap(bitmap), 100, 100);
 // Recycle image to free memory
 image.recycle(bitmap)
}

main();
```

### image.getPixelsBitmap Get Bitmap Region Colors

* Get region pixel colors from Bitmap; same as Bitmap.getPixels
* @param bitmap Image object
* @param arraySize Size of region array to return
* @param offset First pixel index in pixels[]
* @param stride Row stride in pixels[] (>= bitmap width); may be negative
* @param x First pixel X in bitmap
* @param y First pixel Y in bitmap
* @param width Pixel width to read per row
* @param height Number of rows to read
* @return int color value array

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 logd("Screen capture request result... " + request)
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
 let bitmap = image.captureFullScreen();
 let w = bitmap.getWidth();
 let h = bitmap.getHeight();
 let mPixels = image.getPixelsBitmap(image.imageToBitmap(bitmap), w * h, 0, w, 0, 0, w, h);
 // Recycle image to free memory
 image.recycle(bitmap)
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
 logd("Screen capture request result... " + request)
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
 let imageX = image.captureFullScreen();
 let r = image.saveTo(imageX, "D:/a.png");
 logd("result " + r);
 // Recycle image to free memory
 image.recycle(imageX)
}

main();
```

### image.saveBitmap Save Bitmap Image

* Save Bitmap image
* Supported Versions(EC 5.15.0+)
* @param bitmap Image
* @param format Save format: png, jpg, webp
* @param q Save quality 1–100; ignored for png
* @param path Image save path
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {

 let bot = image.readBitmap("D:/yyb2.png");
 logd("bot " + bot);
 // Save to file
 let saved = image.saveBitmap(bot, "png", 100, "D:/tmp.png");
 logd("saved " + saved);
 // Recycle to prevent memory growth
 if (bot) {
 bot.recycle()
 }

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
 logd("Screen capture request result... " + request)
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
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
 logd("Screen capture request result... " + request)
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
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
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
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

### image.recycleAllImage Recycle Image

* Recycle all images
* Requires EC iOS 7.16.0+
* @return `boolean` true on success

```javascript showLineNumbers
function main() {
 let imageX = image.captureFullScreen();
 // Recycle image to free memory
 image.recycleAllImage()

}

main();
```

### image.clipBitmap (Clip Bitmap)

* Clip image; verify parameters yourself
* @param bitmap Image
* @param x Start X
* @param y Start Y
* @param w Clip width
* @param h Clip height
* @return `{Bitmap}` Android Bitmap object

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 logd("Screen capture request result... " + request)
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
 for (let i = 0; i < 100; i++) {
 let d = image.captureFullScreen();
 logd(d)
 sleep(1000);
 if (d) {
 d = image.imageToBitmap(d)
 d = image.clipBitmap(d, 100, 100, 200, 200);
 let ds = image.bitmapBase64(d, "jpg", 100);
 logd(ds)
 loge(image.base64Bitmap(ds, 0))
 // Recycle image to free memory
 image.recycle(d)
 }
 }
}

main();
```

### image.base64Bitmap (Base64 to Bitmap)

* Convert base64 string to Bitmap
* @param data Base64 data
* @param flag Base64 flag, usually 0
* Optional: 0 default, 1 no padding, 2 no wrap, 4 wrap mode
* @return `{Bitmap}` Android Bitmap object

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 logd("Screen capture request result... " + request)
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
 for (let i = 0; i < 100; i++) {
 let d = image.captureFullScreen();
 logd(d)
 sleep(1000);
 if (d) {
 d = image.clipBitmap(d, 100, 100, 200, 200);
 let ds = image.bitmapBase64(d, "jpg", 100);
 logd(ds)
 loge(image.base64Bitmap(ds, 0))
 // Recycle image to free memory
 image.recycle(d)
 }
 }

}

main();
```

### image.bitmapBase64 (Bitmap to Base64)

* Convert Bitmap to base64
* @param bitmap Image
* @param format Format: jpg or png
* @param q Quality 1–100; ignored for png
* @return `{string}` base64 string

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 logd("Screen capture request result... " + request)
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
 for (let i = 0; i < 100; i++) {
 let d = image.captureFullScreen();
 logd(d)
 sleep(1000);
 if (d) {
 d = image.imageToBitmap(d)
 d = image.clipBitmap(d, 100, 100, 200, 200);
 let ds = image.bitmapBase64(d, "jpg", 100);
 logd(ds)
 loge(image.base64Bitmap(ds, 0))
 // Recycle image to free memory
 image.recycle(d)
 }
 }

}

main();
```

### image.imageToBitmap (AutoImage to Bitmap)

* Convert AutoImage to native BufferedImage
* @param img `{AutoImage}`
* @return `{BufferedImage}` object

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 logd("Screen capture request result... " + request)
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
 for (let i = 0; i < 100; i++) {
 let d = image.captureFullScreen();
 logd(d)
 sleep(1000);
 if (d) {
 let ds = image.imageToBitmap(d);
 logd(ds)
 image.recyle(d);
 }
 }

}

main();
```

### image.bitmapToImage (BufferedImage to AutoImage)

* Convert BufferedImage to AutoImage
* @param img `{BufferedImage}` object
* @return `{AutoImage}` object

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 logd("Screen capture request result... " + request)
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
 for (let i = 0; i < 100; i++) {
 let d = image.captureFullScreen();
 logd(d)
 sleep(1000);
 if (d) {
 let ds = image.imageToBitmap(d);
 logd(ds)
 // Convert back to AutoImage
 let sy = image.bitmapToImage(ds);
 logd(sy)
 image.recyle(d);
 }

 }
}

main();
```

### image.imageToMatFormat (BufferedImage to Mat AutoImage)

* Convert to Mat storage format
* Requires EC iOS 7.11.0+
* @param img `{AutoImage}` object
* @return AutoImage in MAT storage format or null

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 logd("Screen capture request result... " + request)
 // Wait ≥1s after permission (more on slow devices) before capture
 sleep(1000)
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

### image.matToImageFormat (Mat AutoImage to BufferedImage AutoImage)

* Convert to normal image storage format
* Requires EC iOS 7.11.0+
* @param img `{AutoImage}` object
* @return AutoImage in normal storage format or null

```javascript showLineNumbers
function main() {
 let req = startEnv();
 if (!req) {
 logd("permission request failed");
 return;
 }
 logd("Screen capture request result... " + request)
 sleep(1000)
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
