---
title: EasyClick安卓文档_安卓手机自动化脚本_HID
hide_title: false
hide_table_of_contents: false
sidebar_label: 蓝牙HID硬件教程
description: 'EasyClick 代码热更新,无需无障碍和USB调试实现自动化'
keywords:
  - EasyClick
  - 手机自动化脚本
  - 自动化软件
  - 脚本热更新
  - 代码热更新
  - 无需无障碍和USB调试实现自动化
  - ESP32
  - S3
  - download
  - flash
  - tool
  - HID
  - C3
  - zip
  - APP
  - 手机自动化
  - 自动化测试
  - 脚本开发
---
# 说明
:::tip
- 目前支持的硬件有 ESP32-S3，ESP32-C3，下面的例子是以ESP32-S3为例子，其他硬件类似
- 固件是免费的，开发板自己去淘宝、拼多多、1688去买
:::


## 下载固件
- 网盘下载，地址 [软件下载区](/community/download_area)
- 找到--> 开发工具 - 安卓资源 - 蓝牙HID固件-ESP32-S3或者ESP32-C3文件夹，找到对应硬件的固件bin文件并下载
- 分为带键盘和不带键盘两种固件，有的app检测带键盘的，所以分开的，不带键盘的固件无法使用home等输入函数
- 下载ESP32的 flash_download_tool.zip文件，准备拿刷入固件
## 刷入固件
- 解压flash_download_tool.zip文件，打开 flash_download_tool.exe文件
-     <img src="/androidimg/ble/flash-1.png" alt="" style={{zoom:'30%'}} />
- 这个时候会让你选择芯片类型，这里演示的是 ESP32-S3，我们选择ESP32S3，点击OK
-     <img src="/androidimg/ble/flash-2.png" alt="" style={{zoom:'30%'}} />
- 将我们的芯片通过USB数据线链接到电脑，
  - 读取mac地址
    - 切换到**clipInfoDump**选项，选择 port，我这里是COM3，具体根据电脑实际情况而定
    - 点击**Clip Info**按钮开启读取，成功后有mac地址信息，后6位对应的就是**蓝牙地址**，记下这个地址
    -   <img src="/androidimg/ble/flash-3.png" alt="" style={{zoom:'30%'}} />

- 切换到**SPIDownload**选项，第一个填写项目选择bin文件，并且勾选，最右侧填写 **0x0**，变成绿色为正确
- COM口选择COM3，你的电脑不一定是COM3，根据实际情况而定，点击**START**按钮
-   <img src="/androidimg/ble/flash-4.png" alt="" style={{zoom:'30%'}} />
-  **刷入中**
-   <img src="/androidimg/ble/flash-5.png" alt="" style={{zoom:'30%'}} />
- **成功**
-   <img src="/androidimg/ble/flash-6.png" alt="" style={{zoom:'30%'}} />
- 如果出现错误，可以重新启动刷入工具，也可以点击**ERASE**,格式化系统固件
- 上述都成功后，重新对ESP32芯片通电，在手机上可以扫描到蓝牙名称
## 手机链接蓝牙
- 进入手机设置-蓝牙，找到对应的BLE名称，点击链接并配对，直到链接成功为止
- 例如这里需要链接 8ce1e4 这个蓝牙，图标显示为键盘，有的手机显示为鼠标，新版本可以显示的是普通的蓝牙鼠标，名称长度是8位<br/>
    <img src="/androidimg/ble/blename.png" alt="" style={{zoom:'30%'}} />
    <img src="/androidimg/ble/ble-c2.png" alt="" style={{zoom:'30%'}} />
## 初始化APP
- 进入APP的系统设置,找到蓝牙HID设置，先进行扫描，选择需要的ble名称，这里选择 8ce1e4
-     <img src="/androidimg/ble/ble-c3.png" alt="" style={{zoom:'30%'}} />
- 完毕后，**蓝牙设备名称**会自动填写选择的名称，继续点击**测试**，如果测试成功会直接返回主页
- 最后点击**保存按钮**
- 如果扫描不到你要的蓝牙，看下**常见问题说明**
## 脚本调用 
- 上述步骤完成，并且测试成功了可以开始脚本编写，调用脚本函数

## 开放接口
- 如果需要通过自己中控管理蓝牙的可以看这个接口： http://2tsre28hlt.apifox.cn
- 如果想扫描蓝牙的IP地址，请自行使用局域网IP循环请求这个接口 【获取MAC地址（蓝牙名称）】
  - 将IP地址替换为接口的地址，获取到的蓝牙名称和IP是对应在一起即可

## 常见问题 
- APP在后台无法扫描蓝牙
  - 请到权限管理打开悬浮窗权限，
  - 把位置信息权限 改成 -始终允许
  - 打开允许后台弹窗
  - 高版本的手机需要找到【扫描附近设备】的权限并允许
  - 一定要给权限并且保活APP，否则系统会不让APP在后台链接蓝牙
- 蓝牙名称在手机上看不到或者扫描不到
  - 可以尝试重启蓝牙硬件(直接断电或者按住开发板上的rst键重启)，杀死APP进程,重新链接在手机设置-蓝牙 取消配对 重新链接
  - 比较保险的做法是：手机的设置蓝牙先链接上，然后按开发板RST键，在APP中再扫描链接(因为有的手机设置会第一次连接失败后，占用一个连接数量)
- 如何隐藏蓝牙防止检测
  - 在APP的系统设置中-蓝牙BLE设置-隐藏蓝牙，或者使用代码 bleEvent.hideBleName() 进行隐藏如果不成功可以多调用几次
- 蓝牙提示链接了
  - 可以尝试重启蓝牙硬件(直接断电或者按住开发板上的rst键重启)，杀死APP进程,重新链接在手机设置-蓝牙 取消配对 重新链接
- 蓝牙名称问题
  - 蓝牙名称一般是以硬件的MAC地址 后8位作为名称的，可以在刷入固件工具中看到
