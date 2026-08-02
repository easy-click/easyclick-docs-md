---
title: 安装开发工具
description: EasyClick 自动化脚本 iOS免越狱 教程 资源下载
keywords:
  - EasyClick 自动化脚本 iOS免越狱 教程  资源下载
  - zh
  - cn
  - tools
  - md
  - Windows
  - IPA
  - MacOS
  - win10
  - IDEA
  - br
  - EasyClick
  - 手机自动化
  - 自动化测试
  - 脚本开发
  - 安卓自动化
  - iOS自动化
  - 鸿蒙Next
---


- **开发可以在Windows/MacOS系统, 运行推荐在Windows[win10以上]系统**
> ****
> 开发: IDEA编写代码-->发送-->电脑中控执行逻辑代码-->发送-->手机代理程序IPA-->控制手机执行动作<br/>
> 部署: 编译好的iec源码包-->电脑中控执行逻辑代码-->发送-->手机代理程序IPA-->控制手机执行动作


## 名词解析
```json showLineNumbers
代理程序IPA:
	- 用于安装到iphone/ipad上的程序,打通电脑与手机的通讯,控制手机
	- 由于苹果限制,未上架苹果商店的三方ipa安装包,需要签名方可安装使用
	  因此需要个人开发者或巨魔等进行签名[推荐],也可通过我们提供的源码,用macOS的xcode进行源码编译

中控程序:
	- 中控程序支持Windows[win10以上]，MacOS等系统
	- 用于执行脚本、管理设备、配置UI参数等
	- 也可投屏群控手机

开发插件:
	- 构建于IDEA基础上的开发插件
	- 开发插件支持windows，macos等系统
	- IDEA 最新 2026.2 及以上版本，无需激活 IDEA，直接安装开发插件即可使用

桥接程序[非必选]:
	- 用于链接ios设备的程序,通过网络组网方式,拓展局域网中的设备到主控上,实现一台主控管理多台分控电脑
	- 可以部署到Windows，MacOS，Linux，树莓派、单片机等设备上
	- 也可用中控-高级组网功能,桥接主要为减小软件体积和适配多系统
```



## 安装步骤
- [1、安装代理ipa](/iosdocs/tools/signagent.md)
- [2、安装中控程序](/iosdocs/tools/installcenter.md)
- [3、安装开发插件](/iosdocs/tools/installdevtools.md)
- [4、安装桥接程序[非必选]](/iosdocs/tools/installbridge.md)

