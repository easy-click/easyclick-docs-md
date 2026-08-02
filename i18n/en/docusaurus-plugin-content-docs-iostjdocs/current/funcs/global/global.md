---
title: EasyClick automation scripts — iOS scripts — iOS no jailbreak — iOS no hardware — Global Module
hide_title: false
hide_table_of_contents: false
sidebar_label: Global Module
description: EasyClick automation scripts — iOS no jailbreak — global module
keywords:
 - EasyClickautomation scripts
 - iOS scripts
 - iOS no jailbreak
 - iOS no hardware
 - Global Module
 - resource download
 - JS
 - iOS
 - js
 - return
 - param
 - version
 - exit
 - isScriptExit
 - sleep
 - execScript
 - EasyClick
 - mobile automation
---

# Global Module {#全局模块}

## Overview {#说明}

The global module can be used by calling methods directly, without a prefix object name.

## CLI {#cli相关}

### getCliArgs Get CLI command-line arguments {#getcliargs-获取cli命令行提交的参数}

* Get CLI command-line arguments
* When using with AI, prefer CLI args first; fall back elsewhere if unavailable
* Enables AI auto-parameter passing and post-pack testing from other sources
* Requires EC standalone 6.9.0+
* @return `{null|JSON}` JSON object; null means not launched from CLI

```javascript showLineNumbers
function main() {
    let a = getCliArgs();
    if (a == null) {
        // Get from elsewhere
    }
    logd(JSON.stringify(a))
}

main();
```

## Automation Engine {#自动化引擎相关}

### Get automation engine mode {#获取自动化引擎模式}

* Get automation engine mode
* @return `{string}` single = single-app mode | dual = dual-app mode

```javascript showLineNumbers
function main() {
    logd(getAutomationEngineMode())
}

main();
```

### Set automation engine mode {#设置自动化引擎模式}

* Set automation engine mode
* @param mode `{string}` single = single-app mode | dual = dual-app mode
* @return `{boolean}` true

```javascript showLineNumbers
function main() {
    logd(setAutomationEngineMode("single"))
}

main();
```

## App Version {#应用版本}

### version Get application version {#version-获取应用程序版本}

* Get application version
* @return string, e.g. 2.9.0

```javascript showLineNumbers
function main() {
    logd(version())
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

* Execute JS file or content
* @param a_execType 1 = file, 2 = JS content
* @param _acontent Path[see file module]e.g. /var/a.jsor JS content
* @return Boolean; true = success, false = failure

```javascript showLineNumbers
function main() {
    let d = "logd(1)"
    let dx = execScript(2, d);
    while (true) {
        sleep(2000);
        loge("fsadffsad")
    }
}

main();
```

### restartScript Restart script {#restartscript-重启脚本}

* Supports EC iOS standalone 2.2.0+
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
        restartScript(null, false, 3)
    });

    //setExceptionCallback(function (){
    // restartScript(null,true,3)
    //});
    sleep(1000);
    logd("Script ended")
}

main();
 ```

## JS Import {#js导入}

### require Import JS {#require-导入js}

* Import JS module
* @param path Path, e.g. local JS file or EC project path slib/a.js
* @return module object

```javascript showLineNumbers
function main() {
    // Note: do not put JS files in js/ or subdirectories
    // Note: EC iOS standalone 1.3.+
    let lib1 = require("res/lib.js")
    new lib1(1, 2, 3).say()
    let lib2 = require("res/lib2")
    logd(lib2.add(1, 2))
}

main();
//Video:https://www.bilibili.com/video/BV1ES4y1f7qV?vd_source=2abc6be820f5a6382ebc0ceafc5dbe00&p=39&spm_id_from=333.788.videopod.episodes
```

```javascript showLineNumbers
// res/lib2.js content
function add(a, b) {
    return a + b;
}

var a1 = 1
module.exports = {add, a1};
```

```javascript showLineNumbers
// res/lib2.js content
module.exports = function (name, age, money) {
    this.name = name;
    this.age = age;
    this.money = money;
    this.say = function () {
        console.log('My name: ' + this.name + ', age ' + this.age + ', salary: ' + this.money);
    }
};

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
    var m = {"sss": "a"};
    var d = JSON.stringify(m);
    d = JSON.parse(d);
    logd(d);
}

main();
```

## Script & Service Listeners {#监听脚本和服务}

### setStopCallback Script stop listener {#setstopcallback-脚本停止监听}

* Supports EC iOS standalone 2.2.0+

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

* Supports EC iOS standalone 2.2.0+

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
* @param level Log level: debug, info, warn, error, off (debug < info < warn < error < off)
* e.g. off = disable all; debug = logd/logi/logw/loge; info = logi/logw/loge; warn
 logw/loge only
* @param displaylogd Whether to show logd messages; not implemented
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

### setDisplayLineNumber Display line numbers {#setdisplaylinenumber-显示行号}

* Set whether logs show line numbers
* Requires EC standalone 5.0.0+
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

### setSaveLogEx Save log {#setsavelogex-保存日志}

* Save log output to files; export via iTools/i4
* EC iOS 3.13+ adds level parameter
* @param save Whether to save
* @param level Log level,values: debug,info,warn,error,off,ordered as ```debug<info<warn<error```,
* e.g. off = disable all; debug = logd/logi/logw/loge; info = logi/logw/loge; warn
  logw/loge only
* @return directory where log files are saved

```javascript showLineNumbers
function main() {
 let d = setSaveLogEx(true, "debug")
 logd(d)
}

main();
```

## Log Window {#日志窗口}

### setLogWindowForcePlaybackPaused Set log floating window pause state {#setlogwindowforceplaybackpaused-设置日志浮窗暂停状态}

* Set whether log PiP is forced to paused state (low CPU mode).
* Requires EC standalone 7.3.0+
* System default is true
* @param forcePaused `{boolean}`
* true: always report paused; refresh at interval only; low CPU during script (recommended, default).
* false: sync to playing during script (legacy; higher CPU).
* @return `{boolean}` true on success

```javascript showLineNumbers
function main() {
 // Call directly
 setLogWindowForcePlaybackPaused(true)
}

main()
```

### isLogWindowForcePlaybackPaused Query log floating window pause state {#islogwindowforceplaybackpaused-查询日志浮窗暂停状态}

* Query whether log floating window is forced paused (low CPU mode).
* Requires EC standalone 7.3.0+
* @return `{boolean}` true paused state

```javascript showLineNumbers
function main() {
 // Call directly
 logd(isLogWindowForcePlaybackPaused())
}

main()
```

### setLogWindowInfoVisible Show or hide log floating window info bar {#setlogwindowinfovisible-显示或隐藏日志浮窗自定义信息栏}

* Show or hide log floating window info bar.
* Requires EC standalone 7.3.0+
* @param visible `{boolean}` true = show, false = hide
* @return `{boolean}` true on success

```javascript showLineNumbers
function main() {
 // Call directly
 // Show first
 setLogWindowInfoVisible(true)
 // Set info to display again
 setLogWindowInfoText("Account: test001\nCoins: 999\nStatus: Running\n1\n2", "", 12)
}

main()
```

### setLogWindowInfoText Set log floating window custom info {#setlogwindowinfotext-设置日志浮窗自定义信息}

* Set log floating window custom info (below status bar, independent of log output).
* Requires EC standalone 7.3.0+
* @param text `{string}` important info to display,supports \n newlines (max 5 lines)
* @param color `{string}` Optional hex text color, e.g. #FF6600;empty uses default log window text color
* @param fontSize `{number}` Optional font size; 0 or omit uses default (1pt smaller than log text)
* @return `{boolean}` true on success

```javascript showLineNumbers
function main() {
 // Call directly
 // Show first
 setLogWindowInfoVisible(true)
 // Set info to display again
 setLogWindowInfoText("Account: test001\nCoins: 999\nStatus: Running\n1\n2", "", 12)
}

main()
```

### setLogRefreshInterval Set log floating window refresh interval {#setlogrefreshinterval-设置日志浮窗刷新间隔}

* Set log floating window refresh interval (seconds).Logs sync to PiP window at this interval.
* Requires EC standalone 7.3.0+
* @param intervalSec refresh interval in seconds,valid range 0.2–60, default 1
* @return `{boolean}` true on success, false = invalid parameters

```javascript showLineNumbers
function main() {
 // Call directly
 setLogRefreshInterval(2)
}

main()
```

### setLogViewSizeEx Set log window properties {#setlogviewsizeex-set-log-window属性}

* Set log window size (extended)
* @param map e.g.
* Parameters:
* x: start X position (X currently unused)
* y: start Y position (Y currently unused)
* w: width
* h: height
* textSize:log font size
* textColor: text color #336699
* line: number of lines to show; default 10
* backgroundColor: background color, e.g. #336699
* direction: text direction; 1 = portrait, 0 = landscape
* showTag: show debug tag; 1 = yes, 2 = no
* fitWidth: fit text to window width; 1 = yes, 0 = no

```javascript showLineNumbers
 function setlog() {
 var m = {
 "x": 2,
 "y": 2,
 "w": 300,
 "h": 400,
 "textSize": 26,
 "backgroundColor": "#336699",
 "textColor": "#000000",
 "direction": 0,
 "line": 10,
 "showTag": 0,
 "fitWidth": 0
 }
 // Bring main app to foreground
 takeMeToFront()
 sleep(1000)
 showLogWindow();

 logd("showLogWindow() " + showLogWindow())
 for (let i = 0; i < 11; i++) {
 sleep(1000)
 logd("demo " + new Date())
 if (i == 2) {
 logd("closeLogWindow() " + closeLogWindow())
 setLogViewSizeEx(m);
 }
 if (i == 10) {
 logd("showLogWindow() " + showLogWindow())
 }
 }
}

setlog();

```

### showLogWindow Show log window {#showlogwindow-显示日志窗口}

* [Cannot use while main app is in background]
* Show log window; requires PiP support and PiP enabled on iOS
* @returns `{boolean}` true = success, false = failure

```javascript showLineNumbers
 function setlog() {
 var m = {
 "x": 2,
 "y": 2,
 "w": 300,
 "h": 400,
 "textSize": 26,
 "backgroundColor": "#336699",
 "textColor": "#000000"
 }
 // Bring main app to foreground
 takeMeToFront()
 sleep(1000)
 showLogWindow();

 logd("showLogWindow() " + showLogWindow())
 for (let i = 0; i < 11; i++) {
 sleep(1000)
 logd("demo " + new Date())
 if (i == 2) {
 logd("closeLogWindow() " + closeLogWindow())
 setLogViewSizeEx(m);
 }
 if (i == 10) {
 logd("showLogWindow() " + showLogWindow())
 }
 }
}

setlog();

```

### closeLogWindow Close log window {#closelogwindow-关闭日志窗口}

* [Cannot use while main app is in background]
* Close log window
* @returns `{boolean}` true = success, false = failure

```javascript showLineNumbers
 function setlog() {
 var m = {
 "x": 2,
 "y": 2,
 "w": 300,
 "h": 400,
 "textSize": 26,
 "backgroundColor": "#336699",
 "textColor": "#000000"
 }
 takeMeToFront()

 showLogWindow();

 logd("showLogWindow() " + showLogWindow())
 for (let i = 0; i < 11; i++) {
 sleep(1000)
 logd("demo " + new Date())
 if (i == 2) {
 logd("closeLogWindow() " + closeLogWindow())
 setLogViewSizeEx(m);
 }
 if (i == 10) {
 logd("showLogWindow() " + showLogWindow())
 }
 }
}

setlog();

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

### readResAutoImage Read Image resource {#readresautoimage-读取image资源}

* Read resource from res/ and return AutoImage
* @param fileName File name; do not include the res prefix
* @return string; null means no content

```javascript showLineNumbers
function main() {
 var b = readResAutoImage("img/a.png");
}

main();
```

### saveResToFile Save resource to file {#saverestofile-保存资源为文件}

* Save res/ resource to the given path
* @param fileName File name; do not include the res prefix
* @param path destination path,e.g./var/aa.txt
* @return boolean true if saved successfully

```javascript showLineNumbers
function main() {
 var b = saveResToFile("img/a.png", "/var/a.png");
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

### startEnv Start automation {#startenv-启动自动化}

* Start automation service environment; not implemented — follow log output
* @return true or false

```javascript showLineNumbers
function main() {
 var result = startEnv();
}

main();
 ```

### startActiveMySelf Start self-activation {#startactivemyself-启动自激活}

* Start self-activation; after activation, tap agent IP icon to launch
* This function may take up to 30 seconds
* See docs:https://ieasyclick.com/iostjdocs/advance/activemyself
* Requires EC standalone 6.0.0+
* For external VPN, bring main app to foreground to allow LocalDevVpn launch
* @param openExtVpn string; 1 = external LocalDevVpn, 0 = built-in VPN
* @return `{string}` "ok" = success; otherwise error message

```javascript showLineNumbers
function main() {
 var result = startActiveMySelf("0");
 logd("result " + result)
 logd("mountDevImageOk " + mountDevImageOk())
}

main();
```

### mountDevImageOk Mount developer image result {#mountdevimageok-刷入镜像结果}

* Whether developer image mount succeeded
* Requires EC standalone 6.0.0+
* @return `{bool}` true = success, false = failure

```javascript showLineNumbers
function main() {
 var result = startActiveMySelf("0");
 logd("result " + result)
 logd("mountDevImageOk " + mountDevImageOk())
}

main();
 ```

## Alert Sending {#告警发送}

### sendDingDingMsg Send DingTalk message {#senddingdingmsg-发送钉钉消息}

* Send DingTalk message
* Requires EC standalone 2.0.0+
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

## Time {#时间相关}

### time Current timestamp in milliseconds {#time-毫秒级当前时间戳}

* Current timestamp in milliseconds
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
 logd(timeFormat("yyyy-MM-dd HH:mm:ss"));
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

## Start Debug Server {#开启调试服务}

### startDebugServer Start Debug Server {#startdebugserver-开启调试服务}

* Packaged builds can also start debug server for IDE connection
* @return Boolean; true on success, false on failure

```javascript showLineNumbers
function main() {
 logd(startDebugServer())
}

main();
```

## other {#其他}

### isReleaseIec Whether release version {#isreleaseiec-是否是发布版本}

* Whether IEC script is release version
* Requires EC standalone 5.9.0+
* @return boolean; true = release, else debug

```javascript showLineNumbers
function main() {
 var result = isReleaseIec();
 logd(result);
}

main();
```

### getDeviceExpTime Get authorization time {#getdeviceexptime-获取授权时间}

* Get authorization expiry time
* Supports EC iOS standalone2.0+
* @return `{string}` null or "" = not obtained; otherwise expiry time string

```javascript showLineNumbers
function main() {
 var result = getDeviceExpTime();
 logd(result);
}

main();
```

### setPipCtrlScript Set floating window script start/stop control {#setpipctrlscript-设置悬浮窗控制脚本启停}

* Set whether floating window controls script start/stop; prevents stop when video apps take focus
* @param ctrl true = can control, false = cannot control
* @returns `{boolean}` true = success, false = failure

```javascript showLineNumbers
function main() {
 setPipCtrlScript(true);
}

main();
```

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

### takeMeToFront Bring this app to foreground {#takemetofront-本程序带到前台运行}

* Bring this app to foreground
* @return boolean true = success, false = failure

```javascript showLineNumbers
function main() {
 var result = takeMeToFront();
 logd(result);
}

main();
```

### getMyBundleId Get IPA bundle ID {#getmybundleid-获取ipa自身的bundleid}

* Get IPA bundle ID
* Requires EC iOS 4.8.0+
* @return `{string}` string

```javascript showLineNumbers
function main() {
 var result = getMyBundleId();
 logd(result);
}

main();
```

### getMyAppName Get IPA app name {#getmyappname-获取ipa自身的应用名称}

* Get IPA app name
* Requires EC iOS 4.8.0+
* @return `{string}` string

```javascript showLineNumbers
function main() {
 var result = getMyAppName();
 logd(result);
}

main();
```
