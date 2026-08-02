---
title: 常见问题
description: EasyClick 自动化脚本 iOS免越狱  常见问题
keywords:
  - EasyClick 自动化脚本 iOS免越狱  常见问题
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
  - 手机自动化
  - 自动化测试
  - 脚本开发
  - 安卓自动化
  - iOS自动化
  - 鸿蒙Next
---


## 其他常见问题查阅 
- http://bbs.laoleng.vip

## 桥接内存CPU异常
- 抓取桥接内存占用：
- http://127.0.0.1:8020/debug/pprof/heap
- 内存不正常的时候使用，这样会下载一个内存文件,发给沙书记分析
- 抓取桥接CPU占用：
- http://127.0.0.1:8020/debug/pprof/profile
- CPU不正常的时候使用，这样会下载一个CPU文件,发给沙书记分析
- 手动回收内存:
- http://127.0.0.1:8020/devapi/gc

## 桥接链接设备数量查看
- 查看桥接设备数量:
- http://127.0.0.1:8020/devapi/devicenum 

## iPhone重启后无法启动代理程序

- 由于iPhone需要挂载开发者镜像文件，请到网盘下载DeveloperDiskImage.zip文件
- 解压zip文件到桥接程序的config文件夹，之前这个文件下有就DeveloperDiskImage，按照版本规则放置即可
- 如果没有你的手机型号，可以自己按照这个链接提取developer镜像：https://ighibli.github.io/2017/03/28/Could-not-locate-device-support-files/
- https://github.com/haikieu/xcode-developer-disk-image-all-platforms/tree/master/DiskImages/iPhoneOS.platform/DeviceSupport
- 如果还是没有镜像，可以尝试将最靠近`你的iPhone版本`的`镜像版本文件夹名称修改与手机版本一致 `

## 越狱机器如何安装ipa
- 经过群友测试，先安装AppSync插件，然后在安装代理ipa可以成功
- 成功的系统是 13.4.1,美版信号机

## 脚本运行 显示执行异常：com.js.main

- 检查中控，桥接程序存放的路径，禁止使用中文，空格等特殊字符，只能使用字母或者数字，Windows的毛病
- 检查脚本文件、路径是否包含中文

## 获取屏幕高度宽度不准确

- 历史遗留原因，可以使用device.getDeviceInfo进行获取实时的屏幕高度和宽度

## 3.2.0+ 横竖屏坐标系问题

- 坐标系认知： IDEA截图是横屏的 就是横坐标系， IDEA截图是竖屏的 就代表是竖坐标系
- 横竖屏切换：根据实际情况业务进行判断，
  - 比如从竖换到横，调用adjustScreenOrientation(2) ，执行点击、长按等动作的时候会自动转换横坐标系
  - 比如从横换到竖，调用adjustScreenOrientation(1) ，执行点击、长按等动作的时候会自动转换竖坐标系
- 支持情况：
  - 图色只支持竖屏
  - 节点支持横屏和竖屏，所以在图色、节点混用，或者节点横竖屏的时候，需要调用adjustScreenOrientation函数设置坐标系
- 如果上诉情况不太理解，尽量都固定为竖屏或者横屏，避免切换问题



## 无法复制opencv问题

- <img src="/iosimg/969EAF4C4E9F0A034561E1DAC988E7F7.png" alt="969EAF4C4E9F0A034561E1DAC988E7F7" style={{zoom:'50%'}} />
- 缺少VC 库  无法复制opencv库文件
- 解决方法是去EC的百度 网盘下载 VC类库安装，网盘文件名称是 WindowsVC安装包，下载exe安装
- 如果还不行，就下载vcyunxingkuheji.rar这个安装



## JSON.stringify 内存溢出
- 如果有以下错误，请检查一下要转换的对象中是否有java的string，解决方法将原来的string转为js的字符串
 例如 s= s +""  这样既可
   
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
        
## java-js 插件或者混合项目运行闪退
- 确认jdk是1.8版本的
- 确认项目不包含中文等特殊字符
- 确认项目的名称和路径是一致

## 减少代理IPA包大小
- 9.0+ 代理ipa包自带了ocr模型文件，如果不需要可以按照下面操作
- 使用压缩文件打开ipa文件，例如360压缩打开ipa文件,(ipa本身就是压缩文件)
- 依次进入PlugIns-EasyClick.xctest-文件夹
- 删除 .bin，.param，.onnx结尾的文件
- 删除keys.txt和ppocrv5_mobile_labels.txt文件即可
- 也可以解压ipa后，删除以上文件，重新压缩为ipa文件
