---
title: Global Module
description: EasyClick automation scripts — Android no root — global module
keywords:
 - EasyClick automation scripts Android no root global module
 - apk
 - dex
 - EC
 - checkApkVersion8
 - checkApkVersion9
 - getCenterTaskInfo
 - loadDex
 - JAVA
 - iec
 - setRepeatLoadDex
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview {#说明}

The global module exposes methods you can call directly, without a prefix object name.


## CLI {#cli相关}

### getCliArgs Get CLI command-line arguments {#getcliargs-获取cli命令行提交的参数}

* Get CLI command-line arguments
* When using with AI, prefer CLI args first; fall back elsewhere if unavailable
* Enables AI auto-parameter passing and post-pack testing from other sources
* Requires EC Android 12.0.0+
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

## Version Check {#版本判断}

### checkApkVersion8

* Check whether APK major version is 8; throws if not
* Call in your program to prevent IEC/APK version mismatch
* Requires EC 8.2.0+

```javascript showLineNumbers
function main() {
    checkApkVersion8();
}

main();
```

### checkApkVersion9

* Check whether APK major version is 9; throws if not
* Call in your program to prevent IEC/APK version mismatch
* Requires EC 9.1.0+

```javascript showLineNumbers
function main() {
    checkApkVersion9();
}

main();
```

## Control Center & Screen Mirroring {#中控投屏相关}
### getCenterTaskInfo Get control center task {#getcentertaskinfo-获取中控任务}

* Get task parameters sent from the control center
* Control center can configure params when starting scripts; read them here
* Requires EC Android 9.27.0+
* Note: Requires parameter config. Read order: per-device first, then global if empty.
* If params contain `__from_global__`, the value comes from global config
* @return `{json}` object

```javascript showLineNumbers
function main() {
    while (true) {
        logd("---> " + new Date())
        sleep(2000);
        let info = getCenterTaskInfo()
        logd("info -> " + JSON.stringify(info))

        if (info) {
            logd("test param => " + info['valueJson']['test']);
        }
        sleep(2000);
    }
}

main()
```



## Plugin Module Loading {#插件模块加载}

### loadDex Load dex or apk {#loaddex-载入dex或者apk}

* Load dex file
* @param path Path; loads from plugin dir (e.g. ab.apk) or file path (e.g. /sdcard/ab.apk)
* @return true if loaded successfully, false if load failed

```javascript showLineNumbers
function main() {
    // Loads from the IEC plugin directory first
    //loadDex("ocr.apk");
    // Or load from an absolute path on sdcard
    loadDex("/sdcard/a.apk");
    // Class com.A in the apk can be used directly
    var obj = new com.A();
}

main();
```

### setRepeatLoadDex Set repeat loading of dex or apk {#setrepeatloaddex-设置重复加载dex或者apk}

* Set repeat dex/apk loading to avoid long load times for large plugins
* Requires EC 7.1.0+
* @param r Whether to allow repeat loading; true = yes, false = no
* @return true if loaded successfully, false if load failed

```javascript showLineNumbers
function main() {
    setRepeatLoadDex(false)
    // Loads from the IEC plugin directory first
    //loadDex("ocr.apk");
    // Or load from an absolute path on sdcard
    loadDex("/sdcard/a.apk");
    // Class com.A in the apk can be used directly
    var obj = new com.A();
}

main();
```

### require Import JS {#require-导入js}

* Import JS module
* @param path Path, e.g. local /sdcard/a.js or EC project path slib/a.js
* @return module object

```javascript showLineNumbers
function main() {
    // Note: do not put JS files in js/ or subdirectories
    // Note: EC 3.5 is not supported
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

## Script Start, Stop & Pause {#脚本启停和暂停}

### exit Exit script {#exit-退出脚本}

```javascript showLineNumbers
exit();
```

### isScriptExit Whether script has exited {#isscriptexit-是否已退出脚本}

* Check whether the current EC thread has exited (main or child thread)
* Requires EC 6.2.0+
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
* @param content Path e.g. /sdcard/a.js or JS content
* @return Boolean; true = success, false = failure

```javascript showLineNumbers
function main() {
    var d = 'while(true){sleep(1000);logd(111111);}';

    thread.execAsync(function () {
        //execScript(1,"/sdcard/ad.js")
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

* Restart script; useful for infinite loops or errors. Download latest IEC and rerun without UI hot-update.
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
* Requires EC 10.0.0+
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
* Requires EC 10.0.0+
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
    toast(d);
}

main();
```

### JSON.parse Convert to JSON object {#jsonparse-转换为json对象}

* Parse JSON string to object
* @param string
* @return object

```javascript showLineNumbers
function main() {
    var m = {"sss": "a"};
    var d = JSON.stringify(m);
    d = JSON.parse(d);
    toast(d);
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
        toast("Success");
    } else {
        toast("Failed");
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
        toast("Success");
    } else {
        toast("Failed");
    }
    // Exception thrown here
    result.length();
}

main();
```

### observeEvent Listen to system events {#observeevent-对系统事件进行监听}

* Listen to system events
* Requires EC agent mode ( 6.0.0+)
* @param event Event types:
* activity-change: page change; supports accessibility and agent mode
* notification-show: notification shown; supports accessibility and agent mode
* toast-show: Toast message shown; supports accessibility and agent mode
* key-down: key down; accessibility mode only
* key-up: key up; accessibility mode only
* acc-service-interrupt: accessibility service interrupted; accessibility mode only
* acc-service-destroy: accessibility service destroyed; accessibility mode only
* acc-event: accessibility node event; supports accessibility and agent mode
* acc-service-connected: accessibility service connected; accessibility mode only
* auto-service-status: automation service status; accessibility mode only
* @param callback Event callback
* @return `{bool}` | true on success, false on failure

```javascript showLineNumbers
function main() {
    startEnv();
    logd("Start listening");
    observeEvent("activity-change", function (key, data) {
        logd("Activity change: " + typeof data)
        logd("Activity change: " + data)
    });
    // Listen to accessibility node events
    observeEvent("acc-event", function (key, data) {
        logd("acc-event: " + typeof data)
        logd("acc-event: " + data)
    });
    while (true) {
        sleep(1000)
    }
    // Cancel event listener
    cancelObserveEvent("acc-event")
}

main();
```

### cancelObserveEvent Cancel system event listener {#cancelobserveevent-取消对系统事件监听}

* Cancel event listener
* @param event Event type
* @return `{bool}` | true on success, false on failure

```javascript showLineNumbers
function main() {
    startEnv();
    logd("Start listening");
    observeEvent("activity-change", function (key, data) {
        logd("Activity change: " + typeof data)
        logd("Activity change: " + data)
    });
    // Listen to accessibility node events
    observeEvent("acc-event", function (key, data) {
        logd("acc-event: " + typeof data)
        logd("acc-event: " + data)
    });
    sleep(10000)
    // Cancel event listener
    cancelObserveEvent("acc-event")
}

main();
```

## Alert Sending {#告警发送}

### sendDingDingMsg Send DingTalk message {#senddingdingmsg-发送钉钉消息}

* Send DingTalk message
* Requires EC 9.11.0+
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

## Logging Methods {#日志消息方法}





### setSaveLog Save log {#setsavelog-保存日志}

* Save log output to files
* @param save Whether to save
* @param path Custom folder
* @param size Max size per log file
* @return directory where log files are saved

```javascript showLineNumbers
function main() {
 var s = setSaveLog(true, "/sdcard/aaa/", 1024 * 1024);
 logd("save dir is:" + s);
}

main();
```

### setSaveLogEx Save log {#setsavelogex-保存日志}

* Save log output to files
* @param save Whether to save
* @param path Custom folder
* @param size Max size per log file
* @param fileName Custom log file name
* @return directory where log files are saved

```javascript showLineNumbers
function main() {
 var s = setSaveLogEx(true, "/sdcard/aaa/", 1024 * 1024, "testlog");
 logd("save dir is:" + s);
}

main();
```

### setFloatDisplayLineNumber Print log line numbers {#setfloatdisplaylinenumber-打印日志行号}

* Whether the floating window shows line numbers when logging; in release builds, hide line numbers without affecting debug or file logs
* @param ds true = show, false = hide

```javascript showLineNumbers
function main() {
 setFloatDisplayLineNumber(true);

}

main();
```

### toast Toast message with parameters {#toast-带参数toast消息}

* Show Toast message
* Requires EC 5.21.0+
* @param msg Message string
* @param extra Extended map params; effective only with floating window permission
    * x: X coordinate
    * y: Y coordinate
    * duration: duration in milliseconds
    * textColor: hex text color, e.g. #888888
    * width: toast width
    * height: toast height
    * draggable: whether draggable; true = yes

```javascript showLineNumbers
function main() {
 let toastExtra = {
 "x": 100,
 "y": 1200,
 "duration": 1000,
 "textColor": "#778899",
 "width": 200,
 "height": 200,
 "draggable": true
 }
 for (var i = 0; i < 3; i++) {
 sleep(500)
 toast(time() + "ddd", toastExtra);
 }
 logd(time() + " 222");
}

main();
```

### toast1 Toast1 message {#toast1-toast1消息}

* Show Toast message (extended)
* @param msg Message string

```javascript showLineNumbers
function main() {
 toast1("msg");
}

main();
```

### toast2 Toast2 message {#toast2-toast2消息}

* Show Toast message (extended)
* @param msg Message string

```javascript showLineNumbers
function main() {
 toast2("msg");
}

main();
```

### setLogLevel Set log level {#setloglevel-设置日志的等级}

* Set log level; enable or disable logging as needed
* Requires EC 5.21.0+
* @param level Log level: debug, info, warn, error, off (debug < info < warn < error < off)
* e.g. off = disable all; debug = logd/logi/logw/loge; info = logi/logw/loge; warn logw/loge only
* @param displayToast Whether to show toast
* @return `{bool}` Boolean true = success, false = failure

```javascript showLineNumbers
function main() {
 setLogLevel("info", false)
 for (var i = 0; i < 1; i++) {
 sleep(10);
 //logd(time()+" debug");
 logi(time() + " info");
 //logw(time()+" warn");
 //loge(time()+" error");
 toast("--- " + time());
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

### clearLog Clear log {#clearlog-清除日志}

* Clear log
* @param lines int; lines to clear; -1 = clear all

```javascript showLineNumbers
function main() {
 showLogWindow()
 sleep(1000)
 for (var i = 0; i < 4; i++) {
 logd(" " + i);
 }
 sleep(2000)
 // Clear first 3 lines
 clearLog(3)
 // Clear all
 clearLog(-1)
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
 importPackage(android.graphics)
 let ad = BitmapFactory.decodeByteArray(d, 0, d.length)
 logd(d)
 logd(d.length)
 logd(ad);
}

main();
```

### readResString Read string resource {#readresstring-读取字符串资源}

* Read resource from res/ and return string
* @param fileName File name; do not include the res prefix
* @return string; null means no content
:::warning
- Do not use **Run Selection**; it does not include attachment files. Run the entire project.
  :::

```javascript showLineNumbers
function main() {
 var testData = readResString("a.txt");
}

main();
```

### readResBitmap Read Bitmap resource {#readresbitmap-读取bitmap资源}

* Read resource from res/ and return Bitmap
* @param fileName File name; do not include the res prefix
* @return Bitmap; null means no content
:::warning
- Do not use **Run Selection**; it does not include attachment files. Run the entire project.
  :::

```javascript showLineNumbers
function main() {
 var b = readResBitmap("a.txt");
}

main();
```

### readResAutoImage Read Image resource {#readresautoimage-读取image资源}

* Read resource from res/ and return AutoImage
* @param fileName File name; do not include the res prefix
* @return AutoImage; null means no content
:::warning
- Do not use **Run Selection**; it does not include attachment files. Run the entire project.
  :::

```javascript showLineNumbers
function main() {
 var b = readResAutoImage("img/a.png");
}

main();
```

### saveResToFile Save resource to file {#saverestofile-保存资源为文件}

* Save res/ resource to the given path
* @param fileName File name; do not include the res prefix
* @param path Destination path, e.g. /sdcard/aa.txt
* @return `{boolean}` true if saved successfully

```javascript showLineNumbers
function main() {
 var b = saveResToFile("img/a.png", "/sdcard/a.png");
}

main();
```

### findIECFile Find IEC file {#findiecfile-查找iec的文件}

* Find IEC files
* Requires EC 8.0.0+
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

## UI Parameter Reading {#ui参数读取}

### deleteConfig Delete config value {#deleteconfig-删除配置值}

* @param key Key configured in the UI
* @return `{bool}` true = success, false = failure

```javascript showLineNumbers
function main() {
 var testData = deleteConfig("test_key");
}

main();
```

### readConfigInt Read int config {#readconfigint-读取整型配置}

* @description Read UI parameter; returns int
* @param key Key configured in the UI
* @return int; returns 0 if not found

```javascript showLineNumbers
function main() {
 var testData = readConfigInt("test_key");
}

main();
```

### readConfigString Read string config {#readconfigstring-读取字符串配置}

* Read UI parameter; returns string
* @param key Key configured in the UI
* @return string; returns empty string if not found

```javascript showLineNumbers
function main() {
 var testData = readConfigString("test_key");
}

main();
```

### readConfigDouble Read double config {#readconfigdouble-读取double配置}

* Read UI parameter; returns double
* @param key Key configured in the UI
* @return double

```javascript showLineNumbers
function main() {
 var testData = readConfigDouble("test_key");
}

main();
```

### readConfigBoolean Read boolean config {#readconfigboolean-读取布尔型配置}

* Read UI parameter; returns boolean
* @param key Key configured in the UI
* @return true or false

```javascript showLineNumbers
function main() {
 var testData = readConfigBoolean("test_key");
}

main();
 ```

### getConfigJSON Get all config {#getconfigjson-取所有配置}

* Get config as JSON
* @return JSON data

```javascript showLineNumbers
function main() {
 var testData = getConfigJSON();
}

main();
 ```

### updateConfig Update config {#updateconfig-更新配置}

* Update config
* @param key Key
* @param value Value
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
 updateConfig("a", "sss");
}

main();
 ```

## EC System Settings {#ec-系统设置}

### setECSystemConfig Set EC parameters {#setecsystemconfig-设置ec参数}

* Set EC system parameters
* @param params Map formate.g. ```{"running_mode":"accessibility"}```,

```json showLineNumbers
{
 "running_mode": "accessibility",
 "auto_start_service": "yes",
 "log_float_window": "no",
 "ctrl_float_window": "no"
}
```

* Parameter reference:<br/>
* running_mode: Run mode — accessibility or agent
* auto_start_service: Boot auto-start — yes or no
* log_float_window: Log floating window — yes or no
* ctrl_float_window: Start/stop control floating window — yes or no
* ctrl_float_window_h: Horizontal start/stop control window — yes or no
* agent_node_support: Agent node support — 1=default, 2=shell dump, 3=no node service
* @return Boolean; true = yes, false = no

```javascript showLineNumbers
function main() {
 var m = {
 "node_service": "required",
 "proxy_service": "not required",
 "running_mode": "accessibility",
 "log_float_window": "no",
 "ctrl_float_window": "no"
 };
 setECSystemConfig(m);
}

main();
```

### openECSystemSetting Open EC system settings {#openecsystemsetting-打开ec系统界面}

* Open EC system settings page
* @return true on success, false on failure

```javascript showLineNumbers
function main() {
 var result = openECSystemSetting();
}

main();
 ```

### setWallpaperService Set wallpaper service {#setwallpaperservice-设置壁纸服务函数}

* Set wallpaper service
* Requires EC 6.1.0+
* @return Boolean; true = started successfully, false = start failed

```javascript showLineNumbers
function main() {
 var result = setWallpaperService();
}

main();
 ```

### isWallpaperServiceSet Whether wallpaper was set successfully {#iswallpaperserviceset-是否设置壁纸成功}

* Whether wallpaper was set successfully
* Requires EC 6.1.0+
* @return Boolean; true = started successfully, false = start failed

```javascript showLineNumbers
function main() {
 var result = isWallpaperServiceSet();
}

main();
 ```

### openECloudSetting Open EC cloud control settings {#openecloudsetting-打开ec云控界面}

* Open EC cloud control settings
* @return true on success, false on failure

```javascript showLineNumbers
function main() {
 var result = openECloudSetting();
}

main();
 ```

## Set IEC File (Hot Update in Script) {#设置iec文件脚本中的热更新}

### setIECPath Set script path {#setiecpath-设置脚本路径}

* Set IEC file path to execute
* @return true on success, false on failure

```javascript showLineNumbers
function main() {
 var result = setIECPath("/sdcard/release.iec");
 logd("result : " + result);
 logd("Current path " + getIECPath());
 // Start scheduled task for next run
 var id2 = startJob("task2", "2", true);
 logd("job id " + id2);
}

main();
 ```

## Run Mode {#运行模式}

### activeSelf Activate self {#activeself-激活自己}

* Activate self. Requires USB debugging and ADB Wi-Fi debugging. See activation docs.
* Requires EC 5.15.0+
* @param activeType Activation type: 0=auto, 1=mode1, 2=mode2
* @param timeout Timeout
* @return `{string}` "Activation succeeded" on success; otherwise error message

```javascript showLineNumbers
function main() {
 var result = activeSelf(0, 10 * 1000);
 logd(result)
}

main();
```

### activeDevice Activate other device by IP {#activedevice-通过ip激活其他设备}

* Target device IP. Requires USB debugging and ADB Wi-Fi on target. See activation docs.
* Requires EC 5.15.0+
* @param ip Device IP
* @param activeType Activation type: 0=auto, 1=mode1, 2=mode2
* @param timeout Timeout
* @return `{string}` "Activation succeeded" on success; otherwise error message

```javascript showLineNumbers
function main() {
 var result = activeDevice("192.168.1.108", 0, 10 * 1000);
 logd(result)
}

main();
```

### isAccMode Accessibility mode check {#isaccmode-无障碍模式判断}

* Whether in accessibility mode
* @return true or false

```javascript showLineNumbers
function main() {
 var result = isAccMode();
}

main();
 ```

### isAgentMode Agent mode check {#isagentmode-代理模式判断}

* Whether in agent mode
* @return true or false

```javascript showLineNumbers
function main() {
 var result = isAgentMode();
}

main();
 ```

### isServiceOk Automation service status {#isserviceok-自动化服务状态}

* Whether automation service is OK
* @return true or false

```javascript showLineNumbers
function main() {
 var result = isServiceOk();
}

main();
 ```

### startEnv Start automation {#startenv-启动自动化}

* Start automation service environment
* @return true or false

```javascript showLineNumbers
function main() {
 var result = startEnv();
}

main();
 ```

### daemonEnv Daemon automation environment {#daemonenv-守护自动化环境}

* Daemon automation environment
* When activated or accessibility keep-alive, try to keep the service online
* Requires EC 6.7.0+
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
* @param skinAccPage If accessibility stop fails, jump to accessibility settings
* @return Boolean; true = started successfully, false = start failed

```javascript showLineNumbers
function main() {
 var result = closeEnv(false);
}

main();
 ```

## Time {#时间相关}

### time Current timestamp in milliseconds {#time-毫秒级当前时间戳}


* Requires EC 5.14.0+
* Current timestamp in milliseconds
* @return `{long}` time in milliseconds

```javascript showLineNumbers
function main() {
 logd(time());
}

main();
```

### timeFormat Format time {#timeformat-格式化时间}

* Requires EC 5.14.0+
* Format current time, e.g.:```yyyy-MM-dd HH:mm:ss```
* @return `{string}` formatted current time

```javascript showLineNumbers
function main() {
 logd(timeFormat("yyyy-MM-dd HH:mm:ss"));
}

main();
```

### console.time Start timer {#consoletime-计时开始}

* Requires EC 5.14.0+
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

* Requires EC 5.14.0+
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
