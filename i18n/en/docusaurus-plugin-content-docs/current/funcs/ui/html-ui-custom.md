---
title: Native UI Customization
description: EasyClick automation scripts — Android no root — native UI customization
keywords:
 - EasyClick automation scripts Android no root native UI customization
 - js
 - html
 - JS
 - ui
 - UI
 - xml
 - css
 - tab
 - HTML
 - http
 - EasyClick
 - mobile automation
 - automation testing
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview
- EasyClick uses WebView to support HTML-based UI and extends JS methods for controlling the EC program.
- When creating a new project, you can choose a corresponding template; the Materialize template is recommended. Documentation: [http://www.materializecss.cn](http://www.materializecss.cn)
- You can also write your own HTML pages; refer to the template for more JS method usage examples

:::tip Note
 - When using an HTML template, the `layout` folder includes default files: css, htmljs, subjs, ui.js
 - `ui.js` is the default UI entry point and will be compiled to bytecode
 - JS files in the `subjs` folder are compiled to bytecode for `ui.js` to call; other HTML files cannot call them
 - `htmljs` stores JS files called from HTML; HTML files can use them normally
 - `css` stores CSS files
 - Incorrect usage may cause unexpected issues — please follow these rules
:::

## Multi-Tab Support
Simply create a `ui.js` file in the project's `layout` folder with the following content:

```javascript showLineNumbers
function main() {
    ui.layout("Parameter Settings", "main.html");
    ui.layout("Register & Use", "reg.html");
    ui.layout("Usage Instructions", "intr.html");
}

main();
```

## How Scripts Interact with JS

### Writing the XML View
- Use WebView to load local `main.html` with `tag=web`
- Note: You need both `layout.attr.xsd` and `layout.xsd` files — copy them manually from a native UI project's `layout` folder into your current project's `layout` folder
```xml showLineNumbers
<?xml version="1.0" encoding="UTF-8" ?>
<LinearLayout
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns:android="http://schemas.android.com/apk/res/android"
        xsi:noNamespaceSchemaLocation="layout.xsd"
        android:layout_height="match_parent"
        android:layout_width="match_parent">
    <WebView android:layout_width="wrap_content"
              android:tag="web"
              android:layout_height="wrap_content"
              android:url="main.html"/>

</LinearLayout>
```


### Loading XML
- Load the XML view in `ui.js`

```javascript showLineNumbers
function main() {
    ui.layout("Parameter Settings", "main.xml");
}

main();
```

### HTML Code

```html showLineNumbers
<!doctype html>
<html lang="en">
<head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
</head>
<body style="margin-left: 10px;margin-right: 10px">
I am a test page
</body>
<script>
    // Exposed for WebView to call
    function myalert() {
        alert("fdsafsad");
        // Store data in memory for the script to read
        window.ec.putShareData("mymsg", "Temporary data from the web page");
    }

    // Exposed for WebView to call — with parameters
    function myalert(msg) {
        alert("I am msg " + msg);
    }
</script>
</html>
```

### Calling from the Script

- Call the view from the `js/main.js` script

```javascript showLineNumbers
function main() {
    // Reset variables
    ui.resetUIVar();
    // Read data stored in memory by the UI
    logd(ui.getShareData("mymsg"))
    logd(ui.web)
    // Use the view with tag=web in the UI
    if (ui.web) {
        // myalert is a method exposed in HTML
        // Execute JS methods in the web page
        ui.web.quickCallJs("myalert();");
        ui.web.quickCallJs("myalert('bbbbb');");
    }
}

main();
```



## Browser Extension Methods

- Browser extension methods are mainly used for interaction between web pages and the EC program; these methods can only be called from within web pages


### Calling Custom Extensions
### call — Call Extension Function
 * Call an extension function injected by the script or ui.js
 * @param funcName Name of the injected function
 * @param data Can be a plain string or a JSON string
 * @return `{string}` Data returned by the called extension function

```javascript showLineNumbers
// Injection in ui.js
function main() {
    // Inject a new extension in the UI
    ui.registeH5Function("customFunction", function (data) {
        logd("h5 call-" + data);
        // Data returned to the web page
        return new Date().toString()
    })
    ui.layout("main", "main.html");
}

main();
 ```
Calling from the web page

```html showLineNumbers
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<button onclick="test()">Test Extension Function</button>
<script>
    function test() {
        // Call the extension injected by ui.js
        let d = window.ec.call('customFunction', JSON.stringify({'action': 'android.settings.SETTINGS'}));
        console.log("ddd " + d)
    }
</script>
</body>
</html>
 ```

### Start Script

```javascript showLineNumbers
window.ec.start()
```

### Stop Script

```javascript showLineNumbers
window.ec.stopTask()
```

### setAgentSupportNode — Set Node Retrieval Mode in Agent Mode

* Set the node retrieval mode in agent mode
* This method only takes effect in agent mode
* EC Android 11.2.0+
* Call this method before starting the agent service; using 2 and 3 can reduce detection signatures
* Mode 1 may trigger ruru detection of AccessibilityManager.isEnabled; modes 2 and others do not
* Mode 1 has stronger node capabilities; mode 2 has weaker node features; mode 3 has no node functionality
* @param support 1 — similar to accessibility service; 2 — shell dump; 3 — do not start node service
* @return `{boolean}` true for success, false for failure

```javascript showLineNumbers
function main() {
  window.ec.setAgentSupportNode("2");
}

main();
```



### Check if Script Is Running


```javascript showLineNumbers
window.ec.isScriptRunning()
```

### Hide Start Button


```javascript showLineNumbers
window.ec.hideStartBtn()
```


### Show Start Button

```javascript showLineNumbers
window.ec.displayStartBtn()
```


### Get All Configuration as JSON String

```javascript showLineNumbers
var s = window.ec.getConfigJSON();
alert(s);
```


### Get a Single Configuration String from Config File

```javascript showLineNumbers
var s = window.ec.getConfig("name");
alert(s);
```

### Save a Single Configuration to Config File

```javascript showLineNumbers
var s = window.ec.saveConfig("name", "123");
alert(s);
```

### Remove All UI Configuration

```javascript showLineNumbers
var s = window.ec.removeAllUIConfig();
alert(s);
```

### Save Data to Storage Area
- This data is stored in memory

```javascript showLineNumbers
window.ec.putShareData("name", "123");
```

### Read Data from Storage Area
- This data is stored in memory

```javascript showLineNumbers
var d = window.ec.getShareData("name");
alert(d);
```

### Clear Storage Area Data
- This data is stored in memory

```javascript showLineNumbers
var d = window.ec.clearAllShareData();
alert(d);
```


### Open EC System Settings


```javascript showLineNumbers
window.ec.openECSystemSetting()
```



### Save EC System Parameters
 * Set EC system parameters
 * @param params Map format, e.g. ```{"running_mode":"accessibility"}```,
 * "running_mode":"accessibility",<br/>
 * "auto_start_service":"yes",<br/>
 * "log_float_window":"no",<br/>
 * "ctrl_float_window":"no"<br/>
 * "service_start_run_script":"no"<br/>
 * Parameters:
 * running_mode: accessibility or agent (two options)
 * auto_start_service: start service on boot — values: yes (yes), no (no)
 * log_float_window: show log floating window — values: yes (yes), no (no)
 * ctrl_float_window: show start/stop control floating window — values: yes (yes), no (no)
 * service_start_run_script: auto re-run script when service restarts — values: yes (yes), no (no)
 * home_key_start_stop: triple-tap HOME to start/stop script — values: yes (yes), no (no)
 * @return Boolean — true or false

```javascript showLineNumbers
var m = {
 "running_mode": "accessibility",
 "auto_start_service": "yes",
 "log_float_window": "no",
 "ctrl_float_window": "no"
};
window.ec.setECSystemConfig(JSON.stringify(m));

```

### Show Toast Message

```javascript showLineNumbers
window.ec.toast("I am a toast message");
```

### Show Log Message Window


```javascript showLineNumbers
window.ec.showLogWindow();
```

## Close Log Message Window

```javascript showLineNumbers
window.ec.closeLogWindow();
```





### Show Log Message

```javascript showLineNumbers
window.ec.logd("I am a log message");
```



## Show Start/Stop Control Window


```javascript showLineNumbers
window.ec.showCtrlWindow();
```


### Close Start/Stop Control Window

```javascript showLineNumbers
window.ec.closeCtrlWindow();
```


### Open Other Applications — openActivity

```javascript showLineNumbers
// Open browser to download test
var m = {
 "action": "android.intent.action.VIEW",
 "uri": "https://imtt.dd.qq.com/16891/apk/55259F8EF9824AF1BF80106B0E00BCD1.apk?"

};
var x = window.ec.openActivity(JSON.stringify(m));
window.ec.logd("x " + x);

var map = {
 "uri": "xx://xx/live/6701887916223941379",
};
window.ec.openActivity(JSON.stringify(map));
```

## Service Status Control

### Check if Accessibility Service Mode

```javascript showLineNumbers
var s = window.ec.isAccMode();
alert(s);
```



### Check if Agent Service Mode

```javascript showLineNumbers
var s = window.ec.isAgentMode();
alert(s);
```

### Check if Accessibility Service Is Started

```javascript showLineNumbers
var s = window.ec.isAccServiceOk();
alert(s);
```


### Check if Agent Service Is Started

```javascript showLineNumbers
var s = window.ec.isAgentServiceOk();
alert(s);
```


### Start Service Environment

```javascript showLineNumbers
var s = window.ec.startEnv();
alert(s);
```

## Floating Window Control

### Check Floating Window Permission

```javascript showLineNumbers
var s = window.ec.hasFloatViewPermission();
alert(s);
```


### Request Floating Window Permission

```javascript showLineNumbers
// Parameter is timeout in seconds
var s = window.ec.requestFloatViewPermission(10);
alert(s);
```



### Show Floating Window

```javascript showLineNumbers
var m = {
 "path": "main.html",
 "tag": "tag",
 "titleBg": "#888888",
 "x": 10,
 "y": 10,
 "w": 100,
 "h": 200
};
var xd = window.ec.showFloatView(JSON.stringify(m));
alert(xd);
```



### Close Floating Window

```javascript showLineNumbers
var m = {
 "path": "main.html",
 "tag": "tag",
 "titleBg": "#888888",
 "x": 10,
 "y": 10,
 "w": 100,
 "h": 200
};
var xd = window.ec.showFloatView(JSON.stringify(m));
setTimeout(function () {
 window.ec.closeFloatView("tag");
}, 3000);
alert(xd);
```


### Close All Floating Windows (Excluding Log Floating Window)

```javascript showLineNumbers
var m = {
 "path": "main.html",
 "tag": "tag",
 "titleBg": "#888888",
 "x": 10,
 "y": 10,
 "w": 100,
 "h": 200
};
var xd = window.ec.showFloatView(JSON.stringify(m));
setTimeout(function () {
 window.ec.closeAllFloatView();
}, 3000);
alert(xd);
```



## Scheduled Tasks



### Start a Scheduled Task
 * Start a scheduled script task
 * @param tag Unique task identifier (required); the script can use readConfigString("jobTaskTag") to get the current tag and determine which task triggered execution
 * @param execTime Scheduled time format: 2020-04-17 19:20:00, or seconds directly, e.g. 3 for 3 seconds later
 * @param cancelBeforeRunning
 * @return Integer jobid

```javascript showLineNumbers
var time = "2020-04-17 09:00:00";
var id = window.ec.startJob("task1", time, true);
alert("job id " + id);
```

### Get All Scheduled Task TAGs

```javascript showLineNumbers
var t = window.ec.getAllJobTag();
alert("job tags " + t);
```

### Cancel All Scheduled Tasks

```javascript showLineNumbers
var t = window.ec.cancelAllJob();
alert("job cancel " + t);
```


### Cancel Scheduled Task by TAG

```javascript showLineNumbers
// Parameter task1 is the tag value used when creating the scheduled task
var t = window.ec.cancelJob("task1");
alert("job cancel " + t);
```
