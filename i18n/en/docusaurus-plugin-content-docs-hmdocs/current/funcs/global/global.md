---
title: Global Module
description: EasyClick automation scripts HarmonyOS Next automation Global Module resource download
keywords:
 - EasyClick automation scripts HarmonyOS Next automation Global Module resource download
 - jar
 - JAVA
 - param
 - js
 - JS
 - return
 - class
 - version
 - loadDex
 - require
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview {#说明}

The global module can be used by calling methods directly, without a prefix object name.
## CLI {#cli相关}

### getCliArgs Get CLI command-line arguments {#getcliargs-获取cli命令行提交的参数}

* Get CLI command-line arguments
* When using with AI, prefer CLI args first; fall back elsewhere if unavailable
* Enables AI auto-parameter passing and post-pack testing from other sources
* Requires EC HarmonyOS Next 2.16.0+
* @return {null|JSON} JSON object; null means not launched from CLI

```javascript showLineNumbers
function main() {
    let a = getCliArgs();
    if (a == null){
        // Get from elsewhere
    }
    logd(JSON.stringify(a))
}

main();
```
## Control Center Version {#中控版本}

### version Get control center version {#version-获取中控版本}

* Get control center version
* @return string, e.g. 2.9.0

```javascript showLineNumbers
function main() {
    logd(version())
}

main();
```



## Plugin Module Loading {#插件模块加载}

### loadDex Load jar package {#loaddex-载入jar包}

* Load dex file
* @param path Path; loads from plugin dir (e.g. ab.jar) or file path (e.g. D:/ab.jar)
* @return true if loaded successfully, false if load failed

```javascript showLineNumbers
function main() {
    // Enter filename only; loads from project plugin directory
    loadDex("ocr.apk");
    // Enter absolute path; loads from PC path
    loadDex("D:/a.jar");
    // Class com.A in the apk can be used directly
    var obj = new com.A();
}

main();
```

### require Import JS {#require-导入js}

* Import JS module
* @param path Path, e.g. local D:/a.js or EC project path slib/a.js
* @return module object

```javascript showLineNumbers
function main() {
    // Note: do not put JS files in js/ or subdirectories
    test = require("slib/a.js")
    logd(test.c());
}

main();
//Video:https://www.bilibili.com/video/BV1ES4y1f7qV?vd_source=2abc6be820f5a6382ebc0ceafc5dbe00&p=39&spm_id_from=333.788.videopod.episodes
```

### importClass Import Java class {#importclass-导入java类}

* Import a Java class for use in JS
* @param clz Class name, e.g. com.A

```javascript showLineNumbers
function main() {
    importClass(com.A);
    var obj = new com.A();
}

main();
```

### importPackage Import Java package {#importpackage-导入java包}

* Import all classes under a Java package for JS
* @param clz Package name, e.g. com.b

```javascript showLineNumbers
function main() {
    importPackage(com.b);
    var obj = new com.b.A();
}

main();
```

## Script Start & Stop {#脚本启停}

### exit Exit script {#exit-退出脚本}

```javascript showLineNumbers
exit();
```

### isScriptExit Whether script has exited {#isscriptexit-是否已退出脚本}

* Check whether the current EC thread has exited (main or child thread)
* @return true if exited

```javascript showLineNumbers
function main() {
    try {
        while (true) {
            sleep(1000)
            logd("222")
            if (isScriptExit()) {
                break
            }
        }
        logd("222")
    } catch (e) {
        logd(e)
        if (isScriptExit()) {
            return
        }
    }
}

main();
```

### sleep Pause execution {#sleep-暂停执行}

* Sleep
* @param miSecond Milliseconds

```javascript showLineNumbers
function main() {
    sleep(1000);
}

main();
```

### execScript Load JS {#execscript-载入js}

* Execute JS file or content, [If illegalStateException occurs, try changing how **eval** executes JS]
* eval is built into JS; pass JS content directly
* @param type 1 = file, 2 = JS content
* @param content Path e.g. D:/a.js or JS content
* @return Boolean; true = success, false = failure

```javascript showLineNumbers
function main() {
    var d = 'while(true){sleep(1000);logd(111111);}';
    thread.execAsync(function () {
        //execScript(1,"D:/ad.js")
        execScript(2, d);
    });

    while (true) {
        sleep(2000);
        loge("fsadffsad")
    }
}

main();
```

### restartScript Restart script {#restartscript-重启脚本}

* Restart script; useful for infinite loops or on exception.
* Warning: powerful; control auto-restart carefully or force-kill to stop
* @param path New IEC path, or null if not needed
* @param stopCurrent Whether to stop the current script
* @param delay Delay in seconds before execution
* @return bool; true = success, false = failure

```javascript showLineNumbers
function main() {
    logd("Running in script");
    setStopCallback(function () {
        restartScript(null, true, 3)
    });

    //setExceptionCallback(function (){
    // restartScript(null,true,3)
    //});
    sleep(1000);
    logd("Script ended")
}

main();
```

### setScriptPause Set script pause or resume {#setscriptpause-设置脚本暂停或者继续}

* Set script pause or resume
* Requires EC HarmonyOS Next 1.0.0+
* @param pause true = pause, false = resume
* @param timeout Auto-resume timeout (ms); 0 = wait for external resume
* @return `{boolean}` true if paused, false if running

```javascript showLineNumbers
function main() {
    sleep(1000);
    logd("start....")
    // Pause script execution; auto-resumes after 3 seconds
    // Demo only; in practice, pause based on your business logic or from the UI
    setScriptPause(true, 3000)

    logd("Log after 3 seconds")
}

main();
```

### isScriptPause Whether script is paused {#isscriptpause-脚本是否处于暂停中}

* Whether the script is paused
* Requires EC HarmonyOS Next 1.0.0+
* @return `{boolean}` true if script is paused

```javascript showLineNumbers
function main() {
    sleep(1000);
    logd("start....")
    // Pause script execution; auto-resumes after 3 seconds
    // Demo only; in practice, pause based on your business logic or from the UI
    // Function call demo only; handle per your business logic
    logd("isScriptPause " + isScriptPause())
    setScriptPause(true, 3000)
    logd("isScriptPause " + isScriptPause())
    logd("Log after 3 seconds")
}

main();
```

## JSON Processing {#json处理}

### JSON.stringify Format to JSON string {#jsonstringify-格式化为json字符串}

* Format object to JSON string
* @param object
* @return string

```javascript showLineNumbers
function main() {
    var m = {"sss": "a"};
    var d = JSON.stringify(m);
    logd(d);
}

main();
```

### JSON.parse Convert to JSON object {#jsonparse-转换为json对象}

* Parse JSON string to object
* @param string
* @return object

```javascript showLineNumbers
function main() {
    let m = `{"sss": "a"}`
    let d = JSON.parse(m);
    logd(d);
}

main();
```

## Script & Service Listeners {#监听脚本和服务}

### setStopCallback Script stop listener {#setstopcallback-脚本停止监听}
- Call once at the start of the script

```javascript showLineNumbers
function main() {
    setStopCallback(function () {
        logd("Stop callback")
    });
    var result = sleep(1000);
    if (result) {
        logd("Success");
    } else {
        logd("Failed");
    }
}

main();
```

### setExceptionCallback Script exception stop listener {#setexceptioncallback-脚本异常停止监听}
- Call once at the start of the script

```javascript showLineNumbers
function main() {
    setExceptionCallback(function (msg) {
        logd("Exception stop message: " + msg)
    });
    var result = sleep(1000);
    if (result) {
        logd("Success");
    } else {
        logd("Failed");
    }
    // Exception thrown here
    result.length();
}

main();
```

## Logging Methods {#日志消息方法}

### setLogLevel Set log level {#setloglevel-设置日志的等级}

* Set log level; enable or disable logging as needed
* Files saved under control center install dir logs/device
* @param level Log level: debug, info, warn, error, off (debug < info < warn < error < off)
* e.g. off = disable all; debug = logd/logi/logw/loge; info = logi/logw/loge; warn
 logw/loge only
* @param displaylogd Whether to show logd messages
* @return `{bool}` Boolean true = success, false = failure

```javascript showLineNumbers
function main() {
    setLogLevel("info", false)
    for (var i = 0; i < 1; i++) {
        sleep(10);
        //logd(time()+" debug");
        logi(time() + " info");
        //logw(time()+" warn");
        // loge(time()+" error");
        logd("--- " + time());
    }
    //logd(time()+" 222");
}

main();
```
### setDisplayLineNumber Display line numbers {#setdisplaylinenumber-显示行号}

* Set whether logs show line numbers
* Requires EC HarmonyOS Next USB 2.0.0+
* @param display true = show line numbers

```javascript showLineNumbers
function main() {
  setDisplayLineNumber(true)
    for (var i = 0; i < 1; i++) {
        sleep(10);
        //logd(time()+" debug");
        logi(time() + " info");
        //logw(time()+" warn");
        // loge(time()+" error");
        logd("--- " + time());
    }
    //logd(time()+" 222");
}

main();
```

### setDeviceRecordLog Record log {#setdevicerecordlog-记录日志}

* Record and save logs for the current device
* Off by default
* Requires EC HarmonyOS Next 1.0.0+
* @param open true = record to file, false = no action
* @param level Log level: debug, info, warn, error, off (debug < info < warn < error < off)
* e.g. off = disable all; debug = logd/logi/logw/loge; info = logi/logw/loge; warn
 logw/loge only
* @return `{bool}` Boolean true = success, false = failure

```javascript showLineNumbers
function main() {
    setDeviceRecordLog(true, "", debug)
    for (var i = 0; i < 1; i++) {
        sleep(10);
        //logd(time()+" debug");
        logi(time() + " info");
        //logw(time()+" warn");
        // loge(time()+" error");
        logd("--- " + time());
    }
    //logd(time()+" 222");
}

main();
```

### logd Debug log {#logd-调试日志}

* Debug log
* @param msg Message string

```javascript showLineNumbers
function main() {
    logd("msg");
    // Variadic arguments
    logd("Message {},{}", "test1", 2)
}

main();
```

### loge Error log {#loge-错误日志}

* Error log
* @param msg Message string

```javascript showLineNumbers
function main() {
    loge("msg");
    // Variadic arguments
    loge("Message {},{}", "test1", 2)
}

main();
```

### logw Warning log {#logw-警告日志}

* Warning log
* @param msg Message string

```javascript showLineNumbers
function main() {
    logw("msg");
    // Variadic arguments
    logw("Message {},{}", "test1", 2)
}

main();
```

### logi Info log {#logi-信息日志}

* Info log
* @param msg Message string

```javascript showLineNumbers
function main() {
    logi("msg");
    // Variadic arguments
    logi("Message {},{}", "test1", 2)
}

main();
```

## Read IEC Package Resources {#读取iec包资源}

### readIECFileAsString Read IEC internal file as string {#readiecfileasstring-读取iec内部文件为字符串}

* Read resource from IEC file and return string
* @param fileName File name; include folder path if in a subfolder
* @return `{string}`; null means no content

```javascript showLineNumbers
function main() {
    var testData = readIECFileAsString("res/a.txt");
    logd(testData)
}

main();
```

### readIECFileAsByte Read IEC internal file as byte array {#readiecfileasbyte-读取iec内部文件为数组资源}

* Read resource from IEC file and return Java byte array
* @param fileName File name; include folder path if in a subfolder
* @return `{byte array}`; null means no content

```javascript showLineNumbers
function main() {
    // Example: read an image
    var d = readIECFileAsByte("res/a.png")
    logd(d)
    logd(d.length)
}

main();
```

### readResString Read string resource {#readresstring-读取字符串资源}

* Read resource from res/ and return string
* @param fileName File name; do not include the res prefix
* @return string; null means no content

```javascript showLineNumbers
function main() {
    var testData = readResString("a.txt");
}

main();
```

### readResBitmap Read Bitmap resource {#readresbitmap-读取bitmap资源}

* Read resource from res/ and return Bitmap
* @param fileName File name; do not include the res prefix
* @return BufferedImage; null means no content

```javascript showLineNumbers
function main() {
    // If under res/ directory
    var b = readResBitmap("a.png");
    // If under res/img/ directory
    var b = readResBitmap("img/a.png");
}

main();
```

### readResAutoImage Read Image resource {#readresautoimage-读取image资源}

* Read resource from res/ and return AutoImage
* @param fileName File name; do not include the res prefix
* @return string; null means no content

```javascript showLineNumbers
function main() {
    // If under res/ directory
    var b = readResAutoImage("a.png");
    // If under res/img/ directory
    var b = readResAutoImage("img/a.png");
}

main();
```

### saveResToFile Save resource to file {#saverestofile-保存资源为文件}

* Save res/ resource to the given path
* @param fileName File name; do not include the res prefix
* @param path Destination path, e.g. D:/aa.txt
* @return boolean true if saved successfully

```javascript showLineNumbers
function main() {
    // If under res/ directory
    var b = saveResToFile("a.png", "D:/a.png");
    // If under res/img/ directory
    var b = saveResToFile("img/a.png", "D:/a.png");
}

main();
```

### findIECFile Find IEC file {#findiecfile-查找iec的文件}

* Find IEC files
* @param dir Folder name; null = res/ only; default res/; e.g. res/aaa/
* @param names File name prefix; null = no filter; separate with |, e.g. aaa|bb|cc
* @param ext File extension; null = no filter; separate with |, e.g..png|.jpg|.bmp
* @param recursion Whether to recurse subdirs; true = yes
* @return `{array}` JSON array of file names

```javascript showLineNumbers
function main() {
    let res = findIECFile("res/", "dd2", ".png|.jpg", true)
    logd("findIECFile {}", JSON.stringify(res));
}

main();
```

## Automation Service {#自动化服务相关}

### isServiceOk Automation service status {#isserviceok-自动化服务状态}

* Whether automation service is OK
* @return true or false

```javascript showLineNumbers
function main() {
    var result = isServiceOk();
}

main();
```

### isDeviceOnline Whether device is online {#isdeviceonline-设备是否在线}

* Whether device is online
* @return true or false

```javascript showLineNumbers
function main() {
    var result = isDeviceOnline();
}

main();
 ```

### startEnv Start automation {#startenv-启动自动化}

* Start automation environment and auto-correct coordinate system to prevent drift
* @return true or false

```javascript showLineNumbers
function main() {
    var result = startEnv();
}

main();
 ```

### getStartEnvMsg Get automation message {#getstartenvmsg-获取自动化消息}

* Get automation startup message
* @return string

```javascript showLineNumbers
function main() {
    var result = getStartEnvMsg();
    logd(result)
}

main();
```

### daemonEnv Daemon automation environment {#daemonenv-守护自动化环境}

* Daemon automation environment
* When activated or accessibility keep-alive, try to keep the service online
* @param daemon Whether to daemonize; true = yes, false = no
* @return Boolean; true = started successfully, false = start failed

```javascript showLineNumbers
function main() {
    var result = daemonEnv(true);
}

main();
```

### closeEnv Close automation {#closeenv-关闭自动化}

* Close automation environment
* @return Boolean; true = started successfully, false = start failed

```javascript showLineNumbers
function main() {
    var result = closeEnv();
}

main();
 ```

## Time {#时间相关}

### time Current timestamp in milliseconds {#time-毫秒级当前时间戳}

* Current 13-digit timestamp in milliseconds
* @return `{long}` time in milliseconds

```javascript showLineNumbers
function main() {
    logd(time());
}

main();
```

### timeFormat Format time {#timeformat-格式化时间}

* Format current time, e.g.:```yyyy-MM-dd HH:mm:ss```
* @return `{string}` formatted current time

```javascript showLineNumbers
function main() {
 logd(timeFormat("yyyy-MM-dd HH:mm:ss") + "");
}

main();
```

### console.time Start timer {#consoletime-计时开始}

* Start timer; pair with timeEnd to measure duration
* @param label Label
* @return `{long}` current time

```javascript showLineNumbers
function main() {
 console.time("1");
 sleep(1000)
 logd(console.timeEnd("1"))
}

main();
```

### console.timeEnd End timer {#consoletimeend-计时结束}

* End timer; pair with time start to measure duration
* @param label Label
* @return `{long}` elapsed since timer start

```javascript showLineNumbers
function main() {
 console.time("1");
 sleep(1000)
 logd(console.timeEnd("1"))
}

main();
```

## other {#其他}

### random Random function {#random-随机函数}

* Random value in range
* @param min Minimum
* @param max Maximum
* @return int between min and max inclusive

```javascript showLineNumbers
function main() {
 var result = random(100, 1000);
 sleep(result);
}

main();
```

## Alert Sending {#告警发送}

### sendDingDingMsg Send DingTalk message {#senddingdingmsg-发送钉钉消息}

* Send DingTalk message
* Requires EC HarmonyOS Next 1.0.0+
* @param url Group/dept bot Webhook URL
* @param secret Bot Webhook secret; optional if using keyword filter
* @param msg Message to send
* @param atMobile Mobile numbers to @; comma-separated
* @param atAll Whether to @all; true or false
* @return `{string}` DingTalk JSON result, e.g. ```{"errcode":0,"errmsg":"ok"}```; errcode=0 = success

```javascript showLineNumbers
function main() {
 // Demo URL and secret. See this page for details: https://www.dingtalk.com/qidian/help-detail-20781541.html
 // https://blog.csdn.net/weixin_44646065/article/details/110637713
 let url = "https://oapi.dingtalk.com/robot/send?access_token=59735fa75d835dbfaa502bb42886fca982960d20sac5e1df6bba4dd1aba02999c"
 let sec = "SEC2305788ab08e9534a33b86ae376697d3c9ee3095f331345d5ccd6e2e065ca8069"
 var res = sendDingDingMsg(url, sec, "My message", "", true);
 logd("sendDingDingMsg:" + res);
}

main();
```

## authorization {#授权}

### isDeviceAuthOk Whether authorization is valid {#isdeviceauthok-授权是否正常}

* Check whether device authorization is expired
* @param type 1 = control center device auth, 2 = screen mirroring auth
* @return `{boolean}` true = expired, false = not expired

```javascript showLineNumbers
function main() {
 var result = isDeviceAuthOk(1);
 logd(result);
}

main();
```

### getDeviceAuth Get device authorization time {#getdeviceauth-获取设备授权时间}

* Get device authorization expiry
* Requires EC HarmonyOS Next USB 2.7.0+
* @param type 1 = control center device auth, 2 = screen mirroring auth
* @return `{string}` JSON string; useSerialNo: 1 = serial-number auth, else device ID auth; isExp: 1 = expired; expTime = expiry time


```javascript showLineNumbers
function main() {
 var result = getDeviceAuth(1);
 logd(result);
}

main();
```
