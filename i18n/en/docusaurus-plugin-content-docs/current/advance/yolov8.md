---
title: EasyClick Android Docs_Android Phone Automation Scripts_YOLO Usage
hide_title: false
hide_table_of_contents: false
sidebar_label: YOLO Usage
description: 'EasyClick code hot update — automation without accessibility or USB debugging'
keywords:
 - EasyClick
 - mobile automation scripts
 - automation software
 - script hot update
 - code hot update
 - YOLO usage
 - https
 - com
 - YOLO
 - ncnn
 - EC
 - docs
 - download
 - anaconda
 - YOLOV8
 - yolov8
 - mobile automation
 - test automation
---
# YOLO Usage

:::tip Notes
- EC YOLOv8 requires EC Android 10.15.0+
- EC Android integrates Tencent/ncnn optimized for mobile devices
- ncnn supports YOLOv5–YOLOv8; this guide uses YOLOv8 for training, inference, and detection
- YOLOv8 official site [https://docs.ultralytics.com/modes/train/](https://docs.ultralytics.com/modes/train/)
- YOLO Demo download: [Resources](/docs/tools/download_resources), cloud drive `YOLO Resources` → `YOLODemo.zip`
 :::

## Install & Train Model
- YOLO training uses Python — download **Anaconda**
 - Download [https://www.anaconda.com/download](https://www.anaconda.com/download)
 - Tsinghua mirror: https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/Anaconda3-2024.06-1-Windows-x86_64.exe
 - More Anaconda info: https://blog.csdn.net/ddafei/article/details/140326536

### Install Software
- After installing **Anaconda**, create a virtual environment
- Click **Create**, name it **yolotest** (or other name), Python **3.8.19**, Create
 <br/><img src="/androidimg/yolo/yolo1.png" alt="" style={{zoom:'30%'}} />
- Click green triangle on **yolotest** → **Open Terminal**
 <br/><img src="/androidimg/yolo/yolo2.png" alt="" style={{zoom:'30%'}} />
 - Terminal prompt should show **(yolotest)**
 <br/><img src="/androidimg/yolo/yolo3.png" alt="" style={{zoom:'30%'}} />

### Configure Environment
- Install Python packages in terminal:
 ```shell showLineNumbers
  pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple/
  pip config set install.trusted-host pypi.tuna.tsinghua.edu.cn
  # yolo 0.3.1
  # Latest: pip install yolo
  pip install yolo==0.3.1
  # ultralytics 8.2.79
  # Latest: pip install ultralytics
  pip install ultralytics==8.2.79
  pip install ncnn==1.0.20240410
  pip install labelimg
  # Finally: pip list
  ```


- yolo, ultralytics installed
 <br/><img src="/androidimg/yolo/yolo4.png" alt="" style={{zoom:'20%'}} /><img src="/androidimg/yolo/yolo5.png" alt="" style={{zoom:'20%'}} />
- Run `yolo` in terminal to verify
 <br/><img src="/androidimg/yolo/yolo6.png" alt="" style={{zoom:'30%'}} />
- Run `labelimg` to open annotation tool
 <br/><img src="/androidimg/yolo/yolo7.png" alt="" style={{zoom:'30%'}} />

### Create Training Directory
- Create folder e.g. `yolotrain` on E: drive
- Under `yolotrain`: `labels/`, `images/`; under `images/`: `test`, `train`, `val`; under `labels/`: `train`, `val`
- Structure:
 <br/><img src="/androidimg/yolo/yolo8.png" alt="" style={{zoom:'30%'}} />
- Put same training images in `images/train`, `images/val`, and `images/test` (copy files)
- `labels/train` and `labels/val` hold labelimg annotation files

### Annotate Data
- Put images in **images/train**, copy to **images/val** and **images/test**
- Run **labelimg** from configured terminal
 - **Open Dir** → **images/train** (e.g. E:/yolotest/images/train)
 - **Change Save Dir** → **labels/train** (e.g. E:/yolotest/labels/train)
 - Set format to **YOLO** via **Save** format button
 <br/><img src="/androidimg/yolo/yolo9.png" alt="" style={{zoom:'30%'}} />

- Annotate
 - Example: **Like** and **Comment** buttons on a short-video app
 - Right-click → **Create RectBox** or click left **Create RectBox**
 <img src="/androidimg/yolo/yolo10.png" alt="" style={{zoom:'30%'}} />
 - Draw box on **Like** area, class name **aixin**, OK — used in API calls later
 <img src="/androidimg/yolo/yolo11.png" alt="" style={{zoom:'30%'}} />
 - Draw box on **Comment**, class **pinglun**, OK
 <img src="/androidimg/yolo/yolo12.png" alt="" style={{zoom:'30%'}} />

 - **Save**, **Next Image** for next image
 - **labels/train** gets `classes.txt` and per-image `.txt` files
 <img src="/androidimg/yolo/yolo13.png" alt="" style={{zoom:'30%'}} />
 - Copy **labels/train** to **labels/val** for validation

### Train Model
- In **yolotrain**, create **aixin.yaml**:
 ```yaml showLineNumbers
  path: E:/yolotrain
  train: images/train
  val: images/val
  test: images/test
  nc: 2
  names: ["aixin","pinglun"]
  ```
- Parameters:
 - path: training root (adjust drive/path)
 - train, val, test: image folders relative to path
 - nc: number of classes (2 here)
 - names: class names from labelimg — order matters

- Train in cmd: `e:/` then `cd yolotrain`
- Training command (pick one):
 ```shell showLineNumbers
  yolo detect train data=e:/yolotrain/aixin.yaml model=e:/yolotrain/yolov8s.pt imgsz=640
  ```
 ```shell showLineNumbers
  yolo detect train data=e:/yolotrain/aixin.yaml model=e:/yolotrain/yolov8s.pt epochs=100 imgsz=640
  ```

- Downloads `yolov8s.pt` — if fail, get from demo zip or https://github.com/ultralytics/assets/releases/download/v8.2.0/yolov8s.pt
 <img src="/androidimg/yolo/yolo14.png" alt="" style={{zoom:'30%'}} />
- Training in progress
 <img src="/androidimg/yolo/yolo15.png" alt="" style={{zoom:'30%'}} />

- Done — **Results saved** path varies; example `runs/detect/train` under E:/yolotrain
 <img src="/androidimg/yolo/yolo16.png" alt="" style={{zoom:'30%'}} />
- Find `best.pt` and annotated result images in output folder

### Validate Model
- Validate on images:
 ```shell showLineNumbers
  yolo detect val data=e:/yolotrain/aixin.yaml model=e:/yolotrain/runs/detect/train/weights/best.pt
  ```
- Check **Results saved** path for annotated images
 <br/><img src="/androidimg/yolo/yolo17.png" alt="" style={{zoom:'30%'}} />

### Export ONNX Model
- Export trained `.pt` to ONNX for EC API
 ```shell showLineNumbers
  yolo export model=e:/yolotrain/runs/detect/train/weights/best.pt format=onnx
  ```
- `export success` shows path — copy `.onnx` to phone `/sdcard/` (or other path)
- Initialize with `yolov8Api.newYolov8Onxx`; other API calls are the same


### Export NCNN Model
- Export to ncnn for EC
 ```shell showLineNumbers
  yolo export model=e:/yolotrain/runs/detect/train/weights/best.pt format=ncnn
  ```
- Downloads `pnnx` — use demo `pnnx-20240819-windows.zip` in e:/yolotrain/ if download fails
 - Match filename in error screenshot or download manually
 <br/><img src="/androidimg/yolo/yolo18.png" alt="" style={{zoom:'30%'}} />
- Output has `model.ncnn.param` and `model.ncnn.bin` — copy both to phone sdcard
 <br/><img src="/androidimg/yolo/yolo19.png" alt="" style={{zoom:'20%'}} />
- Initialize with `yolov8Api.newYolov8` for ncnn

## API Usage
- Put trained model on phone `/sdcard/` (or other path)
- See [YOLOv8 API Module](/docs/funcs/yolo-api)

## GPU
- GPU training: https://blog.csdn.net/shangyanaf/article/details/139029717

## CONDA Virtual Environment Error
- Cannot select Python version when creating env — often network; switch to Tsinghua mirror
- 1. Click channels
 <br/><img src="/androidimg/yolo/q1.png" alt="" style={{zoom:'50%'}} />
- 2. Remove default channels
 <br/><img src="/androidimg/yolo/q2.png" alt="" style={{zoom:'50%'}} />
- 3. Add Tsinghua channels (Enter after each)
 ```text showLineNumbers
  https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
  ```
 ```text showLineNumbers
  https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
  ```
 <br/><img src="/androidimg/yolo/q3.png" alt="" style={{zoom:'50%'}} />
- 4. Click Update channels
 <br/><img src="/androidimg/yolo/q4.png" alt="" style={{zoom:'50%'}} />

## Arial.ttf Download Error
- Download [Arial.ttf](/docs/Arial.ttf)
- Place in `C:\Users\Administrator\AppData\Roaming\Ultralytics` as `Arial.ttf` (`Administrator` = your Windows username)
