---
title: IME Functions
description: EasyClick automation scripts — iOS no jailbreak — IME (input method) functions
keywords:
 - EasyClick automation scripts — iOS no jailbreak — IME functions
 - imeApi
 - isOk
 - iOS
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
- IME functions use the keyboard built into **EasyClick Cloud Test**, designed specifically for text input
- Enable the IME before use:
 - Go to Settings → General → Keyboard → Keyboards → Add New Keyboard and enable the [packaged app name (default: EasyClick Cloud Test; custom name after packaging)] keyboard. If you cannot find it, restart the device
 - After setup, on the keyboard screen tap the keyboard name and select [Allow Full Access]
- When an input field is focused, the keyboard appears. If multiple keyboards are installed, tap the [Globe] button at the bottom-left to switch to EasyClick Cloud Test
- Once the [EasyClick Cloud Test (goose-yellow background)] keyboard is visible, the functions are ready to use
- Important: functions only work when the EasyClick Cloud Test keyboard is shown; otherwise they have no effect. Use `imeApi.isOk()` to check whether the IME is ready
- The IME can fully replace previous input functions and avoids freezing the proxy app
- ***Not suitable for: password fields and fields that disallow third-party keyboards — iOS will switch to the system keyboard automatically ***
:::

## imeApi.isOk Check IME Status

* Whether the IME is available
* Available in EC iOS standalone 3.15.0+
* @return `{boolean}` `true` if available, `false` if not

```javascript showLineNumbers
function main() {
    var ok = imeApi.isOk();
    if (!ok) {
        logw("IME unavailable. Go to Settings → General → Keyboard and enable the [packaged app name (default: EasyClick Cloud Test)] third-party keyboard. If the option is missing, restart the device")
        logw("After enabling, tap an input field and show the [packaged app name (default: EasyClick Cloud Test, goose-yellow background)] keyboard before using these functions")
        return
    }
    logd("IME is ready")
}

main();
```

## imeApi.input Input String

* Input a string
* Available in EC iOS standalone 3.15.0+
* @param content string
* @returns `{string}` empty if input failed; non-empty if input succeeded (returns the entered data)

```javascript showLineNumbers
function main() {
    var ok = imeApi.isOk();
    if (!ok) {
        logw("IME unavailable. Go to Settings → General → Keyboard and enable the [packaged app name (default: EasyClick Cloud Test)] third-party keyboard. If the option is missing, restart the device")
        logw("After enabling, tap an input field and show the [packaged app name (default: EasyClick Cloud Test, goose-yellow background)] keyboard before using these functions")
        return
    }
    let result = imeApi.input("sample data")
    logd("Input field data: " + result)
}

main();
```

## imeApi.paste Paste String

* Paste a string — copy to clipboard, then insert into the input field
* Available in EC iOS standalone 3.15.0+
* @param content string; if empty, uses clipboard data directly
* @returns `{string}` empty on failure; non-empty on success (returns the inserted data)

```javascript showLineNumbers
function main() {
    var ok = imeApi.isOk();
    if (!ok) {
        logw("IME unavailable. Go to Settings → General → Keyboard and enable the [packaged app name (default: EasyClick Cloud Test)] third-party keyboard. If the option is missing, restart the device")
        logw("After enabling, tap an input field and show the [packaged app name (default: EasyClick Cloud Test, goose-yellow background)] keyboard before using these functions")
        return
    }
    let result = imeApi.paste("paste data")
    logd("Pasted data in input field: " + result)
}

main();
```

## imeApi.pressDel Delete Input Field Text

* Delete text in the input field
* Available in EC iOS standalone 3.15.0+
* @returns `{string}` empty if the field has no data; non-empty if data remains

```javascript showLineNumbers
function main() {
    var ok = imeApi.isOk();
    if (!ok) {
        logw("IME unavailable. Go to Settings → General → Keyboard and enable the [packaged app name (default: EasyClick Cloud Test)] third-party keyboard. If the option is missing, restart the device")
        logw("After enabling, tap an input field and show the [packaged app name (default: EasyClick Cloud Test, goose-yellow background)] keyboard before using these functions")
        return
    }
    let result = imeApi.pressDel()
    logd("Remaining data in input field: " + result)
}

main();
```

## imeApi.pressEnter Enter Key

* Press Enter
* Available in EC iOS standalone 3.15.0+
* @returns `{boolean}` `true` on success, `false` on failure

```javascript showLineNumbers
function main() {
    var ok = imeApi.isOk();
    if (!ok) {
        logw("IME unavailable. Go to Settings → General → Keyboard and enable the [packaged app name (default: EasyClick Cloud Test)] third-party keyboard. If the option is missing, restart the device")
        logw("After enabling, tap an input field and show the [packaged app name (default: EasyClick Cloud Test, goose-yellow background)] keyboard before using these functions")
        return
    }
    let result = imeApi.pressEnter()
    logd("pressEnter: " + result)
}

main();
```

## imeApi.dismiss Hide Keyboard

* Hide the keyboard
* Available in EC iOS standalone 3.15.0+
* @returns `{boolean}` `true` on success, `false` on failure

```javascript showLineNumbers
function main() {
    var ok = imeApi.isOk();
    if (!ok) {
        logw("IME unavailable. Go to Settings → General → Keyboard and enable the [packaged app name (default: EasyClick Cloud Test)] third-party keyboard. If the option is missing, restart the device")
        logw("After enabling, tap an input field and show the [packaged app name (default: EasyClick Cloud Test, goose-yellow background)] keyboard before using these functions")
        return
    }
    let result = imeApi.dismiss()
    logd("dismiss: " + result)

}

main();
```

## imeApi.copyToClipboard Copy Input Field Data to Clipboard

* Copy input field data to the clipboard
* Available in EC iOS standalone 3.15.0+
* @returns `{string}` empty if the field has no data; non-empty if data was copied to the clipboard

```javascript showLineNumbers
function main() {
    var ok = imeApi.isOk();
    if (!ok) {
        logw("IME unavailable. Go to Settings → General → Keyboard and enable the [packaged app name (default: EasyClick Cloud Test)] third-party keyboard. If the option is missing, restart the device")
        logw("After enabling, tap an input field and show the [packaged app name (default: EasyClick Cloud Test, goose-yellow background)] keyboard before using these functions")
        return
    }
    let result = imeApi.copyToClipboard()
    logd("copyToClipboard data: " + result)
}

main();
```

## imeApi.changeKeyboard Switch to Another Keyboard

* Switch to another keyboard
* Waits 2 seconds after returning before switching
* Available in EC iOS standalone 3.15.0+
* @returns `{boolean}` `true` on success, `false` on failure

```javascript showLineNumbers
function main() {
    var ok = imeApi.isOk();
    if (!ok) {
        logw("IME unavailable. Go to Settings → General → Keyboard and enable the [packaged app name (default: EasyClick Cloud Test)] third-party keyboard. If the option is missing, restart the device")
        logw("After enabling, tap an input field and show the [packaged app name (default: EasyClick Cloud Test, goose-yellow background)] keyboard before using these functions")
        return
    }
    let result = imeApi.changeKeyboard()
    logd("changeKeyboard data: " + result)
}

main();
```

## imeApi.removeAllContent Clear Input Field

* Clear all content in the input field
* Available in EC iOS standalone 3.15.0+
* @returns `{boolean}` `true` on success, `false` on failure

```javascript showLineNumbers
function main() {
    var ok = imeApi.isOk();
    if (!ok) {
        logw("IME unavailable. Go to Settings → General → Keyboard and enable the [packaged app name (default: EasyClick Cloud Test)] third-party keyboard. If the option is missing, restart the device")
        logw("After enabling, tap an input field and show the [packaged app name (default: EasyClick Cloud Test, goose-yellow background)] keyboard before using these functions")
        return
    }
    let result = imeApi.removeAllContent()
    logd("removeAllContent : " + result)
}

main();
```

## imeApi.getClipboard Read Clipboard Data

* Read clipboard data
* Available in EC iOS standalone 3.15.0+
* @returns `{string}` clipboard data

```javascript showLineNumbers
function main() {
    var ok = imeApi.isOk();
    if (!ok) {
        logw("IME unavailable. Go to Settings → General → Keyboard and enable the [packaged app name (default: EasyClick Cloud Test)] third-party keyboard. If the option is missing, restart the device")
        logw("After enabling, tap an input field and show the [packaged app name (default: EasyClick Cloud Test, goose-yellow background)] keyboard before using these functions")
        return
    }
    let result = imeApi.getClipboard()
    logd("getClipboard data : " + result)
}

main();
```

## imeApi.setClipboard Set Clipboard Data

* Set clipboard data
* Available in EC iOS standalone 3.15.0+
* @param content string
* @param type `1` = plain string, `2` = URL data
* @returns `{boolean}` `true` on success, `false` on failure

```javascript showLineNumbers
function main() {
    var ok = imeApi.isOk();
    if (!ok) {
        logw("IME unavailable. Go to Settings → General → Keyboard and enable the [packaged app name (default: EasyClick Cloud Test)] third-party keyboard. If the option is missing, restart the device")
        logw("After enabling, tap an input field and show the [packaged app name (default: EasyClick Cloud Test, goose-yellow background)] keyboard before using these functions")
        return
    }
    let result = imeApi.setClipboard("clipboard data","1")
    logd("setClipboard : " + result)
}

main();
```

## imeApi.openUrl Open URL

* Open a URL
* Available in EC iOS standalone 3.15.0+
* @param url URL address, e.g. `http://baidu.com`
* @returns `{boolean}` `true` on success, `false` on failure

```javascript showLineNumbers
function main() {
    var ok = imeApi.isOk();
    if (!ok) {
        logw("IME unavailable. Go to Settings → General → Keyboard and enable the [packaged app name (default: EasyClick Cloud Test)] third-party keyboard. If the option is missing, restart the device")
        logw("After enabling, tap an input field and show the [packaged app name (default: EasyClick Cloud Test, goose-yellow background)] keyboard before using these functions")
        return
    }
    let result = imeApi.openUrl("http://baidu.com")
    logd("openUrl : " + result)
}

main();
```

## imeApi.getText Get Input Field Data

* Get input field data
* Available in EC iOS standalone 3.15.0+
* @returns `{string}` empty if no data or not retrieved; non-empty if data was retrieved

```javascript showLineNumbers
function main() {
    var ok = imeApi.isOk();
    if (!ok) {
        logw("IME unavailable. Go to Settings → General → Keyboard and enable the [packaged app name (default: EasyClick Cloud Test)] third-party keyboard. If the option is missing, restart the device")
        logw("After enabling, tap an input field and show the [packaged app name (default: EasyClick Cloud Test, goose-yellow background)] keyboard before using these functions")
        return
    }
    let result = imeApi.getText()
    logd("getText : " + result)
}

main();
```
