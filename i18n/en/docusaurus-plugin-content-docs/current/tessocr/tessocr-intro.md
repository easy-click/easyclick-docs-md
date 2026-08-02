---
title: EasyClick Android Docs_Android Phone Automation Scripts_Tesseract OCR Font Editor Usage
hide_title: false
hide_table_of_contents: false
sidebar_label: Tesseract Font Editor Usage
description: EasyClick Tesseract OCR font editor
keywords:
 - EasyClick
 - mobile automation scripts
 - automation software
 - Tesseract
 - OCR
 - font editor
 - game cloud control
 - tessocr
 - exe
 - TesseractOCR
 - tif
 - box
 - EC
 - iOS
 - editor
 - mobile automation
 - test automation
 - script development
---


# Tesseract Font Editor Usage

:::tip
- Trains Tesseract font libraries; based on jTessBoxEditor with simplified workflow for easier editing
- Trained Tesseract libraries work with EC Android, iOS USB, iOS standalone, HarmonyOS Next, etc.
:::

## Download & Install
- EC cloud drive → **Dev Tools → OCR Resources** → download **EasyClick-TesseractOCR-字库编辑器.zip**
- Extract and run **tessocr-editor.exe**
- Note: extract path must not contain Chinese, spaces, or special characters — otherwise validation may fail
- Download area: [Download Area](/community/download_area)

## Train Font Library

- Open **tessocr-editor.exe** — interface:
<img src='/tessocr/1.png' style={{zoom:'20%'}}/>
- Tesseract path — leave default (bundled tesseract). If changing, point to tesseract.exe

### Step 1: `Merge Images to TIF` Button
 - Merges jpg, png, bmp into a tif file — select folder or single image
 - Example: folder `E:/jtess/img/`, new font name `newfont` — log shows `生成tif文件成功` (TIF generated successfully)
 <img src='/tessocr/2.png' style={{zoom:'30%'}}/>
 <img src='/tessocr/3.png' style={{zoom:'30%'}}/>
 <img src='/tessocr/4.png' style={{zoom:'30%'}}/>

### Step 2: `Select TIF File`
 - Click select button to choose tif file
 <img src='/tessocr/5.png' style={{zoom:'30%'}}/>

### Step 3: Generate box Font File
 - Select `Generate box file`, click `Start`. Default PSM 6, base language `chi_sim+eng` (Chinese simplified + English)
 - box generation log
 <img src='/tessocr/6.png' style={{zoom:'30%'}}/>
 - System creates a box file in the tif folder
 <img src='/tessocr/7.png' style={{zoom:'30%'}}/>

### Step 4: Train Font Library
 - Generates Tesseract traineddata file
 - Select `Generate traineddata training set file`, click `Start`; log shows traineddata output path
 <img src='/tessocr/8.png' style={{zoom:'30%'}}/>

### Step 5: Validate
- Click `Validate`, pick an image to test the trained library
- Can clear `Base language set` value for validation
- If results are poor, use `Font Editor` tab to edit box and retrain/validate
 <img src='/tessocr/9.png' style={{zoom:'30%'}}/>

## Edit Font Library

- Interface:
<img src='/tessocr/10.png' style={{zoom:'30%'}}/>

### Open box File
- Click `Open` (shortcut `ctrl+o`), select a tif file — box opens automatically
- Left table: characters and coordinates; right: image and character boxes

### Edit Text
- Double-click table text and press Enter to edit
- Or edit Character field and coordinates at top

### Change Character Box Coordinates
- Select table row — character highlighted with red box on image
- Drag red box corner arrows to resize
- Drag red box to move
- Or use arrow keys to adjust coordinates
 <img src='/tessocr/11.png' style={{zoom:'30%'}}/>

### Zoomed Character View
- Click Character View tab; arrow keys move red box
- `ctrl+arrow` zoom box in/out until text is fully enclosed
<img src='/tessocr/12.png' style={{zoom:'30%'}}/>

## Image Editing
- Uses OpenCV for grayscale and binarization when training/recognition quality is poor

## Other Features
- Merge: merge multiple box characters
- Split: split one box into two
- Insert: new box
- Delete: remove box
- Font: table font setting (optional)
- box table right-click menu has additional actions

### Shortcuts
- `ctrl+o` — open tif/box
- `ctrl+s` — save box
- `ctrl+p` or `alt+p` — previous character
- `ctrl+n` or `alt+n` — next character
- Arrow keys — move box
- `ctrl+arrow` or `alt+arrow` — resize box
- Mouse drag — move box
- Mouse drag corner — resize box
- `ctrl+d` or `alt+d` — previous image
- `ctrl+g` or `alt+g` — next image

## Usage
- After editing and retraining to traineddata, use per tessocr tutorials

## Save
- Save box edits before closing to avoid data loss

## FAQ
- Validation fails
 - Extract path may contain Chinese/special chars — use English path
- App crashes on open
 - Install VC runtime from cloud drive, restart PC
- Training error FAILURE! couldn't find a matching blob
 - Tesseract cannot match shapes/features in the image. Try:
 - Higher image quality — clear, good contrast
 - Preprocessing — binarization, denoise, deskew
 - Tune Tesseract params — `psm`, `oem`
 - Adjust box ranges
