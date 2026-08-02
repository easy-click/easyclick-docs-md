---
title: FAQ
description: EasyClick automation scripts — iOS, no jailbreak — FAQ
keywords:
 - EasyClick automation scripts iOS no jailbreak FAQ
 - CPU
 - http
 - 127.0.0.1
 - iPhone
 - js
 - debug
 - pprof
 - devapi
 - DeveloperDiskImage
 - zip
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---


## Other FAQ Resources
- http://bbs.laoleng.vip

## Bridge Memory/CPU Anomaly
- Capture bridge memory usage:
- http://127.0.0.1:8020/debug/pprof/heap
- Use when memory is abnormal; this downloads a memory file to send to Secretary Sha for analysis.
- Capture bridge CPU usage:
- http://127.0.0.1:8020/debug/pprof/profile
- Use when CPU is abnormal; this downloads a CPU file to send to Secretary Sha for analysis.
- Manually reclaim memory:
- http://127.0.0.1:8020/devapi/gc

## View Connected Device Count
- View bridge device count:
- http://127.0.0.1:8020/devapi/devicenum

## Agent App Won't Start After iPhone Reboot

- iPhone requires a developer disk image to be mounted. Download DeveloperDiskImage.zip from the cloud drive.
- Extract the zip into the bridge program's config folder. If DeveloperDiskImage already exists there, place files according to the version rules.
- If your phone model is not listed, extract the developer image using this link: https://ighibli.github.io/2017/03/28/Could-not-locate-device-support-files/
- https://github.com/haikieu/xcode-developer-disk-image-all-platforms/tree/master/DiskImages/iPhoneOS.platform/DeviceSupport
- If you still cannot find a matching image, try renaming the folder for the version **closest to your iPhone version** so the folder name matches your phone's iOS version.

## Installing IPA on Jailbroken Devices
- Community testing shows that installing the AppSync tweak first, then installing the agent IPA, works successfully.
- Confirmed working on system 13.4.1, US carrier model.

## Script Run Shows Execution Error: com.js.main

- Check the storage path of the control center and bridge program. Chinese characters, spaces, and other special characters are prohibited — only letters or numbers are allowed (a Windows limitation).
- Check whether script files and paths contain Chinese characters.

## Inaccurate Screen Width and Height

- Due to legacy behavior, use `device.getDeviceInfo` to get real-time screen width and height.

## 3.2.0+ Portrait/Landscape Coordinate System

- Coordinate system basics: if an IDEA screenshot is landscape, the coordinate system is landscape; if the IDEA screenshot is portrait, the coordinate system is portrait.
- Switching orientation: judge based on your business logic.
 - To switch from portrait to landscape, call `adjustScreenOrientation(2)`. Tap, long-press, and similar actions will automatically convert to landscape coordinates.
 - To switch from landscape to portrait, call `adjustScreenOrientation(1)`. Tap, long-press, and similar actions will automatically convert to portrait coordinates.
- Supported scenarios:
 - Image/color matching supports portrait only.
 - Nodes support both portrait and landscape. When mixing image/color with nodes, or when nodes switch orientation, call `adjustScreenOrientation` to set the coordinate system.
- If the above is unclear, keep a fixed portrait or landscape orientation to avoid switching issues.



## Unable to Copy OpenCV

- <img src="/iosimg/969EAF4C4E9F0A034561E1DAC988E7F7.png" alt="969EAF4C4E9F0A034561E1DAC988E7F7" style={{zoom:'50%'}} />
- Missing VC runtime libraries — unable to copy OpenCV library files.
- Solution: download the VC library installer from EC's Baidu Netdisk. The file is named **WindowsVC安装包** (Windows VC redistributable installer); download the exe and install it.
- If that still does not work, download and install **vcyunxingkuheji.rar**.



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
- Confirm JDK is version 1.8.
- Confirm the project does not contain Chinese or other special characters.
- Confirm the project name and path are consistent.

## Reducing Agent IPA Package Size
- Agent IPA 9.0+ includes OCR model files by default. If you do not need them, follow these steps:
- Open the IPA with an archive tool such as 360 Zip (an IPA is itself a zip archive).
- Navigate to PlugIns-EasyClick.xctest- folder.
- Delete files ending in `.bin`, `.param`, and `.onnx`.
- Delete `keys.txt` and `ppocrv5_mobile_labels.txt`.
- Alternatively, extract the IPA, delete the files above, and recompress it as an IPA.
