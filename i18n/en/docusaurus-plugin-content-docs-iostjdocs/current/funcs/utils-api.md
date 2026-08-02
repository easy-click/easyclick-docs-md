---
title: Utility Functions
description: EasyClick automation scripts — iOS no jailbreak utility functions
keywords:
 - EasyClick automation scripts iOS no jailbreak utility functions
 - URL
 - param
 - takeMeToFront
 - 'true'
 - 'false'
 - url
 - setClipboardText
 - getClipboardText
 - openUrl
 - saveImageToAlbum
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview {#说明}

## Clipboard {#剪切板}

### setClipboardText Set Clipboard {#setclipboardtext-设置剪切板}

* Set clipboard text. Note: enable Picture-in-Picture or call `takeMeToFront` to bring this app to the foreground
* @param text Text content
* @param type 1 = text, 2 = link
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    takeMeToFront()
    sleep(1000)
    var r = utils.setClipboardText("22222", 1);
    logd(r)
}

main();
```

### getClipboardText Read Clipboard {#getclipboardtext-读取剪切板}

* Read clipboard text. Note: enable Picture-in-Picture or call `takeMeToFront` to bring this app to the foreground
* @return `{string}` Clipboard data

```javascript showLineNumbers
function main() {
    takeMeToFront()
    sleep(1000)
    var r = utils.getClipboardText();
    logd(r)
}

main();
```

## Open URL {#打开url}

### openUrl Open URL {#openurl-打开url}

* Open a URL. Note: bring the app to the foreground first with `takeMeToFront`
* @param url URL address
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    takeMeToFront()
    sleep(1000)
    var r = utils.openUrl("http://baidu.com");
    logd(r)
}

main();
```

## Photo Library {#相册相关}

### saveImageToAlbum Save Image to Album {#saveimagetoalbum-保存图像到相册}

* Save an image to the photo library
* @param img AutoImage object
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    // Request photo permission once at script start; grant manually or in Settings → EC app → Photos
    utils.requestPhotoAuthorization()
    sleep(1000)
    let img = image.captureFullScreen()
    logd("img getHeight " + image.getHeight(img))
    logd("img getWidth " + image.getWidth(img))
    var r = utils.saveImageToAlbum(img);
    logd(r)
}

main();
```

### saveImageToAlbumPath Save Image Path to Album {#saveimagetoalbumpath-保存图像路径到相册}

* Save an image file path to the photo library
* @param path File path
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    // Request photo permission once at script start; grant manually or in Settings → EC app → Photos
    utils.requestPhotoAuthorization()
    sleep(1000)
    let url = "http://192.168.2.10:8199/resource/image/wx.png"
    let box = file.getInternalDir("documents")
    logd("sandbox " + box)
    let filex = box + "/bb.png"
    logd("file " + filex)
    let r = http.downloadFile(url, filex, 10000, null)
    logd("download " + r)
    let rx = utils.saveImageToAlbumPath(filex)
    logd("r " + rx)
}

main();
```

### saveVideoToAlbumPath Save Video Path to Album {#savevideotoalbumpath-保存视频路径到相册}

* Save a video file path to the photo library
* @param path Video file path
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    // Request photo permission once at script start; grant manually or in Settings → EC app → Photos
    utils.requestPhotoAuthorization()
    sleep(1000)
    let url = "http://192.168.2.10:8199/resource/image/wx.mp4"
    let box = file.getInternalDir("documents")
    logd("sandbox " + box)
    let filex = box + "/bb.mp4"
    logd("file " + filex)
    let r = http.downloadFile(url, filex, 10000, null)
    logd("download " + r)
    let rx = utils.saveVideoToAlbumPath(filex)
    logd("r " + rx)
}

main();
```

### utils.requestPhotoAuthorization Request Photo Library Permission {#utilsrequestphotoauthorization-请求相册权限}

* Request photo library permission
* First request shows a permission dialog — allow it, or go to Settings → scroll to the bottom → EC app → enable Photos
* Note: These are async; ignore return values to avoid blocking simulated taps
* Requires EC standalone 4.9.0+
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    let rx = utils.requestPhotoAuthorization()
    logd("r " + rx)
    // Then call delete functions
    let a = utils.deleteAllPhotos()
    logd("a " + a)

}

main();
```

### utils.deleteAllPhotos Delete All Photos {#utilsdeleteallphotos-清空相册中的图片}

* Delete all photos in the library
* A confirmation dialog appears — simulate tapping Delete
* Note: These are async; ignore return values to avoid blocking simulated taps
* Requires EC standalone 4.9.0+
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    let rx = utils.requestPhotoAuthorization()
    logd("r " + rx)
    // Then call delete functions
    let a = utils.deleteAllPhotos()
    logd("a " + a)
}

main();
```

### utils.deleteAllVideos Delete All Videos {#utilsdeleteallvideos-清空相册中的视频}

* Delete all videos in the library
* A confirmation dialog appears — simulate tapping Delete
* Note: These are async; ignore return values to avoid blocking simulated taps
* Requires EC standalone 4.9.0+
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    let rx = utils.requestPhotoAuthorization()
    logd("r " + rx)
    // Then call delete functions
    let a = utils.deleteAllVideos()
    logd("a " + a)
}

main();
```

## Other {#其他}

### utils.dataMd5 Data MD5 {#utilsdatamd5-数据的md5}

* Compute MD5 of data
* Requires EC 4.10.0+
* @param data Data string
* @return `{string}` MD5 string or null

```javascript showLineNumbers
function main() {
    let docs = file.getInternalDir("documents")
    var md5 = utils.fileMd5(docs + "/aaa.png");
    logd(md5)
}

main();
```

### utils.fileMd5 File MD5 {#utilsfilemd5-文件的md5}

* Compute MD5 of a file
* @param file File path
* @returns `{string}` File MD5 string or null

```javascript showLineNumbers
function main() {
    let docs = file.getInternalDir("documents")
    var md5 = utils.fileMd5(docs + "/aaa.png");
    logd(md5)
}

main();
```

## Random {#随机}

### utils.getRangeInt Get Random Value in Range {#utilsgetrangeint-取得某个范围的随机值}

* Get a random integer within a range
* @param min Minimum value
* @param max Maximum value
* @return Integer between min and max, inclusive of min, exclusive of max

```javascript showLineNumbers
function main() {
    var r = utils.getRangeInt(2, 10);
}

main();
```

## Base64 {#base64}

### utils.base64Encode Base64 Encode {#utilsbase64encode-base64编码}

* Base64 encode
* @param data String to encode
* @returns `{string}` Encoded result

```javascript showLineNumbers
function main() {
    var r = utils.base64Encode("111");
    logd(r)
}

main();
```

### utils.base64Decode Base64 Decode {#utilsbase64decode-base64解码}

* Base64 decode
* @param data String to decode
* @returns `{string}` Decoded result

```javascript showLineNumbers
function main() {
    var r = utils.base64Decode("MjIy");
    logd(r)
}

main();
```

## Audio Playback {#音乐播放}

### utils.playMp3 Play MP3 {#utilsplaymp3-播放mp3音乐}

* Play MP3 asynchronously
* Requires EC standalone 4.5.0+
* @param path File path
* @param loop Loop playback; true to loop
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    let path = file.getSandBoxFilePath("test.mp3");
    // Copy test.mp3 from script res directory to device path
    saveResToFile("test.mp3", path)
    let d = utils.playMp3(path, false)
    logd("dd " + d)
    // Wait for playback to start
    sleep(30 * 1000)
    utils.stopMp3()
    logd("stop play ")
}

main();
```

### utils.playMp3WaitEnd Play MP3 Synchronously {#utilsplaymp3waitend-同步播放mp3音乐}

* Play MP3 and wait until finished
* Requires EC standalone 4.5.0+
* @param path File path
* @param loop Loop playback; true to loop
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    let path = file.getSandBoxFilePath("test.mp3");
    // Copy test.mp3 from script res directory to device path
    saveResToFile("test.mp3", path)
    let d = utils.playMp3WaitEnd(path, false)
    logd("dd " + d)
}

main();
```

### utils.stopMp3 Stop MP3 Playback {#utilsstopmp3-停止播放mp3音乐}

* Stop MP3 playback
* Requires EC standalone 4.5.0+
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {

    let path = file.getSandBoxFilePath("test.mp3");
    // Copy test.mp3 from script res directory to device path
    saveResToFile("test.mp3", path)
    let d = utils.playMp3(path, false)
    logd("dd " + d)
    // Wait for playback to start
    sleep(30 * 1000)
    utils.stopMp3()
    logd("stop play ")
}

main();
```

## Notifications {#通知栏}

### utils.requestNotificationAuthorization Request Notification Permission {#utilsrequestnotificationauthorization-请求通知授权}

* Request notification permission
* A permission dialog appears — simulate tapping Allow
* Note: These are async; ignore return values to avoid blocking simulated taps
* Requires EC standalone 4.10.0+
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    let rx = utils.requestNotificationAuthorization()
    logd("r " + rx)
    // Then show notification
    let a = utils.showNotification("Title", "Content", 1)
    logd("a " + a)

}

main();
```

### utils.showNotification Show Notification {#utilsshownotification-显示通知}

* Show a notification
* Requires EC standalone 4.10.0+
* @param title Title
* @param content Content
* @param delay Delay before showing, in seconds (integer)
* @return `{string}` Notification ID for later cancel operations

```javascript showLineNumbers
function main() {
    let rx = utils.requestNotificationAuthorization()
    logd("r " + rx)
    // Then show notification
    let a = utils.showNotification("Title", "Content", 1)
    logd("a " + a)

}

main();
```

### utils.removeNotification Remove Notification {#utilsremovenotification-清除通知}

* Remove a notification
* Requires EC standalone 4.10.0+
* Async function; ignore return value
* @param id Notification ID
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    let rx = utils.requestNotificationAuthorization()
    logd("r " + rx)
    // Then show notification
    let a = utils.showNotification("Title", "Content", 1)
    logd("a " + a)

    sleep(5000)
    utils.removeNotification(a)

}

main();
```

### utils.removeAllNotification Remove All Notifications {#utilsremoveallnotification-清除所有已经显示的通知}

* Remove all displayed notifications
* Note: These are async; ignore return values to avoid blocking simulated taps
* Requires EC standalone 4.10.0+
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    let rx = utils.requestNotificationAuthorization()
    logd("r " + rx)
    // Then show notification
    let a = utils.showNotification("Title", "Content", 1)
    logd("a " + a)

    sleep(5000)
    utils.removeAllNotification()

}

main();
```

## QR Code {#二维码}

### utils.createQRCode Generate QR Code {#utilscreateqrcode-生成二维码}

* Generate a QR code
* Requires EC standalone 4.12.0+
* @param content QR code string content
* @param width Image width
* @param height Image height
* @param logoImage Optional center logo AutoImage object; see image module
* @return `{AutoImage}` To save to file, see image module

```javascript showLineNumbers
function main() {

  let da = "test qrcode content"
  let logo = readResAutoImage("logo.png")
  let img = utils.createQRCode(da,200,100,logo)
  logd(img)
  
  // Decode result
  logd(utils.decodeQRCode(img));
  // Save to file
  image.saveTo(img,file.getSandBoxFilePath("/qr.code.jpg"))
  image.recycle(img)

}

main();
```

### utils.decodeQRCode Decode QR Code {#utilsdecodeqrcode-解析二维码}

* Decode a QR code
* Requires EC standalone 4.12.0+
* @param img AutoImage object; see image module
* @return `{string}` Decoded string

```javascript showLineNumbers
function main() {

  let da = "test qrcode content"
  let logo = readResAutoImage("logo.png")
  // Generate
  let img = utils.createQRCode(da,200,100,logo)
  logd(img)
  
  // Decode result
  logd(utils.decodeQRCode(img));
  // Save to file
  image.saveTo(img,file.getSandBoxFilePath("/qr.code.jpg"))
  image.recycle(img)

}

main();
```
