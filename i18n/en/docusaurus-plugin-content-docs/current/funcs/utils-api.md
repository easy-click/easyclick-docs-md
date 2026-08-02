---
title: Utility Functions
description: EasyClick automation scripts — Android no root utility functions
keywords:
 - EasyClick automation scripts Android no root utility functions
 - utils
 - APP
 - activity
 - param
 - return
 - App
 - openApp
 - getStartAppCmd
 - app
 - openAppByName
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview

- Utility module functions are mainly related to commonly used Android information
- The utility module uses the `utils` prefix, e.g. `utils.requestShowLogPermission()`

## Open App

### utils.openApp Open App

* Open an app
* @param packageName App package name
* @return boolean — true on success, false on failure

```javascript showLineNumbers
function main() {
    utils.openApp("com.xx");
}

main();
```

### utils.getStartAppCmd Open App via Command

* Build a command to open an app
* Requires EC 6.8.0+
* @param packageName App package name
* @return `{string}` Command string

```javascript showLineNumbers
function main() {
    startEnv()
    let pkg = "com.youdao.translator";
    // Get command
    let d = utils.getStartAppCmd(pkg);

    logd(d);

    // Execute activation command
    //let result = shell.execCommand(d);
    //logd("result ="+result);
    // Execute via root
    //var result = shell.sudo(d);
    //logd("result ="+result);
    // Enable agent service and execute command via agent service
    var result = shell.execAgentCommand(d);
    logd("result =" + result);
}

main();
```

### utils.openAppByName Open App (by App Name)

* Open an app by app name
* @param appName App name, e.g. Toutiao
* @return boolean — true on success, false on failure

```javascript showLineNumbers
function main() {
    utils.openAppByName("xx");
}

main();
```

### utils.openActivity Open Activity

* Open an activity via a map parameter
 * @param map e.g. ```{"action":""}``` — fixed keys:
    * action: standard Android action string
    * uri: can be an http URL, a file `file:///` path, or URL scheme syntax
    * type: MIME type for the uri, e.g. APK install type is `application/vnd.android.package-archive`
    * pkg: package name of the activity to open
    * className: class name of the activity to open
    * flag: standard Android activity flag; set only in special cases
    * all other keys are intent parameters
* @return boolean — true on success, false on failure

```javascript showLineNumbers
function main() {
 // Open APK installer screen
 var m = {
 "action": "android.intent.action.VIEW",
 "uri": "file:///sdcard/a.apk",
 "type": "application/vnd.android.package-archive"
 };
 var x = utils.openActivity(m);
 logd("x " + x);

 var map = {
 "uri": "xx://xx/live/6701887916223941379",
 };
 utils.openActivity(map);
}

main();
```

### utils.getStartActivityCmd Open Activity via Command

- Requires EC 6.8.0+

* Open an activity via a map parameter
    * @param map e.g. ```{"action":""}``` — fixed keys:
    * action: standard Android action string
    * uri: can be an http URL, a file `file:///` path, or URL scheme syntax
    * type: MIME type for the uri, e.g. APK install type is `application/vnd.android.package-archive`
    * pkg: package name of the activity to open
    * className: class name of the activity to open
    * flag: standard Android activity flag; set only in special cases
    * all other keys are intent parameters
* @return boolean — true on success, false on failure

```javascript showLineNumbers


function main() {
 startEnv()
 //let pkg = "com.youdao.translator";

 let map1 = {
 "action": "android.intent.action.VIEW",
 "pkg": "com.youdao.translator"
 }
 // URI demo for dialing; uri can be other values
 let map2 = {
 "action": "android.intent.action.CALL",
 "uri": "tel:1000"
 }
 // Get command
 let d = utils.getStartActivityCmd(map1);

 logd(d);

 // Execute activation command
 //let result = shell.execCommand(d);
 //logd("result ="+result);
 // Execute via root
 //var result = shell.sudo(d);
 //logd("result ="+result);
 // Enable agent service and execute command via agent service
 var result = shell.execAgentCommand(d);
 logd("result =" + result);

}


main()


```

### utils.openIntentAction Open a Screen via Action

* Open a screen via action
* @param action action string, e.g. `android.settings.ACCESSIBILITY_SETTINGS` for accessibility settings
* If this method does not meet your needs, use Intent directly:
* Common actions:
* android.settings.ACCESSIBILITY_SETTINGS // Accessibility
* android.settings.ADD_ACCOUNT_SETTINGS // Add account
* android.settings.AIRPLANE_MODE_SETTINGS // System settings home
* android.settings.APN_SETTINGS // APN settings
* android.settings.APPLICATION_SETTINGS // App management
* android.settings.BATTERY_SAVER_SETTINGS // Battery saver
* android.settings.BLUETOOTH_SETTINGS // Bluetooth
* android.settings.CAPTIONING_SETTINGS // Captions
* android.settings.CAST_SETTINGS // Wireless display
* android.settings.DATA_ROAMING_SETTINGS // Mobile network
* android.settings.DATE_SETTINGS // Date and time
* android.settings.DEVICE_INFO_SETTINGS // About phone
* android.settings.DISPLAY_SETTINGS // Display
* android.settings.DREAM_SETTINGS // Daydream
* android.settings.HARD_KEYBOARD_SETTINGS // Physical keyboard
* android.settings.HOME_SETTINGS // App permissions, default apps, special permissions
* android.settings.IGNORE_BATTERY_OPTIMIZATION_SETTINGS // Ignore battery optimization
* android.settings.INPUT_METHOD_SETTINGS // Virtual keyboard
* android.settings.INPUT_METHOD_SUBTYPE_SETTINGS // Android keyboard language (AOSP)
* android.settings.INTERNAL_STORAGE_SETTINGS // Storage
* android.settings.LOCALE_SETTINGS // Language preferences
* android.settings.LOCATION_SOURCE_SETTINGS // Location services
* android.settings.MANAGE_ALL_APPLICATIONS_SETTINGS // All apps
* android.settings.MANAGE_APPLICATIONS_SETTINGS // App management
* android.settings.MANAGE_DEFAULT_APPS_SETTINGS // Same as ACTION_HOME_SETTINGS
* android.settings.action.MANAGE_OVERLAY_PERMISSION // Display over other apps, floating window
* android.settings.MANAGE_UNKNOWN_APP_SOURCES // Install unknown apps (Android 8.0+)
* android.settings.action.MANAGE_WRITE_SETTINGS // Modify system settings permission
* android.settings.MEMORY_CARD_SETTINGS // Memory and storage
* android.settings.NETWORK_OPERATOR_SETTINGS // Available networks
* android.settings.NFCSHARING_SETTINGS // NFC sharing
* android.settings.NFC_SETTINGS // More network settings
* android.settings.ACTION_NOTIFICATION_LISTENER_SETTINGS // Notification access
* android.settings.NOTIFICATION_POLICY_ACCESS_SETTINGS // Do Not Disturb access
* android.settings.ACTION_PRINT_SETTINGS // Print services
* android.settings.PRIVACY_SETTINGS // Backup and reset
* android.settings.SECURITY_SETTINGS // Security
* android.settings.SHOW_REGULATORY_INFO // Regulatory info
* android.settings.SOUND_SETTINGS // Sound
* android.settings.SYNC_SETTINGS // Add account
* android.settings.USAGE_ACCESS_SETTINGS // Usage access
* android.settings.USER_DICTIONARY_SETTINGS // Personal dictionary
* android.settings.VOICE_INPUT_SETTINGS // Assistive apps and voice input
* android.settings.VPN_SETTINGS // VPN
* android.settings.VR_LISTENER_SETTINGS // VR listener
* android.settings.WEBVIEW_SETTINGS // WebView selection
* android.settings.WIFI_IP_SETTINGS // Advanced WLAN
* android.settings.WIFI_SETTINGS // Wi‑Fi selection and connection
* com.android.settings.Settings$DevelopmentSettingsActivity
* @return null|boolean

```javascript showLineNumbers
function main() {
 utils.openIntentAction("android.settings.ACCESSIBILITY_SETTINGS");
}

main();
```

- Open directly with Intent

```javascript showLineNumbers

importClass(android.content.Intent);
importClass(android.net.Uri)
var intent = new Intent();
intent.setAction("android.settings.APPLICATION_DETAILS_SETTINGS");
intent.setData(Uri.parse("package:com.gibb.easyclick"))
intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
try {
 context.startActivity(intent);
} catch (e) {
 loge(e)
}
```

## Gallery

### utils.insertImageToAlbum Insert Image into Gallery

* Insert an image into the gallery; the gallery is updated immediately
* Prefer placing files under `/sdcard/DCIM`; some devices do not scan files outside the gallery folder
* @param path Image path

```javascript showLineNumbers
function main() {
 utils.insertImageToAlbum("/sdcard/DCIM/a.png");
}

main();
```

### utils.insertVideoToAlbum Insert Video into Gallery

* Insert a video into the gallery; the gallery is updated immediately
* Prefer placing files under `/sdcard/DCIM`; some devices do not scan files outside the gallery folder
* @param path Video path

```javascript showLineNumbers
function main() {
 utils.insertVideoToAlbum("/sdcard/DCIM/a.mp4");
}

main();
```

## Compression

### utils.zip Compress

* Compress multiple files into one zip file
* Requires EC 5.17.0+
* @param zipFile Target zip file path
* @param passwd Zip password; empty means no password
* @param files Files or folders to compress, e.g. ```["/sdcard/a.txt","/sdcard/bb/"]```
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 zipFile = "/sdcard/a.zip"
 // Compress files
 let passwd = "123";
 let files = ["/sdcard/test.json", "/sdcard/gifshow"]
 let re = utils.zip(zipFile, passwd, files);
 logd("Compression result: " + re);

 let ure = utils.unzip(zipFile, passwd, "/sdcard/test111/");
 logd("Decompression result: " + ure);


 let data = utils.readFileInZip("/sdcard/a.zip", passwd, "test.json")
 logd("Read data result: " + data);
}

main()
```

### utils.unzip Decompress

* Extract a zip file to a folder
* Requires EC 5.17.0+
* @param zipFile Target zip file path
* @param passwd Zip password
* @param destDir Destination folder
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {

 zipFile = "/sdcard/a.zip"
 // Compress files
 let passwd = "123";
 let files = ["/sdcard/test.json", "/sdcard/gifshow"]
 let re = utils.zip(zipFile, passwd, files);
 logd("Compression result: " + re);

 let ure = utils.unzip(zipFile, passwd, "/sdcard/test111/");
 logd("Decompression result: " + ure);


 let data = utils.readFileInZip("/sdcard/a.zip", passwd, "test.json")
 logd("Read data result: " + data);
}

main()
```

### utils.unzipWithEncode Decompress with Encoding

* Decompress a zip file
* Extract a zip file to a folder
* Requires EC 10.0.0+
* @param zipFile Target zip file path
* @param passwd Zip password
* @param destDir Destination folder
* @param fileNameEncode File name encoding — use GBK for Windows-created zips; UTF-8 on other platforms
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {

 let zipFile = "/sdcard/a.zip"
 let ure = utils.unzipWithEncode(zipFile, "", "/sdcard/test111/", "GBK");
 logd("Decompression result: " + ure);
}

main()
```

### utils.readFileInZip Read from Zip

* Read data from a zip file
* Requires EC 5.17.0+
* @param zipFile Zip file path
* @param passwd Zip password
* @param filePathInZip Path inside the zip, e.g. a/b.txt
* @return `{string}` Parsed string

```javascript showLineNumbers
function main() {

 zipFile = "/sdcard/a.zip"
 // Compress files
 let passwd = "123";
 let files = ["/sdcard/test.json", "/sdcard/gifshow"]
 let re = utils.zip(zipFile, passwd, files);
 logd("Compression result: " + re);

 let ure = utils.unzip(zipFile, passwd, "/sdcard/test111/");
 logd("Decompression result: " + ure);


 let data = utils.readFileInZip("/sdcard/a.zip", passwd, "test.json")
 logd("Read data result: " + data);
}

main()
```

## Encryption and Decryption

### encodeDecoder.aesEncrypt AES Encrypt

* AES encryption, algorithm AES/ECB/PKCS5Padding
* Requires EC 5.19.0+
* @param data Data string
* @param password Password, at least 8 characters
* @return `{string}` Base64-encoded ciphertext

```javascript showLineNumbers
function main() {
 let dat = "sample data"
 let pwd = "12356783";
 let d = encodeDecoder.aesEncrypt(dat, pwd)
 logd("Encrypted data-" + d);
 let dd = encodeDecoder.aesDecrypt(d, pwd)
 logd("Decrypted data-" + dd);
}

main();
```

### encodeDecoder.aesDecrypt AES Decrypt

* AES decryption, algorithm AES/ECB/PKCS5Padding
* Requires EC 5.19.0+
* @param data Base64-encoded ciphertext
* @param password Password, at least 8 characters
* @return `{string}` Decrypted string

```javascript showLineNumbers
function main() {
 let dat = "sample data"
 let pwd = "12356783";
 let d = encodeDecoder.aesEncrypt(dat, pwd)
 logd("Encrypted data-" + d);
 let dd = encodeDecoder.aesDecrypt(d, pwd)
 logd("Decrypted data-" + dd);
}

main();
```

### encodeDecoder.desEncrypt DES Encrypt

* DES encryption, ADES algorithm
* Requires EC 5.19.0+
* @param data Data string
* @param password Password, at least 8 characters
* @return `{string}` Base64-encoded ciphertext

```javascript showLineNumbers
function main() {
 let dat = "sample data"
 let pwd = "12356783";
 let d = encodeDecoder.desEncrypt(dat, pwd)
 logd("Encrypted data-" + d);
 let dd = encodeDecoder.desDecrypt(d, pwd)
 logd("Decrypted data-" + dd);
}

main();
```

### encodeDecoder.desDecrypt DES Decrypt

* DES decryption, ADES algorithm
* Requires EC 5.19.0+
* @param data Base64-encoded ciphertext
* @param password Password, at least 8 characters
* @return `{string}` Decrypted string

```javascript showLineNumbers
function main() {
 let dat = "sample data"
 let pwd = "12356783";
 let d = encodeDecoder.desEncrypt(dat, pwd)
 logd("Encrypted data-" + d);
 let dd = encodeDecoder.desDecrypt(d, pwd)
 logd("Decrypted data-" + dd);
}

main();
```

### encodeDecoder.des3Encrypt 3DES Encrypt

* 3DES encryption, algorithm DESede/CBC/PKCS5Padding
* Requires EC 5.19.0+
* @param data Data string
* @param password Password
* @return `{string}` Base64-encoded ciphertext

```javascript showLineNumbers
function main() {
 let dat = "sample data"
 let pwd = "12356783";
 let d = encodeDecoder.des3Encrypt(dat, pwd)
 logd("Encrypted data-" + d);
 let dd = encodeDecoder.des3Decrypt(d, pwd)
 logd("Decrypted data-" + dd);
}

main();
```

### encodeDecoder.des3Decrypt 3DES Decrypt

* 3DES decryption, algorithm DESede/CBC/PKCS5Padding
* Requires EC 5.19.0+
* @param data Base64-encoded ciphertext
* @param password Password
* @return `{string}` Decrypted string

```javascript showLineNumbers
function main() {
 let dat = "sample data"
 let pwd = "12356783";
 let d = encodeDecoder.des3Encrypt(dat, pwd)
 logd("Encrypted data-" + d);
 let dd = encodeDecoder.des3Decrypt(d, pwd)
 logd("Decrypted data-" + dd);
}

main();
```

### encodeDecoder.getErrorMsg Get Encryption/Decryption Error

* Get the error message from the last encryption or decryption operation
* Requires EC 5.19.0+
* @return `{string}` null means no error

```javascript showLineNumbers
function main() {
 let dat = "sample data"
 let pwd = "12356783";
 let d = encodeDecoder.des3Encrypt(dat, pwd)
 logd("Encrypted data-" + d);
 let dd = encodeDecoder.des3Decrypt(d, pwd)
 logd("Decrypted data-" + dd);
 logd("getErrorMsg -" + encodeDecoder.getErrorMsg());
}

main();
```

## QR Code

### utils.createQRCode Generate QR Code

* Generate a QR code
* Requires EC 5.15.0+
* @param content QR code content string
* @param width Image width
* @param height Image height
* @param logo Optional center logo — Bitmap object; see the image module to load a file as Bitmap
* @return `{Bitmap}` Android Bitmap object; see the image module to save to a file

```javascript showLineNumbers
function main() {

 // Generate a QR code bitmap with logo
 let bot = utils.createQRCode("Hello World", 1000, 1000, image.readBitmap("/sdcard/yyb2.png"));
 logd("bot " + bot);
 // Save to file
 let saved = image.saveBitmap(bot, "png", 100, "/sdcard/tmp.png");
 logd("saved " + saved);
 // Recycle to prevent memory growth
 if (bot) {
 bot.recycle()
 }
 // Scan QR code
 let bitmap = image.readBitmap("/sdcard/tmp.png")
 let data = utils.decodeQRCode(bitmap);
 logd("data " + data);
 // Recycle to prevent memory growth
 if (bitmap) {
 bitmap.recycle()
 }
}

main();
```

### utils.decodeQRCode Decode QR Code

* Decode a QR code
* Requires EC 5.15.0+
* @param src Bitmap object; see the image module to load a file as Bitmap
* @return `{string}` Decoded string

```javascript showLineNumbers
function main() {

 // Generate a QR code bitmap with logo
 let bot = utils.createQRCode("Hello World", 1000, 1000, image.readBitmap("/sdcard/yyb2.png"));
 logd("bot " + bot);
 // Save to file
 let saved = image.saveBitmap(bot, "png", 100, "/sdcard/tmp.png");
 logd("saved " + saved);
 // Recycle to prevent memory growth
 if (bot) {
 bot.recycle()
 }
 // Scan QR code
 let bitmap = image.readBitmap("/sdcard/tmp.png")
 let data = utils.decodeQRCode(bitmap);
 logd("data " + data);
 // Recycle to prevent memory growth
 if (bitmap) {
 bitmap.recycle()
 }
}

main();
```

## Other

### utils.readConfigInt Read Integer Config

* Read an integer from JSON
* @param jsonObject JSON object
* @param key Config key
* @return integer — 0 if not found

```javascript showLineNumbers
function main() {
 var testData = utils.readConfigInt(jsonObject, "test_key");
}

main();
```

### utils.readJSONString Read String from JSON

* Read a string from JSON
* @param jsonObject JSON object
* @param key Config key
* @return string — empty string if not found

```javascript showLineNumbers
function main() {
 var testData = utils.readConfigString(jsonObject, "test_key");
}

main();
```

### utils.isObjectNull Check Object Is Null

* Check whether an object is null
* @param o Object
* @return true or false

```javascript showLineNumbers
function main() {
 var isNull = utils.isObjectNull("test_key");
}

main();
```

### utils.isObjectNotNull Check Object Is Not Null

* Check whether an object is not null
* @param o Object
* @return true or false

```javascript showLineNumbers
function main() {
 var isNull = utils.isObjectNotNull("test_key");
}

main();
```

### utils.getRatio Get Ratio

* Get a ratio — e.g. parameter 10 means 10% probability; returns true if the random draw succeeds, false otherwise
* @param ratio float 1–100
* @return true or false

```javascript showLineNumbers
function main() {
 var ratio = utils.getRatio(20)
 toast(ratio);
}

main();
```

### utils.getRangeInt Random Integer in Range

* Get a random integer in a range
* @param min Minimum value
* @param max Maximum value
* @return value between min and max, inclusive

```javascript showLineNumbers
function main() {
 var value = utils.getRangeInt(1, 100);
 toast(value);
}

main();
```

### utils.isTrue Check Boolean Is True

* Check whether a boolean value is true
* @param r Boolean value
* @return true or false

```javascript showLineNumbers
function main() {
 var value = utils.isTrue(true);
}

main();
```

### utils.fileMd5 File MD5

* Compute MD5 of a file
* @param file File path
* @return MD5 string or null

```javascript showLineNumbers
function main() {
 var md5 = utils.fileMd5("/sdcard/a.txt");
}

main();
```

### utils.dataMd5 Data MD5

* Compute MD5 of data
* @param data Data
* @return MD5 string or null

```javascript showLineNumbers
function main() {
 var md5 = utils.dataMd5("data");
}

main();
```

### utils.randomInt Random Integer

* Generate a random integer with the given number of digits
* @param length Number of digits
* @return integer

```javascript showLineNumbers
function main() {
 var r = utils.randomInt(2);
}

main();
```

### utils.randomCharNumber Random Alphanumeric String

* Get a random string of digits and letters
* @param length Length
* @return mixed alphanumeric string

```javascript showLineNumbers
function main() {
 var r = utils.randomCharNumber(2);
}

main();
```

### utils.getApkPkgName Get App Package Name from APK

* Get the package name from an APK file
* @param filePath File path
* @return string

```javascript showLineNumbers
function main() {
 var pkgName = utils.getApkPkgName("/sdcard/app.apk");
}

main();
```

### utils.isAppExist Check App Is Installed

* Check whether an app is installed
* @param packageName App package name
* @return true if installed, false if not

```javascript showLineNumbers
function main() {
 var result = utils.isAppExist("com.xx");
}

main();
```

### utils.getAppVersionCode Get App Version Code

* Get the installed app version code (integer)
* @param packageName App package name
* @return integer

```javascript showLineNumbers
function main() {
 var versionCode = utils.getAppVersionCode("com.xx");
}

main();
```

### utils.getAppVersionName Get App Version Name

* Get the installed app version name (string)
* @param packageName App package name
* @return string, e.g. 1.0.0

```javascript showLineNumbers
function main() {
 var r = utils.getAppVersionName("com.xx");
}

main();
```

### utils.setClipboardText Set Clipboard Text

* Set clipboard text
* @param text Text
* @return boolean

```javascript showLineNumbers
function main() {
 var r = utils.setClipboardText("com.xx");
 toast("Set result:" + r);
}

main();
```

### utils.getClipboardText Get Clipboard Text

* Read clipboard text
* @return string

```javascript showLineNumbers
function main() {
 var r = utils.getClipboardText();
 toast("Read result:" + r);
}

main();
```

### utils.playMp3 Play MP3 (Async)

* Play MP3 asynchronously
* Requires EC 5.12.0+
* @param path File path, e.g. /sdcard/a.mp3
* @param loop Whether to loop — true to loop
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 // To bundle MP3 in the script, put the MP3 file in the res directory
 saveResToFile("a.mp3", "/sdcard/a.mp3")
 let d = utils.playMp3("/sdcard/a.mp3", true)
 logd("dd " + d)
 // Add delay to wait for music to start playing
 sleep(30 * 1000)
 utils.stopMp3()
 logd("stop play ")
}

main();
```

### utils.playMp3WaitEnd Play MP3 (Sync)

* Play MP3 and wait until finished
* Requires EC 10.3.0+
* @param path File path, e.g. /sdcard/a.mp3
* @param loop Whether to loop — true to loop
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 // To bundle MP3 in the script, put the MP3 file in the res directory
 saveResToFile("a.mp3", "/sdcard/a.mp3")
 let d = utils.playMp3WaitEnd("/sdcard/a.mp3", false)
 logd("dd " + d)
}

main();
```

### utils.stopMp3 Stop MP3 Playback

* Stop MP3 playback
* Requires EC 5.12.0+
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 // To bundle MP3 in the script, put the MP3 file in the res directory
 saveResToFile("a.mp3", "/sdcard/a.mp3")
 let d = utils.playMp3("/sdcard/a.mp3", true)
 logd("dd " + d)
 // Add delay to wait for music to start playing
 sleep(30 * 1000)
 utils.stopMp3()
 logd("stop play ")
}

main();
```
