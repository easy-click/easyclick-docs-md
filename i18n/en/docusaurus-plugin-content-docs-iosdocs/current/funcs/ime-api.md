---
title: IME Functions
description: EasyClick automation scripts — iOS no jailbreak IME (input method) functions
keywords:
 - EasyClick automation scripts iOS no jailbreak IME functions
 - imeApi
 - isOk
 - tip
 - input
 - paste
 - pressDel
 - pressEnter
 - dismiss
 - copyToClipboard
 - changeKeyboard
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview

:::tip
- IME functions are provided by the **EasyClick Cloud Test** built-in keyboard, designed specifically for text input
- Enable the IME before use:
 - Go to Settings > General > Keyboard > Keyboards > Add New Keyboard and enable the [packaged app name (default: EasyClick Cloud Test; custom name after packaging)] keyboard. If you cannot find it, restart the device
 - After setup, tap the keyboard name on the keyboard screen and select [Allow Full Access]
- Once enabled, the keyboard appears when a text field is focused. If multiple keyboards are available, tap the [Globe] button at the bottom-left to switch to EasyClick Cloud Test
- When the [EasyClick Cloud Test (goose-yellow background)] keyboard is visible, the functions are ready to use
- Important: functions work only when the EasyClick Cloud Test keyboard is shown; otherwise they have no effect. Use `imeApi.isOk()` to check whether the IME is ready
- The IME can fully replace previous input functions and avoids freezing the proxy app
- ***Not suitable for: password fields and fields that disallow third-party keyboards — iOS will switch back to the system keyboard automatically ***
:::

:::tip
- File Transfer Assistant IPA is EasyClick Cloud Test — they are the same package
- File Transfer Assistant module functions and IME functions share the same IPA package
:::

## imeApi.isOk Check IME Status

* Check whether the IME is available
* Supported EC iOS USB 6.37.0+
* @return `{boolean}` true if available, false otherwise

```javascript showLineNumbers
function main() {
    var ok = imeApi.isOk();
    if (!ok) {
        logw("IME unavailable. Go to Settings > General > Keyboard and enable the [packaged app name (default: EasyClick Cloud Test)] third-party keyboard. If the option is missing, restart the device")
        logw("After enabling, tap a text field and show the [packaged app name (default: EasyClick Cloud Test, goose-yellow background)] keyboard before using these functions")
        return
    }
    logd("IME is ready")
}

main();
```

## imeApi.input Input String

* Input a string
* Supported EC iOS USB 6.37.0+
* @param content string
* @returns `{string}` empty if input failed; non-empty if input succeeded (returns the entered data)

```javascript showLineNumbers
function main() {
    var ok = imeApi.isOk();
    if (!ok) {
        logw("IME unavailable. Go to Settings > General > Keyboard and enable the [packaged app name (default: EasyClick Cloud Test)] third-party keyboard. If the option is missing, restart the device")
        logw("After enabling, tap a text field and show the [packaged app name (default: EasyClick Cloud Test, goose-yellow background)] keyboard before using these functions")
        return
    }
    let result = imeApi.input("我是数据")
    logd("Text field data: " + result)
}

main();
```

## imeApi.paste Paste String

* Paste a string — copy to clipboard first, then insert into the text field
* Supported EC iOS USB 6.37.0+
* @param content string; if empty, uses clipboard data directly
* @returns `{string}` empty if failed; non-empty if succeeded (returns the pasted data)

```javascript showLineNumbers
function main() {
    var ok = imeApi.isOk();
    if (!ok) {
        logw("IME unavailable. Go to Settings > General > Keyboard and enable the [packaged app name (default: EasyClick Cloud Test)] third-party keyboard. If the option is missing, restart the device")
        logw("After enabling, tap a text field and show the [packaged app name (default: EasyClick Cloud Test, goose-yellow background)] keyboard before using these functions")
        return
    }
    let result = imeApi.paste("我是粘贴数据")
    logd("Pasted text field data: " + result)
}

main();
```

## imeApi.pressDel Delete Text in Input Field

* Delete text in the input field
* Supported EC iOS USB 6.37.0+
* @returns `{string}` empty if the field has no data; non-empty if data remains

```javascript showLineNumbers
function main() {
    var ok = imeApi.isOk();
    if (!ok) {
        logw("IME unavailable. Go to Settings > General > Keyboard and enable the [packaged app name (default: EasyClick Cloud Test)] third-party keyboard. If the option is missing, restart the device")
        logw("After enabling, tap a text field and show the [packaged app name (default: EasyClick Cloud Test, goose-yellow background)] keyboard before using these functions")
        return
    }
    let result = imeApi.pressDel()
    logd("Remaining text field data: " + result)
}

main();
```

## imeApi.pressEnter Press Enter Key

* Press the Enter key
* Supported EC iOS USB 6.37.0+
* @returns `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    var ok = imeApi.isOk();
    if (!ok) {
        logw("IME unavailable. Go to Settings > General > Keyboard and enable the [packaged app name (default: EasyClick Cloud Test)] third-party keyboard. If the option is missing, restart the device")
        logw("After enabling, tap a text field and show the [packaged app name (default: EasyClick Cloud Test, goose-yellow background)] keyboard before using these functions")
        return
    }
    let result = imeApi.pressEnter()
    logd("pressEnter: " + result)
}

main();
```

## imeApi.dismiss Hide Keyboard

* Hide the keyboard
* Supported EC iOS USB 6.37.0+
* @returns `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    var ok = imeApi.isOk();
    if (!ok) {
        logw("IME unavailable. Go to Settings > General > Keyboard and enable the [packaged app name (default: EasyClick Cloud Test)] third-party keyboard. If the option is missing, restart the device")
        logw("After enabling, tap a text field and show the [packaged app name (default: EasyClick Cloud Test, goose-yellow background)] keyboard before using these functions")
        return
    }
    let result = imeApi.dismiss()
    logd("dismiss: " + result)

}

main();
```

## imeApi.copyToClipboard Copy Input Field Data to Clipboard

* Copy input field data to the clipboard
* Supported EC iOS USB 6.37.0+
* @returns `{string}` empty if the field has no data; non-empty if data was copied to the clipboard

```javascript showLineNumbers
function main() {
    var ok = imeApi.isOk();
    if (!ok) {
        logw("IME unavailable. Go to Settings > General > Keyboard and enable the [packaged app name (default: EasyClick Cloud Test)] third-party keyboard. If the option is missing, restart the device")
        logw("After enabling, tap a text field and show the [packaged app name (default: EasyClick Cloud Test, goose-yellow background)] keyboard before using these functions")
        return
    }
    let result = imeApi.copyToClipboard()
    logd("copyToClipboard data: " + result)
}

main();
```

## imeApi.changeKeyboard Switch to Another Keyboard

* Switch to another keyboard
* Returns first, then switches after a 2-second wait
* Supported EC iOS USB 6.37.0+
* @returns `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    var ok = imeApi.isOk();
    if (!ok) {
        logw("IME unavailable. Go to Settings > General > Keyboard and enable the [packaged app name (default: EasyClick Cloud Test)] third-party keyboard. If the option is missing, restart the device")
        logw("After enabling, tap a text field and show the [packaged app name (default: EasyClick Cloud Test, goose-yellow background)] keyboard before using these functions")
        return
    }
    let result = imeApi.changeKeyboard()
    logd("changeKeyboard data: " + result)
}

main();
```

## imeApi.removeAllContent Clear Input Field

* Clear all content in the input field
* Supported EC iOS USB 6.37.0+
* @returns `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    var ok = imeApi.isOk();
    if (!ok) {
        logw("IME unavailable. Go to Settings > General > Keyboard and enable the [packaged app name (default: EasyClick Cloud Test)] third-party keyboard. If the option is missing, restart the device")
        logw("After enabling, tap a text field and show the [packaged app name (default: EasyClick Cloud Test, goose-yellow background)] keyboard before using these functions")
        return
    }
    let result = imeApi.removeAllContent()
    logd("removeAllContent : " + result)
}

main();
```

## imeApi.getClipboard Read Clipboard Data

* Read clipboard data
* Supported EC iOS USB 6.37.0+
* @returns `{string}` clipboard data

```javascript showLineNumbers
function main() {
    var ok = imeApi.isOk();
    if (!ok) {
        logw("IME unavailable. Go to Settings > General > Keyboard and enable the [packaged app name (default: EasyClick Cloud Test)] third-party keyboard. If the option is missing, restart the device")
        logw("After enabling, tap a text field and show the [packaged app name (default: EasyClick Cloud Test, goose-yellow background)] keyboard before using these functions")
        return
    }
    let result = imeApi.getClipboard()
    logd("getClipboard data : " + result)
}

main();
```

## imeApi.setClipboard Set Clipboard Data

* Set clipboard data
* Supported EC iOS standalone 3.15.0+
* @param content string
* @param type 1 for plain string, 2 for URL data
* @returns `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    var ok = imeApi.isOk();
    if (!ok) {
        logw("IME unavailable. Go to Settings > General > Keyboard and enable the [packaged app name (default: EasyClick Cloud Test)] third-party keyboard. If the option is missing, restart the device")
        logw("After enabling, tap a text field and show the [packaged app name (default: EasyClick Cloud Test, goose-yellow background)] keyboard before using these functions")
        return
    }
    let result = imeApi.setClipboard("我是剪切板的的数据","1")
    logd("setClipboard : " + result)
}

main();
```

## imeApi.openUrl Open URL Link

* Open a URL link
* Supported EC iOS standalone 3.15.0+
* @param url URL address, e.g. http://baidu.com
* @returns `{boolean}` true on success, false on failure

```javascript showLineNumbers
function main() {
    var ok = imeApi.isOk();
    if (!ok) {
        logw("IME unavailable. Go to Settings > General > Keyboard and enable the [packaged app name (default: EasyClick Cloud Test)] third-party keyboard. If the option is missing, restart the device")
        logw("After enabling, tap a text field and show the [packaged app name (default: EasyClick Cloud Test, goose-yellow background)] keyboard before using these functions")
        return
    }
    let result = imeApi.openUrl("http://baidu.com")
    logd("openUrl : " + result)
}

main();
```

## imeApi.getText Get Input Field Data

* Get input field data
* Supported EC iOS standalone 3.15.0+
* @returns `{string}` empty if no data or not retrieved; non-empty if data was retrieved

```javascript showLineNumbers
function main() {
    var ok = imeApi.isOk();
    if (!ok) {
        logw("IME unavailable. Go to Settings > General > Keyboard and enable the [packaged app name (default: EasyClick Cloud Test)] third-party keyboard. If the option is missing, restart the device")
        logw("After enabling, tap a text field and show the [packaged app name (default: EasyClick Cloud Test, goose-yellow background)] keyboard before using these functions")
        return
    }
    let result = imeApi.getText()
    logd("getText : " + result)
}

main();
```

## imeApi.forwardImeServer Forward Input Method Service

* Forward the input method service
* Supported EC iOS USB 9.20.0+
* This avoids enabling the proxy service; if forwarding is used, the proxy service must be enabled for forwarding
* @returns `{string}` null or empty on success; otherwise an error message

```javascript showLineNumbers
function testImeServer() {
    imeApi.closeForwardImeServer()
    let forward = imeApi.forwardImeServer();
    if (forward == null) {
        console.log("Input method forwarding succeeded; proxy service is not required")
    } else {
        logw("Forward failed")
        return
    }
    sleep(1000)
    let ok = imeApi.isOk()
    if (!ok) {
        logw("Input method not ready")
        return
    }

    let rr = imeApi.input("123")
    console.log("Input result: " + rr)
    logd(imeApi.paste("bgigm"))
}

testImeServer()
```

## imeApi.closeForwardImeServer Close Input Method Forwarding

* Close input method forwarding
* Supported EC iOS USB 9.20.0+
* @returns `{string}` null or empty on success; otherwise an error message

```javascript showLineNumbers
function testImeServer() {
    imeApi.closeForwardImeServer()
    let forward = imeApi.forwardImeServer();
    if (forward == null) {
        console.log("Input method forwarding succeeded; proxy service is not required")
    } else {
        logw("Forward failed")
        return
    }
    sleep(1000)
    let ok = imeApi.isOk()
    if (!ok) {
        logw("Input method not ready")
        return
    }

    let rr = imeApi.input("123")
    console.log("Input result: " + rr)
    logd(imeApi.paste("bgigm"))
}

testImeServer()
```
