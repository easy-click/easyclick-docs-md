---
title: EasyClick安卓文档_安卓手机自动化脚本_中控投屏平台说明
hide_title: false
hide_table_of_contents: false
sidebar_label: USB HID主机
description: EasyClick 中控投屏说明，用于局域网投屏，管理脚本，同步操作，
keywords:
  - EasyClick
  - 手机自动化脚本
  - 自动化软件
  - 中控投屏
  - 抖音群控
  - 快手群控
  - 游戏群控
  - HID
  - Ubuntu
  - Server
  - tip
  - '4.0'
  - windows
  - winusb
  - linux
  - EC
  - 手机自动化
  - 自动化测试
---

# HID主机

:::tip 说明
    - 采用HID主机的方式进行，优势在于免驱动，成本低，带机量大
    - 可以同时支持HID脚本和HID投屏
:::

:::tip 新版本提示
- HID主控4.0+以上支持windows的winusb驱动，可以不适用linux主机也能跑
:::
## 硬件配套
### 升腾主机
    - 拼多多搜索关键字`升腾小主机`，EC官方购买的是 升腾C92系统，8G内存，120G硬盘，价格是 217，可以购买更低配置的，约 150 左右
    - 安装 Ubuntu Server系统后，实际测试可以带20手机设备，成本核算按照150/20 = 7.5元/台手机，更高带机量的因为没有更多手机测试了
    - 这个成本远低于市面上的 单片机以及蓝牙方案，建议采用
        <br/><img src='/andqk/sthost.png' style={{zoom:'10%'}}/>
### 主板机改造
    - 部分主板机机箱内部自带小电脑主机，可以将其改造为Ubuntu Server系统，直接安装HID主控，实测效果也是很好
### 电脑主机
    - 如果有不用的电脑主机，直接安装Ubuntu Server系统，作为HID主机即可
### 其他
    - 其他的主机配置，可以自行改造为Ubuntu Server系统即可
## Ubuntu Server安装
    - Ubuntu Server安装教程市面上较多，这里就不在做过多介绍，这里测试的是 24.04版本
    - 教程地址参考:
        - https://zhuanlan.zhihu.com/p/698434939
        - https://www.bilibili.com/video/BV1ZAxqedEJy/?vd_source=58a0fd7e5e5cdf152718d0faed99c04f
        - https://blog.csdn.net/ziqibit/article/details/129932038
## HID主控安装
    - 在Ubuntu系统安装完成后，点击投屏系统的`HID主控`-`安装HID主控`
    - 输入当前Ubuntu的ip段，用户名和密码，点击安装
        <img src='/andqk/n_aqk5.png' style={{zoom:'20%'}}/>
    - 提示安装成功即可，重启机器也会自启动hid服务
        <img src='/andqk/n_aqk6.png' style={{zoom:'20%'}}/>  
## HID内网穿透
    - 对于需要跨网络的情况下，投屏程序提供了HID内网穿透功能
    - 请参考 [投屏内网穿透](/docs/centerscreen/openscreen#搭建内网穿透)
    - 投屏穿透成过后，HID也是顺带一起穿透的，可以访问网页 `外网IP:8989`地址浏览到本机的HID主控

## HID组网
    - 点击HID主控，点击高级组网,点击`广播式扫描` 或者填写IP段，进行网段扫描
        <br/><img src='/andqk/n_aqk7.png' style={{zoom:'20%'}}/>
    - 扫描完成后，HID主机会提交数据到本机上
        <br/><img src='/andqk/n_aqk8.png' style={{zoom:'20%'}}/>
    - 点击主控的 `激活HID模式`,可以开始激活HID
        <br/><img src='/andqk/n_aqk9.png' style={{zoom:'20%'}}/>
## 投屏使用HID
    - 上面设置完成后，主机组网等完成了可以进行投屏操作
    - 详细参考 [hid投屏](/docs/centerscreen/openscreen#hid投屏)
## 脚本使用HID
    - 局域网使用[设置hid主控地址](/docs/funcs/hid-event-api#sethidcenter-设置hid主控地址)函数更改为本机的局域网IP
    - 广域网内网穿透，使用设置hid主控地址函数，设置为 `外网IP:8989` 的地址
    

