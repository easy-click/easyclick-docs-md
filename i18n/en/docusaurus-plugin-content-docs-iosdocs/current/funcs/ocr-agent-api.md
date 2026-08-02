---
title: OCR Recognition — On-Device Execution
description: EasyClick automation scripts — iOS no jailbreak OCR recognition resource download
keywords:
 - EasyClick automation scripts iOS no jailbreak OCR recognition resource download
 - OCR
 - ocrAgent
 - initOcr
 - appleVision
 - releaseAll
 - ocrImage
 - ocr
 - getErrorMsg
 - newOcr
 - 9.0.0
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview {#说明}

- The OCR module performs image recognition
- The OCR module uses the `ocrAgent` prefix, e.g. `ocrAgent.initOcr()`
- Currently includes appleVision

:::tip

- This module runs on the device; data is stored on the device
- 9.0.0+ adds multi-instance OCR functions supporting appleVision, ocrlite, and paddliteOnnxOcr — see newOcr
- 9.8.0+ adds paddleOcrNcnnV5 support
:::

## ocrAgent.releaseAll Release All Instances {#ocragentreleaseall-释放所有实例}

* Release all instances
* Requires EC 9.0.0+

```javascript
// See code examples below
```

## ocrAgent.initOcr Initialize {#ocragentinitocr-初始化}

* Initialize the OCR module
* @param map Parameter map with the following keys:
* type: OCR type; appleVision = iOS built-in Vision module
* For appleVision, use: ```{"type":"appleVision","level":"fast","languages":"zh-Hans,en-US"}```<br/>
* level: fast = fast mode, accurate = accurate mode
* languages: Recognition languages; default zh-Hans,en-US (Simplified Chinese and English)
* Supported: ```["en-US", "fr-FR", "it-IT", "de-DE", "es-ES", "pt-BR", "zh-Hans", "zh-Hant"]```
* @return `{bool}` true on success, false on failure

- appleVision OCR example

```javascript showLineNumbers
function main() {

 let appleVision = {
 "type": "appleVision"
 }
 ocrAgent.releaseAll()
 let inited = ocrAgent.initOcr(appleVision)
 logd("init result -" + inited);
 if (!inited) {
 loge("error : " + ocrAgent.getErrorMsg());
 return;
 }

 for (var ix = 0; ix < 20; ix++) {

 // Read bitmap
 let img = imageAgent.captureFullScreen();
 if (!img) {
 loge("failed to read image");
 continue;
 }
 console.time("1")
 logd("start---ocr");
 // Recognize image
 let result = ocrAgent.ocrImage(img, 20 * 1000, {});
 logd(result)
 if (result) {
 logd("ocr result -> " + JSON.stringify(result));
 for (var i = 0; i < result.length; i++) {
 var value = result[i];
 logd("text : " + value.label + " x: " + value.x + " y: " + value.y + " width: " + value.width + " height: " + value.height);
 }
 } else {
 logw("no result");
 }

 logd("elapsed: " + console.timeEnd(1) + " ms")
 imageAgent.recycle(img)
 sleep(1000);
 logd("ix = " + ix)
 }
 // Release all resources
 ocrAgent.releaseAll();
}

main();
```

## ocrAgent.ocrImage Recognize Text {#ocragentocrimage-识别文字}

* Perform OCR on an AutoImage; returns JSON data like:

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
* @param bitmap Image
* @param timeout Timeout in milliseconds
* @param extra Extra parameters as a map, e.g. ```{"token":"xxx"}```
* @return `{json}` JSON object

```javascript showLineNumbers
See OCR initialization examples
```

## ocr.getErrorMsg Get Error Message {#ocrgeterrormsg-获取错误消息}

* Get OCR error message
*
* @return `{string}` null means no error

```javascript showLineNumbers
See OCR initialization examples
```

## ocrAgent.releaseAllEx Release OCR Resources {#ocragentreleaseallex-释放ocr资源}

* Release OCR resources
*
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
See OCR initialization examples
```

## Multi-Instance OCR Mode {#多实例ocr模式}

### ocrAgent.newOcr Create Instance {#ocragentnewocr-实例化}

* Create an OCR instance
* @return `{OcrInstAgent|null}`

```javascript showLineNumbers
See OCR initialization examples
```

### initOcr Initialize OCR {#initocr-初始化ocr}

* Initialize the OCR module
* @param map Parameter map with the following keys:<br/>
* type: OCR type — paddleOcrNcnnV5 = NCNN PPOCR-v5, ocrLite = ocrLite, paddleLiteOcr = paddleLite, appleVision = iOS Vision module<br/>
* For `appleVision`:
 * Use: `{"ocrType":"appleVision","level":"fast","languages":"zh-Hans,en-US"}`<br/>
 * level: fast = fast mode, accurate = accurate mode
 * languages: Recognition languages; default zh-Hans,en-US (Simplified Chinese and English)
 * Supported: ["en-US", "fr-FR", "it-IT", "de-DE", "es-ES", "pt-BR", "zh-Hans", "zh-Hant"]
* For paddleNcnnOcrV5:
 * numThread: Thread count; -1 = all, -2 = half of device CPUs, 0 = unset. Adjust for recognition speed
 * modelsDir: Model directory path; omit to use built-in models
 * padding: White border around image to improve recognition when text boxes are incomplete; default 32
 * maxSideLen If the longest image side exceeds max_side_len, scale it down proportionally; default 640
 * keysName: Label file name, e.g. keys.txt; omit for built-in models
 * detName: Detection model file name (param suffix) under modelsDir; e.g. det.param → write det
 * recName: Recognition model file name (onnx suffix) under modelsDir; e.g. rec.param → write rec
* For `paddleLiteOcr` (paddleLiteOcr = paddleLite; paddleOnnxOcr = onnxruntime PPOCR-v5; onnxruntime and paddlelite conflict, so onnx reimplements paddlelite):
 * Use: `{"ocrType":"paddleLiteOcr","cpuThreadNum":2}`
 * cpuThreadNum: CPU threads; omit if unknown. -1 = all CPUs, -2 = half
 * modelPath: Model path; external e.g. /sdcard/models/; omit for built-in models
 * labelPath: Label file path, e.g. /sdcard/labels/ppocr_keys_v1.txt; omit for built-in models
 * detModelFilename: Detection model filename (.onnx) under modelPath; omit for built-in models
 * recModelFilename: Recognition model filename (.onnx) under modelPath; omit for built-in models
 * clsModelFilename: Classification model filename (.onnx) under modelPath; omit for built-in models
 * padding: White border around image; default 10
 * boxThresh Text/background segmentation threshold [0,1]; default 0.3
 * boxScoreThresh Detection box retention threshold [0,1]; default 0.5
 * unClipRatio Text box size multiplier [1.6, 2.0]; default 1.6
 * doAngle: 1 enable / 0 disable text orientation detection; default 1
 * mostAngle: 1 enable / 0 disable angle voting; default 1
 * maxSideLen: Scale longest side; default 960
* For `ocrLite`:
 * Use: `{"ocrType":"ocrLite","numThread":2,"padding":10,"maxSideLen":0}`<br/>
 * numThread: Thread count<br/>
 * padding: Add white border around image to improve recognition<br/>
 * maxSideLen: Scale by longest side; 0 = no scaling. E.g. 1024 scales down if longer than 1024; scales up to 32 if shorter than 32<br/>
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function usbagentocr() {
 ocrAgent.releaseAll()
 let ocrInstance = ocrAgent.newOcr();
 logd(ocrInstance.ocrId)
 // numThread should not be too large or recognition may fail
 let ocrLiteConfig = {"ocrType": "ocrLite", "numThread": 2, "padding": 10, "maxSideLen": 960}
 // apple vision config
 let appleVisionConfig = {"ocrType": "appleVision", "level": "accurate", "languages": "zh-Hans,en-US"}
 // paddleOnnxOcr config
 let paddleOnnxOcrConfig = {"ocrType": "paddleOnnxOcr", "cpuThreadNum": 2, "padding": 10, "maxSideLen": 960}

 // For custom paddleonnx models, upload files to agent and pass returned paths
 // let keysFilePath = utils.uploadAgentFile("/Volumes/dev/ppocrv5_mobile_labels.txt", "ppocrv5_mobile_labels.txt")
 // let clsFilePath = utils.uploadAgentFile("/Volumes/dev/ch_ppocr_mobile_v2.0_cls_infer.onnx", "cls.txt")
 // let detFilePath = utils.uploadAgentFile("/Volumes/dev/ch_PP-OCRv5_mobile_det.onnx", "det.txt")
 // let recFilePath = utils.uploadAgentFile("/Volumes/dev/ch_PP-OCRv5_rec_mobile_infer.onnx", "rec.txt")
 // logd("custom keysFilePath " + keysFilePath)
 // logd("custom clsFilePath " + clsFilePath)
 // logd("custom detFilePath " + detFilePath)
 // logd("custom recFilePath " + recFilePath)
 // modelPath can be the labels path
 // paddleOnnxOcrConfig["modelPath"] = keysFilePath
 // paddleOnnxOcrConfig["labelPath"] = keysFilePath
 // paddleOnnxOcrConfig["clsModelFilename"] = clsFilePath
 // paddleOnnxOcrConfig["detModelFilename"] = detFilePath
 // paddleOnnxOcrConfig["recModelFilename"] = recFilePath


 let initResult = ocrInstance.initOcr(ocrLiteConfig)


 logd("initResult " + initResult)
 if (!initResult) {
 logd("errorMsg:" + ocrInstance.getErrorMsg())
 } else {
 for (let i = 0; i < 10; i++) {
 let img = imageAgent.captureFullScreen();
 logd("img " + img)
 if (img) {
 console.time(1)
 let result = ocrInstance.ocrImage(img, 20000, {});
 let goTime = console.timeEnd(1)
 logd("elapsed " + goTime + " result " + JSON.stringify(result))
 if (result) {
 for (let j = 0; j < result.length; j++) {
 let rs = result[j];
 console.log("label: {} confidence:{} range: {},{},{},{}", rs.label, rs.confidence, rs.x, rs.y, (rs.x + rs.width), (rs.y + rs.height))
 }
 }
 }

 imageAgent.recycle(img)
 }
 }
 ocrInstance.releaseAll();
}
usbagentocr()
```

### PaddleNcnnOcrV5 Example {#paddlencnnocrv5例子}

```javascript showLineNumbers
function usbagentocr() {
 ocrAgent.releaseAll()
 let ocrInstance = ocrAgent.newOcr();
 logd(ocrInstance.ocrId)
 // config
 let paddleNcnnOcrConfig = {"ocrType": "paddleNcnnOcrV5", "cpuThreadNum": 2, "padding": 32, "maxSideLen": 640}

 // For custom paddleNcnnOcrV5 models, upload files to agent and pass returned paths
 // let keysFilePath = utils.uploadAgentFile("/Volumes/dev/keys.txt", "keys.txt")
 // let detParamFilePath = utils.uploadAgentFile("/Volumes/dev/det.ncnn.param", "det.ncnn.param")
 // let detBinFilePath = utils.uploadAgentFile("/Volumes/dev/det.ncnn.bin", "det.ncnn.bin")
 // let recParamFilePath = utils.uploadAgentFile("/Volumes/dev/rec.ncnn.param", "rec.ncnn.param")
 // let recBinFilePath = utils.uploadAgentFile("/Volumes/dev/rec.ncnn.bin", "rec.ncnn.bin")
 // logd("custom keysFilePath " + keysFilePath)
 // logd("custom detParamFilePath " + detParamFilePath)
 // logd("custom detBinFilePath " + detBinFilePath)
 // logd("custom recParamFilePath " + recFilePath)
 // logd("custom recBinFilePath " + recBinFilePath)
 // modelDirs can be the labels path
 // paddleOnnxOcrConfig["modelDirs"] = keysFilePath
 // paddleOnnxOcrConfig["keysName"] = keys.txt
 // paddleOnnxOcrConfig["detName"] = "det.ncnn"
 // paddleOnnxOcrConfig["recName"] = "rec.ncnn"


 let initResult = ocrInstance.initOcr(paddleNcnnOcrConfig)

 logd("initResult " + initResult)
 if (!initResult) {
 logd("errorMsg:" + ocrInstance.getErrorMsg())
 } else {
 for (let i = 0; i < 10; i++) {
 let img = imageAgent.captureFullScreen();
 logd("img " + img)
 if (img) {
 console.time(1)
 let result = ocrInstance.ocrImage(img, 20000, {"paddinng":32,"numThread":1});
 let goTime = console.timeEnd(1)
 logd("elapsed " + goTime + " result " + JSON.stringify(result))
 if (result) {
 for (let j = 0; j < result.length; j++) {
 let rs = result[j];
 console.log("label: {} confidence:{} range: {},{},{},{}", rs.label, rs.confidence, rs.x, rs.y, (rs.x + rs.width), (rs.y + rs.height))
 }
 }
 }

 imageAgent.recycle(img)
 }
 }
 ocrInstance.releaseAll();
}
usbagentocr()
```

### ocrImage Recognize Image {#ocrimage-获取错误信息}

* Perform OCR on an image
* @param img AutoImage object
* @param timeout Timeout in milliseconds
* @param extra JSON parameters; default `{}`
* @return `{null|JSON}` JSON object

```javascript showLineNumbers
See OCR initialization examples
```

### getErrorMsg Get Error Message {#geterrormsg-获取错误信息}

* Get error message
* @return `{string}` null means no error

```javascript showLineNumbers
See OCR initialization examples
```

### releaseAll Release OCR Resources {#releaseall-获取错误信息}

* Release OCR resources
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
See OCR initialization examples
```
