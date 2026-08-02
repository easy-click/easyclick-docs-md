---
title: UI and Script Interoperability (Advanced)
description: EasyClick automation scripts — Android no root
keywords:
 - EasyClick automation scripts Android no root
 - js
 - ui
 - H5
 - main
 - tip
 - UI
 - EC
 - 11.15.0
 - idea
 - vue
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
- This chapter covers interoperability among ui.js, scripts, and H5
- Available from EC Android 11.15.0+
:::
## How to Use
- Create a new project in the IDE — interaction examples are included by default
- For a Vue project, see the example where the script updates ui.js and then the H5 page


## ui.js → Script Call Flow
:::tip
- The script registers a function for ui.js via `registeScriptFunctionToUI`
- `ui.callScriptRegisteFunction` calls the script-registered function
:::


### ui.js File Content
```javascript ui.js
function main() {
  // This template uses VUE + ELEMENT UI — visit https://uc.ieasyclick.com/designer to drag-and-drop UI
  // Copy into your HTML file for easy use
  // Parameter Settings = main.html
  // Instructions = intr.html
  // Other = other.html
  ui.layout("Parameter Settings", "main.html");

  // Example of UI/script mutual registration
  regFuncToScript()

}

function regFuncToScript() {
  // Register uihello for the script to call
  ui.registeFunctionToScript("uihello", function (data) {
    logd("registeFunctionToScript log: " + data)
    // Update the name field in the Vue template
    let up = `updateUserName('${data}')`
    // ui.quickCallJs is a new function — see documentation
    ui.quickCallJs("Parameter Settings", up)
    // Call into Vue
    return "Value returned from UI"
  })
  // When the script is running
  // Call scripthello after 3 seconds
  // scripthello is unreachable if the script is not running
  ui.run(3000, function () {
    ui.callScriptRegisteFunction("scripthello", "123")
  })
}

main();

```

### main.js Script Content
```javascript
function main() {
  // Start writing your code here!!
  toast("Hello World");
  var name = readConfigString("name");
  logd("Name: " + name);

  // Example: register function for UI to call
  regFuncToUI();
  // Example: call UI uihello function
  let uih = callUIRegisteFunction("uihello", "-date-" + new Date());
  logd("uihello call result: " + uih)

  
  home();
}

function regFuncToUI() {
  // Register scripthello for UI to call
  registeScriptFunctionToUI("scripthello", function (data) {
    logd("regFuncToUI log: " + data)
    return "" + new Date();
  })

  // Remove registered functions when appropriate — usually safe to leave them
  //removeAllUIToScriptFunc();
  //removeAllScriptToUIFunc()
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
  toast("Hello World");
  var name = readConfigString("name");
  logd("Name: " + name);

  // Example: register function for UI to call
  regFuncToUI();
  // Example: call UI uihello function
  let uih = callUIRegisteFunction("uihello", "-date-" + new Date());
  logd("uihello call result: " + uih)
  
  home();
}

function regFuncToUI() {
  // Register scripthello for UI to call
  registeScriptFunctionToUI("scripthello", function (data) {
    logd("regFuncToUI log: " + data)
    return "" + new Date();
  })

  // Remove registered functions when appropriate — usually safe to leave them
  //removeAllUIToScriptFunc();
  //removeAllScriptToUIFunc()
}
main();

```


### ui.js Content
```javascript

function main() {
  // This template uses VUE + ELEMENT UI — visit https://uc.ieasyclick.com/designer to drag-and-drop UI
  // Copy into your HTML file for easy use
  // Parameter Settings = main.html
  // Instructions = intr.html
  // Other = other.html
  ui.layout("Parameter Settings", "main.html");

  // Example of UI/script mutual registration
  regFuncToScript()

}

function regFuncToScript() {
  // Register uihello for the script to call
  ui.registeFunctionToScript("uihello", function (data) {
    logd("registeFunctionToScript log: " + data)
    // Update the name field in the Vue template
    let up = `updateUserName('${data}')`
    // ui.quickCallJs is a new function — see documentation
    ui.quickCallJs("Parameter Settings", up)
    // Call into Vue
    return "Value returned from UI"
  })
  // When the script is running
  // Call scripthello after 3 seconds
  // scripthello is unreachable if the script is not running
  ui.run(3000, function () {
    ui.callScriptRegisteFunction("scripthello", "123")
  })
}

main();
```

### H5 File Content
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <!-- import CSS -->
    <link rel="stylesheet" href="/css/elementui.css">
</head>
<body>
<div id="app">
    <el-tag type="success">Demo template built with bundled Element UI + Vue</el-tag>
    <el-form style="margin-top: 10px" :rules="rules" ref="form" :model="form" label-width="80px" size="mini">
        <el-form-item label="License Key" prop="cardNo">
            <el-input v-model="form.cardNo"></el-input>
        </el-form-item>
        <el-form-item label="Name" prop="name">
            <el-input v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item label="Gender" prop="sex">
            <el-radio-group v-model="form.sex" size="medium">
                <el-radio border label="Male"></el-radio>
                <el-radio border label="Female"></el-radio>
            </el-radio-group>
        </el-form-item>
        <el-form-item size="large">
            <el-button type="primary" @click="submitForm">Save</el-button>
            <el-button type="primary" @click="runScript">Run Script</el-button>
        </el-form-item>
    </el-form>
</div>
</body>
<!-- import Vue before Element -->
<script src="/htmljs/vue2.7.16.js"></script>
<!-- import JavaScript -->
<script src="/htmljs/elementui2.15.14.js"></script>
<script src="/htmljs/form-create.min.js"></script>
<script>
    // Callable from ui.js to update Vue data
    function updateUserName(data) {
        vueInstance.updateUserName(data);
    }

    let vueInstance = new Vue({
        el: '#app',
        data: function () {
            return {
                form: {
                    name: '',
                    cardNo: '',
                    sex: '',
                },
                rules: {
                    name: [
                        {required: true, message: 'Please enter name', trigger: 'blur'},
                    ],
                    cardNo: [
                        {required: true, message: 'Please enter license key', trigger: 'blur'},
                    ],
                    sex: [
                        {required: true, message: 'Please select gender', trigger: 'blur'},
                    ],
                }
            }
        },
        created() {
            this.resetValue()
        },
        methods: {
            updateUserName(data) {
                console.log("updateUserName " + data)
                this.form.name = data;
            },
            resetValue() {
                let name = window.ec.getConfig("name");
                let sex = window.ec.getConfig("sex");
                let cardNo = window.ec.getConfig("cardNo");
                this.form.name = name
                this.form.cardNo = cardNo
                this.form.sex = sex
            },
            save() {
                window.ec.saveConfig("name", this.form.name);
                window.ec.saveConfig("cardNo", this.form.cardNo);
                window.ec.saveConfig("sex", this.form.sex);

                this.$notify.info({
                    title: 'Message',
                    duration: 2000,
                    message: 'Parameters saved successfully'
                });
            },
            submitForm() {

                this.$refs.form.validate((valid) => {
                    if (valid) {
                        this.save();
                    } else {
                        this.$notify.error({
                            title: 'Error',
                            message: 'Incomplete parameters'
                        });
                        return false;
                    }
                });
            },
            runScript() {
                window.ec.start()
            },
        }
    })
</script>
</html>
```




## ui.js Available Functions

### ui.registeFunctionToScript Inject Function for Script

* Register a function for the script to call
* Requires EC Android 11.15.0+
* @param funcName function name
* @param callback function callback
* @return `{boolean}` true = success, false = failure

```javascript showLineNumbers
 // ui.js file
function main() {
  ui.layout("Parameter Settings", "main.html");

  ui.registeFunctionToScript("uihello", function (data) {
    logd("registeFunctionToScript log: " + data)
    // Update the name field in the Vue template
    let up = `updateUserName('${data}')`
    // ui.quickCallJs is a new function — see documentation
    ui.quickCallJs("Parameter Settings", up)
    // Call into Vue
    return "Value returned from UI"
  })
  
}

main();
```


### ui.callScriptRegisteFunction Call Script-Registered Function

* Call a function registered by the script
* Requires EC Android 11.15.0+
* @param funcName function name
* @param data data to pass
* @return `{string}` returned data

```javascript showLineNumbers
 // ui.js file
function main() {
   // Call the script-injected scripthello function
    let a = ui.callScriptRegisteFunction("scripthello", "hello")
    logd(a)
}

main();
```


### ui.quickCallJs Call H5 Page JS Code

* Execute JS in the WebView inside the UI
* Requires EC Android 11.15.0+
* Automatically finds the WebView page by tab label
* @param name page tab name — must match ui.layout name
* @param jscode JS code
* @return `{string}` returned data

```javascript showLineNumbers
 // ui.js file
function main() {
    let code = 'alert("123")';
    let a = ui.quickCallJs("Parameter Settings",code)
    logd(a)
}

main();
```



### ui.removeFunctionToScript Remove UI Function Injected for Script

* Remove a UI function injected for the script
* @param funcName function name
* Requires EC Android 11.15.0+
* @return `{boolean}` true = success, false = failure

```javascript showLineNumbers
 // ui.js file
function main() {
    ui.removeFunctionToScript("uihello")
}

main();
```


### ui.removeAllScriptToUIFunc Remove All Script Functions Registered to UI

* Remove all functions the script registered with the UI
* Requires EC 11.15.0+
* @return `{boolean}` true = success

```javascript showLineNumbers
 // ui.js file
function main() {
    ui.removeAllScriptToUIFunc()
}

main();
```


### ui.removeAllUIToScriptFunc Remove All UI Functions Registered to Script

* Remove all functions the UI registered with the script
* Requires EC Android 11.15.0+
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
* Requires EC 11.15.0+
* @param funcName function name
* @param callback function callback
* @return `{boolean}` true = success, false = failure

```javascript showLineNumbers
 // main.js file
function main() {
    registeScriptFunctionToUI("scripthello", function (data) {
        logd("script hello call-> " + data);
        // Data returned to the web page
        return new Date().toString()
    })
}
main();
```


### callUIRegisteFunction Call UI Function Registered for Script

* Call a function the UI registered for the script
* Requires EC 11.15.0+
* @param funcName function name
* @param data data
* @return `{string}` returned data

```javascript showLineNumbers
 // main.js file
function main() {
   // Call the ui-hello function injected from the script
    let a = callUIRegisteFunction("uihello", "hello")
    logd(a)
}

main();
```


### removeFunctionToUI Remove Script Function Registered to UI
* Remove a script function registered to the UI
* Requires EC 11.15.0+
* @param funcName function name
* @return `{boolean}` true = success, false = failure

```javascript showLineNumbers
 // main.js file
function main() {
    removeFunctionToUI("scripthello")
}

main();
```




### removeAllScriptToUIFunc Remove All Script Functions Registered to UI
* Remove all script functions registered to the UI
* Requires EC 11.15.0+
* @return `{boolean}` true = success, false = failure

```javascript showLineNumbers
 // main.js file
function main() {
  removeAllScriptToUIFunc();
}

main();
```


### removeAllUIToScriptFunc Remove All UI Functions Registered to Script
* Remove all UI functions registered to the script
* Requires EC 11.15.0+
* @return `{boolean}` true = success, false = failure

```javascript showLineNumbers
 // main.js file
function main() {
  removeAllUIToScriptFunc();
}

main();
```
