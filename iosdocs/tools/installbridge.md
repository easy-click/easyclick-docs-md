---
title: '安装桥接程序[非必选]'
description: EasyClick 自动化脚本 iOS免越狱 安装桥接程序 资源下载
keywords:
  - EasyClick 自动化脚本 iOS免越狱 安装桥接程序  资源下载
  - iosdocs
  - zh
  - cn
  - tools
  - Windows
  - img
  - src
  - iosimg
  - image
  - download
  - EasyClick
  - 手机自动化
  - 自动化测试
  - 脚本开发
  - 安卓自动化
  - iOS自动化
  - 鸿蒙Next
---


- 桥接程序是链接手机和中控程序的中间桥梁
- 可部署在局域网的电脑上,代替中控链接手机, 并与主电脑上的中控进行高级组网,实现一台电脑控制所有电脑上的手机
- 直接部署中控也能达到同样效果,桥接只是单独提炼了核心功能,安装包比较小



### 下载桥接程序

- 请到[资源区的网盘](/iosdocs/tools/download_resources)，下载桥接程序
- 该程序支持Windows，macos，linux等系统
- 请下载对应的版本, 解压, 解压路径不能包含空格/特殊符号/括号等
- IOS17以下系统需手动放入开发者镜像到 `config\DeveloperDiskImage` ,方法参考-->[下载开发者镜像](/iosdocs/tools/installcenter#下载开发者镜像可选)



<img src="/iosimg/image-20220320213338751.png" alt="image-20220320213338751" style={{zoom:'50%'}} />







### 启动桥接程序

> 这里以mac系统为例子，Windows系统类似

<img src="/iosimg/image-20220320213531804.png" alt="image-20220320213531804" style={{zoom:'50%'}}/>

- config: 是配置文件夹

- DeveloperDiskImage: ios系统的开发者镜像文件，用于iPhone启动后自动刷入开发者镜像的文件
- ios-bridge :  中控的二进制文件，直接运行



> Windows直接双击 ios-bridge.exe 就可以运行
>
> mac、linux用终端执行
>
> 有8020端口出现 ，代表启动成功
>

<img src="/iosimg/image-20220320213819393.png" alt="image-20220320213531804" style={{zoom:'50%'}} />



### 桥接程序配置(可选)

- 记事本或者editpad++编辑 `config/config.toml`

> ```toml
> [site]
> ### 中控程序部署的地址，支持本机，局域网，以及远程服务器部署
> ### 默认都是本机的地址
> remote = "ws://127.0.0.1:8019"
> 
> ```





### 安装爱思助手
- Windows系统由于需要驱动才能链接苹果手机,因此百度下载安装爱思助手,并正确链接手机后, 方可使用
