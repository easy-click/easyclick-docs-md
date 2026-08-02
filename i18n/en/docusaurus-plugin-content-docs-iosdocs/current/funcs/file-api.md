---
title: File Functions
description: EasyClick automation scripts — iOS no jailbreak file functions
keywords:
 - EasyClick automation scripts iOS no jailbreak file functions
 - file
 - getSandBoxDir
 - getSandBoxFilePath
 - readFile
 - return
 - param
 - deleteLine
 - path
 - listDir
 - writeFile
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
- Note: The control center runs on the computer, so `file` operations use computer paths. Each device has its own sandbox folder — use `file.getSandBoxDir()` to get it
- To get a file path inside the device sandbox folder, use `file.getSandBoxFilePath()`

## file.getSandBoxDir Get Sandbox Folder Path

* Get the sandbox folder path for the current device
* @return string

```javascript showLineNumbers
function main() {
    var data = file.getSandBoxDir();
    logd(data);
}

main();
```

## file.getSandBoxFilePath Get File Path in Sandbox

* Build a file path that includes the sandbox directory
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
    var data = file.readFile("D:/test.txt");
    logd(data);
}

main();
```

## file.deleteLine Delete a Line from a File

* Delete a specific line from a file, or delete lines matching a substring
* Runtime: no restrictions
* @param path file path
* @param line line number; use -1 to disable this condition
* @param contains delete lines containing this string; use null to disable this condition
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    // Delete lines containing the string "时间"
    let r = file.deleteLine("D:/a.txt", -1, "时间");
    logd("r " + r);
    // Delete line 3
    r = file.deleteLine("D:/a.txt", 3, null);
    logd("r " + r);
}

main();
```

## file.listDir List All Files

* List all files under a path
* @param path directory path
* @return array of path strings

```javascript showLineNumbers
function main() {
    var data = file.listDir("D:/");
    for (var i = 0; i < data.length; i++) {
        logd("path " + data[i]);
    }

}

main();
```

## file.writeFile Write to File

* Write a string to a file
* @param data string data
* @param path file path

```javascript showLineNumbers
function main() {
    var data = "Test";
    file.writeFile(data, "D:/test.txt");
}

main();
```

## file.create Create File

* Create a file
* @param path file path
* @return boolean true if creation succeeded

```javascript showLineNumbers
function main() {
    var create = file.create("D:/test.txt");
    logd(create);
}

main();
```

## file.mkdirs Create Directory

* Create a directory
* @param path directory path
* @return boolean true on success, false on failure

```javascript showLineNumbers
function main() {
    var t = file.mkdirs("D:/testdir/");
    logd(t);
}

main();
```

## file.deleteAllFile Delete

* Delete a file or directory
* @param path file or directory path

```javascript showLineNumbers
function main() {
    // Delete a file
    file.deleteAllFile("D:/test.txt");
    // Delete a directory
    file.deleteAllFile("D:/test/");
}

main();
```

## file.appendLine Append String

* Append a line to a file
* @param data line data
* @param path file path
* @return boolean true on success, false on failure

```javascript showLineNumbers
function main() {
    var data = "sss";
    var t = file.appendLine(data, "D:/test.txt");
    logd(t);
}

main();
```

## file.readLine Read One Line

* Read one line; returns empty if the line number is invalid
* @param path file path
* @param lineNo line number
* @return string one line of text

```javascript showLineNumbers
function main() {
    var t = file.readLine("D:/test.txt", 1);
    logd(t);
}

main();
```

## file.readAllLines Read All Lines

* Read all lines from a file
* @param path file path
* @return string array or null

```javascript showLineNumbers
function main() {
    var t = file.readAllLines("D:/test.txt");
    logd(t);
}

main();
```

## file.exists Check Existence

* Check whether a file or directory exists
* @param path path
* @return boolean true if it exists, false otherwise

```javascript showLineNumbers
function main() {
    var t = file.exists("D:/testdir/");
    logd(t);
}

main();
```

## file.copy Copy File

* Copy a file
*
* @param src source file path
* @param dest destination file path
* @return boolean true on success

```javascript showLineNumbers
function main() {
    var t = file.copy("D:/a.png", "D:/b.png");
    logd(t);
}

main();
```

## file.readExcelRow Read One Excel Row

* Read one row from an Excel file
* Supported EC iOS 4.0.0+
* @param path Excel file path
* @param sheetIndex sheet index, starting from 0
* @param row row number, starting from 0
* @return JSON JSON object

```javascript showLineNumbers

function testexcel() {
    logd(file.getSandBoxDir())

    let data = file.readExcelAllRow("工作簿1.xlsx", 0)
    logd(JSON.stringify(data))
    let data2 = file.readExcelRow("工作簿1.xlsx", 0, 1)
    logd(JSON.stringify(data2))

}

testexcel();
```

## file.readExcelAllRow Read All Excel Data

* Read all data from an Excel file
* The first row is treated as the header; remaining rows are data. Returns assembled map data like ```[{"名称":2,"性别":222}]```
* Supported EC iOS 4.0.0+
* @param path Excel file path
* @param sheetIndex sheet index, starting from 0
* @return JSON JSON object

```javascript showLineNumbers

function testexcel() {
 logd(file.getSandBoxDir())

 let data = file.readExcelAllRow("工作簿1.xlsx", 0)
 logd(JSON.stringify(data))
 let data2 = file.readExcelRow("工作簿1.xlsx", 0, 1)
 logd(JSON.stringify(data2))

}

testexcel();
```
