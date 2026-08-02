---
title: YOLO Functions
description: EasyClick automation scripts — iOS no jailbreak YOLO functions
keywords:
 - EasyClick automation scripts iOS no jailbreak YOLO functions
 - yolov8
 - Yolov8Util
 - yolov8Api
 - param
 - EC
 - YOLO
 - newYolov8
 - releaseAll
 - getDefaultConfig
 - onnx
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
- 5.16.0+ adds ONNX support

## yolov8Api.newYolov8 Initialize YOLOv8 Instance {#yolov8apinewyolov8-初始化yolov8实例}

* Initialize a YOLOv8 instance
* Requires EC standalone 4.3.0+
* @return `Yolov8Util` object

```javascript showLineNumbers
function main() {
    // Release all previous YOLO instances before script runs to avoid resource usage
    yolov8Api.releaseAll();
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
    // Demo only: put model files in the script res directory
    // Copy to device storage at runtime, or download from the network
    let param = file.getSandBoxFilePath("model.ncnn.param")
    let bin = file.getSandBoxFilePath("model.ncnn.bin")
    let img1 = file.getSandBoxFilePath("1.png")
    let img2 = file.getSandBoxFilePath("2.png")
    saveResToFile("model.ncnn.param", param)
    saveResToFile("model.ncnn.bin", bin)
    // Copy test images for demo
    saveResToFile("1.png", img1)
    saveResToFile("2.png", img2)

    let inted = yolov8s.initYoloModel(config, param, bin);
    if (inted) {
        logd("yolov8s init success");
    } else {
        logd("yolov8s init failed: " + yolov8s.getErrorMsg());
        return;
    }
    // Read image
    let bitmap = image.readImage(img1);
    // Or, after enabling automation service, capture screen
    //let bitmap = image.captureFullScreenEx({type:1,quality:100})
    let result = yolov8s.detectImage(bitmap, []);
    // Or:
    // let img = image.readImage("c:/a.png")
    // let result2 = yolov8s.detectImage("c:/a.png", [])
    image.recycle(bitmap);
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

## yolov8Api.releaseAll Release All Instances {#yolov8apireleaseall-释放所有实例}

* Requires EC standalone 5.17.0+

```text
   See the `Initialize YOLOv8 instance` example
```

## Yolov8Util.getDefaultConfig Get YOLOv8 Default Config {#yolov8utilgetdefaultconfig-获取-yolov8-默认配置}

* Get the default YOLOv8 configuration
* Requires EC standalone 4.3.0+
* @param model_name Model name; use `yolov8s-640` by default
* @param input_size YOLOv8 training imgsz parameter; use 640 by default
* @param box_thr Detection box coefficient; use 0.25 by default
* @param iou_thr Output coefficient; use 0.35 by default
* @param bind_cpu Whether to bind CPU; options are ALL, BIG, LITTLE; use ALL by default
* @param use_vulkan_compute Enable hardware acceleration: 1 yes, 0 no; use 0 by default
* @param obj_names JSON array of class names from training, e.g. `["star","common","face"]`
* @return JSON data

## yolov8Api.newYolov8Onxx Initialize YOLOv8 ONNX Instance {#yolov8apinewyolov8onxx-初始化yolov8-onnx-实例}

* Initialize a YOLOv8 instance (ONNX version)
* Requires EC standalone 5.16.0+
* @return `Yolov8Util` object

```javascript showLineNumbers
function main() {
  // Release all previous YOLO instances before script runs to avoid resource usage
  yolov8Api.releaseAll();
  // Initialize YOLO instance
  let yolov8s = yolov8Api.newYolov8Onxx();
  // Initialize config options
  let config = yolov8s.getOnnxConfig([
    "aixin",
    "pinglun"
  ], 0, 0, 0.35, 0.55, 2)
  config["debug"] = 0;
  logd("config : " + JSON.stringify(config))
  // Initialize trained model
  // Demo only: put model files in the script res directory
  // Copy to device storage at runtime, or download from the network
  let param = file.getSandBoxFilePath("best.onnx")

  let img1 = file.getSandBoxFilePath("1.jpg")

  saveResToFile("best.onnx", param)
  // Copy test image for demo
  saveResToFile("1.png", img1)


  let inted = yolov8s.initYoloModel(config, param, "");
  if (inted) {
    logd("yolov8s onnx init success");
  } else {
    logd("yolov8s onnx init failed: " + yolov8s.getErrorMsg());
    return;
  }
  logd("img1 " + img1)
  // Read image
  let imgx = image.readImage(img1);

  // Or, after enabling automation service, capture screen
  // let imgx = image.captureFullScreenEx({type:1,quality:100})
  logd("image " + imgx)
  for (var i = 0; i < 1; i++) {
    console.time("1")
    let result = yolov8s.detectImage(imgx, []);
    // With parameters, only filter pinglun class data
    //let result = yolov8s.detectBitmap(bitmap, ["pinglun"]);
    logd("elapsed: " + console.timeEnd("1") + " ms")
    if (result == null || result == "") {
      logd("yolov8s no result: " + yolov8s.getErrorMsg());
    } else {
      logd("yolov8s result: " + result);
      result = JSON.parse(result)
      let size = result.length;
      for (let j = 0; j < size; j++) {
        let name = result[j]["name"];
        logd("name: " + name + " coords: " + result[j]["left"] + "," + result[j]["top"] + "," + result[j]["right"] + "," + result[j]["bottom"])
      }
    }

    sleep(1000)
  }
  image.recycle(imgx);
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
* @param confThreshold Minimum confidence threshold for detections during ONNX inference; default 0.25
* @param iouThreshold IoU threshold used in NMS during ONNX inference; default 0.45
* @param numThread Thread count; usually the CPU count. If unknown, omit. -1 means all CPUs, -2 means half the CPU count
* @return JSON data

```text
   See the `Initialize YOLOv8 ONNX instance` example
```

## Yolov8Util.initYoloModel Initialize YOLOv8 Model {#yolov8utilinityolomodel-初始化yolov8模型}

* Initialize the YOLOv8 model
* To generate param and bin files, see the YOLO usage chapter: convert YOLO pt to ncnn param/bin files
* Requires EC standalone 4.3.0+
* @param map Parameter map; use getDefaultConfig for default parameters
* @param paramPath Path to the param file
* @param binPath Path to the bin file
* @return boolean true on success, false on failure

```text
   See the `Initialize YOLOv8 instance` example
```

## Yolov8Util.detectBitmap Detect Image {#yolov8utildetectbitmap-检测图片}

* Detect image (UIImage iOS image object)
* Requires EC standalone 4.3.0+
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

* Detect image
* Requires EC standalone 4.3.0+
* Example return data:
* `[{"name":"heart","confidence":0.92,"left":957,"top":986,"right":1050,"bottom":1078}]`
* name: class name; confidence: confidence score; left, top, right, bottom: bounding box coordinates
* @param image AutoImage object
* @param obj_names JSON array; omit to skip filtering, or provide class names to keep
* @return string string data

```text
   See the `Initialize YOLOv8 instance` example
```

## Yolov8Util.release Release YOLOv8 Resources {#yolov8utilrelease-插入数据}

* Release YOLOv8 resources
* Requires EC standalone 4.3.0+
* @return boolean

```text
   See the `Initialize YOLOv8 instance` example
   Call release when the script ends; no need to release after every use
```

## Yolov8Util.getErrorMsg Get YOLOv8 Error Message {#yolov8utilgeterrormsg-获取yolov8错误消息}

* Get the YOLOv8 error message
* Requires EC standalone 4.3.0+
* @return string

```text
   See the `Initialize YOLOv8 instance` example
```
