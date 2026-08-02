---
title: Control Center Screen Mirroring Module
description: EasyClick automation scripts — Android no root device functions
keywords:
 - EasyClick automation scripts Android no root control center screen mirroring functions
 - centerApi
 - getFileData
 - EC
 - getCenterTaskInfo
 - 9.29.0
 - return
 - addFileData
 - deleteFile
 - insertFileData
 - popFileData
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview

- The control center screen mirroring module works with the EC control center screen mirroring system
- Installation and usage docs: [Control Center Screen Mirroring Manual](/docs/centerscreen/openscreen)
- The module uses the `centerApi` prefix, e.g. `centerApi.getFileData()`

## centerApi.getCenterTaskInfo Get Control Center Task Info

* Get task parameters sent from the control center
* When the control center starts a script, it can pass parameters; use this function to read them in the script
* Requires EC Android 9.29.0+
* Note: parameter configuration is required. Read order: per-device config first; if empty, fall back to global config
* If the returned object contains a key like `__from_global__`, the value came from global parameters
* @return `{json}` object

```javascript showLineNumbers
function main() {
    while (true) {
        logd("---> " + new Date())
        sleep(2000);
        let info = centerApi.getCenterTaskInfo()
        logd("info -> " + JSON.stringify(info))
        if (info) {
            logd("test param => " + info['valueJson']['test']);
        }
        sleep(2000);
    }
}

main()
```

## centerApi.getFileData Read Data File Content

* Read the contents of a data file
* Requires EC 9.29.0+
* @param name File name — the control center data file name
* @return `{json}` JSON object

```javascript showLineNumbers
function main() {
    let data = centerApi.getFileData("testfile")
    logd(JSON.stringify(data))
    if (!data) {
        logd("No data returned");
    } else {
        if (data["code"] != 0) {
            logd("Error: " + data["msg"]);
        } else {
            logd("Data: " + data["data"]);
        }
    }
}

main();
```

## centerApi.addFileData Add Data File

* Add a data file
* Requires EC 9.29.0+
*
* @param name File name — the control center data file name
* @param content File content
* @param rewrite Allow overwriting existing file: 1 yes, 2 no. If 2 and the file exists, an error is returned
* @param append Append mode: 1 append content, 2 do not append
* @return `{json}` JSON object

```javascript showLineNumbers
function main() {
    let data = centerApi.addFileData("testfile", "\n123", "1", "1")
    logd(JSON.stringify(data))
    if (!data) {
        logd("No data returned");
    } else {
        if (data["code"] != 0) {
            logd("Error: " + data["msg"]);
        } else {
            logd("Success");
        }
    }
}

main();
```

## centerApi.deleteFile Delete Data File

* Delete a data file
* Requires EC 9.29.0+
* @param name File name — the control center data file name
* @return `{json}` JSON object

```javascript showLineNumbers
function main() {
    let data = centerApi.deleteFile("testfile")
    logd(JSON.stringify(data))
    if (!data) {
        logd("No data returned");
    } else {
        if (data["code"] != 0) {
            logd("Error: " + data["msg"]);
        } else {
            logd("Success");
        }
    }
}

main();
```

## centerApi.insertFileData Insert Data

* Insert data
* Requires EC 9.29.0+
* @param name File name — the control center data file name
* @param content Content to insert
* @param create Create file if missing: 1 yes, 2 no. If 2 and the file does not exist, an error is returned
* @param append Append mode: 1 append content, 2 do not append
* @return `{json}` JSON object

```javascript showLineNumbers
function main() {
    let data = centerApi.insertFileData("testfile", "123", "1", "2")
    logd(JSON.stringify(data))
    if (!data) {
        logd("No data returned");
    } else {
        if (data["code"] != 0) {
            logd("Error: " + data["msg"]);
        } else {
            logd("Success");
        }
    }
}

main();
```

## centerApi.popFileData Pop Data

* Pop data from a file
* Requires EC 9.29.0+
* @param name File name — the control center data file name
* @param popType Retrieval mode: 1 from head, 2 from tail, 3 random
* @return `{json}` JSON object

```javascript showLineNumbers
function main() {
    let data = centerApi.popFileData("testfile", "3")
    logd(JSON.stringify(data))
    if (!data) {
        logd("No data returned");
    } else {
        if (data["code"] != 0) {
            logd("Error: " + data["msg"]);
        } else {
            logd(data["data"]);
        }
    }
}

main();
```

## centerApi.removeOneLineData Remove One Line

* Remove one line of data
* Requires EC 9.29.0+
* @param name File name — the control center data file name
* @param content Content to remove
* @return `{json}` JSON object

```javascript showLineNumbers
function main() {
    let data = centerApi.removeOneLineData("testfile", "2")
    logd(JSON.stringify(data))
    if (!data) {
        logd("No data returned");
    } else {
        if (data["code"] != 0) {
            logd("Error: " + data["msg"]);
        } else {
            logd("Success");
        }
    }
}

main();
```

## centerApi.appendOneLineData Append One Line

* Append one line of data
* Requires EC 9.29.0+
*
* @param name File name — the control center data file name
* @param content Content to append
* @param appendType Append position: 1 head, 2 tail
* @return `{json}` JSON object

```javascript showLineNumbers
function main() {
    let data = centerApi.appendOneLineData("testfile", "2", "2")
    logd(JSON.stringify(data))
    if (!data) {
        logd("No data returned");
    } else {
        if (data["code"] != 0) {
            logd("Error: " + data["msg"]);
        } else {
            logd("Success");
        }
    }
}

main();
```
