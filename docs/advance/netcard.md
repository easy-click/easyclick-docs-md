---
title: EasyClick安卓文档_安卓手机自动化脚本_网络验证
hide_title: false
hide_table_of_contents: false
sidebar_label: 网络验证平台
description: 'EasyClick 代码热更新,无需无障碍和USB调试实现自动化'
keywords:
  - EasyClick
  - 手机自动化脚本
  - 自动化软件
  - 脚本热更新
  - 代码热更新
  - 无需无障碍和USB调试实现自动化
  - http
  - uc
  - ieasyclick
  - com
  - apk
  - iec
  - oss
  - OSS
  - appid
  - 手机自动化
  - 自动化测试
  - 脚本开发
---
# 网络平台使用说明

## 登录并注册
- 网址 [http://uc.ieasyclick.com](http://uc.ieasyclick.com)
- 注册后登录到系统

## 软件列表
- 新增一个软件，就会获取到appid和密钥，这个是ecNetCard.netCardInit 初始化卡密需要的参数
- 字段说明：
  - 软件类型: 注意选择你使用的安卓还是iOS，选择错了可能无法正确匹配参数
  - 验证包指纹: 这个是对于apk是否进行验证，需要在脚本列表中上传apk文件
  - 验证脚本指纹: 这个是对脚本的iec文件进行验证，也是需要在脚本列表中上传iec文件
  - 心跳错误次数: 这个是心跳请求容错的设置，例如网络不通等情况一个容错率，达到设置的标准值才会在在脚本中提示错误
  - 卡密验证错误次数: 这个是对卡密进行容错的设置，也是为了防止误判，达到了设置的标准值才会提示错误
  - 脚本验证错误次数: 这个是对脚本文件进行验证的容错设置，也是为了防止误判，达到了设置的标准值才会提示错误
  - 开启验证: 这个选项是是否验证整个软件验证的开发，开发期间可以设置否
  - 状态: 选择禁用，这个卡密都是不可用的，脚本也会收到错误信息
<br/><img src='/androidimg/netcard-1.png' width="300" />
    
:::tip
建议开发期间，不要开启验证包指纹和验证脚本指纹，因为还没有上传iec和apk进行验证，
可以将打包的apk和iec保留，后期发下有被破解或者想禁用的情况，就进行上传apk和iec并开启验证
:::

## 脚本列表
- 脚本列表相当于写的一个脚本，并且编译为iec文件,这个归纳为一个脚本信息记录
- 字段说明
  - 软件版本: 相当于脚本的版本信息，比如1.0等
  - 软件包名: 只有安卓才会填写打包的apk包名，填写了会开启验证，如果apk包名匹配失败，脚本将无法运行
  - 包指纹: apk的文件指纹信息，如果填写了，将会验证apk文件，
  - 脚本指纹: 打包好的iec文件指纹，如果填写了，将会验证iec文件信息
<br/><img src='/androidimg/netcard-2.png' width="300" />

:::tip
软件包名、包指纹、脚本指纹三选一填写，这里上传不会记录你的文件，会计算文件的md5值，请放心使用
另外计时不新建脚本列表，网络验证也是可以正常使用的
:::

## 卡密管理
- 卡密管理是用来生成网络验证卡，支持一个设备一张卡，也支持多个设备一张卡
- 新增字段说明
  - 生成数量: 代码要生产多少张卡
  - 卡有效天数: 卡密失效的日期，卡第一次绑定设备，将会开始计时，不使用不计时
  - 解绑密码: 卡绑定到设备后，如果想解绑，就需要使用这个密码，不设置密码说明不用密码即可解绑
  - 顶号功能: 对于一张卡多个设备使用情况，如果这个选项开启，对于后请求的设备，会自动将最先绑定的设备踢下线，以此类推
  - 状态: 禁用状态卡密将无法使用，会在脚本中提示错误
  <br/><img src='/androidimg/netcard-3.png' width="300" />
- 批量操作
  - 批量加时间: 对于选择的卡，会自动加上时间，避免卡过期，也不需要客户重复绑定卡号
  - 批量加在线量: 对于已选择的卡，增加可以承受的设备量，不需要客户重复绑卡
  <br/><img src='/androidimg/netcard-4.png' width="300" />


:::tip
生成卡密和批量操作卡，都会根据选项进行动态计算要扣除的金额，同意扣除后，就会生成卡密，请保证余额充足。
充值可以找沙书记qq 2557945562 或者客服qq 2050858539
:::

## 云端变量
- 云端变量是在云端设置一个变量，通过 ecNetCard.netCardGetCloudVar 函数获取后在脚本中动态使用
- 这个变量可以是普通的字符串，也可以是一串js代码，通过在脚本中通过eval执行
- 字段说明
  - 变量名称: 变量名称，可以是中文可以是因为，是ecNetCard.netCardGetCloudVar的参数
  - 变量内容: 变量对应的内容
  - 支持远程修改: 是否可以通过脚本对这个值进行修改，修改函数是 ecNetCard.netCardUpdateCloudVar
    <br/><img src='/androidimg/netcard-5.png' width="300" />

## 热更新管理
- 网络验证自带的热更新管理，对接阿里云的OSS存储系统，可以直接上传iec文件

### 生成阿里云oss配置
- 获取阿里云的 AccessKeyId 和AccessKeySecret
- 进入 ：https://ram.console.aliyun.com/users/create , 点击创建用户
  <br/><img src='/img/oss/1.png' width="300" />
- 用户名随便写，勾选控制台访问和openapi调用访问，点击确定
  <br/><img src='/img/oss/2.png' width="500" />

- 创建用户完成后，即可进入复制AccessKeyId和AccessKeySecret界面了，点击***复制***按钮即可
  - **一定记得及时复制 AccessKeyId和AccessKeySecret**
<br/><img src='/img/oss/4.png' width="500" />  
<br/><img src='/img/oss/3.png' width="500" />
- 用户授权
- 选择用户，点击添加权限按钮，勾选 AliyunOSSFullAccess ,点击确认新增授权即可
  <br/><img src='/img/oss/5.png' width="500" />
  <br/><img src='/img/oss/6.png' width="500" />

### 配置OSS信息
- 按照 **生成阿里云oss配置** 生成授权 AccessKeyId和AccessKeySecret,进行配置即可
- 点击OSS配置管理，新增OSS存储，根据提示填写阿里云的OSS信息，
  - 需要填写取 AccessKeyId 和AccessKeySecret [OSS如何获取AccessKeyId和AccessKeySecret?点我了解 ](https://www.aliyun.com/search?k=oss+accessKeyId&scene=all)
  - 遇到**You have no right to access this object because of bucket acl.**错误，
    - 解决办法: https://blog.csdn.net/weixin_38106322/article/details/106873104
    - https://blog.csdn.net/qq_53763141/article/details/136428817
  - 如果上面操作了，**还是有权限问题， 就手动去阿里云oss创建bucket , 创建的 bucket 还需要开启 公共读**
  <br/><img src='/androidimg/cardhotupdate-1.png' width="300" />
  - 新增OSS存储示意图
  <br/><img src='/androidimg/cardhotupdate-2.png' width="300" />

### 上传热更新脚本文件
- 点击新增脚本,填写并上传iec脚本文件，版本配置为整数，**上传脚本之前，要到软件列表中，新建一个软件**
  <br/><img src='/androidimg/cardhotupdate-3.png' width="300" />
- 上传脚本后，会自动根据配置文件传到阿里云的OSS存储中，并且生成下载地址和md5
- 点击保存即可，其他的编辑、启用、对话框等和之前热更新配置一样

### 脚本配置热更新地址
- 进入软件列表，鼠标放在软件名称上面或者在右侧【复制热更新地址】
  <br/><img src='/androidimg/cardhotupdate-4.png' width="300" />
- 复制地址后，将地址放到脚本的update.json文件中即可
- 提示：新版本热更地址适合EC 安卓9.31.0+ 、iOS脱机版本3.17.0+,通信是安全加密的
- 老地址是为了兼容老的，通信是不加密的
- 如果需要屏蔽老地址，在软件列表中编辑单个软件信息，设置热更新兼容选项为仅新版本

## 防破解建议
:::tip
- 使用网络验证卡密
- 部分关键代码进行混淆
- 开启包名、apk、脚本指纹验证，稍微繁琐一些，每次打包都需要进行上传记录
- 增加关键业务代码的远程变量和远程js代码，在发现被破解，及时替换远程的代码并停止脚本
- 有条件的作者，可以自行加入一些破坏性的代码在手机中，例如格式手机sdcard、删除联系人等，发现有破解，可以通过远程变量加入该功能进行操作
:::
