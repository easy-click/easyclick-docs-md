---
title: File Functions
description: EasyClick automation scripts — iOS no jailbreak — file functions
keywords:
 - EasyClick automation scripts — iOS no jailbreak — file functions
 - file
 - return
 - getSandBoxFilePath
 - readFile
 - getInternalDir
 - getSandBoxDir
 - deleteLine
 - param
 - documents
 - deleteLineEx
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview

- File module functions are related to file operations
- The file module uses the `file` prefix, e.g. `file.readFile()`
- To get a file path inside the device sandbox, use `file.getSandBoxFilePath()`

## file.getInternalDir Get Internal Storage Path

* Get internal storage path
* @param type — `documents`, `library`, `temp`, or `libraryCaches`; the `documents` folder can be exported via iTools/i4
* @return string

```javascript showLineNumbers
function main() {
    var data = file.getInternalDir("documents");
    logd(data);
}

main();
```

## file.getSandBoxDir Get Sandbox Directory Path

* Get the current device sandbox directory path
* @return string

```javascript showLineNumbers
function main() {
    var data = file.getSandBoxDir();
    logd(data);
}

main();
```

## file.getSandBoxFilePath Get Sandbox File Path

* Build a file path under the sandbox
* @return string

```javascript showLineNumbers
function main() {
    var data = file.getSandBoxFilePath("a.txt");
    logd(data);
}

main();
```

## file.readFile Read as String

* Read a file as a string
* @param path file path
* @return string

```javascript showLineNumbers
function main() {
    let p = file.getSandBoxFilePath("a.txt");
    var data = file.readFile(p);
    logd(data);
}

main();
```

## file.deleteLine Delete a Line from File

* Delete a specific line or lines matching a substring
* Runtime: no restrictions
* @param path file path
* @param line line number; `-1` disables this condition
* @param contains delete lines containing this string; `null` disables this condition
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    let p = file.getSandBoxFilePath("a.txt");
    // Delete lines containing "time"
    let r = file.deleteLine(p, -1, "time");
    logd("r " + r);
    // Delete line 3
    r = file.deleteLine(p, 3, null);
    logd("r " + r);
}

main();
```

## file.deleteLineEx Delete a Line from File (Extended)

* Delete a specific line or lines matching a substring
* Suitable for large files
* Requires EC 4.7.3+
* @param path file path
* @param line line number; `-1` disables this condition
* @param contains delete lines containing this string; `null` disables this condition
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    let p = file.getSandBoxFilePath("a.txt");
    // Delete lines containing "time"
    let r = file.deleteLineEx(p, -1, "time");
    logd("r " + r);
    // Delete line 3
    r = file.deleteLineEx(p, 3, null);
    logd("r " + r);
}

main();
```

## file.listDir List All Files

* List all files under a path (recursive)
* @param path directory path
* @return array of path strings

```javascript showLineNumbers
function main() {
    let p = file.getSandBoxDir();
    var data = file.listDir(p);
    for (var i = 0; i < data.length; i++) {
        logd("path " + data[i]);
    }
}

main();
```

## file.listDir2 List Files (Recursive Option)

* List files and folders under a path
* Requires EC standalone 4.1.0+
* @param path directory path
* @param recursion `true` to recurse into subfolders, `false` to list only the immediate directory
* @return array of path strings

```javascript showLineNumbers
function main() {
    let p = file.getSandBoxDir();
    var data = file.listDir2(p, false);
    for (var i = 0; i < data.length; i++) {
        logd("path " + data[i]);
    }
}

main();
```

## file.writeFile Write File

* Write a string to a file
* @param data string data
* @param path file path

```javascript showLineNumbers
function main() {
    var data = "Test";
    let p = file.getSandBoxFilePath("a.txt");
    file.writeFile(data, p);
}

main();
```

## file.create Create File

* Create a file
* @param path file path
* @return boolean — true if created successfully

```javascript showLineNumbers
function main() {
    let p = file.getSandBoxFilePath("a.txt");
    var create = file.create(p);
    logd(create);
}

main();
```

## file.mkdirs Create Directory

* Create a directory
* @param path directory path
* @return boolean — true on success, false on failure

```javascript showLineNumbers
function main() {
    let p = file.getSandBoxDir();
    p = p + "/ad"
    var t = file.mkdirs(p);
    logd(t);
}

main();
```

## file.deleteAllFile Delete

* Delete a file or directory
* @param path file or directory path

```javascript showLineNumbers
function main() {
    // Delete file
    let filePath = file.getSandBoxFilePath("a.txt");
    file.deleteAllFile(filePath);
    // Delete folder
    let dirPath = file.getSandBoxFilePath("a/");
    file.deleteAllFile(dirPath);
}

main();
```

## file.appendLine Append Line

* Append a line to a file
* @param data line data
* @param path file path
* @return boolean — true on success, false on failure

```javascript showLineNumbers
function main() {
    var data = "sss";
    let p = file.getSandBoxFilePath("a.txt");
    var t = file.appendLine(data, p);
    logd(t);
}

main();
```

## file.appendLineEx Append Line (Extended)

* Append a line to a file
* Suitable for large files
* Requires EC 4.7.3+
* @param data line data
* @param path file path
* @return boolean — true on success, false on failure

```javascript showLineNumbers
function main() {
    var data = "sss";
    let p = file.getSandBoxFilePath("a.txt");
    var t = file.appendLineEx(data, p);
    logd(t);
}

main();
```

## file.readLine Read One Line

* Read one line; returns empty if the line number is invalid
* @param path file path
* @param lineNo line number (1-based)
* @return string

```javascript showLineNumbers
function main() {
    let p = file.getSandBoxFilePath("a.txt");
    var t = file.readLine(p, 1);
    logd(t);
}

main();
```

## file.readLineEx Read One Line (Extended)

* Read one line; returns empty if the line number is invalid
* Suitable for large files
* Requires EC 4.7.3+
* @param path file path
* @param lineNo line number (1-based)
* @return string

```javascript showLineNumbers
function main() {
    let p = file.getSandBoxFilePath("a.txt");
    var t = file.readLineEx(p, 1);
    logd(t);
}

main();
```

## file.readAllLines Read All Lines

* Read all lines
* @param path file path
* @return string array or null

```javascript showLineNumbers
function main() {
    let p = file.getSandBoxFilePath("a.txt");
    var t = file.readAllLines(p);
    logd(t);
}

main();
```

## file.exists Check Existence

* Whether a file or directory exists
* @param path path
* @return boolean — true if exists, false otherwise

```javascript showLineNumbers
function main() {
    let p = file.getSandBoxFilePath("a.txt");
    var t = file.exists(p);
    logd(t);
}

main();
```

## file.copy Copy File

* Copy a file
* @param src source file path
* @param dest destination file path
* @return boolean — true on success

```javascript showLineNumbers
function main() {
    let p1 = file.getSandBoxFilePath("a1.txt");
    let p2 = file.getSandBoxFilePath("a2.txt");
    var t = file.copy(p1, p2);
    logd(t);
}

main();
```
