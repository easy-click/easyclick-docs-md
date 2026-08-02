---
title: Shell Command Functions
description: EasyClick automation scripts — Android no root Shell command functions
keywords:
 - EasyClick automation scripts Android no root Shell command functions
 - shell
 - APP
 - Shell
 - param
 - return
 - installApp
 - root
 - 'true'
 - 'false'
 - uninstallApp
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview

- These functions are available only in proxy mode; they let the user execute shell commands
- The shell module uses the `shell` prefix, e.g. `shell.installApp()`

## shell.installApp Install App

* Install an APK
* @param path File path
* @return true if installation succeeded, false if it failed

```javascript showLineNumbers
function main() {
    var result = shell.installApp("/sdcard/app.apk");
}

main();
```

## shell.uninstallApp Uninstall App

* Uninstall an application
* @param packageName Application package name
* @return true if uninstall succeeded, false if it failed

```javascript showLineNumbers
function main() {
    var result = shell.uninstallApp("com.xx");
}

main();
```

## shell.stopApp Stop App

* Stop a running application
* @param packageName Application package name
* @return true on success, false on failure

```javascript showLineNumbers
function main() {
    var result = shell.stopApp("com.xx");
}

main();
```

## shell.execCommand Execute Shell Command

* Execute a shell command; automatically chooses proxy or root mode
* @param command Command, e.g. install app: `pm install /sdcard/app.apk`
* @return String result returned by the command

```javascript showLineNumbers
function main() {
    var result = shell.execCommand("pm install /sdcard/app.apk");
}

main();
```

## shell.sudo Execute Command Under Root

* Execute a command in root mode; root permission is required
* @param command Command, e.g. install app: `pm install /sdcard/app.apk`
* @return String result returned by the command

```javascript showLineNumbers
function main() {
    var result = shell.sudo("pm install /sdcard/app.apk");
}

main();
```

## shell.su Request Root Grant

* Request root permission
* Supported version (EC 6.0.0+)
* Runtime: unrestricted
* @return `{boolean}` true if root permission is granted

```javascript showLineNumbers
function main() {
    var result = shell.su();
}

main();
```

## shell.execAgentCommand Execute Shell in Proxy Mode

* Execute a shell command; the proxy service must be running
* @param command Command, e.g. install app: `pm install /sdcard/app.apk`
* @return String result returned by the command

```javascript showLineNumbers
function main() {
    var result = shell.execAgentCommand("pm install /sdcard/app.apk");
}

main();
```

## shell.execAgentCommandEx Execute Shell in Proxy Mode

* Execute a shell command and return both stdout and stderr as a JSON array; inspect the array to determine success or failure
* Requires EC 7.6.0+
* @param command Command string
* @return JSON array of shell results

```javascript showLineNumbers
function main() {
    startEnv()

    let cmd = "ls /sdcard/"
    let d = agentEvent.execShellCommandEx(cmd)
    if (d) {
        for (var i = 0; i < d.length; i++) {
            var value = d[i];
            logd(value);

        }
    }
}

main();
```

## shell.addSuBin Add Root Command

* Add a root command
* Runtime: unrestricted
* @param cmd New command
* @return `{boolean}` true

```javascript showLineNumbers
function main() {
    shell.addSuBin("222ddd")
    // After adding, call su() to request root authorization
    var result = shell.su();
}

main();
```

## shell.execShizukuCommand Execute Shizuku Shell Command

* Execute a shell command via Shizuku
* Runtime: proxy mode; the proxy service must be running
* Supported EC 9.9.0+
* @param command Command string
* @return String shell result

```javascript showLineNumbers
function main() {
    let cmd = "ls /sdcard/"
    let d = shell.execShizukuCommand(cmd)
    logd(d);
}

main();
```

## shell.isShizukuOk Check Shizuku Service Status

* Check whether the Shizuku service is healthy
* Supported EC 9.9.0+
* @return bool true if supported, false if not healthy

```javascript showLineNumbers
function main() {

    let d = shell.isShizukuOk()
    logd(d);

}

main();
```
