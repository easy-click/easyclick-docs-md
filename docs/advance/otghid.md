---
title: EasyClick安卓文档_安卓手机自动化脚本_HID
hide_title: false
hide_table_of_contents: false
sidebar_label: OTG HID硬件教程
description: 'EasyClick 代码热更新,无需无障碍和USB调试实现自动化'
keywords:
  - EasyClick
  - 手机自动化脚本
  - 自动化软件
  - 脚本热更新
  - 代码热更新
  - 无需无障碍和USB调试实现自动化
  - ESP32
  - OTG
  - HID
  - S2
  - S3
  - download
  - tip
  - img
  - src
  - 手机自动化
  - 自动化测试
  - 脚本开发
---
# 说明
:::tip
- 目前支持的硬件有 ESP32-S2，ESP32-S3
- 固件是免费的，开发板自己去淘宝、拼多多、1688去买
:::
## 硬件图
- 例子中是普通的转接头，如果需要充电+OTG就去买三合一的
- <img src="/androidimg/otg/s1.png" alt="" style={{zoom:'20%'}} />

## 下载固件
- 网盘下载，地址 [软件下载区](/community/download_area)
- 找到--> **开发工具 - 安卓资源 - OTG HID固件-ESP32-S3或者ESP32-S2**文件夹，找到对应硬件的固件bin文件并下载
- 分为带键盘和不带键盘两种固件，有的app检测带键盘的，所以分开的，不带键盘的固件无法使用home等输入函数
- 下载ESP32的 flash_download_tool.zip文件，准备拿刷入固件


## 刷入固件
- 参考 蓝牙的刷入步骤即可 [蓝牙固件刷入](/docs/advance/blehid#刷入固件)
- OTG固件是不需要记下蓝牙MAC地址的，遇到这个自动忽略，刷入固件就行
- S2 需要按住IO键，再按下RST键，再同时松开，电脑才能识别
- ESP32S2 刷入固件，需要按一下“按住 BOOT -> 点按 RST”动作
- ESP32S3 刷好了链接手机，需要插入USB-OTG口，不要插入COM口
## 授权和测试
- 首次插上设备，可能会弹窗以下授权
- <img src="/androidimg/otg/s2.png" alt="" style={{zoom:'20%'}} />
- 勾选**一律使用**，然后确定，不同可能显示不一样，只要确即可
- 进入APP的系统设置,找到**OTG HID设置**，点击链接OTG，如果弹出了授权窗口，一定要允许
- 然后点击**测试HOME**按钮，如返回到桌面，就代表成功，下一步就是在脚本中调用了
- <img src="/androidimg/otg/s3.png" alt="" style={{zoom:'20%'}} />
## 脚本调用 
- 上述步骤完成，并且测试成功了可以开始脚本编写，调用脚本函数

