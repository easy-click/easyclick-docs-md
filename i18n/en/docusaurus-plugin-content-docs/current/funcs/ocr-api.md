---
title: OCR Recognition
description: EasyClick automation scripts — Android no root OCR recognition
keywords:
 - EasyClick automation scripts Android no root proxy events
 - OCR
 - ocr
 - '9.17'
 - Tesseract
 - initOcr
 - ocrLite
 - PPOCR
 - newOcr
 - paddleOcrOnline
 - paddleLiteOcr
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
- Current OCR engines include mlkit, ocrLite, Baidu AI easyedge, paddleOcrNcnnV5, paddleOcrOnnxV4, paddleOcrOnnxV5, paddleOcrOnnxV6, Tesseract, paddleOcrOnline, and Baidu online recognition
- Supports PPOCR-V4, PPOCR-V5, and PPOCR-V6 models (V6 requires Android 8.0/API 26+, default modelTier=small)
- For Tesseract, download the corresponding language packs or create your own
- For versions above 9.17.0, see the **Tesseract example [9.17+]** section because the API has changed


## ocr.newOcr — Create an OCR Instance {#ocrnewocr-实例一个ocr}

* Create an OCR instance
* Supported: EC Android 9.17.0+

```javascript showLineNumbers
function main() {
    let o = ocr.newOcr();
    // Initialize and recognize here


    o.releaseAll()
}
```



## ocr.initOcr — Initialize {#ocrinitocr-初始化}

* Initialize the OCR module
* @param map parameter map
* Keys:<br/>
* type: OCR type — OCR, paddleOcrNcnnV5 (ncnn PPOCR-V5), paddleLiteOcr (Paddle Lite), paddleOcrOnnxV4 (onnx PPOCR-V4), paddleOcrOnnxV5 (onnx PPOCR-V5), paddleOcrOnnxV6 (official ppocr-sdk + ORT PPOCR-V6, API≥26, default small), tess (Tesseract), baiduOnline (Baidu online), paddleocr (Baidu offline PaddleOCR), easyedge (Baidu AI OCR)<br/>
* ocrLite = ocrLite, paddleOcrOnline = EC built-in PC PaddleOCR service<br/>
* If type is `tess`, put trained models in `/sdcard/tessdata/`<br/>
 - Example: ```{"type":"tess","language":"chi_sim","debug":false,"ocrEngineMode":3}```
  - language: language data file; e.g. chi_sim.traineddata is Simplified Chinese, use chi_sim; combine with +, e.g. chi_sim+eng+num<br/>
  - ocrEngineMode: engine type — 0 OEM_TESSERACT_ONLY, 1 OEM_LSTM_ONLY, 2 OEM_TESSERACT_LSTM_COMBINED, 3 OEM_DEFAULT<br/>
  - rilLevel: PageIteratorLevel — -1 adaptive, 0 RIL_BLOCK, 1 RIL_PARA, 2 RIL_TEXTLINE, 3 RIL_WORD, 4 RIL_SYMBOL<br/>
  - debug: enable debug mode; usually false<br/>
  - path: folder containing tessdata (parent of tessdata, do not include tessdata)<br/>
* If type is `baiduOnline`,
  * Example: ```{"type":"baiduOnline","ak":"xxx","sk":"xx"}```
  - ak = API key, sk = secret key; Baidu OCR docs: https://ai.baidu.com/ai-doc/OCR/Ck3h7y2ia<br/>
* If type is `paddleOcrNcnnV5`
  * Example: `{"type":"paddleOcrNcnnV5","numThread":2,"padding":32,"maxSideLen":640}`
  - Parameters:
  - numThread: CPU threads; default 0; -1 means max CPU
  - padding: white border around image to improve accuracy; 48, 32, etc.; default 32; affects speed and accuracy
  - modelsDir: model path; external path like /sdcard/models/; built-in models used if omitted
  - keysName: label file path, e.g. ppocr_keys_v1.txt; built-in if omitted
  - detName: detection model filename without.param/.bin, under modelsDir; built-in if omitted
  - recName: recognition model filename without.param/.bin, under modelsDir; built-in if omitted
  - maxSideLen: scale by long edge; larger = slower but more accurate; 960, 640, 480, 320; default 640
* If type is `paddleOcrOnnxV4` or `paddleOcrOnnxV5`
  * Example: `{"type":"paddleOcrOnnxV4","numThread":2,"padding":50,"maxSideLen":960}`
  - Parameters:
  - numThread: CPU threads
  - modelsDir: model path; external like /sdcard/models/; built-in if omitted
  - keysName: label file, e.g. /sdcard/labels/ppocr_keys_v1.txt; built-in if omitted
  - detName: detection model.onnx filename under modelsDir; built-in if omitted
  - recName: recognition model.onnx filename under modelsDir; built-in if omitted
  - clsName: classification model.onnx filename under modelsDir; built-in if omitted
  - padding: white border; default 50<br/>
  - maxSideLen: scale by long edge; 0 = no scaling<br/>
  - boxScoreThresh: box confidence threshold<br/>
  - boxThresh: same purpose; tune experimentally<br/>
  - unClipRatio: box size multiplier<br/>
  - doAngleFlag: enable (1)/disable (0) text orientation detection; only for upside-down images (90–270°); default off<br/>
  - mostAngleFlag: enable (1)/disable (0) angle voting; ineffective when doAngleFlag is off; default off<br/>
* If type is `paddleOcrOnnxV6`
  * Example: `{"type":"paddleOcrOnnxV6","modelTier":"small","numThread":2,"padding":32,"maxSideLen":640}`
  - Requires Android 8.0 (API ≥ 26); use paddleOcrNcnnV5 / paddleOcrOnnxV5 on lower versions
  - modelTier: model tier; default `small` (`PP-OCRv6_small`)
  - numThread: CPU threads
  - padding: white border; default 32
  - maxSideLen: scale by long edge; default 640
* If type is `paddleLiteOcr`, Paddle Lite v2.14-rc; built-in ppocrv4 optimized nb models
  * Example: `{"type":"paddleLiteOcr","cpuThreadNum":2,"cpuPowerMode":"LITE_POWER_FULL"}`
  - Parameters:
  - cpuThreadNum: CPU threads
  - cpuPowerMode: LITE_POWER_FULL, LITE_POWER_HIGH, LITE_POWER_LOW, LITE_POWER_NO_BIND, LITE_POWER_RAND_HIGH, LITE_POWER_RAND_LOW
  - modelPath: model path; external like /sdcard/models/; built-in if omitted
  - labelPath: label file, e.g. /sdcard/labels/ppocr_keys_v1.txt; built-in if omitted
  - detModelFilename: detection.nb filename under modelPath; built-in if omitted
  - recModelFilename: recognition.nb filename under modelPath; built-in if omitted
  - clsModelFilename: classification.nb filename under modelPath; built-in if omitted
* If type is `ocrLite`,
  - Example: ```{"type":"ocrLite","numThread":4,"padding":10,"maxSideLen":0}```
  - numThread: thread count<br/>
  - padding: white border preprocessing<br/>
  - maxSideLen: scale by longest edge; 0 = no scaling; e.g. 1024 scales down if longer; minimum 32<br/>
* If type is `paddleOcrOnline`, download **EasyClick-PaddleOcr.zip** from the cloud drive, extract, and run<br/>

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
* - ocrType: ONNX_PPOCR_V3, ONNX_PPOCR_V4, NCNN_PPOCR_V3
* - serverUrl: Paddle OCR server address; deploy on another PC and connect from control center, e.g. 192.168.2.8; port 9022 optional
* - padding: white border; default 50<br/>
* - maxSideLen: scale by long edge; 0 = no scaling<br/>
* - boxScoreThresh, boxThresh, unClipRatio, doAngleFlag, mostAngleFlag: same as above<br/>
* - limit: OCR requests per second; default 1000; lower to reduce CPU<br/>
* - checkImage: verify input is image (1 yes, 0 no); default off<br/>
```
* @return `{bool}` true on success, false on failure
### 9.17+ — Single Instance — paddleOcrOnline {#917版本-单实例-paddleocronline}
- Note: 9.17+ OCR changed from singleton to multi-instance; follow the pattern below and adjust initOcr parameters; ocrLite and paddleOcrOnline are supported

```js showLineNumbers
let paddleOcrOnline = null

// Script stop callback
setStopCallback(function () {
 // Release resources; usually in setStopCallback
 logi("Releasing paddleOcrOnline")
 paddleOcrOnline && paddleOcrOnline.releaseAll()
})

// Initialize automation environment
function initEnv() {
 if (!startEnv()) {
 loge("Automation failed to start; exiting")
 exit()
 }
 if (!image.requestScreenCapture(10000, 0)) {
 loge("Screenshot permission denied; check background pop-up and overlay permissions")
 exit()
 }
 // Wait at least 1s after permission (more on slow devices) before capture
 sleep(1000)
}

// Initialize all OCR instances
function initAllOcr() {
 // paddleOcrOnline; see parameters above
 let paddleOcrOnlineMap = {
 "serverUrl": "10.0.0.112",
 "type": "paddleOcrOnline",
 "ocrType": "ONNX_PPOCR_V3",
 }
 // Create once at script start
 paddleOcrOnline = ocr.newOcr()
 // Initialize once at script start
 if (!paddleOcrOnline.initOcr(paddleOcrOnlineMap)) {
 loge("OCR init failed: " + paddleOcrOnline.getErrorMsg())
 exit()
 }
}

// OCR recognition
function ocrFunc() {
 // Screenshot
 let img = image.captureFullScreenEx()
 if (!img) {
 loge("Screenshot failed")
 return
 }
 logi("=================== paddleOcrOnline ======================")
 let result = paddleOcrOnline.ocrImage(img, 20 * 1000, {})
 if (result) {
 logd("OCR result -> " + JSON.stringify(result))
 for (let i = 0; i < result.length; i++) {
 let value = result[i]
 logd("Text: " + value.label + " x: " + value.x + " y: " + value.y + " width: " + value.width + " height: " + value.height)
 }
 } else {
 logw("No recognition result")
 }

 image.recycle(img)
}

function main() {
 initEnv()
 initAllOcr()
 ocrFunc()
 ocrFunc()
 ocrFunc()
}

main()
```


### 11.17+ paddleLiteOcr Example {#1117版本paddleliteocr例子}

```javascript showLineNumbers
let paddlelite = null
setStopCallback(function () {
 logi("Releasing paddleLite")
 paddlelite && paddlelite.releaseAll()
})

function initEnv() {
 if (!startEnv()) {
 loge("Automation failed to start; exiting")
 exit()
 }
 if (!image.requestScreenCapture(10000, 0)) {
 loge("Screenshot permission denied; check background pop-up and overlay permissions")
 exit()
 }
 sleep(1000)
}

function initOcrpaddle() {
 let paddleliteMap = {"type": "paddleLiteOcr"}
 paddlelite = ocr.newOcr()
 if (!paddlelite.initOcr(paddleliteMap)) {
 loge("OCR init failed: " + paddlelite.getErrorMsg())
 exit()
 }
}

function ocrFunc() {
 let img = image.captureFullScreenEx()
 if (!img) {
 loge("Screenshot failed")
 return
 }
 let result = paddlelite.ocrImage(img, 20 * 1000, {})
 if (result) {
 logd("OCR result -> " + JSON.stringify(result))
 for (let i = 0; i < result.length; i++) {
 let value = result[i]
 logd("Text: " + value.label + " x: " + value.x + " y: " + value.y + " width: " + value.width + " height: " + value.height)
 }
 } else {
 logw("No recognition result")
 }
 image.recycle(img)
}

function main() {
 initEnv()
 initOcrpaddle()
 ocrFunc()
 ocrFunc()
 ocrFunc()
}

main()
```

### 11.26+ paddleOcrOnnxV4 / paddleOcrOnnxV5 Example {#1126版本-paddleocronnxv4paddleocronnxv5-例子}

```javascript showLineNumbers
let paddleOcrOnnx = null
setStopCallback(function () {
 logi("Releasing paddleOcrOnnx")
 paddleOcrOnnx && paddleOcrOnnx.releaseAll()
})

function initEnv() {
 if (!startEnv()) {
 loge("Automation failed to start; exiting")
 exit()
 }
 if (!image.requestScreenCapture(10000, 0)) {
 loge("Screenshot permission denied; check background pop-up and overlay permissions")
 exit()
 }
 sleep(1000)
}

function initOcrpaddle() {
 // type can be paddleOcrOnnxV4 or paddleOcrOnnxV5
 let paddleOnnxMap = {"type": "paddleOcrOnnxV4","modelsDir":"","numThread":2,"padding":60,"maxSideLen":960}
 paddleOcrOnnx = ocr.newOcr()
 if (!paddleOcrOnnx.initOcr(paddleOnnxMap)) {
 loge("OCR init failed: " + paddleOcrOnnx.getErrorMsg())
 exit()
 }
}

function ocrFunc() {
 let img = image.captureFullScreenEx()
 if (!img) {
 loge("Screenshot failed")
 return
 }
 let result = paddleOcrOnnx.ocrImage(img, 20 * 1000, {})
 if (result) {
 logd("OCR result -> " + JSON.stringify(result))
 for (let i = 0; i < result.length; i++) {
 let value = result[i]
 logd("Text: " + value.label+" confidence:"+value.confidence + " bounds: " + value.x + "," + value.y + "," + (value.x+value.width) + "," + (value.y+value.height))
 }
 } else {
 logw("No recognition result")
 }
 image.recycle(img)
}

function main() {
 initEnv()
 initOcrpaddle()
 ocrFunc()
 ocrFunc()
 ocrFunc()
}

main()
```



### 12.4.0+ paddleOcrOnnxV6 Example {#1240版本-paddleocronnxv6-例子}

* Requires Android 8.0 (API ≥ 26); default `modelTier=small`

```javascript showLineNumbers
let paddleOcrOnnxV6 = null
setStopCallback(function () {
 logi("Releasing paddleOcrOnnxV6")
 paddleOcrOnnxV6 && paddleOcrOnnxV6.releaseAll()
})

function initEnv() {
 if (!startEnv()) {
 loge("Automation failed to start; exiting")
 exit()
 }
 if (!image.requestScreenCapture(10000, 0)) {
 loge("Screenshot permission denied; check background pop-up and overlay permissions")
 exit()
 }
 sleep(1000)
}

function initOcrpaddle() {
 let paddleOnnxMap = {
 "type": "paddleOcrOnnxV6",
 "modelTier": "small",
 "numThread": 2,
 "padding": 32,
 "maxSideLen": 640
 }
 paddleOcrOnnxV6 = ocr.newOcr()
 if (!paddleOcrOnnxV6.initOcr(paddleOnnxMap)) {
 loge("OCR init failed: " + paddleOcrOnnxV6.getErrorMsg())
 exit()
 }
}

function ocrFunc() {
 let img = image.captureFullScreenEx()
 if (!img) {
 loge("Screenshot failed")
 return
 }
 let result = paddleOcrOnnxV6.ocrImage(img, 20 * 1000, {})
 if (result) {
 logd("OCR result -> " + JSON.stringify(result))
 for (let i = 0; i < result.length; i++) {
 let value = result[i]
 logd("Text: " + value.label + " confidence:" + value.confidence
 + " bounds: " + value.x + "," + value.y + ","
 + (value.x + value.width) + "," + (value.y + value.height))
 }
 } else {
 logw("No recognition result")
 }
 image.recycle(img)
}

function main() {
 initEnv()
 initOcrpaddle()
 ocrFunc()
 ocrFunc()
 ocrFunc()
}

main()
```



### 11.28+ paddleOcrNcnnV5 Example {#1128版本-paddleocrncnnv5-例子}

```javascript showLineNumbers
let ncnnOcr = null
setStopCallback(function () {
 logi("Releasing ncnnOcr")
 ncnnOcr && ncnnOcr.releaseAll()
})

function initEnv() {
 if (!startEnv()) {
 loge("Automation failed to start; exiting")
 exit()
 }
 if (!image.requestScreenCapture(10000, 0)) {
 loge("Screenshot permission denied; check background pop-up and overlay permissions")
 exit()
 }
 sleep(1000)
}

function initOcrpaddle() {
 let config = {"type": "paddleOcrNcnnV5","modelsDir":"","numThread":0,"padding":32,"maxSideLen":640}
 ncnnOcr = ocr.newOcr()
 if (!ncnnOcr.initOcr(config)) {
 loge("OCR init failed: " + ncnnOcr.getErrorMsg())
 exit()
 }
}

function ocrFunc() {
 let img = image.captureFullScreenEx()
 if (!img) {
 loge("Screenshot failed")
 return
 }
 let result = ncnnOcr.ocrImage(img, 20 * 1000, {"padding":32})
 if (result) {
 logd("OCR result -> " + JSON.stringify(result))
 for (let i = 0; i < result.length; i++) {
 let value = result[i]
 logd("Text: " + value.label+" confidence:"+value.confidence + " bounds: " + value.x + "," + value.y + "," + (value.x+value.width) + "," + (value.y+value.height))
 }
 } else {
 logw("No recognition result")
 }
 image.recycle(img)
}

function main() {
 initEnv()
 initOcrpaddle()
 ocrFunc()
 ocrFunc()
 ocrFunc()
}

main()
```



### 9.17+ — Single Instance — ocrLite {#917版本-单实例-ocrlite}
- Note: 9.17+ OCR changed from singleton to multi-instance

```javascript showLineNumbers
let ocrLite = null
setStopCallback(function () {
 logi("Releasing ocrLite")
 ocrLite && ocrLite.releaseAll()
})

function initEnv() {
 if (!startEnv()) {
 loge("Automation failed to start; exiting")
 exit()
 }
 if (!image.requestScreenCapture(10000, 0)) {
 loge("Screenshot permission denied; check background pop-up and overlay permissions")
 exit()
 }
 sleep(1000)
}

function initOcrLite() {
 let ocrLiteMap = {"type": "ocrLite", "numThread": 1, "padding": 10, "maxSideLen": 0}
 ocrLite = ocr.newOcr()
 if (!ocrLite.initOcr(ocrLiteMap)) {
 loge("OCR init failed: " + ocrLite.getErrorMsg())
 exit()
 }
}

function ocrFunc() {
 let img = image.captureFullScreenEx()
 if (!img) {
 loge("Screenshot failed")
 return
 }
 let result = ocrLite.ocrImage(img, 20 * 1000, {})
 if (result) {
 logd("OCR result -> " + JSON.stringify(result))
 for (let i = 0; i < result.length; i++) {
 let value = result[i]
 logd("Text: " + value.label + " x: " + value.x + " y: " + value.y + " width: " + value.width + " height: " + value.height)
 }
 } else {
 logw("No recognition result")
 }
 image.recycle(img)
}

function main() {
 initEnv()
 initOcrLite()
 ocrFunc()
 ocrFunc()
 ocrFunc()
}

main()
```
### 9.17+ — Multi-Instance — ocrLite + mlkit {#917版本-多实例-ocrlite--mlkit}
- Note: 9.17+ OCR changed from singleton to multi-instance

```js showLineNumbers
let ocrLite = null
let mlkit = null
setStopCallback(function () {
 logi("Releasing ocrLite")
 ocrLite && ocrLite.releaseAll()
 logi("Releasing mlkit")
 mlkit && mlkit.releaseAll()
})

function initEnv() {
 if (!startEnv()) {
 loge("Automation failed to start; exiting")
 exit()
 }
 if (!image.requestScreenCapture(10000, 0)) {
 loge("Screenshot permission denied; check background pop-up and overlay permissions")
 exit()
 }
 sleep(1000)
}

function initAllOcr() {
 let ocrLiteMap = {"type": "ocrLite", "numThread": 1, "padding": 10, "maxSideLen": 0}
 ocrLite = ocr.newOcr()
 if (!ocrLite.initOcr(ocrLiteMap)) {
 loge("OCR init failed: " + ocrLite.getErrorMsg())
 exit()
 }

 let mlkitMap = {
 "type": "mlkit",
 }
 mlkit = ocr.newOcr()
 if (!mlkit.initOcr(mlkitMap)) {
 loge("OCR init failed: " + mlkit.getErrorMsg())
 exit()
 }
}

function ocrFunc() {
 let img = image.captureFullScreenEx()
 if (!img) {
 loge("Screenshot failed")
 return
 }
 logi("=================== ocrLite ======================")
 let result = ocrLite.ocrImage(img, 20 * 1000, {})
 if (result) {
 logd("OCR result -> " + JSON.stringify(result))
 for (let i = 0; i < result.length; i++) {
 let value = result[i]
 logd("Text: " + value.label + " x: " + value.x + " y: " + value.y + " width: " + value.width + " height: " + value.height)
 }
 } else {
 logw("No recognition result")
 }
 logi("=================== mlkit ======================")
 // orz = rotation: 0 none, 90 left, also 180, 270, 360
 result = mlkit.ocrImage(img, 20 * 1000, {"orz": 0})
 if (result) {
 logd("OCR result -> " + JSON.stringify(result))
 for (let i = 0; i < result.length; i++) {
 let value = result[i]
 logd("Text: " + value.label + " x: " + value.x + " y: " + value.y + " width: " + value.width + " height: " + value.height)
 }
 } else {
 logw("No recognition result")
 }
 image.recycle(img)
}

function main() {
 initEnv()
 initAllOcr()
 ocrFunc()
 ocrFunc()
 ocrFunc()
}

main()
```

### easyedge OCR Example [Below 9.17] {#easyedge-ocr例子-低于917版本}
:::tip
Not recommended
:::
```javascript showLineNumbers
function main() {
 logd("isServiceOk " + isServiceOk());
 startEnv()
 logd("isServiceOk " + isServiceOk());
 let request = image.requestScreenCapture(10000, 0);
 if (!request) {
 request = image.requestScreenCapture(10000, 0);
 }
 logd("Screenshot permission result... " + request)
 if (!request) {
 loge("Screenshot permission denied; check background pop-up and overlay permissions")
 exit()
 }
 sleep(1000)

 let paddleocr = {
 "type": "paddleocr"
 }

 let easyedge = {
 "type": "easyedge",

 }
 let ocrlite = {
 "type": "ocrLite",

 }
 let inited = ocr.initOcr(easyedge)
 logd("Init result -" + inited);
 if (!inited) {
 loge("error : " + ocr.getErrorMsg());
 return;
 }
 // initServer not needed for ocrLite mode
 let initServer = ocr.initOcrServer(5 * 1000);
 logd("initServer " + initServer);
 if (!initServer) {
 loge("initServer error : " + ocr.getErrorMsg());
 return;
 }
 ocr.setDaemonServer(true, 500);
 for (var ix = 0; ix < 20; ix++) {

 let bitmap = image.readBitmap("/sdcard/Screenshot_20210127_152932_com.huawei.android.lau.jpg");
 if (!bitmap) {
 loge("Failed to read image");
 continue;
 }
 console.time("1")
 logd("start---ocr");
 let result = ocr.ocrBitmap(bitmap, 20 * 1000, {});
 logd(result)
 if (result) {
 logd("OCR result -> " + JSON.stringify(result));
 for (var i = 0; i < result.length; i++) {
 var value = result[i];
 logd("Text: " + value.label + " x: " + value.x + " y: " + value.y + " width: " + value.width + " height: " + value.height);
 }
 } else {
 logw("No recognition result");
 }
 bitmap.recycle();
 logd("Elapsed: " + console.timeEnd(1) + " ms")
 sleep(1000);
 logd("ix = " + ix)
 }
 ocr.releaseAll();
}

main();
```

### Paddle OCR Example [Below 9.17] {#paddle-ocr例子-低于917版本}
:::tip
Not recommended
:::
```javascript showLineNumbers
function main() {
 logd("isServiceOk " + isServiceOk());
 startEnv()
 logd("isServiceOk " + isServiceOk());
 let request = image.requestScreenCapture(10000, 0);
 if (!request) {
 request = image.requestScreenCapture(10000, 0);
 }
 logd("Screenshot permission result... " + request)
 if (!request) {
 loge("Screenshot permission denied; check background pop-up and overlay permissions")
 exit()
 }
 sleep(1000)

 let paddleocr = {
 "type": "paddleocr"
 }

 let easyedge = {
 "type": "easyedge",
 }

 let inited = ocr.initOcr(paddleocr)
 logd("Init result -" + inited);
 if (!inited) {
 loge("error : " + ocr.getErrorMsg());
 return;
 }

 let initServer = ocr.initOcrServer(5 * 1000);
 logd("initServer " + initServer);
 if (!initServer) {
 loge("initServer error : " + ocr.getErrorMsg());
 return;
 }
 ocr.setDaemonServer(true, 500);
 for (var ix = 0; ix < 20; ix++) {

 let bitmap = image.readBitmap("/sdcard/Screenshot_20210127_152932_com.huawei.android.lau.jpg");
 if (!bitmap) {
 loge("Failed to read image");
 continue;
 }
 console.time("1")
 logd("start---ocr");
 let result = ocr.ocrBitmap(bitmap, 20 * 1000, {});
 logd(result)
 if (result) {
 logd("OCR result -> " + JSON.stringify(result));
 for (var i = 0; i < result.length; i++) {
 var value = result[i];
 logd("Text: " + value.label + " x: " + value.x + " y: " + value.y + " width: " + value.width + " height: " + value.height);
 }
 } else {
 logw("No recognition result");
 }
 bitmap.recycle();
 logd("Elapsed: " + console.timeEnd(1) + " ms")
 sleep(1000);
 logd("ix = " + ix)
 }
 ocr.releaseAll();
}

main();
```

### Tesseract Example [Below 9.17] {#tesseract-例子-低于917版本}

```javascript showLineNumbers
function main() {
 logd("isServiceOk " + isServiceOk());
 startEnv()
 logd("isServiceOk " + isServiceOk());
 let request = image.requestScreenCapture(10000, 0);
 if (!request) {
 request = image.requestScreenCapture(10000, 0);
 }
 logd("Screenshot permission result... " + request)
 if (!request) {
 loge("Screenshot permission denied; check background pop-up and overlay permissions")
 exit()
 }
 sleep(1000)

 let tessInitMap = {
 "type": "tess",
 "language": "chi_sim",
 "debug": true
 }

 let inited = ocr.initOcr(tessInitMap)
 logd("Init result -" + inited);
 if (!inited) {
 loge("error : " + ocr.getErrorMsg());
 return;
 }

 let bitmap = image.readBitmap("/sdcard/a.png");
 if (!bitmap) {
 loge("Failed to read image");
 return;
 }
 let result = ocr.ocrBitmap(bitmap, 20 * 1000, {});
 if (result) {
 logd("OCR result -> " + JSON.stringify(result));
 for (var i = 0; i < result.length; i++) {
 var value = result[i];
 logd("Text: " + value.label + " x: " + value.x + " y: " + value.y + " width: " + value.width + " height: " + value.height);
 }
 } else {
 logw("No recognition result");
 }

 bitmap.recycle();
 ocr.releaseAll();

}


main();
```

### Baidu Online OCR Example [Below 9.17] {#百度在线ocr例子-低于917版本}

```javascript showLineNumbers
function main() {
 logd("isServiceOk " + isServiceOk());
 startEnv()
 logd("isServiceOk " + isServiceOk());
 let request = image.requestScreenCapture(10000, 0);
 if (!request) {
 request = image.requestScreenCapture(10000, 0);
 }
 logd("Screenshot permission result... " + request)
 if (!request) {
 loge("Screenshot permission denied; check background pop-up and overlay permissions")
 exit()
 }
 sleep(1000)

 let baiduOnlineInitMap = {
 "type": "baiduOnline",
 "ak": "xx",
 "sk": "xx"
 }

 let inited = ocr.initOcr(baiduOnlineInitMap)
 logd("Init result -" + inited);
 if (!inited) {
 loge("error : " + ocr.getErrorMsg());
 return;
 }

 let bitmap = image.readBitmap("/sdcard/a.png");
 if (!bitmap) {
 loge("Failed to read image");
 return;
 }
 // URL params: https://ai.baidu.com/ai-doc/OCR/tk3h7y2aq
 let result = ocr.ocrBitmap(bitmap, 20 * 1000, {"url": "https://aip.baidubce.com/rest/2.0/ocr/v1/accurate"});
 if (result) {
 logd("OCR result -> " + JSON.stringify(result));
 for (var i = 0; i < result.length; i++) {
 var value = result[i];
 logd("Text: " + value.label + " x: " + value.x + " y: " + value.y + " width: " + value.width + " height: " + value.height);
 }
 } else {
 logw("No recognition result " + ocr.getErrorMsg());
 }

 bitmap.recycle();
 ocr.releaseAll();

}

main();
```

### Mlkit OCR Example [Below 9.17] {#mlkit-ocr例子-低于917版本}

```javascript showLineNumbers
function main() {
 logd("isServiceOk " + isServiceOk());
 startEnv()
 logd("isServiceOk " + isServiceOk());
 let request = image.requestScreenCapture(10000, 0);
 if (!request) {
 request = image.requestScreenCapture(10000, 0);
 }
 logd("Screenshot permission result... " + request)
 if (!request) {
 loge("Screenshot permission denied; check background pop-up and overlay permissions")
 exit()
 }
 sleep(1000)

 let mlkit = {
 "type": "mlkit"
 }

 let inited = ocr.initOcr(mlkit)
 logd("Init result -" + inited);
 if (!inited) {
 loge("error : " + ocr.getErrorMsg());
 return;
 }
 for (var ix = 0; ix < 20; ix++) {
 let tmpImage = image.captureFullScreen();
 // orz = rotation: 0 none, 90 left, also 180, 270, 360
 let result = ocr.ocrImage(tmpImage, 20000, {"orz": 0});
 logd("Elapsed {}", console.timeEnd(1))
 if (result) {
 for (let i = 0; i < result.length; i++) {
 logd(JSON.stringify(result[i]))
 }
 }
 image.recycle(tmpImage)
 }
 ocr.releaseAll();
}

main();
```

### ocrLite OCR Example [Below 9.17] {#ocrlite-ocr例子-低于917版本}
```javascript showLineNumbers
function main() {
 let s = image.requestScreenCapture(10000, 0);
 logd("s {}", s)


 logd("Initializing ocrLite")

 let m = {
 "type": "ocrLite"
 }
 m = {"type": "ocrLite", "numThread": 1, "padding": 10, "maxSideLen": 0};
 let iniit = ocr.initOcr(m);
 logd("Init o " + iniit)
 image.initOpenCV()
 sleep(1000)
 let id = thread.execAsync(function () {
 while (true) {
 sleep(1000)

 let tmpImage = image.captureFullScreen();
 logd("Screenshot tmpImage {}", tmpImage)
 let tt = image.binaryzation(tmpImage, 1, 100)
 console.time(1)
 let result = ocr.ocrImage(tt, 10000, {"maxSideLen": 1024});
 if (result) {
 for (let i = 0; i < result.length; i++) {
 logd(JSON.stringify(result[i]))
 }
 }
 logd("Elapsed {}", console.timeEnd(1))
 image.recycle(tt)
 image.recycle(tmpImage)
 }
 })

 logd("Thread id = {}", id)

 sleep(115 * 1000)
 thread.cancelThread(id)
 sleep(1000)
}

main();
```

## ocr.setOcrType — Set Type {#ocrsetocrtype-设置类型}

* Set the OCR implementation
* Supported: EC 5.17.0+
* @param type tess = Tesseract, baiduOnline = Baidu online
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
See common examples in
OCR initialization
```

## ocr.setDaemonServer — Daemon OCR Service {#ocrsetdaemonserver-守护ocr服务}

* Set whether to daemon the OCR service
* Supported: EC 6.9.0+
* @param daemon true to daemon, false otherwise
* @param delay interval between daemon checks in milliseconds
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
See common examples in
OCR initialization
```

## ocr.ocrBitmap — Recognize Text {#ocrocrbitmap-识别文字}

* OCR on a Bitmap; returns JSON like:
* Supported: EC 5.17.0+

```json showLineNumbers
[
 {
 "label": "Sample recognized text",
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
* x: start X coordinate
* y: start Y coordinate
* width: width
* height: height
* @param bitmap image
* @param timeout timeout in milliseconds
* @param extra extra parameters as map, e.g. ```{"token":"xxx"}```
* @return `{json}` JSON object

```javascript showLineNumbers
See common examples in
OCR initialization
```

## ocr.ocrImage — Recognize Text {#ocrocrimage-识别文字}

* OCR on an AutoImage; returns JSON like:
* Supported: EC 8.2.0+

```json showLineNumbers
[
 {
 "label": "Sample recognized text",
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
* x: start X coordinate
* y: start Y coordinate
* width: width
* height: height
* @param image image
* @param timeout timeout in milliseconds
* @param extra extra parameters as map, e.g. ```{"token":"xxx"}```
* @return `{json}` JSON object


## ocr.getErrorMsg — Get Error Message {#ocrgeterrormsg-获取错误消息}

* Get OCR error message
* Supported: EC 5.17.0+
* @return `{string}` null if no error

```javascript showLineNumbers
See common examples in
OCR initialization
```

## ocr.releaseAll — Release OCR Resources {#ocrreleaseall-释放ocr资源}

* Release OCR resources
* Supported: EC 5.17.0+
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
See common examples in
OCR initialization
```

