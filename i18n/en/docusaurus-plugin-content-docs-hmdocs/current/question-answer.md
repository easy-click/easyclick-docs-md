---
title: FAQ
description: EasyClick automation scripts — HarmonyOS Next — FAQ
keywords:
 - EasyClick automation scripts HarmonyOS Next FAQ
 - CPU
 - js
 - http
 - 127.0.0.1
 - com
 - main
 - JSON
 - stringify
 - debug
 - pprof
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---



## Bridge Memory/CPU Anomaly
- Capture bridge memory usage:
- http://127.0.0.1:8026/debug/pprof/heap
- Use when memory is abnormal; this downloads a memory file to send to Secretary Sha for analysis
- Capture bridge CPU usage:
- http://127.0.0.1:8026/debug/pprof/profile
- Use when CPU is abnormal; this downloads a CPU file to send to Secretary Sha for analysis
- Manually reclaim memory:
- http://127.0.0.1:8026/devapi/gc



## Script Run Shows Execution Error: com.js.main

- Check the storage path of the control center and bridge program. Chinese characters, spaces, and other special characters are prohibited — only letters or numbers are allowed (a Windows limitation)
- Check whether script files and paths contain Chinese characters



## Control Center Crashes

- Missing VC runtime libraries — unable to copy OpenCV library files
- Solution: download the VC library installer from EC's Baidu Netdisk. The file is named **WindowsVC安装包** (Windows VC redistributable installer); download the exe and install it
- If that still does not work, download and install **vcyunxingkuheji.rar**



## JSON.stringify Memory Overflow
- If you see the following error, check whether the object being converted contains Java strings. Convert the original string to a JS string.
 For example: `s = s + ""`

 STACK_TRACE=java.lang.StackOverflowError: stack size 1039KB
 at java.lang.reflect.Method.invoke(Native Method)
 at org.mozilla.javascript.MemberBox.invoke(Unknown Source:4)
 at org.mozilla.javascript.JavaMembers.get(Unknown Source:58)
 at org.mozilla.javascript.NativeJavaObject.get(Unknown Source:16)
 at org.mozilla.javascript.ScriptableObject.getProperty(Unknown Source:1)
 at org.mozilla.javascript.NativeJSON.str(Unknown Source:7)
 at org.mozilla.javascript.NativeJSON.jo(Unknown Source:63)
 at org.mozilla.javascript.NativeJSON.str(Unknown Source:237)
 at org.mozilla.javascript.NativeJSON.jo(Unknown Source:63)

## Java-JS Plugin or Hybrid Project Crashes
- Confirm JDK is version 1.8
- Confirm the project does not contain Chinese or other special characters
- Confirm the project name and path are consistent
