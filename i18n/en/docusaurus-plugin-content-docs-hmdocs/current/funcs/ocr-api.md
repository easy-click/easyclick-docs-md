---
title: OCR Recognition
description: EasyClick automation scripts — HarmonyOS Next automation OCR recognition resource download
keywords:
 - EasyClick automation scripts HarmonyOS Next automation OCR recognition resource download
 - ocr
 - OCR
 - ocrLite
 - initOcr
 - cpu
 - newOcr
 - tess
 - paddleOcrOnnxV4
 - map
 - PPOCR
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
- Current OCR engines include ocrLite, TesseractOCR, paddleocr, and paddleOcrOnnxV4


## ocr.newOcr Create an OCR Instance {#ocrnewocr-实例一个ocr}

* Create an OCR instance

```javascript showLineNumbers
function main() {
    let o = ocr.newOcr();
    // Initialize and recognize here
    o.releaseAll()
}
```


## ocr.initOcr Initialize {#ocrinitocr-初始化}

* Initialize the OCR module
* @param map parameter map
* Keys:
 * type: OCR type — `paddleOcrOnnxV4` = PPOCR-V4 model, `paddleOcrOnnxV5` = PPOCR-V5 model, `paddleOcrOnnxV6` = PPOCR-V6_small model, `ocrLite` = ocrLite module, `tess` = tesseractOcr, `paddleOcr` = Baidu PaddleOCR
* If type is `ocrLite`
 * Example: `{"type":"ocrLite","cpuType:"","baseDir":""}`,
 * baseDir: ocrLite library path; usually in the same directory as EC; folder name: OcrLiteNcnn
 * cpuType: host CPU type — win-lib-cpu-x64, win-lib-cpu-x86, Linux-Lib-CPU, Darwin-Lib-CPU
 * single: setting to 0 means each device gets its own OCR instance; default is 1; try changing this on different machines
 * matMode: 1 = convert the image to OpenCV mat in memory and pass it to OCR; 0 = use a file-based approach without conversion
 * If baseDir and cpuType are omitted, the program finds them automatically
* If type is `baiduOnline`
 - Example: `{"type":"baiduOnline","ak":"xxx","sk":"xx"}`
* If type is `paddleNcnnOcrV5`, configure parameters as follows
 * numThread: thread count; -1 = all, -2 = half of device CPU, 0 = do not set; adjust recognition speed with this value
 * modelsDir: model file path; built-in models are used if omitted
 * padding: white border around the image to improve accuracy; increase when the text box does not cover all text; default 32; affects recognition speed
 * maxSideLen: if the image's longest edge exceeds maxSideLen, scale it down proportionally; default 640; affects recognition speed
 * keysName: training label file name; can be external, e.g. keys.txt; built-in model is used if omitted
 * detName: detection model filename (`.param`); place under modelsDir; for det.param write `det` without the param suffix; built-in model is used if omitted
 * recName: recognition model filename (`.onnx`); place under modelsDir; built-in model is used if omitted

* If type is `paddleOcrOnnxV4` or `paddleOcrOnnxV5`,
 * Example: `{"type":"paddleOcrOnnxV5","single":0,"cpuType:"","baseDir":"","matModel":1}`,
 * baseDir: ocrLite library path; usually in the same directory as EC; folder name: PaddleOcrOnnx; optional
 * numThread: number of CPU cores; default is 1; optional
 * single: setting to 0 means each device gets its own OCR instance; default is 0; try changing this on different machines; optional
 * matMode: 1 = convert the image to OpenCV mat in memory to save memory and pass it to OCR; 0 = use a file-based approach without conversion; optional
 * cpuType: host CPU type — win-lib-cpu-x64, win-lib-cpu-x86, Linux-Lib-CPU, Darwin-Lib-CPU; optional
 * keysName: training label file path; place under the models folder in baseDir; built-in model is used if omitted
 * detName: detection model filename (`.onnx`); place under the models folder in baseDir; built-in model is used if omitted
 * recName: recognition model filename (`.onnx`); place under the models folder in baseDir; built-in model is used if omitted
 * clsName: classification model filename (`.onnx`); place under the models folder in baseDir; built-in model is used if omitted
 * padding: white border around the image to improve accuracy; increase when the text box does not cover all text; default 10.
 * boxThresh: threshold for separating text from background; larger values shrink the text region; range [0, 1]; default 0.3.
 * boxScoreThresh: threshold for keeping detected text boxes; larger values reduce recall; range [0, 1]; default 0.6.
 * unClipRatio: controls text box size; larger values make boxes bigger; range [1.6, 2.0]; default 2.0.
 * doAngleFlag: enable (1)/disable (0) text orientation detection; only needed for upside-down images (rotated 90–270°); default 1
 * mostAngleFlag: enable (1)/disable (0) angle voting (recognize the whole image using the most likely text orientation); has no effect when doAngleFlag is disabled; default 1
 * maxSideLen: if the image's longest edge exceeds maxSideLen, scale it down proportionally; default 0
* If type is `paddleOcrOnnxV6`
 * Built-in **PP-OCRv6_small** (det/rec ONNX), coexists with `paddleOcrOnnxV5`; cls reuses v5
 * Same parameter fields as `paddleOcrOnnxV5`; set type to `paddleOcrOnnxV6`, e.g.: `{"type":"paddleOcrOnnxV6","numThread":2,"single":0,"matMode":1}`
 * Default models (optional): `PP-OCRv6_small_det.onnx` / `PP-OCRv6_small_rec.onnx` / `ppocrv6_small_labels.txt`
 * **Faster**: for everyday upright UI, use `maxSideLen:640`, `doAngleFlag:0`, `mostAngleFlag:0`
* If type is `tess`
 * Example: set the Tesseract install path and tessdata path,<br/>
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
  ocrType : ONNX_PPOCR_V3, ONNX_PPOCR_V4, NCNN_PPOCR_V3
  serverUrl: Paddle OCR server address; deploy on another PC and connect from the control center; default is http://127.0.0.1:9022; change the IP when deployed elsewhere; keep the port
  padding: white border around the image to improve accuracy; increase when the text box does not cover all text; default 50.<br/>
  maxSideLen: scale by the image's long edge; enlarging increases recognition time but improves accuracy; shrinking reduces time but lowers accuracy; 0 means no scaling.<br/>
  boxScoreThresh: text box confidence threshold; decrease when the text box does not cover all text <br/>
  boxThresh: same purpose; tune experimentally.<br/>
  unClipRatio: multiplier for individual text box size; larger values make each box bigger.<br/>
  doAngleFlag: enable (1)/disable (0) text orientation detection; only needed for upside-down images (rotated 90–270°); default off.<br/>
  mostAngleFlag: enable (1)/disable (0) angle voting; has no effect when doAngleFlag is disabled; default off.<br/>
  front: console (1)/tray mode (0); default off.<br/>
  daemon: keep OCR service process alive (1)/no (0); default off.<br/>
  limit: OCR requests per second; default 1000; lower to reduce CPU usage<br/>
  checkImage: verify input is an image (1 yes, 0 no); default off.<br/>
 ```
* @return `{boolean}` boolean — success or failure






### Tesseract OCR Example {#tess-ocr例子}

```javascript showLineNumbers
function main() {
    let tess = {"type": "tess", "path": "d:/tesseract-ocr/tessdata", "baseDir": "d:\\tesseract-ocr"}
    // ocrlite
    let ocrLite = {
        "type": "ocrLite",
        "baseDir": "c:/ec/OcrLiteNcnn",
        "single": 0,
        "cpuType": "win-lib-cpu-x64"
    }


    // To use paddleOcr, adjust the parameters yourself
    let paddleOcr = {
        "type": "paddleOcr",
        "ocrType": "ONNX_PPOCR_V3"
    }

    let tocr = ocr.newOcr()
    let inited = tocr.initOcr(ocrLite)
    logd("初始化结果 -" + inited);
    if (!inited) {
        loge("error : " + tocr.getErrorMsg());
        return;
    }

    for (var ix = 0; ix < 20; ix++) {

        // Read a bitmap
        let bitmap = image.readBitmap("D:/Screenshot_20210127_152932_com.huawei.android.lau.jpg");
        if (!bitmap) {
            loge("读取图片失败");
            continue;
        }
        console.time("1")
        logd("start---ocr");
        // Recognize the image
        let result = tocr.ocrBitmap(bitmap, 30 * 1000, {"matMode": 1});
        logd(result)
        if (result) {
            logd("ocr结果-》 " + JSON.stringify(result));
            for (var i = 0; i < result.length; i++) {
                var value = result[i];
                logd("文字 : " + value.label + " x: " + value.x + " y: " + value.y + " width: " + value.width + " height: " + value.height);
            }
        } else {
            logw("未识别到结果");
        }

        logd("耗时: " + console.timeEnd(1) + " ms")
        image.recycle(bitmap)
        sleep(1000);
        logd("ix = " + ix)
    }
    // Release all resources
    // paddleOcr closes the OCR process; skip this if you do not need to shut down
    tocr.releaseAll();
}

main();
```




### PaddleOcrOnnx OCR Example {#paddleocronnx-ocr例子}

```javascript showLineNumbers
function main() {
  
    // To use paddleOcrOnnxV4, adjust the parameters yourself
    // You can also change to paddleOcrOnnxV5 to use PPOCR-V5
    let paddleOcrOn = {
        "type": "paddleOcrOnnxV4",
        "numThread":1,
        "single": 0,
        "matMode": 1
    }

    let tocr = ocr.newOcr()
    let inited = tocr.initOcr(paddleOcrOn)
    logd("初始化结果 -" + inited);
    if (!inited) {
        loge("error : " + tocr.getErrorMsg());
        return;
    }

    for (var ix = 0; ix < 20; ix++) {

        // Capture screenshot
        let bitmap = image.captureFullScreen();
        if (!bitmap) {
            loge("读取图片失败");
            continue;
        }
        console.time("1")
        logd("start---ocr "+bitmap);
        // Recognize the image
        let result = tocr.ocrImage(bitmap, 30 * 1000, {"matMode": 1});
        logd(result)
        if (result) {
            logd("ocr结果-》 " + JSON.stringify(result));
            for (var i = 0; i < result.length; i++) {
                var value = result[i];
                logd("文字 : " + value.label +" confidence:"+value.confidence + " range: " + value.x + "," + value.y + "," + (value.width+value.x) + "," + (value.height+value.y));
            }
        } else {
            logw("未识别到结果");
        }

        logd("耗时: " + console.timeEnd(1) + " ms")
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
* Same parameter fields as `paddleOcrOnnxV5`; uses control center built-in models when model paths are omitted
* **Faster**: for everyday upright UI, use `maxSideLen:640`, `doAngleFlag:0`, `mostAngleFlag:0`

```javascript showLineNumbers
function main() {
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

    ocr.releaseAll()
    let tocr = ocr.newOcr()
    let inited = tocr.initOcr(paddleOcrOnnxV6Fast)
    logd("初始化结果 -" + inited)
    if (!inited) {
        loge("error : " + tocr.getErrorMsg())
        return
    }

    for (var ix = 0; ix < 20; ix++) {
        let bitmap = image.captureFullScreen()
        if (!bitmap) {
            loge("读取图片失败")
            continue
        }
        console.time("1")
        let result = tocr.ocrImage(bitmap, 30 * 1000, {
            "padding": 32,
            "numThread": 2,
            "maxSideLen": 640,
            "doAngleFlag": 0,
            "mostAngleFlag": 0
        })
        if (result) {
            logd("ocr结果-》 " + JSON.stringify(result))
            for (var i = 0; i < result.length; i++) {
                var value = result[i]
                logd("文字 : " + value.label
                    + " confidence:" + value.confidence
                    + " range: " + value.x + "," + value.y + ","
                    + (value.x + value.width) + "," + (value.y + value.height))
            }
        } else {
            logw("未识别到结果")
        }
        logd("耗时: " + console.timeEnd(1) + " ms")
        image.recycle(bitmap)
        sleep(2000)
        logd("ix = " + ix)
    }
    tocr.releaseAll()
}

main()
```



### PaddleOcrNcnnV5 OCR Example [after 2.12.0] {#paddleocrncnnv5-ocr例子2120之后}

```javascript showLineNumbers
function main() {
  
    let paddleOcrOn = {
        "type": "paddleOcrNcnnV5",
        "numThread":1,
        "single": 0,
        "matMode": 1
    }

    // Release first to avoid leftover resources from a previous session
    ocr.releaseAll()
    let tocr = ocr.newOcr()
    let inited = tocr.initOcr(paddleOcrOn)
    logd("初始化结果 -" + inited);
    if (!inited) {
        loge("error : " + tocr.getErrorMsg());
        return;
    }

    for (var ix = 0; ix < 20; ix++) {

        // Capture screenshot
        let bitmap = image.captureFullScreen();
        if (!bitmap) {
            loge("读取图片失败");
            continue;
        }
        console.time("1")
        logd("start---ocr "+bitmap);
        // Recognize the image
        let result = tocr.ocrImage(bitmap, 30 * 1000, {"padding": 32,"numThread":1});
        logd(result)
        if (result) {
            logd("ocr结果-》 " + JSON.stringify(result));
            for (var i = 0; i < result.length; i++) {
                var value = result[i];
                logd("文字 : " + value.label +" confidence:"+value.confidence + " range: " + value.x + "," + value.y + "," + (value.width+value.x) + "," + (value.height+value.y));
            }
        } else {
            logw("未识别到结果");
        }

        logd("耗时: " + console.timeEnd(1) + " ms")
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


## ocr.ocrBitmap Recognize Text {#ocrocrbitmap-识别文字}
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

## ocr.ocrImage Recognize Text {#ocrocrimage-识别文字}

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
See common examples in
OCR initialization
```

## ocr.getErrorMsg Get Error Message {#ocrgeterrormsg-获取错误消息}

* Get the OCR error message
*
* @return `{string}` null means no error

```javascript showLineNumbers
See common examples in
OCR initialization
```

## ocr.releaseAll Release OCR Resources {#ocrreleaseall-释放ocr资源}

* Release OCR resources
*
* @return `{bool}` success or failure

```javascript showLineNumbers
See common examples in
OCR initialization
```

