---
title: EasyClick安卓文档_安卓手机自动化脚本_设备相关介绍
hide_title: false
hide_table_of_contents: false
sidebar_label: 设备链接
description: 'EasyClick手机自动化脚本,游戏自动化脚本,本章节介绍如何链接安卓设备，如何安装EasyClick的apk，如何激活安卓设备'
keywords:
  - EasyClick
  - 手机自动化脚本
  - 游戏自动化
  - IDEA开发工具下载
  - EasyClick激活设备
  - IDEA下载
  - 安卓免root
  - 安卓无障碍点击
  - USB
  - png
  - WIFI
  - img
  - src
  - androidimg
  - APK
  - condevice
  - all
  - 手机自动化
---


# 设备链接

- 位置在: `IDEA菜单栏 - EasyClick安卓版 - 设备连接 选项`

  <img src='/androidimg/condevice/all.png' alt="all.png" style={{zoom:'30%'}} />

## 设备连接
### USB连接
- 插入手机到电脑，并在手机设置中打开USB调试, 具体开启USB调试可以百度
- 打开IDEA底部的`EasyClick安卓版运行日志`窗口
- 选择`IDEA菜单栏 - EasyClick安卓版 - 设备连接 - USB链接`
- 选择后会自动安装EasyClick的apk文件，并且自动激活代理环境

### WIFI直连
- WIFI连接是指手机和电脑处于同一个局域网下，IDEA可以直接链接到调试app
- 在手机上打开EasyClick的APP，如果不知道如何获取EasyClick的App，参考**APK下载安装**
  - 打开App后，点击左上角的 `三` 符号，可以看到`当前手机的IP`
  - 如下图: <br/><img src='/androidimg/condevice/settip.png' alt="settip.png" style={{zoom:'30%'}} />
- 选择`IDEA菜单栏 - EasyClick安卓版 - 设备连接 - WIFI直连`
- 点击wifi连接，然后输入手机ip地址即可
  <br/><img src='/androidimg/condevice/con2.png' alt="con2.png" style={{zoom:'30%'}} />

### APK下载安装
- 部分手机可能无法直接通过adb安装apk，可以通过下载的方式进行
- 选择`IDEA菜单栏 - EasyClick安卓版 - 设备连接 - 下载apk文件`
- 将下载的apk，放到手机中手动安装
- 也可以选择`IDEA菜单栏 - EasyClick安卓版 - 设备连接 - 扫码安装APK`

### 远程调试
- 参考[远程调试章节](/docs/zh-cn/funcs/devtools/dev-tools-remote)



