---
title: EasyClick安卓文档_安卓手机自动化脚本_无障碍与代理
hide_title: false
hide_table_of_contents: false
sidebar_label: 名词解释
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
  - tip
  - docs
  - zh
  - cn
  - centerscreen
  - openscreen
  - ROOT
  - shizuku
  - hid
  - 手机自动化
---


# 名词解释
:::tip
- 对于刚刚使用EasyClick安卓的同学不太懂得常见的术语，这里特地使用一个章节做解释
- EasyClick中的自动化服务在运行期间是可选项，除非你一定要做自动化点击等操作就需要开启
  - 包括IDEA截图、抓节点等都需要打开自动化
:::



## 自动化模式
### 无障碍模式
- 使用安卓自带的无障碍服务，运行自动化操作，可以操作节点
- 在EasyClick APP的`系统设置`中勾选`无障碍模式(自动化)`或者`代码设置`，
- 一般需要**人工手动开启无障碍服务**，但是使用`EasyClick中控投屏的授权无障碍权限或者激活工具激活设备`，可以自动打开无障碍，具体参考[EasyClick中控投屏激活无障碍和代理](/docs/centerscreen/openscreen#其他功能)
- 无障碍模式的权限是最小的
:::tip
- 无障碍自启动权限
    - 激活器授权 到软件资源区的网盘下载，`安卓资源-V1.15.0-EC激活设备-无障碍保活.zip`，下载后按照里面说明书进行
    - 中控投屏授权无障碍自启动，具体使用参考: [中控投屏激活器功能](/docs/centerscreen/openscreen#授权无障碍)
:::

### 代理模式
- 使用EasyClick的激活器、IDEA开发工具、EasyClick中控投屏系统三个软件其中一个激活设备后，即可有权限
- 在EasyClick APP的`系统设置`中勾选`代理模式(自动化)`或者`代码设置`
- 代理模式的权限大于无障碍模式，可以执行的操作更多，节点、点击等都支持，还支持shell模块里面的函数等

### ROOT
- ROOT模式是最大的，这个需要在手机上授予EasyClick APP的root权限，具体每个root软件不同
- 在EasyClick APP的`系统设置`中勾选`ROOT权限授权`，就会有ROOT授权弹窗，授予权限即可
- 在获取ROOT权限后，启动`代理模式自动化`或者`无障碍模式自动化`即可

### shizuku
- 一种和EasyClick激活模式差不多的软件，具体需要百度，可以用 shizuku 开启自动化服务
- 如果授予了shizuku权限，直接开启自动化服务即可
### hid
- 借助外置的电脑硬件，在不开启无障碍和usb调试模式下，可以模拟点击等操作，但是不能使用节点
- 使用hid进行点击，但是不能进行节点操作，输入法、截图都可以，可以使用找图找色ocr等手段进行脚本编写
- 具体参考 [HID主控用法](/docs/advance/hid)和[HID事件](/docs/funcs/hid-event-api)

## 截图
### 有权限截图
- 使用无障碍模式自动化，在申请权限的时候image.requestScreenCapture,会弹出截图授权，部分手机EasyClick自动接受并点击授权按钮

### 无需权限截图
- 使用代理模式自动化的时候，使用image.requestScreenCapture函数，无需授权，即可截图
## 输入法
### 输入法输入
- 用于防止部分app不给输入，所以这里自带了输入法功能，输入法需要在手机的设置-语言与输入法-里面选择EasyClic的键盘
- 在EasyClick App的系统设置，里面找到`设置为默认输入法`并勾选，`显示输入法键盘`是可选项
- 输入法函数都是以ime开头的，具体可以文档搜索相关用法，例如 [输入法输入数据](/docs/funcs/global/global-shortcut#imeinputtext-输入法输入数据)
## 代理激活
- 没有root、没有shizuku、不想使用无障碍，只想用代理模式自动化的情况需要激活
- 激活状态可以看`EasyClick App的系统设置-免ROOT是否激活`，勾选中属于已激活
### IDEA激活
- 使用IDEA激活，路径在`EasyClick安卓版-激活保活设备-模式1激活或者模式二激活`，激活效果请看`EasyClick安卓版运行日志`
### 电脑激活
#### 独立激活器 
    - 到软件资源区的网盘下载，`安卓资源-V1.15.0-EC激活设备-无障碍保活.zip`，下载后按照里面说明书进行
#### 中控投屏激活器
    - 下载中控投屏软件，到软件资源区的网盘下载，`安卓资源-中控投屏系统-EasyClick安卓中控投屏系统v(版本).zip`，下载后按照里面说明书进行
    - 具体使用参考: [中控投屏激活器功能](/docs/centerscreen/otherfunc#激活代理模式)

