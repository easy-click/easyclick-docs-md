---
title: H5 JS and UI Interaction
description: EasyClick automation scripts — iOS no jailbreak, no hardware, iOS scripts, development plugin installation, resource downloads
keywords:
 - EasyClick automation scripts iOS no jailbreak no hardware iOS scripts development plugin resource downloads
 - UI
 - H5
 - JS
 - ec
 - EC
 - isServiceOk
 - start
 - stopTask
 - isScriptRunning
 - getConfig
 - EasyClick
 - mobile automation
 - automation testing
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview

- This chapter covers interaction between JS and UI elements

## How to Use

- In your project's `layout` folder, create `ui.js` and `index.html` with the following content:

```javascript showLineNumbers
function main() {
    ui.layout("index", "index.html");
}

main();
```

- This displays `index.html` in a simple UI

## H5 Calls EC Functions

- Create a new iOS standalone project — default examples are included

## EC H5 Predefined Functions

### Is Automation Service Running isServiceOk

```javascript showLineNumbers
// Called from H5, not from the script
// Async callback
window.ec.isServiceOk(function (resp) {
    // resp = true if the automation service is running
});
```

### Start Script start

```javascript showLineNumbers
// Called from H5, not from the script
// Async callback
window.ec.start(function (resp) {
    // resp = true on success
});
```

### Stop Script stopTask

```javascript showLineNumbers
// Called from H5, not from the script
// Async callback
window.ec.stopTask(function (resp) {
    // resp = true on success
});
```

### Is Script Running isScriptRunning

```javascript showLineNumbers
// Called from H5, not from the script
// Async callback
window.ec.isScriptRunning(function (resp) {
    // resp = true if the script is running
});
```

### Get UI Config getConfig

```javascript showLineNumbers
// Called from H5, not from the script
// Async callback
window.ec.getConfig("key", function (resp) {
    console.log(resp)
});

```

### Remove UI Config removeConfig

```javascript showLineNumbers
// Called from H5, not from the script
// Async callback
window.ec.removeConfig("key", function (resp) {
    console.log(resp)
});

```

### Save UI Config saveConfig

```javascript showLineNumbers
// Called from H5, not from the script
// Async callback
window.ec.saveConfig("key", "data", function (resp) {
    console.log(resp)
});

```

### Get All UI Config getAllConfig

```javascript showLineNumbers
// Called from H5, not from the script
// Async callback
window.ec.getAllConfig(function (resp) {
    console.log(resp)
});

```

## Pause and Resume

### Pause/Resume setScriptPause

```javascript showLineNumbers
function pause() {
    // setScriptPause
    // {"pause":true,"timeout":3330} — pause:true = pause, false = resume
    // timeout: when pause=true, milliseconds; >0 auto-resume after that time, 0 = wait for user to resume
    window.ec.setScriptPause({"pause": true, "timeout": 3330}, function (resp) {
        checkPause()
    });
}

function checkPause() {
    window.ec.isScriptPause(function (resp) {
        alert("isScriptPause " + resp)
    });
}
```

### Pause State isScriptPause

```javascript showLineNumbers
function checkPause() {
    window.ec.isScriptPause(function (resp) {
        // resp = true if paused
        alert("isScriptPause " + resp)
    });
}
```

## Start Debug Server
### startDebugServer Start Debug Server



* Packaged apps can also start the debug server for IDE connection
* @return boolean true = success, false = failure

```javascript showLineNumbers
function main() {
    logd(ui.startDebugServer())
}

main();
```

## Extend H5 Features

### registeH5Function Inject Extension Function into H5

* Inject a JS function into the web page so H5 can call it for script/HTML interoperability
* @param funcName injected function name
* @param callback callback — see common example
* @return boolean true = success, false = failure

```javascript showLineNumbers
 // Injection in ui.js
function main() {
    // Register a new extension in the UI
    ui.registeH5Function("customFunction", function (data) {
        logd("h5 call-> " + data);
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
        // Call the extension injected from ui.js
        window.ec.call('customFunction',
                JSON.stringify({'action': '111'}),
                function (resp) {
                    console.log("ddd " + resp)
                });

    }
</script>
</body>
</html>
 ```
