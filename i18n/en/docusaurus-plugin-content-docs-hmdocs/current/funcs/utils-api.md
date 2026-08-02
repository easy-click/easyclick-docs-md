---
title: Utility Functions
description: EasyClick automation scripts — HarmonyOS Next utility functions
keywords:
 - EasyClick automation scripts HarmonyOS Next utility functions
 - utils
 - param
 - zip
 - MD5
 - zipFile
 - unzipWithEncode
 - unzip
 - passwd
 - ip
 - return
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
* @param files Files or folders to compress as an array, e.g. `["c:/a.txt","c:/bb/"]`
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    let zipFile = "c:/a.zip"
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
* @param zipFile Path to the target zip file
* @param passwd Zip file password
* @param destDir Target folder to extract into
* @param fileNameEncode Filename encoding; use GBK for Windows-created zips, UTF-8 on other platforms
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    let zipFile = "C:/a.zip"
    let ure = utils.unzipWithEncode(zipFile, "", "c:/test111/", "GBK");
    logd("unzip result: " + ure);
}

main()
```

### utils.unzip Unzip {#utilsunzip-解压}

* Extract a zip file into a folder
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
* @param zipFile Path to the zip file
* @param passwd Zip file password
* @param filePathInZip Path of the file inside the zip, e.g. a/b.txt
* @return `{string}` Parsed string content

```javascript showLineNumbers
function main() {

    zipFile = "c:/a.zip"
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
