---
title: EasyClick安卓文档_安卓手机自动化脚本_云控打包配置
hide_title: false
hide_table_of_contents: false
sidebar_label: 云控打包配置
description: EasyClick 云控平台对应的apk打包，可以动态域名打包，也可以静态域名打包，一定要打企业版本的apk
keywords:
  - EasyClick
  - 手机自动化脚本
  - 自动化软件打包
  - 云控平台
  - 抖音云控
  - 快手云控
  - 游戏云控
  - DNS
  - IP
  - EC
  - 7.8.0
  - apk
  - tip
  - image
  - APK
  - ecloudimages
  - 手机自动化
  - 自动化测试
---



# 云控打包配置

## 云控配置说明

1、从 EC 7.8.0 开始，非调试版本的域名只能从开发工具打企业版本包的时候输入，其他无输入途径

2、从 EC 7.8.0 开始，新增了云端直接动态改变apk内部域名功能

3、从 EC 7.8.0 开始，无需在apk中填写设备名称，直接在云端填写设备名称即可



## 打包域名配置



### 直连模式
:::tip
直连模式是打包的APK，通过打包的域名直接和云控链接
:::

打企业版本的包，填写云控地址即可

	![image-20211130151733982](/ecloudimages/image-20211130151733982.png)


### 动态DNS解析域名模式(推荐)
- 该模式好处是使用域名指向ip，但是不需要域名备案
- 只需要在DNS解析到IP即可，原理是通过域名解析到IP，在通过IP链接到服务器，避免更换服务器的问题
- 具体操作是打包企业版本时，填写云控地址为域名+端口号，例如: ecloud.devicefarm.cn:8098， 获取链接模式选择 DNS解析IP后链接

### 动态域名
:::tip
动态域名模式是打包的APK，先通过配置的json地址获取云控真实的地址后，再通过真实地址去链接云控的服务器
:::
打企业版本的包，填写云控地址，例如http://192.168.1.76:8099/remote.json

！！！！一定要以json结尾！！！！

1、这个json文件可以放到阿里云的OSS文件或者是其他可以请求到的地方

2、也可以直接使用云控的站点管理 - 站点授权 - 云控链接地址设置功能，请进入站点看详细说明

3、json文件的数据生成，请进入，云控的站点管理 - 站点授权 - 云控链接地址设置，填写域名后，点击复制按钮，将复制的数据，存储到本地remote.json文件，然后将remote.json文件放到阿里云oss等存储即可

生成格式如下:

```json showLineNumbers
{"data":"BggHBAcEBwADUQJWAlYDAQMJAwICVQMBAwYDCAJVAwECVQMHAwYDUQMIAwADCQMJAFEGCAcEBwQHAANRAlYCVgMBAwkDAgJVAwEDBgMIAlUDAQJVAwcDBgNRAwgDAAMJAwg="}
```

