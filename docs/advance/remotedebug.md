---
title: EasyClick安卓文档_安卓手机自动化脚本_中控投屏平台说明
hide_title: false
hide_table_of_contents: false
sidebar_label: 远程调试
description: EasyClick 中控投屏说明，用于局域网投屏，管理脚本，同步操作，
keywords:
  - EasyClick
  - 手机自动化脚本
  - 自动化软件
  - 中控投屏
  - 抖音群控
  - 快手群控
  - 游戏群控
  - tip
  - iOS
  - NEXT
  - HID
  - exe
  - ip
  - br
  - img
  - src
  - 手机自动化
  - 自动化测试
---

# 远程调试

:::tip
    - 远程调试适合手机和开发工具异地的情况下
    - 该远程调试需要配合外网的服务器进行，目标是让小白快速使用远程调试功能
    - 该远程调试软件适合安卓开发工具、iOS开发工具、鸿蒙NEXT开发工具、以及HID主控
    - 高手请忽略
:::

## 下载软件
    - 软件下载地址在资源区的网盘中，`远程调试`文件夹 - EasyClick内网穿透远程调试,
    - 下载后解压，直接双击exe即可完成打开
## 服务器配置
    - 去阿里云或者腾讯云购买一个服务器，1核心1G内存1M带宽足够使用
    - 获取到服务器的ip,用户名以及密码后，填写到工具的远程服务器配置中
        <br/><img src='/remotedebug/1.png' style={{zoom:'30%'}}/>
    - 填写完成后，点击`安装内网穿透服务`, 等待安装完成
    - 安装成功后,建议`重启服务器`
## 放行端口
    - 如果你的服务器有安全组或者防火墙，需放行以下端口: 7000、10826、10999、8019、8988、8028
    - 阿里云安全组放行端口: [链接](https://help.aliyun.com/zh/ecs/user-guide/add-a-security-group-rule?spm=5176.21213303.J_qCOwPWspKEuWcmp8qiZNQ.33.23ce2f3dWLJXbk&scm=20140722.S_help@@%E6%96%87%E6%A1%A3@@25471._.ID_help@@%E6%96%87%E6%A1%A3@@25471-RL_%E5%AE%89%E5%85%A8%E7%BB%84%E6%94%BE%E8%A1%8C%E7%AB%AF%E5%8F%A3-LOC_llm-OR_ser-V_4-RE_new3@@cardNew-P0_12)

## 开启调试服务
    - 安卓远程调试，在 EasyClick开发插件中 找到 `HTTP远程调试` 然后打开，不要更改端口
    - iOS USB版打开`USB中控`即可
    - iOS 脱机版本在开发插件中找到 `HTTP远程调试` 然后打开，不要更改端口
    - 鸿蒙NEXT USB版打开`USB中控`即可
    - HID主控直接双击exe运行即可
## 开启穿透服务
    - 在`EasyClick内网穿透远程调试`软件中，点击`启动内网穿透服务` 一直等到成功即可
## 调试链接
    - 安卓远程调试，在EC App的远程调试中，选择http模式，输入 服务器IP，端口是 10826 ，然后链接
    - iOS USB版，在idea开发插件中，选择usb版本链接，输入 http://服务器IP+:8019 进行链接
    - iOS 脱机版，在EC App设置中，找到远程调试栏目，输入 服务器IP+:10999 进行链接
    - 鸿蒙NEXT USB版，在idea开发插件中，选择usb版本链接，输入 http://服务器IP+:8028 进行链接
    - 安卓HID主控，在代码中设置主控地址为 `http://服务器IP+:8988`

:::tip
    - 如果你的HID主控含有linux版本，建议使用linux组网到Windows系统的，手机都插在linux电脑上，然后将Windows主控穿透出去
:::

## 加密服务器信息
    - 如果不想让客户知道你的服务器IP,可将IP填写后,点`生成加密信息`,将加密后的信息发给客户,填入加密框,即可替代IP功能
    - 注意生成时删除密码,否则客户可以操作你的服务器
