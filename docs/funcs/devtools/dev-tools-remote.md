---
title: EasyClick安卓文档_安卓手机自动化脚本_远程调试
hide_title: false
hide_table_of_contents: false
sidebar_label: 远程调试
description: 'EasyClick手机自动化脚本,使用idea远程调试EasyClick的项目，只要设备能联网，就可以远程调试脚本'
keywords:
  - EasyClick
  - 手机自动化脚本
  - 自动化软件
  - IDEA远程调试
  - 安卓免root远程调试
  - 脚本远程调试
  - remote
  - png
  - http
  - 192.168.1.1
  - img
  - src
  - androidimg
  - alt
  - APK
  - apk
  - 手机自动化
  - 自动化测试
---


# 远程调试
## 安装APK
- 部分手机可能无法直接通过adb安装apk，可以通过下载的方式进行
- 选择`IDEA菜单栏 - EasyClick安卓版 - 设备连接 - 下载apk文件`
- 将下载的apk，放到手机中手动安装
- 也可以选择`IDEA菜单栏 - EasyClick安卓版 - 设备连接 - 扫码安装APK`

## 开启远程调试
- `菜单栏-EasyClick安卓版-设备连接-开启HTTP远程调试`
- 在弹出框中，输入本地的端口，默认是 10826，点击确定，开启本地端口，注意观察`EasyClick安卓版运行日志栏`

## 路由器暴露端口

- 默认路由器地址一般是 ：[http://192.168.1.1](http://192.168.1.1)，本文档依次为例，路由器是TPLINK，在浏览器输入网址：[http://192.168.1.1](http://192.168.1.1)
- 找到虚拟服务器

<img src='/androidimg/remote_1.png' alt="remote_1.png" style={{zoom:'30%'}} />

- 找到虚拟服务器

<img src='/androidimg/remote_2.png'alt="remote_2.png" style={{zoom:'30%'}} />

- 找到外网IP

<img src='/androidimg/remote_3.png' alt="remote_3.png" style={{zoom:'30%'}} />

## 在手机上EC进行连接
- 在手机上打开EC调试程序

- 进入远程连接页面 

<img src='/androidimg/remote_5.png' alt="remote_5.png" style={{zoom:'30%'}} />


- 输入信息进行连接 

<img src='/androidimg/remote_6.png' alt="remote_6.png" style={{zoom:'30%'}} />


## FRP反向代理
- 使用FRP软件进行反向代理，需要将电脑端口映射到服务器，然后手机连接服务器IP即可，这样就不用路由器IP了

## TCP远程
- TCP远程调试和HTTP远程调试类似,无非是端口不一样，没有公网尽量使用FRP代理软件做反向代理
