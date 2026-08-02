---
title: EasyClick安卓文档_安卓手机自动化脚本_工程创建与运行
hide_title: false
hide_table_of_contents: false
sidebar_label: 工程创建与打包
description: 'EasyClick手机自动化脚本,使用idea创建EasyClick的工程，进行预览，运行，调试，打包apk，查看日志等'
keywords:
  - EasyClick
  - 手机自动化脚本
  - 自动化软件
  - 游戏自动化
  - IDEA开发工具下载
  - IDEA下载
  - 安卓免root
  - 创建工程
  - 预览工程
  - 运行工程
  - 打包工程
  - apk
  - IEC
  - APK
  - tip
  - docs
  - zh
  - cn
---


# 工程创建

## 创建工程

:::tip
    - 创建工程，运行和预览工程请参考[第一个工程](/docs/getting-start)章节
:::

## IEC文件编译
- 热更新一般会使用iec文件
- 选中工程，然后右键菜单，或者顶部IDEA菜单
- `IDEA菜单 - EasyClick安卓版 - 编译IEC文件`
- 编译完成后会存储当前工程的`build/release.iec`文件


## APK打包

- 顶部选择IDEA菜单 `EasyClick安卓版-打包工程`
  - 普通版打包，一般人用这个即可
  - 企业版本打包，专门用于云控，和EC云控结合使用才会使用这个选项
  - 调试版本打包，这个打包用于防止开发期间检测EC调试包的，作为一个基础的调试版本包使用
    - 这个打包后，需要配置一下包名，设置方式为`IDEA菜单-EasyClick安卓版-EasyClick设置-基本设置-调试版本包名`选项，填写后，使用USB链接设备，才可以正确识别到apk

- 打包工程会编译源码并输入apk文件，所有的日志在`EasyClic安卓版运行日志`可看到
- 打包完成后会apk文件会输出到`当前工程文件夹\build\release.apk`文件，把这个文件传到手机上，安装即可使用

:::tip
    - 如果你是使用了代理模式，请需要激活设备，请参考[名词解释](/docs/funcs/devtools/dev-tools-word)章节的激活
:::



## 打包参数说明
<img src='/androidimg/devintro/pkg-1.png'  alt="pkg-1.png" style={{zoom:'30%'}} />
<img src='/androidimg/devintro/pkg-2.png'  alt="pkg-2.png" style={{zoom:'30%'}} />
<img src='/androidimg/devintro/pkg-3.png'  alt="pkg-3.png" style={{zoom:'30%'}} />
<img src='/androidimg/devintro/pkg-4.png'  alt="pkg-4.png" style={{zoom:'30%'}} />
<img src='/androidimg/devintro/pkg-5.png'  alt="pkg-5.png" style={{zoom:'30%'}} />
<img src='/androidimg/devintro/pkg-6.png'  alt="pkg-6.png" style={{zoom:'30%'}} />

