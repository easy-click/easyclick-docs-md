---
title: 常见问题
description: EasyClick 自动化脚本 鸿蒙Next  常见问题
keywords:
  - EasyClick 自动化脚本 鸿蒙Next  常见问题
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
  - 手机自动化
  - 自动化测试
  - 脚本开发
  - 安卓自动化
  - iOS自动化
  - 鸿蒙Next
---



## 桥接内存CPU异常
- 抓取桥接内存占用：
- http://127.0.0.1:8026/debug/pprof/heap
- 内存不正常的时候使用，这样会下载一个内存文件,发给沙书记分析
- 抓取桥接CPU占用：
- http://127.0.0.1:8026/debug/pprof/profile
- CPU不正常的时候使用，这样会下载一个CPU文件,发给沙书记分析
- 手动回收内存:
- http://127.0.0.1:8026/devapi/gc



## 脚本运行 显示执行异常：com.js.main

- 检查中控，桥接程序存放的路径，禁止使用中文，空格等特殊字符，只能使用字母或者数字，Windows的毛病
- 检查脚本文件、路径是否包含中文



## 中控闪退

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
