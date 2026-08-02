---
title: OCR Recognition
description: EasyClick automation scripts — iOS no jailbreak — OCR recognition — resource download
keywords:
 - EasyClick automation scripts iOS no jailbreak OCR recognition resource download
 - OCR
 - ocr
 - initOcr
 - appleVision
 - ocrImage
 - ocrMut
 - releaseAll
 - map
 - getErrorMsg
 - TesseractOcr
 - paddleOnnxOcr
 - paddleOnnxOcrV6
 - paddleNcnnOcrV5
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---


:::tip

- The OCR module performs text recognition on images
- The OCR module uses the `ocr` prefix, e.g. `ocr.initOcr()`
- Current OCR engines include `appleVision`
- 3.18.0+ adds `ocrMut` as the multi-instance OCR prefix
- 5.12.0 adds `paddleLiteOcr`
- 5.21.0 adds `paddleNcnnOcrV5`
- Adds `paddleOnnxOcrV6` (PP-OCRv6_small, coexists with `paddleOnnxOcr` / PP-OCRv5)
- Languages: both `paddleOnnxOcr` (v5) and `paddleOnnxOcrV6` (v6 Medium/Small) can recognize Simplified/Traditional Chinese, English, Japanese, and many Latin-script languages in a single model — usually you do not need to switch models by language (v6 officially supports ~50 languages; v5 also covers many Latin scripts in practice). Cyrillic scripts (e.g. Ukrainian) are not supported and cannot be recognized correctly
- Speed: `paddleOnnxOcrV6` defaults to accuracy-oriented settings (`maxSideLen` 960, angle detection enabled). For speed, use the "fast" parameters below (lower `maxSideLen`, disable `doAngle`, increase thread count) — usually much faster; enable angle detection only when the image may be upside down
:::

## Single-Instance Mode {#单实例模式}

### ocr.initOcr Initialize {#ocrinitocr-初始化}

* Initialize the OCR module
* @param map map parameters:
* keys:
* type: OCR type — `appleVision` = iOS built-in Vision module
* For `appleVision`, set parameters to: ```{"type":"appleVision","level":"fast","languages":"zh-Hans,en-US"}```<br/>
  * level: `fast` = fast, `accurate` = accurate
  * languages: recognition languages; default is `zh-Hans,en-US` (Simplified Chinese and English)
  * Supported: ```["en-US", "fr-FR", "it-IT", "de-DE", "es-ES", "pt-BR", "zh-Hans", "zh-Hant"]```
* @return `{bool}` boolean — success or failure

- appleVision OCR example

```javascript showLineNumbers
function main() {
 let appleVision = {"type": "appleVision", "level": "accurate", "languages": "zh-Hans,en-US"}
 let inited = ocr.initOcr(appleVision)
 logd("Init result -" + inited);
 if (!inited) {
 loge("error : " + ocr.getErrorMsg());
 return;
 }
 for (var ix = 0; ix < 20; ix++) {
 // Read a bitmap
 let img = image.captureFullScreen();
 if (img == null || img == undefined || img.uuid == null || img.uuid == undefined || img.uuid == "") {
 loge("Failed to read image");
 continue;
 }
 console.time("1")
 logd("start---ocr");
 // Recognize the image
 let result = ocr.ocrImage(img, 20 * 1000, {});
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
 image.recycle(img)
 sleep(1000);
 logd("ix = " + ix)
 }
 // Release all resources
 ocr.releaseAll();
}

main();
```

### ocr.ocrImage Recognize Text {#ocrocimage-识别文字}

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
* @return `{JSON}` JSON object

```javascript showLineNumbers
See common code examples
OCR initialization
```

### ocr.getErrorMsg Get Error Message {#ocrgeterrormsg-获取错误消息}

* Get OCR error message
*
* @return `{string}` `null` means no error

```javascript showLineNumbers
See common code examples
OCR initialization
```

### ocr.releaseAll Release OCR Resources {#ocrreleaseall-释放ocr资源}

* Release OCR resources
*
* @return `{bool}` success or failure

```javascript showLineNumbers
See common code examples
OCR initialization
```

## Multi-Instance Mode {#多实例模式}

### ocrMut.releaseAll Release All {#ocrmutreleaseall-释放所有}
* EC standalone 5.17.0+
```javascript
// See code examples
```

### ocrMut.initOcr Initialize {#ocrmutinitocr-初始化}

* Initialize the OCR module
* @param map map parameters:
* keys:
* type: OCR type — `appleVision` = iOS built-in Vision module, `tess` = TesseractOcr, `ocrLite` = ncnn neural network ocrLite,
  `paddleOcrOnline` = EC bundled PC-side PaddleOCR service, `paddleLiteOcr` = PaddleLite, `paddleNcnnOcrV5` = ncnn PaddleOCR,
  `paddleOnnxOcr` = onnxruntime PP-OCRv5 implementation, `paddleOnnxOcrV6` = onnxruntime PP-OCRv6_small implementation (coexists with v5)
* For `appleVision`, set parameters to: ```{"type":"appleVision","level":"fast","languages":"zh-Hans,en-US"}```<br/>
  * level: `fast` = fast, `accurate` = accurate<br/>
  * languages: recognition languages; default is `zh-Hans,en-US` (Simplified Chinese and English)<br/>
  * Supported: ```["en-US", "fr-FR", "it-IT", "de-DE", "es-ES", "pt-BR", "zh-Hans", "zh-Hant"]```<br/>
    `paddleLiteOcr` = PaddleLite, `paddleOnnxOcr` = onnxruntime PP-OCRv5, `paddleOnnxOcrV6` = PP-OCRv6_small
 
* For `paddleLiteOcr` / `paddleOnnxOcr` / `paddleOnnxOcrV6`:
  * Note: since 5.16+, due to onnxruntime and PaddleLite conflicts, PaddleLite was reimplemented with onnx
  * Example parameters: `{"type":"paddleOnnxOcr","cpuThreadNum":2}` or `{"type":"paddleOnnxOcrV6","cpuThreadNum":2}`
  * **Languages (paddleOnnxOcr / paddleOnnxOcrV6)**: a single model recognizes Simplified/Traditional Chinese, English, Japanese, and many Latin-script languages — usually you do not need to switch models by language. v6 Medium/Small officially supports ~50 languages (including 46 Latin-script languages); v5 also recognizes many Latin scripts in practice. **Does does not support** Cyrillic scripts (e.g. Ukrainian) — cannot recognize correctly
  * **Fast configuration (recommended for daily UI screenshots)**: faster than defaults, e.g.
    `{"type":"paddleOnnxOcrV6","cpuThreadNum":-2,"maxSideLen":640,"doAngle":0,"mostAngle":0,"padding":10}`
    - `maxSideLen:640`: lower detection resolution (default 960) — usually the biggest speed gain; small text may be missed slightly
    - `doAngle:0` / `mostAngle:0`: disable orientation detection and angle voting; set to 1 only when the image may be upside down (~90°–270°)
    - `cpuThreadNum:-2` or `-1`: use more CPU threads (default is often 2)
  * cpuThreadNum: CPU thread count; omit if unsure — `-1` = all CPUs, `-2` = half of CPUs; adjust for recognition speed
  * modelPath: model path; external path e.g. `/sdcard/models/` means under sdcard; built-in models are used by default — omit this field
  * labelPath: training label file path; can be external e.g. `/sdcard/labels/ppocr_keys_v1.txt`; built-in models are used by default — omit this field (v6 exports dictionary from rec yml in Bundle when built-in; usually no manual file needed)
  * detModelFilename: detection model filename (.onnx), placed under `modelPath`; built-in models used by default — omit this field
  * recModelFilename: recognition model filename (.onnx), placed under `modelPath`; built-in models used by default — omit this field
  * clsModelFilename: classification model filename (.onnx), placed under `modelPath`; built-in models used by default — omit this field (v6 angle classification reuses v5 cls model)
  * padding: white border around image to improve recognition; increase when text boxes do not fully enclose text. Default 10. Can affect recognition speed
  * boxThresh: threshold separating text from background; higher values shrink the text region. Range [0, 1], default 0.3
  * boxScoreThresh: threshold for keeping detected text boxes; higher values mean lower recall. Range [0, 1], default 0.5
  * unClipRatio: controls detected text box size; larger values produce bigger boxes. Range [1.6, 2.0], default 1.6
  * doAngle: enable (1) / disable (0) text orientation detection; needed only for upside-down images (rotated 90°–270°). Default 1
  * mostAngle: enable (1) / disable (0) angle voting (recognize entire image in the most likely text orientation); has no effect when orientation detection is disabled. Default 1
  * maxSideLen: if the image's longest side exceeds `max_side_len`, scale down proportionally to `max_side_len`. Default 960; adjust for speed (640 for fast mode)
* For `paddleNcnnOcrV5`, parameters:
  * numThread: thread count — `-1` = all, `-2` = half of device CPUs, `0` = not set; adjust for recognition speed
  * modelsDir: model directory path; omit to use built-in models
  * padding: white border around image to improve recognition; increase when text boxes do not fully enclose text. Default 32; can affect speed
  * maxSideLen: if the image's longest side exceeds `max_side_len`, scale down proportionally. Default 640; adjust for speed
  * keysName: training label filename; can be external e.g. `keys.txt`; built-in models used by default — omit this field
  * detName: detection model filename (.param), placed under `modelsDir`; if named `det.param`, use `det` without the `.param` suffix; built-in models used by default — omit this field
  * recName: recognition model filename (.param), placed under `modelsDir`; if named `rec.param`, use `rec` without the `.param` suffix; built-in models used by default — omit this field
* For type `tess`
  * Set parameters to: `{"type":"tess",path:"","rillevel":2,"language":"eng+chi_sim"}`<br/>
  * path: TesseractOcr `traineddata` folder path, e.g. `/Application/tessdata`; best to copy to sandbox with the file module — see example
  * language: TesseractOcr language dataset file to recognize, e.g. `chi_sim.traineddata` = Simplified Chinese, parameter value is
    `chi_sim`; multiple languages joined with `+`, e.g. `chi_sim+eng+num`; e.g. `eng+chi_sim` = TesseractOcr official English and Simplified Chinese; auto-finds `traineddata` files under `path`
  * rilLevel: PageIteratorLevel parameter — `-1` adaptive, `0`: RIL_BLOCK, `1`: RIL_PARA, `2`: RIL_TEXTLINE, `3`: RIL_WORD, `4`: RIL_SYMBOL
  * ocrEngineMode: recognition engine type — `0` OEM_TESSERACT_ONLY, `1` OEM_LSTM_ONLY, `2` OEM_TESSERACT_LSTM_COMBINED, `3` OEM_DEFAULT
  * tessedit_char_blacklist: blacklist
  * tessedit_char_whitelist: whitelist

* For type `ocrLite`, parameters:<br/>

```json showLineNumbers
 {
 "type": "ocrLite",
 "padding": 10,
 "maxSideLen": 0,
 "boxScoreThresh": 0.6,
 "boxThresh": 0.3,
 "unClipRatio": 1.6,
 "doAngle": 0,
 "mostAngle": 0
}
```

```text
* numThread: thread count — `-1` = all, `-2` = half of device CPUs, `0` = not set (program decides); `-2` recommended<br/>
* padding: white border around image to improve recognition; increase when text boxes do not fully enclose text. Default 50.<br/>
* maxSideLen: scale by longest image side; larger = slower but more accurate, smaller = faster but less accurate; `0` = no scaling.<br/>
* boxScoreThresh: text box confidence threshold; decrease when text boxes do not fully enclose text<br/>
* boxThresh: same as above; tune experimentally.<br/>
* unClipRatio: single text box size multiplier; larger values produce bigger boxes.<br/>
* doAngle: enable (1) / disable (0) text orientation detection; needed only for upside-down images (rotated 90°–270°). Default off.<br/>
* mostAngle: enable (1) / disable (0) angle voting; has no effect when orientation detection is disabled. Default off.<br/>
```

* For type `paddleOcrOnline`, download **EasyClick-PaddleOcr.zip from cloud storage, extract and run**<br/>

```json showLineNumbers
 {
 "type": "paddleOcrOnline",
 "ocrType": "ONNX_PPOCR_V3",
 "padding": 50,
 "maxSideLen": 0,
 "boxScoreThresh": 0.5,
 "boxThresh": 0.3,
 "unClipRatio": 1.6,
 "doAngleFlag": 0,
 "mostAngleFlag": 0
}
```

```text
* ocrType: model — ONNX_PPOCR_V3, ONNX_PPOCR_V4, NCNN_PPOCR_V3
* serverUrl: Paddle OCR server address; can deploy on another PC and connect from control center, e.g. 192.168.2.8:9022; change IP when deployed on PC; port 9022 is optional
* padding: white border around image to improve recognition; increase when text boxes do not fully enclose text. Default 50.<br/>
* maxSideLen: scale by longest image side; larger = slower but more accurate, smaller = faster but less accurate; `0` = no scaling.<br/>
* boxScoreThresh: text box confidence threshold; decrease when text boxes do not fully enclose text<br/>
* boxThresh: same as above; tune experimentally.<br/>
* unClipRatio: single text box size multiplier; larger values produce bigger boxes.<br/>
* doAngleFlag: enable (1) / disable (0) text orientation detection; needed only for upside-down images (rotated 90°–270°). Default off.<br/>
* mostAngleFlag: enable (1) / disable (0) angle voting; has no effect when orientation detection is disabled. Default off.<br/>
* limit: OCR requests per second; default 1000. Lower to reduce CPU usage<br/>
* checkImage: verify data is an image (1 yes, 0 no); default off.<br/>
```

* @return `{bool}` boolean — success or failure

```javascript showLineNumbers
function main() {
 // Release all OCR resources at start to avoid leaks from previous runs
 ocrMut.releaseAll();
 logd("Start script...")

 // Initialize an instance
 let ocrtest = ocrMut.newOcr();
 let vision = {"type": "appleVision", "level": "accurate", "languages": "zh-Hans,en-US"}
 // paddleOcr parameters
 let paddleOcrOnline = {
 "type": "paddleOcrOnline",
 "ocrType": "ONNX_PPOCR_V3",
 "serverUrl": "192.168.2.13:9022",
 "limit": 12,
 "checkImage": "1",
 "padding": 200
 }
 let ocrLite = {"type": "ocrLite","numThread":2}
 let paddleLiteOcrMap = {"type": "paddleLiteOcr","cpuThreadNum":2,"cpuPowerMode":"LITE_POWER_FULL"}
 let inited = ocrtest.initOcr(ocrLite)
 // let inited = ocrtest.initOcr(paddleLiteOcrMap)
 if (!inited) {
 loge("inited ocr error : " + ocrtest.getErrorMsg())
 return
 } else {
 logd("ocr inited ok")
 }
 for (let i = 0; i < 3; i++) {
 let img = image.captureFullScreen()
 let ocrResult = ocrtest.ocrImage(img, 20000, null)
 logd("ocrResult " + JSON.stringify(ocrResult));
 if (ocrResult) {
 logd("OCR result -> " + JSON.stringify(ocrResult));
 for (var j = 0; j < ocrResult.length; j++) {
 var value = ocrResult[j];
 logd("Text : " + value.label + " x: " + value.x + " y: " + value.y + " width: " + value.width + " height: " + value.height);
 }
 } else {
 logw("No result recognized");
 }
 image.recycle(img)

 sleep(2000)
 }
 // Release when script finishes; no need to release after every use
 ocrtest.releaseAll()
}

main();
```

### TesseractOcr Example {#tesseractocr-例子}

```javascript showLineNumbers
function main() {
 // Release all OCR resources at start to avoid leaks from previous runs
 ocrMut.releaseAll();
 logd("Start")
 let ts = file.getSandBoxDir()
 let tessdataDir = ts + "/tessdata"
 logd("tessdataDir=> ", tessdataDir)
 file.mkdirs(tessdataDir)
 // Save traineddata datasets to res folder; auto-copied to tessdataDir
 // Copy English dataset
 let saved = saveResToFile("eng.traineddata", tessdataDir + "/eng.traineddata")
 // Copy Chinese dataset
 let saved2 = saveResToFile("chi_sim.traineddata", tessdataDir + "/chi_sim.traineddata")
 logd("saved ", saved)
 if (!saved) {
 logd("copy eng error")
 return;
 }
 if (!saved2) {
 logd("copy chi_sim error")
 return;
 }
 let tessocr = ocrMut.newOcr()
 // Initialize Chinese + English
 let intx = tessocr.initOcr({
 "type": "ocrLite",
 "path": tessdataDir,
 "language": "eng+chi_sim",
 "rilLevel": 2,
 })

 logd("tessocr initOcr " + intx)
 if (!intx) {
 logd(tessocr.getErrorMsg());
 return
 }

 for (let i = 0; i < 10; i++) {
 // Can use screenshot here instead
 //let aa = image.captureFullScreen();
 let aa = readResAutoImage("2.png")

 console.time(1)
 let rse = tessocr.ocrImage(aa, 10 * 1000, {})
 logd("Elapsed time - ", console.timeEnd(1))
 image.recycle(aa)

 logd(JSON.stringify(rse));
 for (let i = 0; i < rse.length; i++) {
 let a = rse[i]
 logd(JSON.stringify(a))
 let b = a.x + "," + a.y + "," + (a.x + a.width) + "," + (a.y + a.height)
 logd(a.label, b)
 }

 }

 tessocr.releaseAll()

 logd("end--")
}


main();
```



### PaddleOnnxOcr Example {#paddleonnxocr-例子}

```javascript showLineNumbers
function main() {
 // Release all OCR resources at start to avoid leaks from previous runs
 ocrMut.releaseAll();
 logd("Start script...")
 let ocrtest = ocrMut.newOcr()
 let modelPath = file.getSandBoxFilePath("")
 let labelPath = file.getSandBoxFilePath("ppocrv5_mobile_labels.txt")
 let detModelFilename = "ch_PP-OCRv5_mobile_det.onnx"
 let recModelFilename = "ch_PP-OCRv5_rec_mobile_infer.onnx"
 let clsModelFilename = "ch_ppocr_mobile_v2.0_cls_infer.onnx"
 // Built-in OCR model
 let paddleOnnxOcrMap1 = {"type": "paddleOnnxOcr", "cpuThreadNum": 2, "padding": 10, "maxSideLen": 960}

 // External model
 let paddleOnnxOcrMap2 = {
 "type": "paddleOnnxOcr", "cpuThreadNum": 2, "padding": 10, "maxSideLen": 960,
 "modelPath": modelPath,
 "labelPath": labelPath,
 "detModelFilename": detModelFilename,
 "recModelFilename": recModelFilename,
 "clsModelFilename": clsModelFilename,
 }
 let inited = ocrtest.initOcr(paddleOnnxOcrMap1)
 if (!inited) {
 loge("inited ocr error : " + ocrtest.getErrorMsg())
 return
 } else {
 logd("ocr inited ok")
 }
 for (let i = 0; i < 100; i++) {
 let img = image.captureFullScreen()


 console.time(1)

 let ocrResult = ocrtest.ocrImage(img, 20000, {"cpuThreadNum": 2, "padding": 10, "maxSideLen": 960})

 logd("ocrResult " + JSON.stringify(ocrResult));
 if (ocrResult) {
 logd("OCR result -> " + JSON.stringify(ocrResult));
 for (var j = 0; j < ocrResult.length; j++) {
 var value = ocrResult[j];
 logd("Text : " + value.label + " " + value.x + "," + value.y + "," + (value.x + value.width) + "," + (value.y + value.height));
 }
 } else {
 logw("No result recognized");
 }
 logd("Elapsed: {} ms", console.timeEnd(1))
 sleep(100)
 image.recycle(img)
 sleep(200)
 }
 // Release when script finishes; no need to release after every use
 ocrtest.releaseAll()
}


main()
```



### PaddleOnnxOcrV6 Example {#paddleonnxocrv6-例子}

* Use type `paddleOnnxOcrV6`; built-in PP-OCRv6_small (det/rec ONNX); coexists with `paddleOnnxOcr` (v5) without conflict
* Parameter fields same as `paddleOnnxOcr`; uses App Bundle built-in models when `modelPath` / filenames are omitted
* **Languages**: similar to `paddleOnnxOcr` (v5) — single model recognizes Simplified/Traditional Chinese, English, Japanese, and many Latin-script languages (v6 Medium/Small officially ~50 languages); Cyrillic scripts like Ukrainian are not supported
* **Fast mode**: for upright daily UI screenshots, use `maxSideLen:640`, `doAngle:0`, `mostAngle:0`, `cpuThreadNum:-2` (see `paddleOnnxOcrV6MapFast` below); use defaults / enable angle detection when higher detection accuracy is needed or the image may be upside down

```javascript showLineNumbers
function main() {
 // Release all OCR resources at start to avoid leaks from previous runs
 ocrMut.releaseAll();
 logd("Start script...")
 let ocrtest = ocrMut.newOcr()
 let modelPath = file.getSandBoxFilePath("")
 let labelPath = file.getSandBoxFilePath("ppocrv6_small_labels.txt")
 let detModelFilename = "PP-OCRv6_small_det.onnx"
 let recModelFilename = "PP-OCRv6_small_rec.onnx"
 // v6 angle classification reuses v5 cls
 let clsModelFilename = "ch_ppocr_mobile_v2.0_cls_infer.onnx"

 // Fast mode (recommended for daily UI screenshots): smaller longest side, angle detection off, more CPU
 let paddleOnnxOcrV6MapFast = {
 "type": "paddleOnnxOcrV6",
 "cpuThreadNum": -2,
 "padding": 10,
 "maxSideLen": 640,
 "doAngle": 0,
 "mostAngle": 0
 }

 // Default accuracy-oriented: maxSideLen 960, orientation detection enabled
 let paddleOnnxOcrV6Map1 = {"type": "paddleOnnxOcrV6", "cpuThreadNum": 2, "padding": 10, "maxSideLen": 960}

 // External model (place onnx / dictionary in sandbox yourself)
 let paddleOnnxOcrV6Map2 = {
 "type": "paddleOnnxOcrV6", "cpuThreadNum": 2, "padding": 10, "maxSideLen": 960,
 "modelPath": modelPath,
 "labelPath": labelPath,
 "detModelFilename": detModelFilename,
 "recModelFilename": recModelFilename,
 "clsModelFilename": clsModelFilename,
 }
 let inited = ocrtest.initOcr(paddleOnnxOcrV6MapFast)
 if (!inited) {
 loge("inited ocr error : " + ocrtest.getErrorMsg())
 return
 } else {
 logd("ocr v6 inited ok")
 }
 for (let i = 0; i < 100; i++) {
 let img = image.captureFullScreen()

 console.time(1)

 let ocrResult = ocrtest.ocrImage(img, 20000, {"cpuThreadNum": -2, "padding": 10, "maxSideLen": 640, "doAngle": 0, "mostAngle": 0})

 logd("ocrResult " + JSON.stringify(ocrResult));
 if (ocrResult) {
 logd("OCR result -> " + JSON.stringify(ocrResult));
 for (var j = 0; j < ocrResult.length; j++) {
 var value = ocrResult[j];
 logd("Text : " + value.label + " " + value.x + "," + value.y + "," + (value.x + value.width) + "," + (value.y + value.height));
 }
 } else {
 logw("No result recognized");
 }
 logd("Elapsed: {} ms", console.timeEnd(1))
 sleep(100)
 image.recycle(img)
 sleep(200)
 }
 // Release when script finishes; no need to release after every use
 ocrtest.releaseAll()
}


main()
```



### PaddleNcnnOcrV5 Example {#paddlencnnocrv5-例子}

```javascript showLineNumbers
function main() {
 // Release all OCR resources at start to avoid leaks from previous runs
 ocrMut.releaseAll();
 logd("Start script...")
 let ocrtest = ocrMut.newOcr()
 let modelPath = file.getSandBoxFilePath("")
 let detName = "det"
 let recName = "rec"
 // Built-in OCR model
 let paddleNcnnOcrMap1 = {"type": "paddleNcnnOcrV5", "numThread": 2, "padding": 32, "maxSideLen": 640}

 // External model
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
 for (let i = 0; i < 100; i++) {
 let img = image.captureFullScreen()


 console.time(1)

 // Dynamic parameters can also be set here
 let ocrResult = ocrtest.ocrImage(img, 20000, {"numThread":2,"padding":32})

 logd("ocrResult " + JSON.stringify(ocrResult));
 if (ocrResult) {
 logd("OCR result -> " + JSON.stringify(ocrResult));
 for (var j = 0; j < ocrResult.length; j++) {
 var value = ocrResult[j];
 logd("Text : " + value.label + " " + value.x + "," + value.y + "," + (value.x + value.width) + "," + (value.y + value.height));
 }
 } else {
 logw("No result recognized");
 }
 logd("Elapsed: {} ms", console.timeEnd(1))
 sleep(100)
 image.recycle(img)
 sleep(200)
 }
 // Release when script finishes; no need to release after every use
 ocrtest.releaseAll()
}


main()
```


### ocrInstance.ocrImage Recognize Text {#ocrinstanceocrimage-识别文字}

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
* @return `{JSON}` JSON object

```javascript showLineNumbers
See common code examples
OCR initialization
```

### ocrInstance.getErrorMsg Get Error Message {#ocrinstancegeterrormsg-获取错误消息}

* Get OCR error message
*
* @return `{string}` `null` means no error

```javascript showLineNumbers
See common code examples
OCR initialization
```

### ocrInstance.releaseAll Release OCR Resources {#ocrinstancereleaseall-释放ocr资源}

* Release OCR resources
*
* @return `{bool}` success or failure

```javascript showLineNumbers
See common code examples
OCR initialization
```

## PaddleOcrOnline HTTP Calls {#paddleocronline-http调用}

- For EC below 3.18.0+, OCR can be invoked via HTTP
- Requires downloading **EasyClick-PaddleOcr.zip, extracting and running it**

```javascript showLineNumbers
function httpPaddleOcr(filePath) {
 // OCR service address
 // See parameter descriptions above for other options
 let url = "http://192.168.2.13:9022/devapi/uploadOcr"
 let ocrType = "ONNX_PPOCR_V3"
 let limit = "1000"
 let ocrParam = {
 "padding": 50,
 "maxSideLen": 0,
 "boxScoreThresh": 0.5,
 "boxThresh": 0.3,
 "unClipRatio": 1.6,
 "doAngleFlag": 0,
 "mostAngleFlag": 0
 }
 ocrParam = utils.base64Encode(JSON.stringify(ocrParam));
 let param = {
 "ocrType": ocrType,
 "limit": limit,
 "ocrParam": ocrParam
 };
 let files = {
 "file": filePath
 }
 let result = http.httpPost(url, param, files, 20 * 1000, {"User-Agent": "test"});
 if (result == null || result == undefined || result == "") {
 return null;
 }

 try {
 result = JSON.parse(result)
 return result["data"]
 } catch (e) {
 return null;
 }

}

function callPaddleOcrTest() {
 let img = image.captureFullScreen()
 if (!img) {
 loge("Screenshot failed");
 return
 }
 let filePath = file.getSandBoxFilePath("ocrtmp.jpg")
 image.saveTo(img, filePath)
 let result = httpPaddleOcr(filePath);
 logd("result " + JSON.stringify(result));


}

callPaddleOcrTest()
```
