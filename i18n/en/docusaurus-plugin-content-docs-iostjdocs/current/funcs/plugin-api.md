---
title: Plugin Functions
description: EasyClick automation scripts — iOS no jailbreak — plugin functions
keywords:
 - EasyClick automation scripts — iOS no jailbreak — plugin functions
 - iOS
 - param
 - loadPlugin
 - makeInstance
 - EC
 - 4.2.0
 - pluginName
 - getErrorMsg
 - callMethodStr
 - callMethodData
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

:::tip

- Due to iOS sandbox and code signing, plugins cannot be loaded dynamically and cannot be placed in the script `plugin` directory. Standalone plugins must be packaged into the IPA
- Standalone plugins support only iOS `.framework` files
- For how to write plugins, see [How to Write Standalone Plugins?](/iostjdocs/advance/pluginguid). Sample plugin code: [Download](/iosdocs/tjplugindemo.zip) (helloworldplugin and helloworldplugin2)
- The plugin module prefix is **pluginLoader**

:::

## loadPlugin Load a Plugin

* Load a plugin
* Available in EC iOS 4.2.0+
* @param pluginName plugin name
* @return `boolean` — `true` on success, `false` on failure

```javascript showLineNumbers
function main() {
    // For a plugin file named helloworldplugin2.framework, the plugin name is helloworldplugin2
    let pluginName = "helloworldplugin2"
    let loaded = pluginLoader.loadPlugin(pluginName)
    if (!loaded) {
        loge("Failed to load plugin")
        return
    } else {
        logd("Plugin loaded successfully")
    }
}

main();
```

## makeInstance Create Class Instance

* Create a class instance
* Available in EC iOS 4.2.0+
* @param pluginName plugin name
* @param clzName class name
* @returns `string` — `null` or empty string on success; otherwise error message

```javascript showLineNumbers
function main() {
    let name = "helloworldplugin2"
    let clzName = "Plugin1"
    let load = pluginLoader.loadPlugin(name)
    if (!load) {
        loge("Failed to load plugin: " + pluginLoader.getErrorMsg())
        return
    }
    logd("Plugin loaded successfully {}", name)
    let ins = pluginLoader.makeInstance(name, clzName)
    if (ins == null || ins == "") {
        logd("Class instantiated successfully: " + clzName)
    } else {
        loge("Failed to instantiate class: " + clzName)
        return
    }
}

main();
```

## getErrorMsg Error Message

* Get error message
* @return `string` — non-empty string indicates an error

```javascript showLineNumbers
function main() {
    let name = "helloworldplugin2"
    let clzName = "Plugin1"
    let load = pluginLoader.loadPlugin(name)
    if (!load) {
        loge("Failed to load plugin: " + pluginLoader.getErrorMsg())
        return
    }
    logd("Plugin loaded successfully {}", name)
    let ins = pluginLoader.makeInstance(name, clzName)
    if (ins == null || ins == "") {
        logd("Class instantiated successfully: " + clzName)
    } else {
        loge("Failed to instantiate class: " + clzName)
        return
    }
}

main();
```

## callMethodStr Call Plugin Method (String Return)

* Call a plugin instance method that returns a string
* Available in EC iOS 4.2.0+
* @param pluginName plugin name
* @param clzName class name
* @param methodName method name (string)
* @param args argument string
* @return `string`

```javascript showLineNumbers
function main() {
    let name = "helloworldplugin2"
    let clzName = "Plugin1"
    let load = pluginLoader.loadPlugin(name)

    if (!load) {
        loge("Failed to load plugin: " + pluginLoader.getErrorMsg())
        return
    }
    logd("Plugin loaded successfully {}", name)
    let ins = pluginLoader.makeInstance(name, clzName)
    if (ins == null || ins == "") {
        logd("Class instantiated successfully: " + clzName)
    } else {
        loge("Failed to instantiate class: " + clzName)
        return
    }
    let args = JSON.stringify({"a": 1, "b": "" + new Date()})
    let rs = pluginLoader.callMethodStr(name, clzName, "testMethod", args)
    logd("testMethod result " + rs)
}

main();
```

## callMethodData Call Instance Method

* Call an instance method that returns string data
* Available in EC iOS 4.2.0+
* @param pluginName plugin name
* @param clzName class name
* @param methodName method name (string)
* @param data Swift `Data` object (byte array)
* @return `string`

```javascript showLineNumbers
function main() {
    let name = "helloworldplugin2"
    let clzName = "Plugin1"
    let load = pluginLoader.loadPlugin(name)
    if (!load) {
        loge("Failed to load plugin: " + pluginLoader.getErrorMsg())
        return
    }
    logd("Plugin loaded successfully {}", name)
    let ins = pluginLoader.makeInstance(name, clzName)
    if (ins == null || ins == "") {
        logd("Class instantiated successfully: " + clzName)
    } else {
        loge("Failed to instantiate class: " + clzName)
        return
    }
    let args = JSON.stringify({"a": 1, "b": "" + new Date()})
    let rs = pluginLoader.callMethodReturnData(name, clzName, "testMethod", args)
    logd("testMethod result " + rs)

    let anyR = pluginLoader.callMethodData(name, clzName, "testMethod", rs)
    logd("callMethodData result " + anyR)
}

main();
```

## callMethodReturnData Call Instance Method

* Call a plugin instance method
* Available in EC iOS 4.2.0+
* @param pluginName plugin name
* @param clzName class name
* @param methodName method name (string)
* @param args argument string
* @return Data — Swift `Data` object (byte array)

```javascript showLineNumbers
function main() {
    let name = "helloworldplugin2"
    let clzName = "Plugin1"
    let load = pluginLoader.loadPlugin(name)
    if (!load) {
        loge("Failed to load plugin: " + pluginLoader.getErrorMsg())
        return
    }
    logd("Plugin loaded successfully {}", name)
    let ins = pluginLoader.makeInstance(name, clzName)
    if (ins == null || ins == "") {
        logd("Class instantiated successfully: " + clzName)
    } else {
        loge("Failed to instantiate class: " + clzName)
        return
    }
    let args = JSON.stringify({"a": 1, "b": "" + new Date()})
    let rs = pluginLoader.callMethodReturnData(name, clzName, "testMethod", args)
    logd("testMethod result " + rs)

    let anyR = pluginLoader.callMethodData(name, clzName, "testMethod", rs)
    logd("callMethodData result " + anyR)
}

main();
```

## callMethodAny Call Instance Method

* Call a plugin method with Any parameters and return value
* Available in EC iOS 4.2.0+
* @param pluginName plugin name
* @param clzName class name
* @param methodName method name (string)
* @param data argument of any type
* @return Any — Swift `Any` object; any JS type

```javascript showLineNumbers
function main() {
    let name = "helloworldplugin2"
    let clzName = "Plugin1"
    let load = pluginLoader.loadPlugin(name)
    if (!load) {
        loge("Failed to load plugin: " + pluginLoader.getErrorMsg())
        return
    }
    logd("Plugin loaded successfully {}", name)
    let ins = pluginLoader.makeInstance(name, clzName)
    if (ins == null || ins == "") {
        logd("Class instantiated successfully: " + clzName)
    } else {
        loge("Failed to instantiate class: " + clzName)
        return
    }
    let args = JSON.stringify({"a": 1, "b": "" + new Date()})
    let rs = pluginLoader.callMethodAny(name, clzName, "testMethod", args)
    logd("callMethodAny result " + rs)


    logd("Starting screenshot Any type test...")

    setComputeMode(1)
    let img1 = image.captureFullScreen();
    logd(img1)

    // Test 1
    let uiimage = image.autoImageToUIImage(img1)
    logd("autoImageToUIImage uiimage " + uiimage)
    let x = pluginLoader.callMethodAny(name, clzName, "testMethod", uiimage)
    logd(x)
    image.recycle(img1)


    let img2 = image.captureFullScreenUIImage({})
    let xX = pluginLoader.callMethodAny(name, clzName, "testMethod", img2)
    logd(xX)


    let au = image.uiimageToAutoImage(uiimage)
    logd("au " + au)

    // Save to file
    image.saveTo(au, file.getSandBoxFilePath("a.jpg"))
    image.recycle(au)
    image.recycle(img2)

}

main();
```

## readBundleFile Read Plugin File

* Read a file bundled in the plugin
* Available in EC iOS 4.2.0+
* @param pluginName plugin name
* @param key file name without extension
* @param ext file extension without leading `.`
* @return `string` file content

```javascript showLineNumbers
function main() {
    let name = "helloworldplugin2"
    let clzName = "Plugin1"
    let load = pluginLoader.loadPlugin(name)
    if (!load) {
        loge("Failed to load plugin: " + pluginLoader.getErrorMsg())
        return
    }
    logd("Plugin loaded successfully {}", name)
    logd("Reading test.txt from plugin bundle")
    let ins = pluginLoader.readBundleFile(name, "test", "txt")
    logd("readBundleFile result " + ins)
}

main();
```

## copyBundleFile Copy Plugin File to Path

* Copy a plugin file to a destination path
* Available in EC iOS 4.2.0+
* @param pluginName plugin name
* @param key file name without extension
* @param ext file extension without leading `.`
* @param dest destination file path
* @return `bool` — `true` on success

```javascript showLineNumbers
function main() {
    let name = "helloworldplugin2"
    let clzName = "Plugin1"
    let load = pluginLoader.loadPlugin(name)
    if (!load) {
        loge("Failed to load plugin: " + pluginLoader.getErrorMsg())
        return
    }
    logd("Plugin loaded successfully {}", name)
    let dest = file.getSandBoxFilePath("a.txt")
    logd("dest" + dest)
    logd("Copying test.txt to " + dest)
    let ins = pluginLoader.copyBundleFile(name, "test", "txt", dest)
    logd("copyBundleFile result " + ins)
}

main();
```
