---
title: Utility Functions
description: EasyClick automation scripts — iOS no jailbreak utility functions
keywords:
 - EasyClick automation scripts iOS no jailbreak utility functions
 - utils
 - zip
 - param
 - MD5
 - unzipWithEncode
 - unzip
 - zipFile
 - passwd
 - ip
 - sdcard
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview {#说明}

## Compression and Decompression {#解压缩}

### utils.zip Compress {#utilszip-压缩}

* Compress multiple files into one zip file
* @param zipFile Path to the target zip file
* @param passwd Zip file password; empty means no password
* @param files Files or folders to compress as an array, e.g. `["/sdcard/a.txt","/sdcard/bb/"]`
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    zipFile = "/sdcard/a.zip"
    // Compress files
    let passwd = "123";
    let files = ["D:/test.json", "D:/gifshow"]
    let re = utils.zip(zipFile, passwd, files);
    logd("compress result: " + re);

    let ure = utils.unzip(zipFile, passwd, "D:/test111/");
    logd("unzip result: " + ure);


    let data = utils.readFileInZip("D:/a.zip", passwd, "test.json")
    logd("read data result: " + data);
}

main()
```

### utils.unzipWithEncode Unzip with Encoding {#utilsunzipwithencode-解压带编码}

* Unzip files
* Extract a zip file into a folder
* Requires EC iOS 6.460+
* @param zipFile Path to the target zip file
* @param passwd Zip file password
* @param destDir Target folder to extract into
* @param fileNameEncode Filename encoding; use GBK for Windows-created zips, UTF-8 on other platforms
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    let zipFile = "C:/a.zip"
    let ure = utils.unzipWithEncode(zipFile, "", "/sdcard/test111/", "GBK");
    logd("unzip result: " + ure);
}

main()
```

### utils.unzip Unzip {#utilsunzip-解压}

* Extract a zip file into a folder
* Requires EC 5.17.0+
* @param zipFile Path to the target zip file
* @param passwd Zip file password
* @param destDir Target folder to extract into
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    zipFile = "C:/a.zip"
    // Compress files
    let passwd = "123";
    let files = ["D:/test.json", "D:/gifshow"]
    let re = utils.zip(zipFile, passwd, files);
    logd("compress result: " + re);

    let ure = utils.unzip(zipFile, passwd, "D:/test111/");
    logd("unzip result: " + ure);


    let data = utils.readFileInZip("D:/a.zip", passwd, "test.json")
    logd("read data result: " + data);
}

main()
```

### utils.readFileInZip Read from ZIP {#utilsreadfileinzip-zip中读取}

* Read data from a zip file
* Requires EC 5.17.0+
* @param zipFile Path to the zip file
* @param passwd Zip file password
* @param filePathInZip Path of the file inside the zip, e.g. a/b.txt
* @return `{string}` Parsed string content

```javascript showLineNumbers
function main() {

    zipFile = "/sdcard/a.zip"
    // Compress files
    let passwd = "123";
    let files = ["D:/test.json", "D:/gifshow"]
    let re = utils.zip(zipFile, passwd, files);
    logd("compress result: " + re);

    let ure = utils.unzip(zipFile, passwd, "D:/test111/");
    logd("unzip result: " + ure);


    let data = utils.readFileInZip("D:/a.zip", passwd, "test.json")
    logd("read data result: " + data);
}

main()
```

### utils.getRatio Get Ratio {#utilsgetratio-取得比例}

* Get a ratio; e.g. parameter 10 returns a 10% probability. Returns true if the random ratio succeeds, false otherwise
* @param ratio Float from 1–100
* @return true or false

```javascript showLineNumbers
function main() {
    var ratio = utils.getRatio(20);
    logd(ratio);
}

main();
```

### utils.getRangeInt Get Random Value in Range {#utilsgetrangeint-取得某个范围的随机值}

* Get a random integer within a range
* @param min Minimum value
* @param max Maximum value
* @return Value between min and max, inclusive of both

```javascript showLineNumbers
function main() {
    var value = utils.getRangeInt(1, 100);
    logd(value);
}

main();
```

## MD5 {#md5}

### utils.fileMd5 File MD5 {#utilsfilemd5-文件的md5}

* Compute MD5 of a file
* @param file File path
* @return File MD5 string or null

```javascript showLineNumbers
function main() {
    var md5 = utils.fileMd5("D:/a.txt");
}

main();
```

### utils.dataMd5 Data MD5 {#utilsdatamd5-数据计算出来的md5}

* Compute MD5 of data
* @param data Data string
* @return Data MD5 string or null

```javascript showLineNumbers
function main() {
    var md5 = utils.dataMd5("data");
}

main();
```

## Random {#随机}

### utils.randomInt Random Integer {#utilsrandomint-随机整型数据}

* Generate a random integer
* @param length Number of digits for the random integer
* @return Integer

```javascript showLineNumbers
function main() {
    var r = utils.randomInt(2);
}

main();
```

### utils.randomCharNumber Random Alphanumeric String {#utilsrandomcharnumber-取得随机的数字和字母}

* Get a random string of letters and digits
* @param length Length
* @return Mixed alphanumeric string

```javascript showLineNumbers
function main() {
    var r = utils.randomCharNumber(2);
}

main();
```

## Audio {#音乐}

### utils.playMp3 Play MP3 {#utilsplaymp3-播放mp3}

* Play an MP3 file
* Requires EC USB 7.16.0+
* @param path File path to an MP3 on the PC
* @param volume Volume level 0–100
* @param queue Queue mode; false = play immediately
* @param stopWhenScriptEnd Stop playback when the script ends
* @return boolean true on success, false on failure

```javascript showLineNumbers
function main() {
    // To bundle MP3 in the script, put the file in the res directory
    saveResToFile("a.mp3", "d:/a.mp3")
    let r = utils.playMp3("d:/a.mp3", 100, false, false);
    sleep(10000)
    utils.stopMp3();
}

main();
```

### utils.stopMp3 Stop MP3 Playback {#utilsstopmp3-停止播放mp3}

* Stop MP3 playback
* Requires EC USB 7.16.0+
* @return boolean true on success, false on failure

```javascript showLineNumbers
function main() {
    // To bundle MP3 in the script, put the file in the res directory
    saveResToFile("a.mp3", "d:/a.mp3")
    let r = utils.playMp3("d:/a.mp3", 100, false, false);
    sleep(10000)
    utils.stopMp3();
}

main();
```

## Photo Library {#相册操作}

### utils.requestPhotoAuthorization Request Photo Library Permission {#utilsrequestphotoauthorization-请求相册权限}

* Request photo library permission
* First request shows a permission dialog — allow it, or go to Settings → scroll to the bottom → EC app → enable Photos
* Note: These are async; ignore return values to avoid blocking simulated taps
* Requires EC 7.18.0+
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
* Requires EC 7.18.0+
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
* Requires EC 7.18.0+
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

## Agent File Operations {#agent文件操作}

### utils.uploadAgentFile Upload File to Agent {#utilsuploadagentfile-发送文件到agent}

* Upload a file to the agent runner storage directory
* @param filePath Local file path
* @param fileName Saved file name
* @return `{string}` Remote file path after upload

```javascript showLineNumbers
function main() {
  let recFilePath = utils.uploadAgentFile("/Volumes/dev/ch_PP-OCRv5_rec_mobile_infer.onnx", "rec.txt")
  logd("custom recFilePath " + recFilePath)
}

main();
```

### utils.uploadToAutoImage Upload Image to Agent {#utilsuploadtoautoimage-上传图片到agent}

* Upload an image to the remote agent and convert it to an AutoImage object
* The object lives on the device; use only imageAgent, ocrAgent, or yoloAgent modules to operate on it
* @param filePath File path
* @return `{AutoImage|null}`

```javascript showLineNumbers
function main() {
  let r = utils.uploadToAutoImage("/Volumes/aa.png")
  logd("AutoImage " + r)
}

main();
```

### utils.deleteAgentFile Delete Agent Internal File {#utilsdeleteagentfile-删除代理ipa内部文件}

* Delete a file from agent IPA internal storage
* @param filePath File path
* @return `{boolean}` true on success

```javascript showLineNumbers
function main() {
  let recFilePath = utils.uploadAgentFile("/Volumes/dev/ch_PP-OCRv5_rec_mobile_infer.onnx", "rec.txt")
  let res = utils.deleteAgentFile(recFilePath)
  logd("delete result " + res)
}

main();
```

## Other {#其他}

### utils.getPCIps Get PC IP Addresses {#utilsgetpcips-获取本电脑的ip}

* Get this PC's IP addresses; multiple addresses are comma-separated
* @return `{null|string}` IP address string

```javascript showLineNumbers
function main() {
    let ips = utils.getPCIps();
    if (_isEmpty(ips)) {
        logd("PC IP not found")
        return false;
    }
    logd("PC IP: {}", ips)
}

main();
```
