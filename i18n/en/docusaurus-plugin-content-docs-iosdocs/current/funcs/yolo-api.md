---
title: YOLO Functions
description: EasyClick automation scripts — iOS no jailbreak YOLO functions resource download
keywords:
 - EasyClick automation scripts iOS no jailbreak YOLO functions resource download
 - yolov8
 - Yolov8Util
 - yolov8Api
 - onnx
 - ONNX
 - EC
 - YOLO
 - newYolov8
 - releaseAll
 - newYolov8Onxx
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview {#说明}

- [YOLO usage guide](/docs/advance/yolov8) — see the Android version; training tutorials are the same

## yolov8Api.newYolov8 Initialize YOLOv8 Instance {#yolov8apinewyolov8-初始化yolov8实例}

* Initialize a YOLOv8 instance
* Requires EC 7.8.0+
* @return `Yolov8Util` object

```javascript showLineNumbers
function main() {
    yolov8Api.releaseAll()
    // Initialize YOLO instance
    let yolov8s = yolov8Api.newYolov8();
    // Initialize config options
    let config = yolov8s.getDefaultConfig("yolov8s-640", 640, 0.25, 0.35, "ALL", 0, [
        "aixin",
        "pinglun"
    ])
    // Set CPU thread count; lower values use less CPU. Default is 4; 1 or 2 is recommended
    config["num_thread"] = 1;
    
    logd("config : " + JSON.stringify(config))
    // Initialize trained model
    let paramPath = "c:/model.ncnn.param";
    let binPath = "c:/model.ncnn.bin";
    let inted = yolov8s.initYoloModel(config, paramPath, binPath);
    if (inted) {
        logd("yolov8s init success");
    } else {
        logd("yolov8s init failed: " + yolov8s.getErrorMsg());
        return;
    }
    let bitmap = image.readBitmap("c:/a.png");
    let result = yolov8s.detectBitmap(bitmap, []);
    // Or:
    let img = image.readImage("c:/a.png")
    let result2 = yolov8s.detectImage("c:/a.png", [])
    image.recycle(img);
    // With parameters, only filter pinglun class data
    //let result = yolov8s.detectBitmap(bitmap, ["pinglun"]);
    if (result == null || result == "") {
        logd("yolov8s no result: " + yolov8s.getErrorMsg());
    } else {
        logd("yolov8s result: " + result);
    }
    // Release when needed; do not release after every call
    yolov8s.release();
}

main();
```

## yolov8Api.releaseAll Release All YOLOv8 Instances {#yolov8apireleaseall-释放所有yolov8实例}

* Release all YOLOv8 instances
* Requires EC 9.0.0+

```javascript
// See code examples below
```

## yolov8Api.newYolov8Onxx Initialize YOLOv8 ONNX Instance (Multi-Instance Supported) {#yolov8apinewyolov8onxx-初始化yolov8-onnx-实例支持多实例}

* Initialize a YOLOv8 ONNX instance
* Requires EC 8.3.0+
* @return `Yolov8Util` instance object

```javascript showLineNumbers
function main() {
    // Initialize YOLO instance
    yolov8Api.releaseAll()
    let yolov8s = yolov8Api.newYolov8Onxx();
    // Initialize config options
    let config = yolov8s.getOnnxConfig([
      "aixin",
      "pinglun"
    ], 640,640, 0.35, 0.55, 2 )
    logd("config : " + JSON.stringify(config))
    // Initialize trained model
    let paramPath = "c:/best.onnx";
    let inted = yolov8s.initYoloModel(config, paramPath, "");
    if (inted) {
        logd("yolov8s init success");
    } else {
        logd("yolov8s init failed: " + yolov8s.getErrorMsg());
        return;
    }
    // Capture screen as img
    let img = image.captureFullScreen()
    // Read local image
    // let bitmap = image.readBitmap("/sdcard/a.png");
    let result = yolov8s.detectImage(img, []);
    // With parameters, only filter pinglun class data
    //let result = yolov8s.detectBitmap(bitmap, ["pinglun"]);
    if (result == null || result == "") {
        logd("yolov8s no result: " + yolov8s.getErrorMsg());
    } else {
        logd("yolov8s result: " + result);
    }
    if (img != null) {
        // Recycle image
        img.recycle();
    }
    // Release when needed; do not release after every call
    yolov8s.release();
}

main();
```

## Yolov8Util.getOnnxConfig ONNX Config Options {#yolov8utilgetonnxconfig-onnx的配置选项}

* ONNX configuration options
* @param obj_names JSON array of class names; if omitted, ONNX reads them from the model, e.g. `["star","common","face"]`
* @param input_width Training image width; 0 lets ONNX extract it automatically
* @param input_height Training image height; 0 lets ONNX extract it automatically
* @param confThreshold Minimum confidence threshold for detections during ONNX inference
* @param iouThreshold IoU threshold used in NMS during ONNX inference
* @param numThread Thread count; usually half the CPU count. If unknown, omit. Use 1 or 2 to reduce CPU usage
* @return `{JSON}`

```text
   See the `Initialize YOLOv8 instance` example
```

## Yolov8Util.getDefaultConfig Get YOLOv8 Default Config {#yolov8utilgetdefaultconfig-获取-yolov8-默认配置}

* Get the default YOLOv8 configuration
* Requires EC 7.8.0+
* @param model_name Model name; use `yolov8s-640` by default
* @param input_size YOLOv8 training imgsz parameter; use 640 by default
* @param box_thr Detection box coefficient; use 0.25 by default
* @param iou_thr Output coefficient; use 0.35 by default
* @param bind_cpu Whether to bind CPU; options are ALL, BIG, LITTLE; use ALL by default
* @param use_vulkan_compute Enable hardware acceleration: 1 yes, 0 no; use 0 by default
* @param obj_names JSON array of class names from training, e.g. `["star","common","face"]`
* @return JSON data

```text
   See the `Initialize YOLOv8 instance` example
```

## Yolov8Util.initYoloModel Initialize YOLOv8 Model {#yolov8utilinityolomodel-初始化yolov8模型}

* Initialize the YOLOv8 model
* To generate param and bin files, see the YOLO usage chapter: convert YOLO pt to ncnn param/bin files
* For ONNX models, set binPath to null; paramPath is the ONNX file path
* Requires EC 7.8.0+
* @param map Parameter map; for ncnn use getDefaultConfig, for onnx use getOnnxConfig
* @param paramPath Path to the param file
* @param binPath Path to the bin file
* @return boolean true on success, false on failure

```text
   See the `Initialize YOLOv8 instance` example
```

## Yolov8Util.detectBitmap Detect Image {#yolov8utildetectbitmap-检测图片}

* Detect objects in an image
* Requires EC 7.8.0+
* Example return data:
* `[{"name":"heart","confidence":0.92,"left":957,"top":986,"right":1050,"bottom":1078}]`
* name: class name; confidence: confidence score; left, top, right, bottom: bounding box coordinates
* @param bitmap Android Bitmap object
* @param obj_names JSON array; omit to skip filtering, or provide class names to keep
* @return string string data

```text
   See the `Initialize YOLOv8 instance` example
```

## Yolov8Util.detectImage Detect AutoImage {#yolov8utildetectimage-检测autoimage}

* Detect objects in an image
* Requires EC 7.8.0+
* Example return data:
* `[{"name":"heart","confidence":0.92,"left":957,"top":986,"right":1050,"bottom":1078}]`
* name: class name; confidence: confidence score; left, top, right, bottom: bounding box coordinates
* @param image AutoImage object
* @param obj_names JSON array; omit to skip filtering, or provide class names to keep
* @return string string data

```text
   See the `Initialize YOLOv8 instance` example
```

## Yolov8Util.release Release YOLOv8 Resources {#yolov8utilrelease-释放yolov8资源}

* Release YOLOv8 resources
* Requires EC 7.8.0+
* @return boolean

```text
   See the `Initialize YOLOv8 instance` example
   Call release when the script ends; no need to release after every use
```

## Yolov8Util.getErrorMsg Get YOLOv8 Error Message {#yolov8utilgeterrormsg-获取yolov8错误消息}

* Get the YOLOv8 error message
* Requires EC 7.8.0+
* @return string

```text
   See the `Initialize YOLOv8 instance` example
```
