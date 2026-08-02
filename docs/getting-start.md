---
title: EasyClick安卓文档_安卓手机自动化脚本_第一个工程
hide_title: false
hide_table_of_contents: false
sidebar_label: 第一个工程
description: 'EasyClick手机自动化脚本,使用idea创建第一个EasyClick的脚本工程，进行激活设备，运行脚本'
keywords:
  - EasyClick
  - 手机自动化脚本
  - 自动化软件
  - EasyClick脚本激活
  - idea下载
  - idea
  - download
  - mac
  - IDEA
  - https
  - tip
  - 2024.1.4
  - EC
  - 10.9.0
  - 手机自动化
  - 自动化测试
  - 脚本开发
---
:::tip
- 本章节基于idea mac版本的2024.1.4版本，EC安卓开发插件为10.9.0版本，其他版本功能略有差别
- Windows版本和mac版本的操作一样
- idea支持2019.1.0 ~ 最新版本，无需担心
- **IDEA 最新 2026.2 及以上版本，无需激活 IDEA，直接安装开发插件即可使用**
:::
# 第一个脚本工程
## 下载开发工具
- 下载开发工具 : [点我下载](tools/download_resources.md)    
- 下载EasyClick最新版本的IDEA插件，并按照到IDEA中，安装教程：https://blog.csdn.net/qq_35246620/article/details/78289074
- 软件下载区链接 [下载区](/community/download_area)
- idea下载地址 [idea官网地址](https://www.jetbrains.com/idea/download/)
- **推荐下载 IDEA 2026.2 及以上版本：无需激活 IDEA，直接安装开发插件即可使用**
- 安装后，打开 `bin/idea64.exe` 文件开始运行
## 创建工程和项目
- 创建工作空间,选择open按钮，然后选择一个空的文件夹
<img src='/androidimg/first/openworkspace.png' alt="openworkspace.png" style={{zoom:'30%'}} />
- 打开后工作空间后如下图
<img src='/androidimg/first/workspace2.png' alt="workspace.png" style={{zoom:'30%'}} />
<br/>
- 创建项目,在工作空间鼠标右键-选择new（新建）- module(模块)
<img src='/androidimg/first/makeproject.png'  alt="makeproject.png" style={{zoom:'30%'}} />
- 选择项目,选择 EasyClick安卓版-脚本项目,点击next（下一步）

<img src='/androidimg/first/selecttype.png'  alt="selecttype.png" style={{zoom:'30%'}} />

- 填写一个工程名称 中文和英文都支持，最好不要用特殊字符、空格等

<img src='/androidimg/first/modulename.png'  alt="modulename.png" style={{zoom:'30%'}} />  
- 创建成功,项目结构如下,更加详细的结构说明请参考开发工具介绍章节
<img src='/androidimg/first/projectstruct.png'  alt="projectstruct.png" style={{zoom:'30%'}} />



## 连接设备
- 点击idea底部的`EasyClick安卓版运行日志`，可以看到详细的日志输出
- 点击idea菜单 `EasyClick安卓版 - 设备连接 - 选择USB链接`，前提是你需要打开安卓USB调试
<img src='/androidimg/first/connectdevice.png'  alt="connectdevice.png" style={{zoom:'30%'}} />
- 链接的同时会自动化安装上apk文件，并且启动文件
- 如果没有安装成功，可以手动下载apk，位置在`IDEA菜单-EasyClick安卓版-设备链接-下载APK`，下载apk后传到手机中进行安装
- 链接成功如下图
<img src='/androidimg/first/conok.png'  alt="conok.png" style={{zoom:'30%'}} />



## 预览运行程序
- 预览UI，在工程名称上`点击鼠标右键-安卓版-预览(工程名称)`
- 预览UI，在工程名称上`点击鼠标右键-安卓版-运行(工程名称)`
<img src='/androidimg/first/previewpro.png'  alt="previewpro.png" style={{zoom:'30%'}} />
- 也可以打开某个js文件，在文件中进行`鼠标右键`菜单中找到运行工程和预览功能
- 一个工程运行成功后，后续可以直接在顶部的工具栏直接运行
<img src='/androidimg/first/runshort.png'  alt="runshort.png" style={{zoom:'30%'}} />

:::tip
！到此第一个工程结束，恭喜你进入了EasyClick的开发世界！
:::
