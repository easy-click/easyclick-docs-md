---
title: EasyClick iOS Standalone — Changelog
hide_title: false
hide_table_of_contents: false
sidebar_label: Changelog
description: 'EasyClick iOS standalone automation changelog — no jailbreak, no hardware; features per release'
keywords:
 - EasyClick
 - EasyClick iOS standalone
 - iOS automation
 - iOS no jailbreak
 - iOS no hardware
 - mobile automation scripts
 - automation testing
 - changelog
 - 7.9.0
 - 7.8.0
 - 7.7.0
 - 7.0.0
 - 6.7.0
 - 6.6.0
 - 6.5.0
 - 6.4.0
 - 6.3.0
 - 6.2.0
 - 6.1.0
 - 6.0.0
 - 5.23.0
 - 5.22.0
 - screen mirroring
 - control center
 - self-activation
---

# Changelog

## Latest release

### 7.9.0

**Released: 2026-08-11**

```text
- Fixed single-app mode engine failing to start on 26.6
- General improvements
```

## Previous releases

### 7.8.0

**Released: 2026-07-25**

```text
- Added PPOCR-V6 text recognition model and functions
- Added commands that support control center screen mirroring
- Added PPOCR-V6 recognition of the current screen for standalone mirroring
- Improved floating window not showing in some cases
- Upgraded onnxruntime to 1.20.1
```

### 7.7.0

**Released: 2026-07-16**

```text
- Hardened code that could cause crashes
- General improvements

```

### 7.6.0

**Released: 2026-07-13**

```text
- Added option for network request mode to ignore SSL errors on all requests
- Removed unnecessary hot-update logs
- Hardened code that could cause crashes
- General improvements

```

### 7.5.0

**Released: 2026-07-01**

```text
 - Fixed code that could cause crashes
```

### 7.4.0

**Released: 2026-06-20**

```text
 - Added Aux remote assist module service for USB control center, bringing standalone features to USB
 - Fixed app crashes with no network or on mobile networks
 - Fixed setStopCallback sometimes not firing
 - Improved Bluetooth tap frame drops during control center screen mirroring
 - Improved Bluetooth firmware; supports ESP32S3
 - Improved App–Bluetooth linking
 - Fixed black screen with wireless control center mirroring
 - 26.5 system images are included in the developer image zip
```

### 7.3.0

**Released: 2026-06-09**

```text
- Added log window title bar showing running status
- Added log window info bar and setLogWindowInfoText for showing related data
- Added log window playback-state control (setLogWindowForcePlaybackPaused)
  -  Set to true: lower CPU use; logs still show; play button is a triangle; script start/stop still controllable
  -  Set to false: logs show; play button is pause; higher CPU use
- Added app settings permission-grant section
- Hardened code and parameters that could cause crashes
- Improved CPU usage from log printing

```

### 7.2.0

**Released: 2026-06-06**

```text
- Fixed orientation detection in single-app mode
- Fixed landscape/portrait coordinate conversion
- Fixed landscape issue with screen-recording mirroring
- Improved slow debug service
- Hardened code that could cause crashes
- Improved standalone control center screen mirroring

```

### 7.1.0

**Released: 2026-05-30**

```text
- Improved node lookup across all apps; often millisecond-level
- Support node lookup on video pages; improved single-app automation with mirroring and more
- Support free Apple ID signing via Sideloadly, i4Tools, and similar tools
- Support keeping the process alive without showing the floating window
- Support using the main app as the USB automation program; USB can use the main app directly without signing a proxy IPA
- General improvements

```

### 7.0.0

**Released: 2026-05-26**

```text
- Merged main app and proxy app; supports single-app and dual-app dual automation engine modes
  - Single-app mode needs only the main app to start automation
- Added Start Automation screen in App Settings
- Added startEnv auto-start automation in single-app mode
- Added createSession; call before fetching nodes to save time
- Added get/set automation engine type functions
- Added CLI command to set node parameters
- Fixed http.request 302 redirect and double URL encoding issues

```

### 6.10.0

**Released: 2026-05-21**

```text
- Fixed IDEA login failing in some regions
- Fixed standalone app login failing in some regions
- Fixed USB control center login failing in some regions
- General improvements

```

### 6.9.0

**Released: 2026-05-11**

```text
- Added CLI preview and run commands with arguments
- Added getCliArgs
- General improvements

```

### 6.8.0

**Released: 2026-04-23**

```text
- Added CLI screen recognition
- Added OTG get MAC address function
- General improvements

```

### 6.7.0

**Released: 2026-04-10**

```text
- Added Bluetooth and OTG startKeepAlive to prevent the mouse from disappearing
  - Not needed for absolute coordinate systems
- Added standalone screen-recording mirroring support
- Added standalone control center mirroring OTG and Bluetooth operation UI
- Added standalone control center mirroring OTG and Bluetooth input
- Improved Bluetooth and OTG firmware

```

### 6.6.0

**Released: 2026-04-03**

```text
- Added OTG HID module and functions
- Added OTG HID settings in the App
- Added Bluetooth and OTG coordinate calibration
- Added shortcut screenshot command
- Added screen-recording screenshot
- Added IDEA packaging options to exclude IME, VPN, and screen-recording plugins
- Added IDEA non-automation screenshot distinction between screen-recording and other modes
  - In the iOS image/color panel → Capture settings
- General improvements

```

### 6.5.0

**Released: 2026-03-28**

```text
- Added Bluetooth BLE HID module
- Added self-activation screenshot and get-orientation functions
- Added Bluetooth BLE settings in Settings
- Refactored Settings page
- General improvements
```

### 6.4.0

**Released: 2026-01-27**

```text
- Fixed possible crash from looping VPN logs
- Disabled unnecessary VPN log output
- General improvements

```

### 6.3.0

**Released: 2026-01-24**

```text
- Added execAsyncFile to run a JS file on a worker thread
- Added execAsyncStr to run a JS string on a worker thread
- Fixed stopping one thread also stopping other threads
- Locked thread shared data to prevent crashes
- Upgraded WebSocket library
- Fixed IDEA remote debug disconnect issues
```

### 6.2.0

**Released: 2026-01-19**

```text
- Added self-activation installApp install/uninstall
- Added option in the app to hide most of the username
- Added NCNN-OCR support for standalone mirroring recognition
- Added timeouts to every self-activation module function
- Added bandwidth saving when the screen is idle
- Added random urlScheme generation in IDEA packaging when unset
- Fixed self-activation killApp PID lookup
- General improvements
```

### 6.1.0

**Released: 2026-01-13**

```text
- Split self-activation into its own module
- Added self-activation open/close app
- Added self-activation get running app processes
- Added self-activation reboot phone
- General improvements
- This version does not require replacing the proxy IPA; replace the main app IPA
```

### 6.0.0

**Released: 2026-01-09**

```text
- Added self-activation: after reboot without a PC, the proxy IPA can start
- Added startActiveMySelf and mountDevImageOk for calling self-activation from scripts
- Added Self-Activation section on the main app Settings page
- General improvements
- This version does not require replacing the proxy IPA; replace the main app IPA
```

### 5.23.0

**Released: 2025-11-10**

```text
- Added IDEA find-color mark coordinates onto image
- Added IDEA clipboard parse into find-color fields
- Added IDEA image/color panel OCR
- General improvements
```

### 5.22.0

**Released: 2025-11-04**

```text
- Hardened thread.getShareValue against concurrent crashes
- Improved storing OpenCV mat data at the lower layer
- Fixed paddleNcnnOcr English word segmentation
- General improvements
```

### 5.21.0

**Released: 2025-10-12**

```text
- Added paddleNcnnOcrV5 text recognition with built-in PPOCR support
- Extended ocrImage parameters; supports dynamic padding, maxSideLen, and more
- General improvements
```

### 5.20.0

**Released: 2025-9-12**

```text
- Added thread KV share functions thread.putShareKeyValue, thread.getShareKeyValue
- Added thread list share functions thread.putShareValue, thread.getShareValue
- Fixed child threads not stopping when the script stops
- General improvements
- Thread share areas help avoid stalls when passing data between threads by calling functions directly
```

### 5.19.0

**Released: 2025-8-27**

```text
- Added cloud control connection DNS resolution; supports domains without China ICP filing
- Added reverse-connect for standalone mirroring — faster, lower resource use
- Added standalone control center mirroring OCR API for current screen text
- Improved crash handling: app stays up and only logs
- General improvements
```

### 5.18.0

**Released: 2025-8-17**

```text
- Added log floating window text orientation setting
- Added log floating window showTag and fitWidth settings
- Fixed floating window log line-number display
- General improvements
```

### 5.17.0

**Released: 2025-7-28**

```text
- Added yolo module releaseAll to free all resources
- Added ocr module releaseAll to free all resources
```

### 5.16.0

**Released: 2025-7-11**

```text
- Added YOLOv8 onnxruntime support and ONNX model files
- Upgraded OpenCV framework to 4.5.0
- Reimplemented ppocr-v5 with onnxruntime
- General improvements
- Note: this version has a paddlelite / onnx conflict; the previous paddlelite backend is now onnx — watch paddleliteocr parameter changes
- Online H5 template designer: https://uc.ieasyclick.com/designer

```

### 5.15.0

**Released: 2025-7-11**

```text
- Added online activation recognition when packaging changes the URL scheme
- Added startDebugServer to start the debug service
- Fixed error when gray-scaling an already gray image
- Fixed issue when binaryzing an already binary image
- Online H5 template designer: https://uc.ieasyclick.com/designer

```

### 5.13.0

**Released: 2025-6-26**

```text
- Upgraded paddleLiteOcr model to PPOCR-V5 for better accuracy
- General improvements
- Online H5 template designer: https://uc.ieasyclick.com/designer

```

### 5.12.0

**Released: 2025-6-21**

```text
- Added paddleLiteOcr using the ppocrv4 model
- General improvements
- Online H5 template designer: https://uc.ieasyclick.com/designer

```

### 5.11.0

**Released: 2025-6-11**

```text
- Added gray grayscale image function
- Added IDEA grayscale image feature
- Online H5 template designer: https://uc.ieasyclick.com/designer
```

### 5.10.0

**Released: 2025-5-6**

```text
- Added IDEA standalone template UI + script interaction code
- Added CORS support for standalone open API
- Added support for newer control center multi-file transfer
- Added mutual function injection and calls between H5 pages, ui.js, and scripts
- Hardened issues that could cause crashes
- Online H5 template designer: https://uc.ieasyclick.com/designer

```

### 5.9.0

**Released: 2025-4-10**

```text
- Added isReleaseIec to detect release scripts
- General improvements
- Online H5 template designer: https://uc.ieasyclick.com/designer

```

### 5.8.0

**Released: 2025-3-26**

```text
- Added TesseractOCR text recognition; see the OCR module
- General improvements
- Online H5 template designer: https://uc.ieasyclick.com/designer

```

### 5.7.0

**Released: 2025-3-24**

```text
- Added IDEA Vue + Element project template with drag-and-drop H5 UI generation
- Added IDEA menu to open the online H5 designer
- Fixed QR code generation in mat mode
- Hardened mat recycle crashes
- Other improvements
- Online H5 template designer: https://uc.ieasyclick.com/designer
```

### 5.6.0

**Released: 2025-3-12**

```text
- Added base64ToImage
- Changed normal multi-point find-color range to apply only to the first point
- Changed mat-mode multi-point find-color range to apply only to the first point
- General improvements
```

### 5.5.0

**Released: 2025-2-20**

```text
- Added maxChildCount (max child node count) to setFetchNodeParam
- Added IDEA node debug max child node count setting
- General improvements
- maxChildCount requires updating both the proxy IPA and the main app IPA
```

### 5.4.0

**Released: 2025-2-6**

```text
- Added online init/license on Settings so the activator is not required for init
- Improved app title bar height on high-resolution devices
- General improvements
```

### 5.3.0

**Released: 2025-1-20**

```text
- Added find-image/find-color functions ending in J that read from JSON files for easier edits
- Added helpers in api_ext.js such as _isNull
- Extended AutoImage so instances can find image/color directly without an image object
- Extended JS string and array helpers; see api_ext.js
- Fixed single-point color compare logic in mat mode
- Aligned multi-point color compare results with documented comments
```

### 5.2.0

**Released: 2025-1-8**

```text
- Added image/color range checks to prevent out-of-bounds
- Added IDEA packaging proxy port and runner packaging output to reduce fingerprints
- General improvements
- Usage: change the proxy IPA port when packaging, and sign/install both the packaged main app and proxy IPA
```

### 5.1.0

**Released: 2024-12-31**

```text
- Added IDEA right-click repair project structure
- Added console.info and related log functions
- Fixed Vue3 UI not running
- General improvements
```

### 5.0.0

**Released: 2024-12-19**

```text
- Added thread module
- Added setDisplayLineNumber
- Added npm support in IDEA and the App (project right-click → Add npm support)
- Added TypeScript support in IDEA and the App (project right-click → Add TypeScript support)
- Added IDEA auto-compile for TypeScript files
- Added IDEA gutter icons for readRes* functions; click to open the matching res file
- Improved require
- Improved keyboard IME start and input
- General improvements
```

### 4.12.0

**Released: 2024-12-09**

```text
- Added createQRCode
- Added decodeQRCode
- Fixed IDEA docs issues
- General improvements
```

### 4.10.0

**Released: 2024-11-14**

```text
- Added IDEA standalone remote debug
- Added View crash logs button in Settings
- Added dataMd5
- Added system notification and related functions
- General improvements
```

### 4.9.0

**Released: 2024-11-07**

```text
- Added requestPhotoAuthorization for photo library permission
- Added deleteAllPhotos to clear album photos
- Added deleteAllVideos to clear album videos
- Fixed packaging config options being overwritten
- Improved standalone cloud control mirroring operations
- General improvements
```

### 4.8.0

**Released: 2024-10-10**

```text
- Added getMyAppName and getMyBundleId for this IPA
- Added recycleAllImage to recycle all images
- Fixed exception log recording and IDEA viewing
- General improvements
```

### 4.7.3

**Released: 2024-10-06**

```text
- Added IDEA standalone normal and crash log viewing
- Added file module readLineEx, appendLineEx, deleteLineEx for large files
- Improved global crash log capture sync to IDEA and save under the app folder (exportable via i4Tools)
- Improved setLogLevel; floating window stays in sync
- Fixed findImageByColor throwing when no image is found
- General improvements
```

### 4.7.0

**Released: 2024-09-29**

```text
- Added xpath node lookup
- Improved find-color and related features in opencvmat mode
- Improved find-image, find-color, OCR, and more in proxy compute mode
- General improvements
Using opencvmat mode: lower memory and CPU; faster find-color/find-image
Measured ~50%–80% less memory, ~20%–30% less CPU, ~100%–200% faster
```

### 4.6.0

**Released: 2024-09-22**

```text
- Added opencvMat format for image data storage
- Reimplemented find-color, find-image, template match, and more with mat
- Added image.useOpencvMat to switch storage and find-color/find-image algorithms
- Added imageToMatFormat and matToImageFormat for format conversion
- Added getLanIp for LAN IP
- Improved log floating window CPU use from Log printing
- Improved memory use in several areas
Using opencvmat mode: lower memory and CPU; faster find-color/find-image
Measured ~50%–80% less memory, ~20%–30% less CPU, ~100%–200% faster
```

### 4.5.0

**Released: 2024-09-08**

```text
- Added ocrlite text recognition; see the OCR chapter
- Added OpenCV findImage and matchTemplate
- Added OpenCV binaryzationEx
- Added IDEA support to test standalone findImage and matchTemplate
- Added IDEA support for standalone OpenCV binarization
- Added play/stop MP3
- General improvements
```

### 4.3.0

**Released: 2024-09-01**

```text
- Added YOLOv8 neural network support (see yolov8 function docs)
- Fixed standalone control center device scan address lookup
- General improvements
```

### 4.2.0

**Released: 2024-08-06**

```text
- Added standalone plugin development support
- Added IDEA debug-build packaging support
- Added captureFullScreenUIImage (returns UIImage)
- Added autoImageToUIImage, uiimageToAutoImage, and related helpers
- Added pluginLoader module for plugin interaction
- Added plugin development demo and docs
- General improvements
- 2.0.0+ requires the standalone activator; see Advanced — Standalone Activator Tutorial
```

### 4.1.0

**Released: 2024-07-26**

```text
- Added IDEA compile-time JS syntax error hints
- Added file.listDir2 with optional recursion
- Fixed screenshot function parameter issues
- General improvements
- 2.0.0+ requires the standalone activator; see Advanced — Standalone Activator Tutorial
```

### 4.0.0

**Released: 2024-07-16**

```text
- Added iOS 17+ standalone support
- Improved proxy IPA APIs for iOS 17 standalone
- General improvements
- [Standalone 3.0.0+ requires iOS developer plugin 6.20.0+]
- [Recommend IDEA compile-time obfuscation; see EC Android obfuscation config]
- 2.0.0+ requires the standalone activator; see Advanced — Standalone Activator Tutorial
```

### 3.19.0

**Released: 2024-07-06**

```text
- Added hotupdater module for in-script hot updates
- Added script pause and resume
- Added pause/resume for standalone control center, cloud control, and IDEA
- Fixed standalone hot update case-sensitivity issue
- Added control center error log recording support
- General improvements
- [Standalone 3.0.0+ requires iOS developer plugin 6.20.0+]
- [Recommend IDEA compile-time obfuscation; see EC Android obfuscation config]
- 2.0.0+ requires the standalone activator; see Advanced — Standalone Activator Tutorial
```

### 3.18.0

**Released: 2024-04-13**

```text
- Added ocrMut module to instantiate multiple OCR engines; existing ocr module unchanged
- Added paddleOcrOnline text OCR type (requires EC paddleOcr PC app)
- Improved wireless mirroring when transferring with no connection
- Improved cloud control mirroring when transferring with no connection
- General improvements
- [Standalone 3.0.0+ requires iOS developer plugin 6.20.0+]
- [Recommend IDEA compile-time obfuscation; see EC Android obfuscation config]
- 2.0.0+ requires the standalone activator; see Advanced — Standalone Activator Tutorial
```

### 3.17.0

**Released: 2024-03-17**

```text
- Added official network verification hot-update private link support
- Improved debug address to show IPv4 only
- Improved Settings to show connection address after enabling the debug service
- Fixed IDEA new projects missing obfuscation files
- General improvements
- [Standalone 3.0.0+ requires iOS developer plugin 6.20.0+]
- [Recommend IDEA compile-time obfuscation; see EC Android obfuscation config]
- 2.0.0+ requires the standalone activator; see Advanced — Standalone Activator Tutorial
```

### 3.16.0

**Released: 2024-02-18**

```text
- Added custom IME special-symbol input
- Fixed IME keyboard landscape issues
- Fixed IME keyboard height growth
- Improved IME keyboard layout
- General improvements
- [Standalone 3.0.0+ requires iOS developer plugin 6.20.0+]
- [Recommend IDEA compile-time obfuscation; see EC Android obfuscation config]
- 2.0.0+ requires the standalone activator; see Advanced — Standalone Activator Tutorial
```

### 3.15.0

**Released: 2024-02-14**

```text
- Added device info to app hot updates
- Added imeApi IME module; see IME module docs
- Added URLScheme launch for the main app (default ieasyclick://)
- Added IDEA packaging custom main app URLScheme
- Added IDEA packaging option to remove the IME plugin module
- General improvements
- [Standalone 3.0.0+ requires iOS developer plugin 6.20.0+]
- [Recommend IDEA compile-time obfuscation; see EC Android obfuscation config]
- 2.0.0+ requires the standalone activator; see Advanced — Standalone Activator Tutorial
```

### 3.13.0

**Released: 2024-01-24**

```text
- Fixed setLogLevel log level not taking effect
- Added log-level parameter to setSaveLogEx
- Improved offline device alerts for standalone control center mirroring
- General improvements
- [Standalone 3.0.0+ requires iOS developer plugin 6.20.0+]
- [Recommend IDEA compile-time obfuscation; see EC Android obfuscation config]
- 2.0.0+ requires the standalone activator; see Advanced — Standalone Activator Tutorial
```

### 3.12.0

**Released: 2024-01-16**

```text
- Added base64Encode and base64Decode
- Improved IDEA save / remember last save path
- Fixed IDEA screenshots for some images
- General improvements
- [Standalone 3.0.0+ requires iOS developer plugin 6.20.0+]
- [Recommend IDEA compile-time obfuscation; see EC Android obfuscation config]
- 2.0.0+ requires the standalone activator; see Advanced — Standalone Activator Tutorial
```

### 3.10.0

**Released: 2023-12-21**

```text
- Added find-not-color image.findNotColor
- Added IDEA image/color panel findNotColor test
- General improvements
- [Standalone 3.0.0+ requires iOS developer plugin 6.20.0+]
- [Recommend IDEA compile-time obfuscation; see EC Android obfuscation config]
- 2.0.0+ requires the standalone activator; see Advanced — Standalone Activator Tutorial
```

:::tip
For businesses where the device ID can change, use ecid licensing.
If you use official network verification, also set ecid verification mode.
This version requires replacing the main app IPA; the proxy IPA does not need replacement.
:::

### 3.9.0

**Released: 2023-12-15**

```text
- Added ecid licensing support
- Added script list
- Added preferring the license with the most remaining time when both device ID and ecid are present
- Added network verification init support for switching between ecid and device ID
- General improvements
- [Standalone 3.0.0+ requires iOS developer plugin 6.20.0+]
- [Recommend IDEA compile-time obfuscation; see EC Android obfuscation config]
- 2.0.0+ requires the standalone activator; see Advanced — Standalone Activator Tutorial
```

### 3.8.0

**Released: 2023-12-04**

```text
- Added option after packaging to manually enable the debug service
- Added wireless control center connection support
- Added wireless screen mirroring support
- Added getCenterTaskInfo for control center script parameters
- General improvements
- [Standalone 3.0.0+ requires iOS developer plugin 6.20.0+]
- [Recommend IDEA compile-time obfuscation; see EC Android obfuscation config]
- 2.0.0+ requires the standalone activator; see Advanced — Standalone Activator Tutorial
```

### 3.6.0

**Released: 2023-11-15**

```text
- Added iOS cloud control screen mirroring
- Added remote operation for iOS cloud control mirroring
- Improved communication with cloud control
- General improvements
- [Standalone 3.0.0+ requires iOS developer plugin 6.20.0+]
- [Recommend IDEA compile-time obfuscation; see EC Android obfuscation config]
- 2.0.0+ requires the standalone activator; see Advanced — Standalone Activator Tutorial
```

### 3.5.0

**Released: 2023-10-25**

```text
- Added activeAppInfo
- Added IPA version info to error logs
- Improved appLaunch and appKillByBundleId with a force parameter
- General improvements
- [Standalone 3.0.0+ requires iOS developer plugin 6.20.0+]
- [Recommend IDEA compile-time obfuscation; see EC Android obfuscation config]
- 2.0.0+ requires the standalone activator; see Advanced — Standalone Activator Tutorial
```

:::tip
** Using the new appLaunch and appKillByBundleId requires updating the proxy IPA **
:::

### 3.2.0

**Released: 2023-10-16**

```text
- Added standalone activator USB reset
- Added downloadFile2 with resume support
- Fixed downloadFile crash
- Improved license initialization
- General improvements
- [Standalone 3.0.0+ requires iOS developer plugin 6.20.0+]
- [Recommend IDEA compile-time obfuscation; see EC Android obfuscation config]
- 2.0.0+ requires the standalone activator; see Advanced — Standalone Activator Tutorial
```

### 3.1.0

**Released: 2023-10-13**

```text
- Added network verification module
- Fixed hot-update crashes and freezes
- Fixed cloud control crash from wrong connection address
- Fixed cloud control get task info
- Improved sleep to avoid long resource holds
- Improved node fetch timing
- General improvements
- [Standalone 3.0.0+ requires iOS developer plugin 6.20.0+]
- [Recommend IDEA compile-time obfuscation; see EC Android obfuscation config]
- 2.0.0+ requires the standalone activator; see Advanced — Standalone Activator Tutorial
```

### 3.0.0

**Released: 2023-10-05**

```text
- Changed iec encryption algorithm for better security
- Fixed binarization find-color issues
- Improved find-image/find-color functions
- Improved findImageByColorEx
- [Standalone 3.0+ requires iOS developer plugin 6.20.0+]
- [Recommend IDEA compile-time obfuscation; see EC Android obfuscation config]
- 2.0.0+ requires the standalone activator; see Advanced — Standalone Activator Tutorial
```

### 2.2.0

**Released: 2023-09-26**

```text
- Added standalone integration with the cloud control system
- Added standalone cloud control module functions
- Added IDEA packaging for enterprise cloud control builds
- Added restartScript
- Added setStopCallback
- Added setExceptionCallback
- Improved standalone license info reading
- General improvements
- [Recommend IDEA compile-time obfuscation; see EC Android obfuscation config]
- 2.0.0+ requires the standalone activator; see Advanced — Standalone Activator Tutorial
```

### 2.1.2

**Released: 2023-09-15**

```text
- Fixed idMatch
- General improvements
- [Recommend IDEA compile-time obfuscation; see EC Android obfuscation config]
- 2.0.0+ requires the standalone activator; see Advanced — Standalone Activator Tutorial
```

### 2.1.0

**Released: 2023-09-10**

```text
- Added tap/swipe functions with pressure values
- Added activator page rebind and transfer license
- Fixed location crash
- Fixed standalone activator login username length limit
- [Recommend IDEA compile-time obfuscation; see EC Android obfuscation config]
- 2.0.0+ requires the standalone activator; see Advanced — Standalone Activator Tutorial
```

### 2.0.0

**Released: 2023-09-06**

```text
- Added device.getDeviceName2
- Implemented device.getDeviceId
- Added getDeviceExpTime for license expiry
- Added sendDingDingMsg DingTalk alert
- Added tjCenter module
- Fixed occasional main app black screen on launch
- General improvements
- [Recommend IDEA compile-time obfuscation; see EC Android obfuscation config]
- 2.0.0+ requires the standalone activator; see Advanced — Standalone Activator Tutorial
```

### 1.7.0

**Released: 2023-08-31**

```text
- Fixed time returning only 3 digits
- Improved file.readAllLines
- Improved http.request requiring a timeout parameter
- Fixed setOrientation
- Fixed http.post when parameters are numbers
- Improved device.getDeviceName
- [Recommend IDEA compile-time obfuscation; see EC Android obfuscation config]
```

### 1.6.0

```text
- Added setPipCtrlScript to control conflicts with the camera
- Added Settings permission request UI
- Fixed image/color *Ex functions
- Fixed readAllLines
- Added rotateImage
- Fixed image to base64
- [Recommend IDEA compile-time obfuscation; see EC Android obfuscation config. Limited production use; this is not a stable release]
```

### 1.5.0

```text
- Fixed multi-thread image/color sync
- Added device.getScreenWidthHeight to get width and height together
  - getScreenWidth and getScreenHeight are deprecated (edge-case issues)
- Added getOrientation
- Added setOrientation
- Added setLogViewSizeEx, showLogWindow, closeLogWindow
- General improvements
- [Recommend IDEA compile-time obfuscation; see EC Android obfuscation config. Limited production use; this is not a stable release]

```

### 1.4.3

```text
- Fixed findImageByColor
- Fixed readResAutoImage return when resource is missing
- Fixed saveResToFile
- General improvements
- General improvements
- [Recommend IDEA compile-time obfuscation; see EC Android obfuscation config. Limited production use; this is not a stable release]
```

### 1.4.0

```text
- Added automatic isolation for multi-worker node operations
- Fixed UI not showing with no network or on 4G
- Fixed infinite loop from duplicate worker names
- General improvements
- [Recommend IDEA compile-time obfuscation; see EC Android obfuscation config. Limited production use; this is not a stable release]
```

### 1.3.0

```text
- Standalone supports iOS 12.0+ installs; on 12–15, start the proxy via activator or control center (post-holiday development); on 15+ tap the icon to start
    - Function notes:
        - 1. Apple Vision OCR needs 13.0+
        - 2. Log floating window needs 15.0+
- Added require
- Added boundsFilter node filter
- Added worker module to replace multi-threading
- Added getCurrentWorkerName
- Added IDEA node capture boundsFilter setting
- Added hot update
- Added WebSocket
- Added Settings restore default script and clear all UI config
- Added utils.fileMd5
- Fixed IDEA find-image code generation wrong method
- Fixed packaging script overwrite on upgrade
- Fixed image.clip
- Fixed lockNode
- Fixed window.ec.call errors
- Fixed execScript dynamic script type issues
- Moved UI parameter config under Documents (backup-readable via i4Tools)
- [Recommend IDEA compile-time obfuscation. Limited production use; this is not a stable release]
```

### 1.2.0

```text
- Improved OCR
- Improved image binarization
- Improved standard packaging
- Open API features for USB edition: clipboard, open URL, album ops, and more
- [Limited production use recommended; this is not a stable release]
```

### 1.1.0

```text
- Completed the following iOS standalone functions:
- readResAutoImage
- utils.getRangeInt, random
- http.request
- utils.saveImageToAlbum
- utils.saveImageToAlbumPath
- utils.saveVideoToAlbumPath
- image.isRecycled
- image.saveTo
- image.toBase64Format
- image.clip
- image.pixel
- image.getWidth
- image.getHeight
- image.argb
- [Not recommended for production; this is not a stable release]
```

### 1.0.0

```text
- Initial release
- [Not recommended for production; this is not a stable release]
```
