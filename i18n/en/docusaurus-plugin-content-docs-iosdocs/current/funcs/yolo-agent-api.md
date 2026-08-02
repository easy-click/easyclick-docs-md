---
title: YOLO Functions — On-Device Execution
description: EasyClick automation scripts — iOS no jailbreak YOLO functions
keywords:
 - EasyClick automation scripts iOS no jailbreak YOLO functions
 - yolov8
 - yolov8Agent
 - Yolov8AgentUtil
 - onnx
 - ONNX
 - EC
 - 9.0.0
 - YOLO
 - releaseAll
 - newYolov8
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview

:::tip
- [YOLO usage guide](/docs/advance/yolov8) — see the Android version; training tutorials are the same
- The yolov8Agent module runs YOLO detection on the device
- This module runs on the device, using device compute to reduce PC load
:::

## yolov8Agent.releaseAll Release All Instances

* Release all instances
* Requires EC 9.0.0+

```javascript
// See code examples below
```

## yolov8Agent.newYolov8 Initialize YOLOv8 Instance

* Initialize a YOLOv8 instance
* Requires EC 9.0.0+
* @return `Yolov8AgentUtil` object

```javascript showLineNumbers
function yoloagenttest2() {
  yolov8Agent.releaseAll()
  let yoloInstance = yolov8Agent.newYolov8()
  logd("yoloInstance " + yoloInstance.yolov8AgentId)
  let config = yoloInstance.getDefaultConfig("yolov8s-640", 640, 0.25,
    0.35, "ALL", 0, ["aixin", "pinglun"])
  config["num_thread"] = 1;
  logd("Start upload model file...")
  // Upload model files to the agent for YOLO initialization
  let paramPath = utils.uploadAgentFile("/Users/x/iosidea/tjyolo/src/res/model.ncnn.param", "model2.ncnn.param")
  let binPath = utils.uploadAgentFile("/Users/x/iosidea/tjyolo/src/res/model.ncnn.bin", "model2.ncnn.bin")
  let ok = yoloInstance.initYoloModel(config, paramPath, binPath)
  if (!ok) {
    console.log("err " + yoloInstance.getErrorMsg())
    return;
  }
  // Upload image to agent for detection; or use imageAgent capture functions
  let img = utils.uploadToAutoImage("/Users/x/iosidea/yolo-onnx/src/res/1.png")
  logd("img -> " + img)
  for (let i = 0; i < 10; i++) {
    console.time(1)
    let result = yoloInstance.detectImage(img, [])
    logd("result " + console.timeEnd(1) + " ms ---> " + result)
  }
  imageAgent.recycle(img)
  yoloInstance.release();

}

yoloagenttest2();
```

## yolov8Agent.newYolov8Onxx Initialize YOLOv8 ONNX Instance (Multi-Instance Supported)

* Initialize a YOLOv8 ONNX instance
* Requires EC 9.0.0+
* @return `Yolov8AgentUtil` instance object

```javascript showLineNumbers
function yoloagenttest() {
  yolov8Agent.releaseAll()
  let yoloOnnxInstance = yolov8Agent.newYolov8Onnx()

  logd("yoloOnnxInstance " + yoloOnnxInstance.yolov8AgentId)
  let onnxConfig = yoloOnnxInstance.getOnnxConfig(["aixin", "pinglun"], 0, 0, 0.35, 0.55, -1)
  logd("Start upload onnx file...")
  // Upload model file and initialize
  let onnxPath = utils.uploadAgentFile("/Users/x/iosidea/yolo-onnx/src/res/best.onnx", "onnx.onnx")
  let onnxOk = yoloOnnxInstance.initYoloModel(onnxConfig, onnxPath, "")
  if (!onnxOk) {
    console.log("err " + yoloOnnxInstance.getErrorMsg())
    return;
  }
  let img = utils.uploadToAutoImage("/Users/x/iosidea/yolo-onnx/src/res/1.png")
  logd("img -> " + img)

  for (let i = 0; i < 10; i++) {
    console.time(1)
    let result = yoloOnnxInstance.detectImage(img, ["aixin"])
    logd("result " + console.timeEnd(1) + " ms ---> " + result)
  }

  imageAgent.recycle(img)

  yoloOnnxInstance.release();

}

yoloagenttest();
```

## Yolov8AgentUtil.getOnnxConfig ONNX Config Options

* ONNX configuration options
* @param obj_names JSON array of class names; if omitted, ONNX reads them from the model, e.g. `["star","common","face"]`
* @param input_width Training image width; 0 lets ONNX extract it automatically
* @param input_height Training image height; 0 lets ONNX extract it automatically
* @param confThreshold Minimum confidence threshold for detections during ONNX inference
* @param iouThreshold IoU threshold used in NMS during ONNX inference
* @param numThread Thread count; usually half the CPU count. If unknown, omit
* @return `{JSON}`

```text
   See the `Initialize YOLOv8 instance` example
```

## Yolov8AgentUtil.getDefaultConfig Get YOLOv8 Default Config

* Get the default YOLOv8 configuration
* Requires EC 9.0.0+
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

## Yolov8Util.initYoloModel Initialize YOLOv8 Model

* Initialize the YOLOv8 model
* To generate param and bin files, see the YOLO usage chapter: convert YOLO pt to ncnn param/bin files
* For ONNX models, set binPath to null; paramPath is the ONNX file path
* Requires EC 9.0.0+
* @param map Parameter map; for ncnn use getDefaultConfig, for onnx use getOnnxConfig
* @param paramPath Path to the param file
* @param binPath Path to the bin file
* @return boolean true on success, false on failure

```text
   See the `Initialize YOLOv8 instance` example
```

## Yolov8AgentUtil.detectImage Detect AutoImage

* Detect objects in an image
* Requires EC 9.0.0+
* Example return data:
* `[{"name":"heart","confidence":0.92,"left":957,"top":986,"right":1050,"bottom":1078}]`
* name: class name; confidence: confidence score; left, top, right, bottom: bounding box coordinates
* @param image AutoImage object
* @param obj_names JSON array; omit to skip filtering, or provide class names to keep
* @return string

```text
   See the `Initialize YOLOv8 instance` example
```

## Yolov8AgentUtil.release Release YOLOv8 Resources

* Release YOLOv8 resources
* Requires EC 9.0.0+
* @return boolean

```text
   See the `Initialize YOLOv8 instance` example
   Release when the script ends; no need after every use
```

## Yolov8AgentUtil.getErrorMsg Get YOLOv8 Error Message

* Get the YOLOv8 error message
* Requires EC 9.0.0+
* @return string

```text
   See the `Initialize YOLOv8 instance` example
```
