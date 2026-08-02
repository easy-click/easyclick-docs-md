---
title: 安装主程序和代理ipa
description: EasyClick 自动化脚本 iOS免越狱 iOS免硬件 iOS脚本 安装代理ipa 资源下载
keywords:
  - EasyClick 自动化脚本 iOS免越狱 iOS免硬件 iOS脚本 安装代理ipa  资源下载
  - ipa
  - download
  - main
  - easyclick
  - tj
  - 6.4.0.
  - zip
  - agent
  - tip
  - iOS
  - EasyClick
  - 手机自动化
  - 自动化测试
  - 脚本开发
  - 安卓自动化
  - iOS自动化
  - 鸿蒙Next
---
:::tip
- 脱机版本只支持iOS 15+版本，低于这个版本不用尝试了
- 由于苹果限制,未上架的三方ipa安装包必须签名才能安装到手机使用,因此需要先签名才能正常使用EasyClick产品
:::

:::tip
- 7.0.0+以上版本支持单App和双App两种自动化引擎模式
- 单App模式只需要安装主程序，在app设置中-自动化选项，选择单app后保存，开启自动化
- 双App需要签名代理ipa才能使用
- 为什么会有两种？
  - 第一是为了兼容老的，单App优势是速度快一些，只需要签名一个ipa
  - 第二双App有独特优势，在不同进程，自动化服务掉了可以使用自激活模式启动，做到无人值守方式
:::
## 下载主程序
- 到[**资源区网盘**](/iostjdocs/tools/download_resources)下载主程序压缩包 
- **EC开发包-->IOS资源-->脱机版本-->最新版本号,如v6.4.0**，
  里面包含了主程序、代理程序打好的ipa和代理的xcode工程源码,后缀的6.4.0代表是当前发布的版本号<br/>  
  <img src="/iostjimg/download_main_zip.png" alt="download_main_zip" style={{zoom:'80%'}} />
    - 脱机主程序-easyclick-tj-main-6.4.0.ipa
      - 脱机版的主程序
    - 脱机代理程序-easyclick-tj-agent-6.4.0.ipa
      - 脱机版代理程序
    - easyclick-tj-agent-source-6.4.0.zip
      - 脱机版代理程序源码xcode工程[非必要]

## 签名主程序并安装
- 主程序是正常的应用，支持个人免费账户签名、开发者签名、企业签名、巨魔签等,支持大部分签名工具
- 签名参考-->[**苹果IPA签名教程**](http://laoleng.vip/docs/tools/easyclick/ipasign/)
- 签名主程序后，爱思或者其他工具安装ipa到手机后即可
- 安装后手机上会有**易点云测**图标，打开app：<br/>  
  <img src="/iostjimg/tj-index.jpg" alt="tj-index.jpg" style={{zoom:'30%'}} />

### 保活设置
- 由于苹果限制后台应用,需进入右上角设置-->保活设置-->保存设置,当弹出悬浮窗后,按住悬浮窗上部,拖到左侧/右侧使其贴边隐藏
  <img src="/iostjimg/tj-index-1.png" alt="tj-index.jpg"  /><br/>
  <img src="/iostjimg/tj-index-2.png" alt="tj-index.jpg" />
- 地理位置授权,首次运行会弹出请求你的位置，点击使用期间允许
- 也可进入手机的设置，找到易点云测，点击进入<br/>
  <img src="/iostjimg/tj-sys-setting.jpg" alt="tj-setting.jpg" style={{zoom:'30%'}} />
- 如下图，位置选择始终允许，后台App刷新选择允许，无线数据选择 WLAN与蜂窝网络,Siri与搜索全部选择允许 <br/>
  <img src="/iostjimg/tj-ec-auth.jpg" alt="tj-setting.jpg" style={{zoom:'30%'}} />
### 授权初始化
- 脱机版需要付费后方可正常使用,按手机收费
- 参考-->[**文字教程**](/iostjdocs/advance/tjcenter)
- 参考-->[**视频教程**](http://laoleng.vip/docs/tools/easyclick/ios-qk/tj/)

## 签名代理程序并安装
- 代理程序IPA比较特殊,不支持个人免费账户签名,并且市面上的爱思/sideloadly等工具无法签名完成，可以到EC论坛搜索**签名**关键字
- 签名参考-->[**苹果IPA签名教程**](http://laoleng.vip/docs/tools/easyclick/ipasign/)
- 如果是MacOS开发者,可以使用代理程序的源码，xcode编译运行到手机,此时支持个人免费开发者账号签名,参考USB版的-->[**MacOS源码编译**](/iosdocs/tools/signagent#macos源码编译)
- 签名后,通过爱思助手将代理程序安装到手机上后,点击EasyClick-Runner图标，手机系统上会出现 **Automation Running** 白色字样，只要出现了，说明代理程序是正常的
- 如未出现**Automation Running** 白色,可能需要手动-->[**刷入开发者镜像**](http://laoleng.vip/docs/tools/easyclick/ios-qk/tj/#%E5%88%B7%E5%85%A5%E5%BC%80%E5%8F%91%E8%80%85%E9%95%9C%E5%83%8F)
