---
title: EasyClick安卓文档_安卓手机自动化脚本_代码热更新
hide_title: false
hide_table_of_contents: false
sidebar_label: 代码热更新
description: 'EasyClick 代码热更新,不需要更新apk，直接热更新代码，免去安卓apk的的繁琐步骤，简单的配置即可实现热更新'
keywords:
  - EasyClick
  - 手机自动化脚本
  - 自动化软件
  - 脚本热更新
  - 代码热更新
  - 免root热更新
  - EC
  - hotupdater
  - update
  - docs
  - zh
  - cn
  - advance
  - netcard
  - json
  - appendDeviceInfo
  - 手机自动化
  - 自动化测试
---

# 代码热更新

## 官方热更新服务
- [点我进入官方热更新](/docs/advance/netcard#热更新管理)

## 什么是热更新

:::tip
热更的是编译的iec文件,不是打包的脚本apk
:::

- 热更新一般用于不用安装程序即可将关键的代码进行更新
- EC的热更新主要用于更新打包后的自动化测试脚本
- 注意: 一定要保持update.json文件和服务端接口返回的版本好一直，否则可能导致异常情况

## EC如何热更新
- 如果不想配置，官方有热更新服务， [点我进入官方热更新](/docs/advance/netcard#热更新管理)

打开工程下面的update.json，内容如下:


 ```json showLineNumbers
{
  "update_url": "http://baidu.com/update",
  "version": "1.0.0",
  "appendDeviceInfo": true,
  "timeout": 30000
}
```

- 参数解析
    - update_url: 代表服务端的更新接口，需要自己编写服务端接口
    - version: 代表当前脚本的版本号
    - timeout: 9.13.0+ 支持请求地址的超时参数，单位是毫秒，最低是1000
    - appendDeviceInfo: 9.23.0+ 是否在请求的时候附加上基础的设备信息
      - 如果 appendDeviceInfo 为true,请求的url会自动加上以下参数,之前的URL的参数还是存在的


:::tip
version=1&deviceId=7521e5d9eeec4f58b71dea8b78c414d5&apkVersion=9.22.0&osVersion=12
&pkgName=com.gibb.easyclick&model=LNA-AL00&ecVersion=9.22.0&brand=HUAWEI&androidId=82a3b055470ebe1a
- 参数说明:
- version: 当前运行的iec版本
- deviceId: EC生成的设备ID，可能会丢失，不一定能作为设备唯一标识
- apkVersion: apk打包的版本
- osVersion: 系统版本
- ecVersion: 真正的使用的EC 版本
- pkgName: 打包的包名
- model: 机型
- brand: 品牌
- androidId: Android的ID

:::
## EC 加载新包

### 客户端请求

- 以上配置好后，打包运行，程序会自动使用GET方式请求 update_url对应的地址并且会带上参数； 例如 ：http://baidu.com/update?version=1.0.0   请自行在服务端做版本比较

### 服务端返回
服务端返回格式如下：
:::tip
热更的是编译的iec文件,不是打包的脚本apk,
如无需更新, 服务器直接返回空字符串即可, 不要返回json,

:::

- 普通更新

```json showLineNumbers
{
  "download_url": "http://baidu.com/aaa.iec",
  "version": "1.1.0",
  "dialog": true,
  "msg": "优化部分问题",
  "force": false
}
```

- 严格方法,校验md5,防止更新失败

```json showLineNumbers
{
  "download_url": "http://baidu.com/aaa.iec",
  "version": "1.1.0",
  "dialog": true,
  "msg": "优化部分问题",
  "force": false,
  "md5": "服务器自行校验的iec文件的md5值",
  "download_timeout": 60
}
```

- download_url： 代表新包的下载地址
- version：代表新包的版本号
- md5: iec文件的MD5，如果有这个值会强制校验文件的准确性，保证文件一定是完整的
- 返回这样的格式的JSON后，EC会下载最新的IEC包并加载使用。
- dialog: 代表是否用对话框的形式展示，true 代表是对话框，false代表不是
- msg: 对话框中要显示的消息
- force : 代码对话框模式下是否强制更新，true 代表强制更新，无法取消，false 代表不是强制更新
- download_timeout: 7.11.0+ 新增的属性，下载iec超时时间，单位是秒，不填写默认是60秒



## UI启动更新

- 如果上述配置无误，打开界面会自动更新

## 脚本内热更新

- 脚本执行期间可以做热更新操作，需要配合代码来进行执行

### hotupdater.updateReq 请求更新

* 请求热更新接口，如果是false，也有可能是无需更新，可以使用getErrorMsg查看具体得信息
* @param updateUrl 更新地址 不写，就使用update.json配置的数据
* @param version 当前版本，使用整形数据，例如 1这样的数字
* @param appendDeviceInfo 是否拼接设备信息数据 true 或者 false
* @param timeout 请求超时时间 单位是毫秒
* @return `{bool}` true 代表需要更新 false代表无需更新

```javascript showLineNumbers
function main() {
    // 从项目文件的update.json中获取版本号
    let version = JSON.parse(readIECFileAsString("update.json")).version
    // 手动定义版本
    // let version = 7;
    toast("Hello World - " + version);
    //请求服务器是否有新版本
    // 使用update.json模式
    //let updateResult = hotupdater.updateReq("",version,true,9000);
    // 使用自定义的模式 url 
    let updateResult = hotupdater.updateReq("http://baidu.com", version, true, 9000);
    logd("请求更新是否有: " + updateResult);
    if (!updateResult) {
        logw("请求失败错误信息: " + hotupdater.getErrorMsg());
    } else {
        logd("请求数据: " + hotupdater.getUpdateResp());
        //有更新得情况下进行下载新的版本
        let path = hotupdater.updateDownload();
        logd("下载路径为: " + path);
        if (!path) {
            logw("下载IEC文件错误信息: " + hotupdater.getErrorMsg());
        } else {
            restartScript(path, true, 3)
            return;
        }
    }
    sleep(1000);
    for (var i = 0; i < 10; i++) {
        logd(time() + " " + version);
        sleep(5000)
    }
}

main();
```

### hotupdater.updateDownload 请求下载IEC

* 下载热更新请求到得IEC文件
* 适用版本(EC 5.20.0+)
* @return `{string}` 下载后热更新文件得路径，如果为空，也有可能是无需更新
* 详细代码看[例子](/docs/advance/hot-update#hotupdaterupdatereq-请求更新)


### hotupdater.getUpdateResp 获取请求结果

* 获取热更新得请求结果
* 适用版本(EC 5.20.0+)
* @return `{string}` 字符串
* 详细代码看[例子](/docs/advance/hot-update#hotupdaterupdatereq-请求更新)


### hotupdater.getErrorMsg 获取错误信息

* 获取热更新重新的错误
* 适用版本(EC 5.20.0+)
* @return `{string}` 字符串
* 详细代码看[例子](/docs/advance/hot-update#hotupdaterupdatereq-请求更新)

