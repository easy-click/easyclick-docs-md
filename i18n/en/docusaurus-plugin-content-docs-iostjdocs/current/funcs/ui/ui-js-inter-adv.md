---
title: UI and Script Interoperability
description: EasyClick automation scripts — iOS no jailbreak, no hardware, iOS scripts, development plugin installation, resource downloads
keywords:
 - EasyClick automation scripts iOS no jailbreak no hardware iOS scripts development plugin resource downloads
 - js
 - ui
 - H5
 - main
 - tip
 - UI
 - JS
 - iOS
 - 5.10.0
 - idea
 - EasyClick
 - mobile automation
 - automation testing
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview
:::tip
- This chapter covers interoperability among H5, UI JS, and scripts
- Available from iOS standalone version 5.10.0+
:::
## How to Use
- Create a standalone project in the IDE — interaction examples are included by default


## H5 → ui.js → Script Call Flow
:::tip
- The script registers a function for ui.js via `registeScriptFunctionToUI`
- ui.js registers an H5 function for the H5 page via `ui.registeH5Function`
- H5 calls ui.js, and ui.js calls the script-registered function
:::
### H5 File Content
```html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
    <link href="htmljs/bootstrap.min.css" rel="stylesheet">
    <script src="htmljs/bootstrap.min.js"></script>
    <script src="htmljs/jquery.min.js"></script>
</head>

<body>
<div class="alert alert-info" role="alert">
    Script Configuration
</div>
<div style="margin: 10px">
    <div class="form-check form-switch">
        <input class="form-check-input" type="checkbox" role="switch" id="autoService" disabled>
        <label class="form-check-label" for="autoService">Automation Service</label>
        <span class="badge text-bg-info bg-primary" onclick="refreshIsServiceOk()">Refresh</span>
        <p>Tap the agent app icon<br/>or use the activator to enable automation<br/>
            When **Automation Running** appears on the phone, you are ready
        </p>
    </div>
    <br/>
    <div class="input-group mb-3">
        <span class="input-group-text" id="username1">Name: </span>
        <input type="text" class="form-control" placeholder="" id="username" aria-label="username"
               aria-describedby="basic-addon1">
    </div>
    <div class="form-group">
        <label>Hobbies: </label>
        <label class="checkbox-inline">
            <input type="checkbox" name="aihao1" id="aihao1" value="1"> Gaming
        </label>
        <label class="checkbox-inline">
            <input type="checkbox" name="aihao2" id="aihao2" value="2"> Photography
        </label>
        <label class="checkbox-inline">
            <input type="checkbox" name="aihao3" id="aihao3" value="3"> Travel
        </label>
    </div>
    <br/>
    <div class="form-group">
        <label>Gender: </label>
        <label class="radio-inline">
            <input type="radio" value="1" name="sex"> Male
        </label>
        <label class="radio-inline">
            <input type="radio" value="2" name="sex"> Female
        </label>
        <label class="radio-inline">
            <input type="radio" value="3" name="sex"> Other
        </label>
    </div>
    <br/>
    <div class="input-group mb-3">
        <span class="input-group-text" id="basic-addon1">Feature </span>
        <select class="form-select" aria-label="" id="func">
            <option value="1">Feature 1</option>
            <option value="2">Feature 2</option>
            <option value="3">Feature 3</option>
        </select>
    </div>
    <div>
        <button type="button" class="btn btn-primary" onclick="saveParam()">Save Parameters</button>
        <button type="button" class="btn btn-success" onclick="start()">Run Script</button>
        <button type="button" class="btn btn-primary" onclick="pause()">Pause</button>
        <button type="button" class="btn btn-success" onclick="continueRun()">Resume</button>

    </div>
    <div>
        <button type="button" class="btn btn-success" onclick="h5calltest()">Test Script Injection</button>
    </div>
</div>
<script>
    function updateUserName(data) {
        $("#username").val(data)
    }
    // User click calls the h5calltest function injected in ui.js
    function h5calltest() {
        window.ec.call("h5calltest", "Data passed from H5")
    }

    function pause() {
        window.ec.setScriptPause({"pause": true, "timeout": 3330}, function (resp) {
            checkPause()
        });
    }

    function checkPause() {
        window.ec.isScriptPause(function (resp) {
            alert("isScriptPause " + resp)
        });
    }
    function continueRun() {
        window.ec.setScriptPause({"pause":false,"timeout":0},function (resp) {

        });
    }
    function refreshIsServiceOk() {
        window.ec.isServiceOk(function (resp) {
            if (resp) {
                $("#autoService").attr("checked", true)
            } else {
                $("#autoService").attr("checked", false)
            }
        })
    }

    function resetParam() {
        window.ec.isServiceOk(function (resp) {
            if (resp) {
                $("#autoService").attr("checked", true)
            } else {
                $("#autoService").attr("checked", false)
            }
        })
        // Read username
        window.ec.getConfig("uiconfig", function (resp) {
            if (resp == null || resp == undefined) {
                return
            }
            console.log("resp " + resp)
            let p = JSON.parse(resp)
            $("#username").val(p["username"])
            $("#func").val(p["func"])
            if (p["aihao1"]) {
                $("#aihao1").prop("checked", true)
            }
            if (p["aihao2"]) {
                $("#aihao2").prop("checked", true)
            }
            if (p["aihao3"]) {
                $("#aihao3").prop("checked", true)
            }
            $("input[type=radio][name='sex'][value='" + p["sex"] + "']").attr("checked", 'checked')
        })
    }

    function saveParam() {
        let username = $("#username").val()
        var aihao1 = $("#aihao1").prop("checked");
        var aihao2 = $("#aihao2").prop("checked");
        var aihao3 = $("#aihao3").prop("checked");
        let sex = $("input[name='sex']:checked").val();
        let func = $("#func").val()
        // Build JSON object
        let mp = {
            "username": username,
            "aihao1": aihao1,
            "aihao2": aihao2,
            "aihao3": aihao3,
            "sex": sex,
            "func": func
        }
        let j = JSON.stringify(mp)
        console.log("saveParam " + j)
        window.ec.saveConfig("uiconfig", j, function (resp) {
            if (resp) {
                alert("Saved successfully")
            } else {
                alert("Save failed")
            }
            console.log("Save result: " + resp)
        })
    }

    function start() {
        window.ec.start()
    }

    $(function () {
        resetParam()
    });
</script>
</body>
</html>
```


### ui.js File Content
```javascript ui.js
// ui.js file content
function main() {
    ui.layout("main", "index.html")
    uiRegToScript();
}

function uiRegToScript() {
    // // UI injects functions for the script to update the UI
    // // Direction: ui.js calls ui.registeFunctionToScript; the script calls callUIRegisteFunction at runtime
    // // Register a function callable from the JS script
    // // Script calls ui.js ui-hello -> H5 updateUserName function
    // ui.registeFunctionToScript("ui-hello", function (data) {
    // logd("UI hello " + data)
    // // Call into the H5 page
    // ui.evaluateJavaScript("updateUserName('" + data + "');")
    // return ""
    // })
  
    // H5 -> UI.JS h5calltest -> script js script-hello
    // Register a function for H5 to call
    ui.registeH5Function("h5calltest", function (data) {
        logd("h5calltest data: " + data)
        // Can call the script-injected script-hello function here
        let bb = ui.callScriptRegisteFunction("script-hello", data)
        logd("bb " + bb)
    })

}
main()

```

### main.js Script Content
```javascript

function main() {
    // Start writing your code here!!
    logd("Checking automation environment...")
    // If automation service is running
    regToUI()
  
    logd("Starting script...")
    
    // For loops, use script exit as the condition — do not use while(true)
    while (!isScriptExit()) {

        // Write your business logic here
        sleep(1000)
        logd(new Date())
    }
}

function regToUI() {
    // Examples below — see uiRegToScript in ui.js for details
    // Inject script-hello for UI.js to call
    registeScriptFunctionToUI("script-hello", function (data) {
        // If the script is not running, this function may not be reachable
        logd("Data from H5 " + data)
        return "script say hello"
    })

    // // Call the ui-hello function injected in UI.js
    // let result = callUIRegisteFunction("ui-hello", "my js data")
    // logd("Data returned from ui.js " + result)

    // Remove when no longer needed
    //removeAllScriptToUIfFunc();
    //removeAllUIToScriptFunc();

}

main();

```

## Script → ui.js → H5 Call Flow
:::tip
- H5 includes executable JS, e.g. the updateUserName function
- ui.js registers a function for the script via `ui.registeFunctionToScript`
- At script runtime, call `callUIRegisteFunction`
:::

### main.js Script Content
```javascript
function main() {
    // Start writing your code here!!
    logd("Checking automation environment...")
    // If automation service is running
    regToUI()
    // For loops, use script exit as the condition — do not use while(true)
    while (!isScriptExit()) {
      
        // Write your business logic here
        sleep(1000)
        logd(new Date())
        // Call the ui-hello function injected in ui.js
        callUIRegisteFunction("ui-hello", "my js data:"+new Date())
    }
}

function regToUI() {
    // Examples below — see uiRegToScript in ui.js for details
    // Call the ui-hello function injected in UI.js
    let result = callUIRegisteFunction("ui-hello", "my js data")
    logd("Data returned from ui.js " + result)

    // Remove when no longer needed
    //removeAllScriptToUIfFunc();
    //removeAllUIToScriptFunc();
    
}

main();
```


### ui.js Content
```javascript

function main() {
    ui.layout("main", "index.html")
    uiRegToScript();
}

function uiRegToScript() {
    // UI injects functions for the script to update the UI
    // Direction: ui.js calls ui.registeFunctionToScript; the script calls callUIRegisteFunction at runtime
    // Register a function callable from the JS script
    // Script calls ui.js ui-hello -> H5 updateUserName function
    ui.registeFunctionToScript("ui-hello", function (data) {
        logd("UI hello " + data)
        // Call into the H5 page
        ui.evaluateJavaScript("updateUserName('" + data + "');")
        return ""
    })

}
main()
```

### H5 File Content
```html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
    <link href="htmljs/bootstrap.min.css" rel="stylesheet">
    <script src="htmljs/bootstrap.min.js"></script>
    <script src="htmljs/jquery.min.js"></script>
</head>

<body>
<div class="alert alert-info" role="alert">
    Script Configuration
</div>
<div style="margin: 10px">
    <div class="form-check form-switch">
        <input class="form-check-input" type="checkbox" role="switch" id="autoService" disabled>
        <label class="form-check-label" for="autoService">Automation Service</label>
        <span class="badge text-bg-info bg-primary" onclick="refreshIsServiceOk()">Refresh</span>
        <p>Tap the agent app icon<br/>or use the activator to enable automation<br/>
            When **Automation Running** appears on the phone, you are ready
        </p>
    </div>
    <br/>
    <div class="input-group mb-3">
        <span class="input-group-text" id="username1">Name: </span>
        <input type="text" class="form-control" placeholder="" id="username" aria-label="username"
               aria-describedby="basic-addon1">
    </div>
    <div class="form-group">
        <label>Hobbies: </label>
        <label class="checkbox-inline">
            <input type="checkbox" name="aihao1" id="aihao1" value="1"> Gaming
        </label>
        <label class="checkbox-inline">
            <input type="checkbox" name="aihao2" id="aihao2" value="2"> Photography
        </label>
        <label class="checkbox-inline">
            <input type="checkbox" name="aihao3" id="aihao3" value="3"> Travel
        </label>
    </div>
    <br/>
    <div class="form-group">
        <label>Gender: </label>
        <label class="radio-inline">
            <input type="radio" value="1" name="sex"> Male
        </label>
        <label class="radio-inline">
            <input type="radio" value="2" name="sex"> Female
        </label>
        <label class="radio-inline">
            <input type="radio" value="3" name="sex"> Other
        </label>
    </div>
    <br/>
    <div class="input-group mb-3">
        <span class="input-group-text" id="basic-addon1">Feature </span>
        <select class="form-select" aria-label="" id="func">
            <option value="1">Feature 1</option>
            <option value="2">Feature 2</option>
            <option value="3">Feature 3</option>
        </select>
    </div>
    <div>
        <button type="button" class="btn btn-primary" onclick="saveParam()">Save Parameters</button>
        <button type="button" class="btn btn-success" onclick="start()">Run Script</button>
        <button type="button" class="btn btn-primary" onclick="pause()">Pause</button>
        <button type="button" class="btn btn-success" onclick="continueRun()">Resume</button>

    </div>
    <div>
        <button type="button" class="btn btn-success" onclick="h5calltest()">Test Script Injection</button>
    </div>
</div>
<script>
    // ui.js callbacks update data through this function
    function updateUserName(data) {
        $("#username").val(data)
    }
    function h5calltest() {
        window.ec.call("h5calltest", "Data passed from H5")
    }

    function pause() {
        window.ec.setScriptPause({"pause": true, "timeout": 3330}, function (resp) {
            checkPause()
        });
    }

    function checkPause() {
        window.ec.isScriptPause(function (resp) {
            alert("isScriptPause " + resp)
        });
    }
    function continueRun() {
        window.ec.setScriptPause({"pause":false,"timeout":0},function (resp) {

        });
    }
    function refreshIsServiceOk() {
        window.ec.isServiceOk(function (resp) {
            if (resp) {
                $("#autoService").attr("checked", true)
            } else {
                $("#autoService").attr("checked", false)
            }
        })
    }

    function resetParam() {
        window.ec.isServiceOk(function (resp) {
            if (resp) {
                $("#autoService").attr("checked", true)
            } else {
                $("#autoService").attr("checked", false)
            }
        })
        // Read username
        window.ec.getConfig("uiconfig", function (resp) {
            if (resp == null || resp == undefined) {
                return
            }
            console.log("resp " + resp)
            let p = JSON.parse(resp)
            $("#username").val(p["username"])
            $("#func").val(p["func"])
            if (p["aihao1"]) {
                $("#aihao1").prop("checked", true)
            }
            if (p["aihao2"]) {
                $("#aihao2").prop("checked", true)
            }
            if (p["aihao3"]) {
                $("#aihao3").prop("checked", true)
            }
            $("input[type=radio][name='sex'][value='" + p["sex"] + "']").attr("checked", 'checked')
        })
    }

    function saveParam() {
        let username = $("#username").val()
        var aihao1 = $("#aihao1").prop("checked");
        var aihao2 = $("#aihao2").prop("checked");
        var aihao3 = $("#aihao3").prop("checked");
        let sex = $("input[name='sex']:checked").val();
        let func = $("#func").val()
        // Build JSON object
        let mp = {
            "username": username,
            "aihao1": aihao1,
            "aihao2": aihao2,
            "aihao3": aihao3,
            "sex": sex,
            "func": func
        }
        let j = JSON.stringify(mp)
        console.log("saveParam " + j)
        window.ec.saveConfig("uiconfig", j, function (resp) {
            if (resp) {
                alert("Saved successfully")
            } else {
                alert("Save failed")
            }
            console.log("Save result: " + resp)
        })
    }

    function start() {
        window.ec.start()
    }

    $(function () {
        resetParam()
    });
</script>
</body>
</html>
```




## ui.js Available Functions

### ui.registeFunctionToScript Inject Function for Script

* Register a function for the script to call
* Requires EC iOS standalone 5.10+
* @param funcName function name
* @param callback function callback
* @return `{boolean}` true = success, false = failure

```javascript showLineNumbers
 // ui.js file
function main() {
    ui.registeFunctionToScript("ui-hello", function (data) {
        logd("ui hello call-> " + data);
        // Data returned to the web page
        return new Date().toString()
    })
    ui.layout("main", "main.html");
}

main();
```


### ui.callScriptRegisteFunction Call Script-Registered Function

* Call a function registered by the script
* Requires EC iOS standalone 5.10+
* @param funcName function name
* @param data data
* @return `{string}` returned data

```javascript showLineNumbers
 // ui.js file
function main() {
   // Call the script-injected script-hello function
    let a = ui.callScriptRegisteFunction("script-hello", "hello")
    logd(a)
}

main();
```


### ui.evaluateJavaScript Call H5 Page JS Code

* Execute JS code in the H5 page
* Requires EC iOS standalone 5.10+
* @param code JS code
* @return `{string}` returned data

```javascript showLineNumbers
 // ui.js file
function main() {
    let code = 'alert("123")';
    ui.evaluateJavaScript(code)
}

main();
```



### ui.removeFunctionToScript Remove UI Function Injected for Script

* Remove a UI function injected for the script
* Requires EC iOS standalone 5.10+
* @param funcName function name
* @return `{boolean}` true = success, false = failure

```javascript showLineNumbers
 // ui.js file
function main() {
    ui.removeFunctionToScript("ui-hello")
}

main();
```


### ui.removeAllScriptToUIfFunc Remove All Script Functions Registered to UI

* Remove all functions the script registered with the UI
* Requires EC iOS standalone 5.10+
* @return `{boolean}` true = success, false = failure

```javascript showLineNumbers
 // ui.js file
function main() {
    ui.removeAllScriptToUIfFunc()
}

main();
```


### ui.removeAllUIToScriptFunc Remove All UI Functions Registered to Script

* Remove all functions the UI registered with the script
* Requires EC iOS standalone 5.10+
* @return `{boolean}` true = success, false = failure

```javascript showLineNumbers
 // ui.js file
function main() {
    ui.removeAllUIToScriptFunc()
}

main();
```

## Script Available Functions

### registeScriptFunctionToUI Inject Function for UI

* Register a function for the UI to use
* Requires EC iOS standalone 5.10+
* @param funcName function name
* @param callback function callback
* @return `{boolean}` true = success, false = failure

```javascript showLineNumbers
 // main.js file
function main() {
    registeScriptFunctionToUI("script-hello", function (data) {
        logd("script hello call-> " + data);
        // Data returned to the web page
        return new Date().toString()
    })
}
main();
```


### callUIRegisteFunction Call UI Function Registered for Script

* Call a function the UI registered for the script
* Requires EC iOS standalone 5.10+
* @param funcName function name
* @param data data
* @return `{string}` returned data

```javascript showLineNumbers
 // main.js file
function main() {
   // Call the ui-hello function injected from the script
    let a = callUIRegisteFunction("ui-hello", "hello")
    logd(a)
}

main();
```


### removeFunctionToUI Remove Script Function Registered to UI
* Remove a script function registered to the UI
* Requires EC iOS standalone 5.10+
* @param funcName function name
* @return `{boolean}` true = success, false = failure

```javascript showLineNumbers
 // main.js file
function main() {
    removeFunctionToUI("script-hello")
}

main();
```




### removeAllScriptToUIfFunc Remove All Script Functions Registered to UI
* Remove all script functions registered to the UI
* Requires EC iOS standalone 5.10+
* @return `{boolean}` true = success, false = failure

```javascript showLineNumbers
 // main.js file
function main() {
  removeAllScriptToUIfFunc();
}

main();
```


### removeAllUIToScriptFunc Remove All UI Functions Registered to Script
* Remove all UI functions registered to the script
* Requires EC iOS standalone 5.10+
* @return `{boolean}` true = success, false = failure

```javascript showLineNumbers
 // main.js file
function main() {
  removeAllUIToScriptFunc();
}

main();
```
