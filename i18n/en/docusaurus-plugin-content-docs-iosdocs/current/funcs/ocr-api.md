---
title: OCR Recognition
description: EasyClick automation scripts — iOS no jailbreak OCR recognition resource download
keywords:
 - EasyClick automation scripts iOS no jailbreak OCR recognition resource download
 - ocr
 - OCR
 - ocrLite
 - ocrInstance
 - releaseAll
 - initOcr
 - 6.23.0
 - PPOCR
 - paddleOcrOnnxV6
 - newOcr
 - tess
 - EasyClick
 - mobile automation
 - automation testing
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview {#说明}

- The OCR module performs image recognition
- The OCR module object prefix is `ocr`, e.g. `ocr.initOcr()`
- Current OCR engines include ocrLite, TesseractOCR, paddleOcr, paddleNcnnOcrV5, paddleOcrOnnxV4, paddleOcrOnnxV5, and **paddleOcrOnnxV6** (PP-OCRv6_small)



## ocr.newOcr Create an OCR Instance {#ocrnewocr-实例一个ocr}

* Create an OCR instance
* Supported: EC iOS 6.23.0+

```javascript showLineNumbers
function main() {
    let o = ocr.newOcr();
    // Initialize and recognize here
    o.releaseAll()
}
```
## ocr.releaseAll Release All OCR Resources {#ocrreleaseall-释放所有ocr资源}

* Release all OCR resources
* Supported: EC iOS 9.0.0+

```javascript showLineNumbers
    // See code examples below
```
## ocr.initOcr Initialize {#ocrinitocr-初始化}

* Initialize the OCR module
* @param map parameter map
* Keys:
 * type: OCR type — paddleNcnnOcrV5 = NCNN PPOCR-V5, paddleOcrOnnxV4 = PPOCR-V4 model, paddleOcrOnnxV5 = PPOCR-V5 model, paddleOcrOnnxV6 = PPOCR-V6_small model, ocrLite = ocrLite module, tess = tesseractOcr, paddleOcr = Baidu PaddleOCR
* If type is `ocrLite`,
 * Example parameters: `{"type":"ocrLite","cpuType:"","baseDir":""}`,
 * baseDir: ocrLite library path; usually in the same directory as EC; folder name: OcrLiteNcnn
 * cpuType: host CPU type — win-lib-cpu-x64, win-lib-cpu-x86, Linux-Lib-CPU, Darwin-Lib-CPU
 * single: set to 0 to allocate one OCR instance per device; default is 1; try changing this on different machines
 * matMode: 1 = convert image to OpenCV mat in memory and pass to OCR; 0 = use file-based approach without conversion
 * If baseDir and cpuType are omitted, the program auto-detects them
* If type is `baiduOnline`
 * Example parameters: `{"type":"baiduOnline","ak":"xxx","sk":"xx"}`

* If type is paddleNcnnOcrV5, parameters:
 * numThread: thread count; -1 = all, -2 = half of device CPUs, 0 = do not set; adjust recognition speed with this value
 * modelsDir: model file path; built-in models used if omitted
 * padding: white border around image to improve accuracy; increase when text boxes do not fully enclose text; default 32; affects recognition speed
 * maxSideLen: if the image's longest side exceeds max_side_len, scale down proportionally to max_side_len; default 640; affects recognition speed
 * keysName: training label file name; can be external, e.g. keys.txt; built-in models used if omitted
 * detName: detection model filename (.param); place under modelsDir; if named det.param, use det without the.param suffix; built-in models used if omitted
 * recName: recognition model filename (.onnx); place under modelsDir; if named rec.param, use rec without the.param suffix; built-in models used if omitted
* If type is `paddleOcrOnnxV4` or `paddleOcrOnnxV5`,
 * Example parameters: `{"type":"paddleOcrOnnxV5","single":0,"cpuType:"","baseDir":"","matModel":1}`,
 * baseDir: library path; usually in the same directory as EC; folder name: PaddleOcrOnnx; optional
 * numThread: CPU core count; default 1; optional
 * single: set to 0 to allocate one OCR instance per device; default 0; try changing this on different machines; optional
 * matMode: 1 = convert image to OpenCV mat in memory to save memory and pass to OCR; 0 = file-based approach; optional
 * cpuType: host CPU type — win-lib-cpu-x64, win-lib-cpu-x86, Linux-Lib-CPU, Darwin-Lib-CPU; optional
 * keysName: training label file path; place under the models folder in baseDir; built-in models used if omitted
 * detName: detection model filename (.onnx); place under the models folder in baseDir; built-in models used if omitted
 * recName: recognition model filename (.onnx); place under the models folder in baseDir; built-in models used if omitted
 * clsName: classification model filename (.onnx); place under the models folder in baseDir; built-in models used if omitted
 * padding: white border around image to improve accuracy; increase when text boxes do not fully enclose text; default 10
 * boxThresh: threshold separating text from background; higher values shrink the text region; range [0, 1]; default 0.3
 * boxScoreThresh: threshold for keeping detected text boxes; higher values mean lower recall; range [0, 1]; default 0.6
 * unClipRatio: controls detected text box size; larger values produce bigger boxes; range [1.6, 2.0]; default 2.0
 * doAngleFlag: enable (1) / disable (0) text orientation detection; needed only for upside-down images (rotated 90°–270°); default 1
 * mostAngleFlag: enable (1) / disable (0) angle voting (recognize entire image in the most likely text orientation); has no effect when orientation detection is disabled; default 1
 * maxSideLen: if the image's longest side exceeds max_side_len, scale down proportionally to max_side_len; default 0
* If type is `paddleOcrOnnxV6`
 * Built-in **PP-OCRv6_small** (det/rec ONNX); coexists with `paddleOcrOnnxV5` without conflict; angle classification cls reuses v5 models
 * Same parameter fields as `paddleOcrOnnxV5`; set type to `paddleOcrOnnxV6`, e.g. `{"type":"paddleOcrOnnxV6","numThread":2,"single":0,"matMode":1}`
 * Default model filenames (optional): `PP-OCRv6_small_det.onnx` / `PP-OCRv6_small_rec.onnx` / `ppocrv6_small_labels.txt`; cls remains `ch_ppocr_mobile_v2.0_cls_infer.onnx`
 * **Languages**: single model recognizes Simplified/Traditional Chinese, English, Japanese, and many Latin-script languages; Cyrillic scripts (e.g. Ukrainian) are not supported
 * **Fast mode**: for upright daily UI screenshots, use a smaller `maxSideLen` (e.g. 640) and disable orientation detection with `doAngleFlag:0`, `mostAngleFlag:0`; enable orientation detection when the image may be upside down
* If type is `tess`
 * Parameters: set Tesseract install path and tessdata path,<br/>
 * Example: `{"type":"tess","baseDir:"d:\\tesseract-ocr","path":"d:\\tesseract-ocr\\tessdata","language":"chi_sim","ocrEngineMode":3}`<br/>
 * - baseDir: Tesseract install path; download from https://github.com/tesseract-ocr/tesseract/releases or the official site; jTessBoxEditor.zip includes training tools and Tesseract DLLs<br/>
 * - path: Tesseract tessdata folder<br/>
 * - language: language data file; e.g. chi_sim.traineddata is Simplified Chinese, use chi_sim; combine with +, e.g. chi_sim+eng+num<br/>
 * - ocrEngineMode: recognition engine type — 0 OEM_TESSERACT_ONLY, 1 OEM_LSTM_ONLY, 2 OEM_TESSERACT_LSTM_COMBINED, 3 OEM_DEFAULT<br/>
 * - rilLevel: PageIteratorLevel — -1 adaptive, 0 RIL_BLOCK, 1 RIL_PARA, 2 RIL_TEXTLINE, 3 RIL_WORD, 4 RIL_SYMBOL<br/>
* If type is `paddleOcr` <br/>
* Example
```json showLineNumbers
{
  "type": "paddleOcr",
  "ocrType":"ONNX_PPOCR_V3",
  "padding": 50,
  "maxSideLen": 0,
  "boxScoreThresh": 0.5,
  "boxThresh": 0.3,
  "unClipRatio": 1.6,
  "doAngleFlag": 0,
  "mostAngleFlag": 0
}
```

```text showLineNumbers
  ocrType : model ONNX_PPOCR_V3, ONNX_PPOCR_V4, NCNN_PPOCR_V3
  serverUrl: Paddle OCR server address; deploy on another PC and connect from control center; default http://127.0.0.1:9022; change IP when deployed elsewhere; keep the port
  padding: white border around image to improve accuracy; increase when text boxes do not fully enclose text; default 50.<br/>
  maxSideLen: scale by long edge; larger values increase time but improve accuracy; smaller values reduce time but lower accuracy; 0 means no scaling.<br/>
  boxScoreThresh: text box confidence threshold; decrease when text boxes do not fully enclose text <br/>
  boxThresh: same purpose; tune experimentally.<br/>
  unClipRatio: single text box size multiplier; larger values produce bigger boxes.<br/>
  doAngleFlag: enable (1) / disable (0) text orientation detection; needed only for upside-down images (rotated 90°–270°); default off.<br/>
  mostAngleFlag: enable (1) / disable (0) angle voting; has no effect when orientation detection is disabled; default off.<br/>
  front: console (1) / tray mode (0); default off.<br/>
  daemon: daemonize OCR service process (1) / no (0); default off.<br/>
  limit: OCR requests per second; default 1000; lower to reduce CPU usage<br/>
  checkImage: verify input is image (1 yes, 0 no); default off.<br/>
 ```
* @return `{boolean}` boolean — success or failure


### ocrLite OCR Example [Before 6.23.0] {#ocrlite-ocr例子6230之前}

```javascript showLineNumbers
function main() {
    // EC 2.8.0+ control center: enable OpenCV on the control center settings page and restart the control center
    let ocrLite = {
        "type": "ocrLite",
        "baseDir": "c:/ec/OcrLiteNcnn",
        "cpuType": "win-lib-cpu-x64"
    }


    let inited = ocr.initOcr(ocrLite)
    logd("Init result -" + inited);
    if (!inited) {
        loge("error : " + ocr.getErrorMsg());
        return;
    }

    for (var ix = 0; ix < 20; ix++) {
        // Read a bitmap
        let bitmap = image.readBitmap("D:/Screenshot_20210127_152932_com.huawei.android.lau.jpg");
        if (!bitmap) {
            loge("Failed to read image");
            continue;
        }
        console.time("1")
        logd("start---ocr");
        // Recognize the image
        let result = ocr.ocrBitmap(bitmap, 20 * 1000, {});
        logd(result)
        if (result) {
            logd("OCR result -> " + JSON.stringify(result));
            for (var i = 0; i < result.length; i++) {
                var value = result[i];
                logd("Text : " + value.label + " x: " + value.x + " y: " + value.y + " width: " + value.width + " height: " + value.height);
            }
        } else {
            logw("No result recognized");
        }

        logd("Elapsed: " + console.timeEnd(1) + " ms")
        image.recycle(bitmap)
        sleep(1000);
        logd("ix = " + ix)
    }
    // Release all resources
    ocr.releaseAll();
}

main();
```




### tess OCR Example [After 6.23.0] {#tess-ocr例子6230之后}

```javascript showLineNumbers
function main() {
    let tess = {"type": "tess", "path": "d:/tesseract-ocr/tessdata", "baseDir": "d:\\tesseract-ocr"}

    let ocrLite = {
        "type": "ocrLite",
        "baseDir": "c:/ec/OcrLiteNcnn",
        "single": 0,
        "cpuType": "win-lib-cpu-x64"
    }


    // To use paddleOcr, change the parameters accordingly
    let paddleOcr = {
        "type": "paddleOcr",
        "ocrType": "ONNX_PPOCR_V3"
    }
    // Release first to avoid holding unreleased resources from before
    ocr.releaseAll()
    let tocr = ocr.newOcr()
    let inited = tocr.initOcr(ocrLite)
    logd("Init result -" + inited);
    if (!inited) {
        loge("error : " + tocr.getErrorMsg());
        return;
    }

    for (var ix = 0; ix < 20; ix++) {

        // Read a bitmap
        let bitmap = image.readBitmap("D:/Screenshot_20210127_152932_com.huawei.android.lau.jpg");
        if (!bitmap) {
            loge("Failed to read image");
            continue;
        }
        console.time("1")
        logd("start---ocr");
        // Recognize the image
        let result = tocr.ocrBitmap(bitmap, 30 * 1000, {"matMode": 1});
        logd(result)
        if (result) {
            logd("OCR result -> " + JSON.stringify(result));
            for (var i = 0; i < result.length; i++) {
                var value = result[i];
                logd("Text : " + value.label + " x: " + value.x + " y: " + value.y + " width: " + value.width + " height: " + value.height);
            }
        } else {
            logw("No result recognized");
        }

        logd("Elapsed: " + console.timeEnd(1) + " ms")
        image.recycle(bitmap)
        sleep(1000);
        logd("ix = " + ix)
    }
    // Release all resources
    // paddleOcr closes the OCR program; skip this if you do not need to shut down
    tocr.releaseAll();
}

main();
```


### PaddleOcrOnnx OCR Example [After 8.21.0] {#paddleocronnx-ocr例子8210之后}

```javascript showLineNumbers
function main() {
  
    // To use paddleOcrOnnxV4, change the parameters accordingly
    // Or change to paddleOcrOnnxV5 to use PPOCR-V5
    let paddleOcrOn = {
        "type": "paddleOcrOnnxV4",
        "numThread":1,
        "single": 0,
        "matMode": 1
    }

    // Release first to avoid holding unreleased resources from before
    ocr.releaseAll()
    let tocr = ocr.newOcr()
    let inited = tocr.initOcr(paddleOcrOn)
    logd("Init result -" + inited);
    if (!inited) {
        loge("error : " + tocr.getErrorMsg());
        return;
    }

    for (var ix = 0; ix < 20; ix++) {

        // Screenshot
        let bitmap = image.captureFullScreen();
        if (!bitmap) {
            loge("Failed to read image");
            continue;
        }
        console.time("1")
        logd("start---ocr "+bitmap);
        // Recognize the image
        let result = tocr.ocrImage(bitmap, 30 * 1000, {"matMode": 1});
        logd(result)
        if (result) {
            logd("OCR result -> " + JSON.stringify(result));
            for (var i = 0; i < result.length; i++) {
                var value = result[i];
                logd("Text : " + value.label +" confidence:"+value.confidence + " range: " + value.x + "," + value.y + "," + (value.width+value.x) + "," + (value.height+value.y));
            }
        } else {
            logw("No result recognized");
        }

        logd("Elapsed: " + console.timeEnd(1) + " ms")
        image.recycle(bitmap)
        sleep(2000);
        logd("ix = " + ix)
    }
    // Release all resources
    // Skip this if you do not need to shut down
    tocr.releaseAll();
}

main();
```



### paddleOcrOnnxV6 Example (PP-OCRv6_small) {#paddleocronnxv6-例子pp-ocrv6_small}

* Use type `paddleOcrOnnxV6`; built-in PP-OCRv6_small; coexists with `paddleOcrOnnxV5`
* Same parameter fields as `paddleOcrOnnxV5`; uses control center bundled models when model path/filenames are omitted
* **Languages**: Simplified/Traditional Chinese, English, Japanese, and many Latin-script languages; Cyrillic scripts (e.g. Ukrainian) are not supported
* **Fast mode**: for upright daily UI, use `maxSideLen:640`, `doAngleFlag:0`, `mostAngleFlag:0` (see `paddleOcrOnnxV6Fast` below)

```javascript showLineNumbers
function main() {
    // Fast mode (recommended for daily UI screenshots)
    let paddleOcrOnnxV6Fast = {
        "type": "paddleOcrOnnxV6",
        "numThread": 2,
        "single": 0,
        "matMode": 1,
        "padding": 32,
        "maxSideLen": 640,
        "doAngleFlag": 0,
        "mostAngleFlag": 0
    }

    // Default accuracy-oriented (engine defaults when maxSideLen / orientation detection omitted)
    let paddleOcrOnnxV6Default = {
        "type": "paddleOcrOnnxV6",
        "numThread": 2,
        "single": 0,
        "matMode": 1,
        "padding": 50,
        "maxSideLen": 960
    }

    // Release first to avoid holding unreleased resources from before
    ocr.releaseAll()
    let tocr = ocr.newOcr()
    let inited = tocr.initOcr(paddleOcrOnnxV6Fast)
    logd("Init result -" + inited)
    if (!inited) {
        loge("error : " + tocr.getErrorMsg())
        return
    }

    for (var ix = 0; ix < 20; ix++) {
        let bitmap = image.captureFullScreen()
        if (!bitmap) {
            loge("Failed to read image")
            continue
        }
        console.time("1")
        // ocrImage extra can override parameters for this recognition
        let result = tocr.ocrImage(bitmap, 30 * 1000, {
            "padding": 32,
            "numThread": 2,
            "maxSideLen": 640,
            "doAngleFlag": 0,
            "mostAngleFlag": 0
        })
        if (result) {
            logd("OCR result -> " + JSON.stringify(result))
            for (var i = 0; i < result.length; i++) {
                var value = result[i]
                logd("Text : " + value.label
                    + " confidence:" + value.confidence
                    + " range: " + value.x + "," + value.y + ","
                    + (value.x + value.width) + "," + (value.y + value.height))
            }
        } else {
            logw("No result recognized")
        }
        logd("Elapsed: " + console.timeEnd(1) + " ms")
        image.recycle(bitmap)
        sleep(2000)
        logd("ix = " + ix)
    }
    tocr.releaseAll()
}

main()
```



### PaddleOcrNcnnV5 OCR Example [After 9.8.0] {#paddleocrncnnv5-ocr例子980之后}

```javascript showLineNumbers
function main() {
  
    let paddleOcrOn = {
        "type": "paddleOcrNcnnV5",
        "numThread":1,
        "single": 0,
        "matMode": 1
    }

    // Release first to avoid holding unreleased resources from before
    ocr.releaseAll()
    let tocr = ocr.newOcr()
    let inited = tocr.initOcr(paddleOcrOn)
    logd("Init result -" + inited);
    if (!inited) {
        loge("error : " + tocr.getErrorMsg());
        return;
    }

    for (var ix = 0; ix < 20; ix++) {

        // Screenshot
        let bitmap = image.captureFullScreen();
        if (!bitmap) {
            loge("Failed to read image");
            continue;
        }
        console.time("1")
        logd("start---ocr "+bitmap);
        // Recognize the image
        let result = tocr.ocrImage(bitmap, 30 * 1000, {"padding": 32,"numThread":1});
        logd(result)
        if (result) {
            logd("OCR result -> " + JSON.stringify(result));
            for (var i = 0; i < result.length; i++) {
                var value = result[i];
                logd("Text : " + value.label +" confidence:"+value.confidence + " range: " + value.x + "," + value.y + "," + (value.width+value.x) + "," + (value.height+value.y));
            }
        } else {
            logw("No result recognized");
        }

        logd("Elapsed: " + console.timeEnd(1) + " ms")
        image.recycle(bitmap)
        sleep(2000);
        logd("ix = " + ix)
    }
    // Release all resources
    // Skip this if you do not need to shut down
    tocr.releaseAll();
}

main();
```


## ocrInstance.ocrBitmap Recognize Text {#ocrinstanceocrbitmap-识别文字}

* Perform OCR on a BufferedImage; returns JSON data similar to:

```json showLineNumbers
[
  {
    "label": "奇趣装扮三阶盘化",
    "confidence": 0.48334712,
    "x": 11,
    "y": 25,
    "width": 100,
    "height": 100
  }
]
```

* label: recognized text
* confidence: recognition confidence
* x: X start coordinate
* Y: Y start coordinate
* width: width
* height: height
* @param bitmap image
* @param timeout timeout in milliseconds
* @param extra extra parameters as a map, e.g. ```{"token":"xxx"}```
* @return `{json}` JSON object

## ocrInstance.ocrImage Recognize Text {#ocrinstanceocrimage-识别文字}

* Perform OCR on an AutoImage; returns JSON data similar to:

```json showLineNumbers
[
 {
 "label": "奇趣装扮三阶盘化",
 "confidence": 0.48334712,
 "x": 11,
 "y": 25,
 "width": 100,
 "height": 100
 }
]
```

* label: recognized text
* confidence: recognition confidence
* x: X start coordinate
* Y: Y start coordinate
* width: width
* height: height
* @param bitmap image
* @param timeout timeout in milliseconds
* @param extra extra parameters as a map, e.g. ```{"token":"xxx"}```
* @return `{json}` JSON object

```javascript showLineNumbers
See common code examples
OCR initialization
```

## ocrInstance.getErrorMsg Get Error Message {#ocrinstancegeterrormsg-获取错误消息}

* Get OCR error message
*
* @return `{string}` null means no error

```javascript showLineNumbers
See common code examples
OCR initialization
```

## ocrInstance.releaseAll Release OCR Resources {#ocrinstancereleaseall-释放ocr资源}

* Release OCR resources
*
* @return `{bool}` success or failure

```javascript showLineNumbers
See common code examples
OCR initialization
```

