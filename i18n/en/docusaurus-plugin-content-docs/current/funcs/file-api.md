---
title: File Functions
description: EasyClick automation scripts — Android no root file functions
keywords:
 - EasyClick automation scripts Android no root file functions
 - file
 - param
 - path
 - readFile
 - return
 - deleteLine
 - listDir
 - writeFile
 - create
 - mkdirs
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview

- File module functions handle file operations
- The file module uses the `file` prefix, e.g. `file.readFile()`

## file.readFile Read as String

* Read a file as a string
* @param path File path
* @return string

```javascript showLineNumbers
function main() {
    var data = file.readFile("/sdcard/test.txt");
    toast(data);
}

main();
```

## file.deleteLine Delete a Line

* Delete a line by number or by matching content
* Note: if deletion fails, use readAllLines, find the line index, delete it, then save
* @param path File path
* @param line Line number; -1 disables this condition
* @param contains Delete lines containing this string; null disables this condition
*
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    // Delete lines containing "time"
    let r = file.deleteLine("/sdcard/a.txt", -1, "time");
    logd("r " + r);
    // Delete line 3
    r = file.deleteLine("/sdcard/a.txt", 3, null);
    logd("r " + r);
}

main();
```

## file.listDir List Files

* List all files under a path
* @param path Path
* @return array of path strings

```javascript showLineNumbers
function main() {
    var data = file.listDir("/sdcard/");
    for (var i = 0; i < data.length; i++) {
        logd("path " + data[i]);
    }

}

main();
```

## file.writeFile Write File

* Write a string to a file
* @param data String data
* @param path File path

```javascript showLineNumbers
function main() {
    var data = "Test";
    file.writeFile(data, "/sdcard/test.txt");
}

main();
```

## file.create Create File

* Create a file
* @param path File path
* @return boolean true if created successfully

```javascript showLineNumbers
function main() {
    var create = file.create("/sdcard/test.txt");
    toast(create);
}

main();
```

## file.mkdirs Create Directory

* Create a directory
* @param path Path
* @return boolean true on success, false on failure

```javascript showLineNumbers
function main() {
    var t = file.mkdirs("/sdcard/testdir/");
    toast(t);
}

main();
```

## file.readAssets Read from assets

* Read data from the APK assets folder as a string
* @param path Path under assets, e.g. `data/a.txt`
* @return string

```javascript showLineNumbers
function main() {
    var data = file.readAssets("data/test.txt");
    toast(data);
}

main();
```

## file.deleteAllFile Delete

* Delete a file or folder
* @param path File or folder path

```javascript showLineNumbers
function main() {
    // Delete file
    file.deleteAllFile("/sdcard/test.txt");
    // Delete folder
    file.deleteAllFile("/sdcard/testdir/");
}

main();
```

## file.appendLine Append Line

* Append one line to a file
* @param data Line data
* @param path File path
* @return boolean true on success, false on failure

```javascript showLineNumbers
function main() {
    var data = "sss";
    var t = file.appendLine(data, "/sdcard/test.txt");
    toast(t);
}

main();
```

## file.readLine Read One Line

* Read one line; returns empty if the line number is invalid
* @param path Path
* @param lineNo Line number
* @return string

```javascript showLineNumbers
function main() {
    var t = file.readLine("/sdcard/test.txt", 1);
    toast(t);
}

main();
```

## file.readAllLines Read All Lines

* Read all lines
* @param path Path
* @return string array|null

```javascript showLineNumbers
function main() {
    var t = file.readAllLines("/sdcard/test.txt");
    toast(t);
}

main();
```

## file.exists Check Existence

* Check whether a file or folder exists
* @param path Path
* @return boolean true if it exists, false otherwise

```javascript showLineNumbers
function main() {
    var t = file.exists("/sdcard/testdir/");
    toast(t);
}

main();
```

## file.copy Copy File

* Copy a file
*
* @param src Source path
* @param dest Destination path
* @return boolean true on success

```javascript showLineNumbers
function main() {
    var t = file.copy("/sdcard/a.png", "/sdcard/b.png");
    toast(t);
}

main();
```
