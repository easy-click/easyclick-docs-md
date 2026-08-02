---
title: 安装中控程序
description: EasyClick 自动化脚本 iOS免越狱 安装中控程序 资源下载
keywords:
  - EasyClick 自动化脚本 iOS免越狱 安装中控程序  资源下载
  - iosdocs
  - zh
  - cn
  - tools
  - download
  - resources
  - EC
  - ioscenter
  - 9.19.0.
  - tip
  - EasyClick
  - 手机自动化
  - 自动化测试
  - 脚本开发
  - 安卓自动化
  - iOS自动化
  - 鸿蒙Next
---

:::tip
- 中控程序是用于执行脚本和自动化环境维护的核心
- 需提前下载并安装 **爱思助手**
- 此处以版本 `9.19.0` 为例
:::

:::tip 10.2.0 新版说明
10.2.0 版本启用了全新界面架构，启动更快、布局更清晰，同时内置了集控投屏和脱机激活器入口。基本操作流程与旧版一致，下载路径请将版本号替换为 `v10.2.0`。新版本详细说明请参考 → [**投屏操作教程**](/iosdocs/advance/ios-usb-screen)
:::

## 下载中控
- 请到[资源区的网盘](/iosdocs/tools/download_resources)，下载中控程序
- Windows为 **EC开发包-->IOS资源-->USB版本-->v9.19.0-->中控-->ioscenter_windows-x64-9.19.0.zip**
- 下载后解压到电脑, 解压路径不能包含空格,特殊符号,括号等
- macOS为 **ioscenter_macos-amd64-9.19.0.dmg** ,自行双击并根据提示安装使用<br/>
  <img src="/iosimg/dl-ios-center.png" alt="image-20220208110050592"/>

### 下载开发者镜像(可选)
- `IOS17`以下手机需要开发者镜像支持,IOS17以上忽略此步
- 请到[资源区的网盘](/iosdocs/tools/download_resources)，下载中控程序 **EC开发包-->IOS资源-->开发者镜像-DeveloperImage12.4-26.0.zip**
- Windows电脑, 解压并替换文件夹中 `bridgebin\config\DeveloperDiskImage` 中内容
- macOS电脑, **访达->应用程序->ioscenter->右键->显示包内容**, 解压并替换文件夹中 `Contents/java/app/bridgebin/config/DeveloperDiskImage` 中内容

## 启动中控程序

- 这里以Windows 为例,运行 **ioscenter.exe**
  <img src="/iosimg/dl-ios-center2.png" alt="image-20220320214851172" style={{zoom:'80%'}} />


### 登录中控
- 中控主界面-授权中心
- 账号注册后,需进行手机验证方可使用-->[**验证地址**](https://uc.ieasyclick.com/validateSelf?sign=BHgPnRRoTf/#/validateSelf)
- 验证时账户类型需切换为 `安卓中控图片/IOS账户(USB,脱机版)/网络验证账号`<br/>
  <img src="/iosimg/dl-ios-center4.png" alt="image-20220320215118697" />
- 如登录失败,检查中控界面-运行状态页, 桥接程序是否运行正常, 如异常可能由于路径存在特殊符号, 或被杀毒软件隔离导致

### 中控配置(可选)

- 如果你修改了agent的源码bundleid，这里一定要进行修改`bundleID`属性，填写agent的bundleID的前缀
- 未修改则忽略此步
  <img src="/iosimg/dl-ios-center3.png" alt="image-20220320215029162" style={{zoom:'50%'}} />


## 开启自动化
- 自动化是脚本/群控执行的前提条件,只有自动化启动成功,方可正常使用所有功能
- 自动化需要手机上代理程序配合,因此需确保手机已正确签名并安装 `ios xx 系统-easyclick-USB代理程序-9.19.0.ipa`
- 启动方法: 设备列表-->右键-->开启自动化<br/>
  <img src="/iosimg/dl-ios-center8.png" alt="image-20260201-1" />
- 如正常开启,则设备列表中-->服务状态一项,会由红色 `否` ,变更为蓝色 `是`<br/>
  <img src="/iosimg/dl-ios-center9.png" alt="image-20260201-2" />
- 如1分钟后未开启成功,参考--> [**测试自动化**](https://www.laoleng.vip/docs/tools/easyclick/ios-qk/qa/testenv)
### 测试自动化启动状态(可选)
- 当 **开启自动化** `失败` 时,需要使用测试自动化,通过返回信息来排查具体失败原因
- 选择设备 - 右键 - 测试自动化<br/>
  <img src="/iosimg/dl-ios-center5.png" alt="image-20220208112514521" style={{zoom:'50%'}} />
- 点击 `测试自动化`, 一般20s内就可以返回结果, IOS17以上系统可能会1分钟以内返回<br/>
  <img src="/iosimg/dl-ios-center6.png" alt="image-20220208112853175" style={{zoom:'50%'}} />
- 具体报错处理方式, 参考--> [**测试自动化及解决方法**](https://www.laoleng.vip/docs/tools/easyclick/ios-qk/qa/testenv#%E5%A4%B1%E8%B4%A5)

## 设备授权
- 按照手机收取授权使用费, 作者不额外收费
- 授权费包含两种, 分开收费, 必须提前授权才能使用相对应的功能
  - USB设备授权: 用于调试/执行脚本, 及群控中的录制脚本的授权
  - USB投屏授权: 用于群控手机授权
- 授权/换绑/转让方法, 参考-->[**授权/换绑/转让方法**](https://www.laoleng.vip/docs/tools/easyclick/ios-qk/qa/authorize)

## 执行脚本
- 确保自动化服务已开启
- 确保已授权 `USB设备授权`
### 选择设备执行
- 中控界面上，选择想要执行的设备, 鼠标右键，选择执行脚本，选择iec文件即可
- iec文件可在idea中编译生成
  <img src="/iosimg/dl-ios-center7.png" alt="image-20220208112853175" style={{zoom:'50%'}} />

### 批量执行
- 或在中控左下角, 脚本区域, 右键-->打开脚本目录, 将iec文件放入, 右键, 刷新
  - 右键脚本,执行脚本,即可批量执行所有设备
  <img src="/iosimg/dl-ios-center10.png" alt="image-202602011833" />
- 也可在左侧 `分组栏` 先进行手机分组,再对方分组进行脚本执行


## 群控手机
- 中控首页-集控投屏
- 确保自动化已开启
- 确保已授权 `USB投屏授权`
- 视频演示-->[**EasyClick苹果IOS免越狱群控投屏控制使用简介**](https://www.bilibili.com/video/BV1UsBKYeEPa/?share_source=copy_web&vd_source=e808b0cf1c36665f5f5c240a5dd8bc60)
  <img src="/iosimg/dl-ios-center11.png" alt="image-202602011833" />
