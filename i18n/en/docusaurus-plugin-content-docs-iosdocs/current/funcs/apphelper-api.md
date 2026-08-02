---
title: Helper Module
description: EasyClick automation scripts — iOS no jailbreak helper module
keywords:
 - EasyClick automation scripts iOS no jailbreak helper module
 - param
 - app
 - ipa
 - setHelperTimeout
 - setParam
 - uploadToAlbum
 - URL
 - EC
 - iOS
 - setPasteboard
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview

- The helper module requires an additional app. Download the File Transfer Helper IPA from the cloud drive, or install the standalone main app IPA 3.16.0+, sign it, and install on the device

:::tip

- The File Transfer Helper IPA is the standalone main app — same package
- The helper module and IME functions share the same IPA
 :::

## Settings

### setHelperTimeout Set Helper Request Timeout

* Requires EC iOS USB 9.19.0+
* @param t Unit is milliseconds; default is 300 seconds

```javascript showLineNumbers
function main() {
    appHelper.setHelperTimeout(500000)
}

main();
```

### setParam Set Helper App Parameters

* @param bundleIdPrefix Helper app bundleId prefix; separate multiple with commas
* @param appHelperPort Helper app port; default 18924; use 0 if unknown

```javascript showLineNumbers
function main() {
    appHelper.setParam("com.ieasyclick.ios.auto3", 0)
}

main();
```

## Photo Album

### uploadToAlbum Add to Photo Album

* Upload an image or video to the photo album
* Supported EC iOS 6.0.0+
* @param openApp Whether to open the helper app
* @param path File path on PC; image or video
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    appHelper.setParam("com.ieasyclick.ios.auto3", 0)
    // Supports PNG, JPEG, MP4
    // If the first parameter is true, the helper app opens automatically
    // If you open the helper app manually, set the first parameter to false
    let r = appHelper.uploadToAlbum(true, "C:/a.png")
    logd(r)
}

main();
```

## Clipboard

### setPasteboard Set Clipboard

* Supported EC iOS 6.0.0+
* @param openApp Whether to open the helper app
* @param content Content
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
 appHelper.setParam("com.ieasyclick.ios.auto3", 0)
 let r = appHelper.setPasteboard(true, "123456")
 logd(r)
}

main();
```

### getPasteboard Read Clipboard

* Read clipboard value
* Supported EC iOS 6.0.0+
* @param openApp Whether to open the helper app
* @return `{string}` returned data

```javascript showLineNumbers
function main() {
 appHelper.setParam("com.ieasyclick.ios.auto3", 0)
 let r = appHelper.getPasteboard(true)
 logd(r)
}

main();
```

## URL Operations

### openUrl Open URL

* Open URL / URL Schemes
* Supported EC iOS 6.0.0+
* @param openApp Whether to open the helper app
* @param url URL
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
 appHelper.setParam("com.ieasyclick.ios.auto3", 0)
 let r = appHelper.openUrl(true, "http://baidu.com")
 logd(r)

 r = appHelper.openUrl(true, "snssdk1128://")
 logd(r)
}

main();
```
