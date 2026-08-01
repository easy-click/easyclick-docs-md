---
title: 第一个工程
description: EasyClick 自动化脚本 iOS免越狱 教程 第一个工程
keywords:
  - EasyClick 自动化脚本 iOS免越狱 教程  第一个工程
  - ios
  - E5
  - E4
  - img
  - src
  - iosimg
  - module
  - BB
  - AE
  - create
  - EasyClick
  - 手机自动化
  - 自动化测试
  - 脚本开发
  - 安卓自动化
  - iOS自动化
  - 鸿蒙Next
---


- 需提前安装好开发插件,参考--> [安装开发插件](/iosdocs/zh-cn/tools/installdevtools)
- 视频教程--> [**工程创建与项目简介**](https://www.laoleng.vip/docs/free-courses/ec-ios-usb#2%E5%BC%80%E5%8F%91%E6%8F%92%E4%BB%B6-%E5%AE%89%E8%A3%85%E4%B8%8E%E7%AE%80%E4%BB%8B)

## 1、新建工程

- EasyClick的开发采用的是多模块方式
- 先创建并打开一个空文件夹,如 `ProjectIOS`
  <img src="/iosimg/ios-create-module-3.png" alt="image-20220105095538754" />
- 右键选择新建 module/模块<br/>
  <img src="/iosimg/ios-create-module-2.png" alt="image-20220105095622057"  />
- 选择 `EasyClick IOS USB版-脚本项目` , 选择 Next/下一步
  <img src="/iosimg/ios-create-module-1.png" alt="image-20220105095702169" />
- 输入模块名,点击 Create/创建
  <img src="/iosimg/ios-create-module-4.png" alt="image-20220105095702169" />

## 2、链接中控开发
- 前置条件
  - 代理程序安装成功
  - 中控自动化启动成功
  - 手机已授权 `USB设备授权`
- 链接中控调试地址<br/>
  <img src="/iosimg/ios-create-module-5.png" alt="image-20220105095753431"/>
  <br/><br/>
- 默认地址 `http://127.0.0.1:8019` , 也可修改IP:端口对其他电脑进行调试<br/><br/>
  <img src="/iosimg/ios-create-module-6.png" alt="image-20220105101413477"/>
- 点击确定，即可在idea下方看到链接日志
- 如链接多个设备,需选中要调试的设备
