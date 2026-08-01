---
title: EasyClick安卓文档_安卓手机自动化脚本_云控版本更新记录
hide_title: false
hide_table_of_contents: false
sidebar_label: 云控安装
description: >-
  EasyClick 云控平台专门用于用于管理脚本、任务,
  安装云控非常简单，使用宝塔一键安装mysql，redis套件，然后复制云控程序到网站目录，更改mysql配置，启动ecloud二进制文件即可
keywords:
  - EasyClick
  - 手机自动化脚本
  - 自动化软件
  - 云控平台安装
  - 抖音云控
  - 快手云控
  - 游戏云控
  - mysql
  - redis
  - iOS
  - ecloudgo
  - nginx
  - USB
  - config
  - link
  - loc
  - Local
  - 手机自动化
---

# 云控安装
- 安卓和脱机iOS、iOS USB共用一套云控系统，安装流程一样

:::tip 
- 云控 5.5.0+以上版本支持 安卓、脱机版本iOS、USB版本iOS
:::

:::danger 升级提示
- 云控 从老版本升级到5.7.0以上版本，需要更改config/config.toml文件中的[database.link]mysql配置
- 在后面加上?loc=Local&parseTime=true,例如 link = "mysql:ecloudgo:ecloudgo@tcp(localhost:3306)/ecloudgo?loc=Local&parseTime=true"
- 防止数据入库的时间不对

:::

## 安装

### 必备的环境

- linux操作系统，ubuntu 18.04或者centos之类，windows也可以
- mysql 5.7
- redis 4
- supervisor进程守护
- 可以使用宝塔一键安装mysql，redis，以下的教程都以宝塔为基础



### 安装mysql，nginx, redis
- 1、进入宝塔,如果弹出以下窗口选择myql5.7，点击安装
<img src="/ecloudimages/image-20211203144143477.png" alt="image-20211203144143477" style={{zoom:'33%'}} />

- 2、如果没就到软件管理，搜索nginx，mysql，phpmyadmin进行安装
- 3、安装redis，进入软件商店，搜索redis
- 4、安装supervisor进程守护，进入软件商店，搜索supervisor
<img src="/ecloudimages/image-20211203144321514.png" alt="image-20211203144321514" style={{zoom:'33%'}} />

- 安装成功后点击在首页显示



<img src="/ecloudimages/image-20211203144420239.png" alt="image-20211203144420239" style={{zoom:'33%'}} />

###

### 下载云控程序

- 1、群文件或者网盘中下载ecloud开头的zip包

### 上传云控程序

- 1、先在宝塔的文件管理新建文件夹ecloudgo

<img src="/ecloudimages/image-20211203144601138.png" alt="image-20211203144601138" style={{zoom:'33%'}} />





> 2、通过宝塔的文件管理上传zip包

<img src="/ecloudimages/image-20211203144634238.png" alt="image-20211203144634238" style={{zoom:'33%'}} />



> 3、解压二进制文件

<img src="/ecloudimages/image-20211203145105395.png" alt="image-20211203145105395" style={{zoom:'33%'}} />

<img src="/ecloudimages/image-20211203145114564.png" alt="image-20211203145114564" style={{zoom:'33%'}} />



### 新建数据库

```text showLineNtumbers
前提是mysql redis都安装好了！！！宝塔左上角 橙色中的为 0  代表都安装完成了
```

<img src="/ecloudimages/image-20211203145436463.png" alt="image-20211203145436463" style={{zoom:'25%'}} />


- 新建一个用户名、密码、数据库名都为ecloudgo的数据库实例

<img src="/ecloudimages/image-20211203145825106.png" alt="image-20211203145825106" style={{zoom:'33%'}} />



- 数据库权限授权
- root用户进入phpmyadmin，点击phpMyAdmin按钮



<img src="/ecloudimages//image-20211203151414051.png" alt="image-20211203151414051" style={{zoom:'33%'}} />



- 选择phpmyadmin的用户一栏

<img src="/ecloudimages/image-20211203151451822.png" alt="image-20211203151451822" style={{zoom:'33%'}} />



- 点击红色框中的编辑权限，然后选择全选


<img src="/ecloudimages/image-20211203151534953.png" alt="image-20211203151534953" style={{zoom:'33%'}} />

- 点击执行

<img src="/ecloudimages/image-20211203151554359.png" alt="image-20211203151554359" style={{zoom:'33%'}} />




- 变为以下状态就是权限分配成功

<img src="/ecloudimages/image-20211203151718322.png" alt="image-20211203151718322" style={{zoom:'33%'}} />



### 编辑数据库配置

- 进入文件管理点击编辑，找到config.toml



<img src="/ecloudimages/image-20211203145911611.png" alt="image-20211203145911611" style={{zoom:'33%'}} />


- 看到下面三个选项，全部更改为ecloudgo

<img src="/ecloudimages/image-20211203150103562.png" alt="image-20211203150103562" style={{zoom:'33%'}} />



### 配置进程启动

- 进入首页，选择supervisor

<img src="/ecloudimages/image-20211203150553998.png" alt="image-20211203150553998" style={{zoom:'33%'}} />

- 点击添加进程守护


<img src="/ecloudimages/image-20211203150616509.png" alt="image-20211203150616509" style={{zoom:'33%'}} />


- 选择目录和执行文件

<img src="/ecloudimages/image-20211203150723547.png" alt="image-20211203150723547" style={{zoom:'33%'}} />

- 启动成功


<img src="/ecloudimages/image-20211203150742524.png" alt="image-20211203150742524" style={{zoom:'33%'}} />

- 日志查看是否成功
<img src="/ecloudimages//image-20211203150820652.png" alt="image-20211203150820652" style={{zoom:'33%'}} />

<img src="/ecloudimages//image-20211203150827519.png" alt="image-20211203150827519" style={{zoom:'33%'}} />




### 防火墙放行端口

- 新的云控端口是 8098

- 进入宝塔安全菜单，放行 8098端口
- 如果遇到提示nginx防火墙升级，就进入软件商店进行安装nginx防火墙

<img src="/ecloudimages/image-20211203151152772.png" alt="image-20211203151152772" style={{zoom:'33%'}} />

<img src="/ecloudimages/image-20211203151226538.png" alt="image-20211203151226538" style={{zoom:'33%'}} />



- 阿里云放行端口:  https://help.aliyun.com/document_detail/25471.html?spm=a2c6h.13066369.0.0.1eec1ecfVVJqw6&source=5176.11533457&userCode=28kqeewo&type=copy

- 腾讯云放行端口:  https://cloud.tencent.com/developer/article/1841261?from=15425



### 访问云控

- 用你的 ip:8098 在浏览器打开即可，默认的用户名是admin 默认的密码是 admin123

## 授权
### 网站授权

- 云控是按照设备量授权的，授权请找当地代理商或者q: 2557945562

- 产品序列号在 站点配置 - 站点授权页面

### APK 链接和授权

- 使用调试版本的apk的时候，进入企业云设置，输入远程IP或者域名，点击测试链路

<img src="/ecloudimages//image-20211203153342198.png" alt="image-20211203153342198" style={{zoom:'33%'}} />

- 使用开发工具打包，无需在apk填写域名
- 一定要打包的的时候输入域名，并且打企业版本的包，这里请看云控域名配置章节
- 设备授权
 - 进入设备管理-设备列表-未授权设备按钮点击下，再点击分配即可


<img src="/ecloudimages//image-20211203153609841.png" alt="image-20211203153609841" style={{zoom:'33%'}} />



## 优化升级

### 升级
:::danger 升级提示
- 云控 从老版本升级到5.6.0以上版本，需要更改config/config.toml文件中的[database.link]mysql配置
- 在后面加上?loc=Local&parseTime=true,例如 link = "mysql:ecloudgo:ecloudgo@tcp(localhost:3306)/ecloudgo?loc=Local&parseTime=true"
- 防止数据入库的时间不对
:::
- 升级前请备份数据库和文件！！！

- 1、上传单独的ecloudgo二进制文件到网站根目录

<img src="/ecloudimages//image-20211203152131132.png" alt="image-20211203152131132" style={{zoom:'33%'}} />

- 2、上传config中的内容到网站的config文件夹下

- 3、编辑数据库信息，请看上面的编辑数据库配置

- 4、配置supervisor，修改为对应的二进制执行文件即可


<img src="/ecloudimages//image-20211203152321572.png" alt="image-20211203152321572" style={{zoom:'33%'}} />



- 点击重启



<img src="/ecloudimages//image-20211203152346117.png" alt="image-20211203152346117" style={{zoom:'33%'}} />







### 服务器优化

- 部分服务器应对大流量的情况 并未做优化

- 1、请在控制台运行./opt.sh文件
- 2、supervisor的配置中新增 一行 minfds=65535 ，主配置和子配置都新增，然后重启云控即可

<img src="/ecloudimages//image-20211203152851539.png" alt="image-20211203152851539" style={{zoom:'33%'}} />

<img src="/ecloudimages//image-20211203152918388.png" alt="image-20211203152918388" style={{zoom:'33%'}} />



## 设备链接
### 安卓设备链接
- apk调试模式下，进入EC APP，点击左上角的`三横线`进入菜单，进入`企业云配置`
- 第一个【远程IP或者域名】填写你的云控地址
- 第二个 设备编号，自己编号好，和云控设备列表的 编号保持一致
- 第三个 设备ID 忽略不管
- 点击测试链路，如果成功，划掉app进程，重启即可
:::tip
- 如果是正式发布，请打包企业版本apk，并在云控一栏，填写云控地址，其他编号等安装好apk后进行编号
- 如果是调试版本，如果出现数字签名错误问题，请链接idea，运行一次脚本或者预览一次UI
- 更多域名配置情况 [打包设置](/docs/zh-cn/ecloud2/pkgset)
:::



### iOS脱机
- 打开EC 的APP脱机版本，进入设置界面，找到【云控设置】，输入云控地址和编号，与后台编号对应
- 点击测试，如果成功，就重启app后，再打开，就自动链接云控了
### iOS USB版本链接
- 打开EC iOS USB版本中控，进入【中控设置】
- 找到【云控地址】一栏，填写云控地址，点击保持，然后重启中控
:::tip
- 由于中控没有设置编号的地方，默认优选使用【ecid】作为编号，如果在中控设置中，设置了云控设备编号使用【别名】，编号就是别名
- 所以这里要注意，如果设置了别名，不要用中文，尽量用数字+英文，防止链接问题
:::

### 鸿蒙Next USB版本链接
- 打开EC 鸿蒙Next USB版本中控，进入【中控设置】
- 找到【云控地址】一栏，填写云控地址，点击保持，然后重启中控
:::tip
- 由于中控没有设置编号的地方，默认优选使用【设备序列号】作为编号，如果在中控设置中，设置了云控设备编号使用【别名】，编号就是别名
- 所以这里要注意，如果设置了别名，不要用中文，尽量用数字+英文，防止链接问题
:::

## MySQL 8.0 
### 监控页面获取数据失败
```sql
## 报错sql
device_monitor.go:77: SELECT COUNT(1) FROM `t_device` AS a LEFT JOIN `t_device_group` as b on a.group_id=b.id LEFT JOIN `sys_user` as c on c.id = a.tenant_id LEFT JOIN `t_device_online` as ol on a.device_no=ol.device_no WHERE `a`.`status`='0': Error 1267 (HY000): Illegal mix of collations (utf8mb4_0900_ai_ci,IMPLICIT) and (utf8mb4_general_ci,IMPLICIT) for operation '=' 
```
- 问题原因：
  - MySQL 5.7.34 升级到 8.0.32 后部分查询语句报错如下：
  - ERROR 1267 (HY000): Illegal mix of collations (utf8mb4_general_ci,IMPLICIT) and (utf8mb4_0900_ai_ci,IMPLICIT) for operation '='
- 解决办法参考 https://zhuanlan.zhihu.com/p/705783937  https://blog.csdn.net/wangchange/article/details/139231166 

## 云控不下发任务
```text
不下发任务有以下几种情况：

1、设备不在线
2、任务配置了单次，已经执行过了
3、任务配置的循环，但是执行次数已经执行完毕
4、脚本未能正确下载
5、用户列表-云控设定-开启了单设备过期时间，但是设备列表的设备编号未设置正确过期时间或者已过期
对于2，3问题，需要清理一下执行记录，
在操作按钮理， 有清理功能

- 如果分配了租户，请给租户分配设备量和过期时间，在用户列表-云控设定按钮，这个不设置也可能导致租户下发任务失败

监控列表 - 点击设备编号，弹出黑色窗口 ，可以看到具体设备任务日志

```
