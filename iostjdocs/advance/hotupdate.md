---
title: EasyClick自动化脚本_iOS脚本_iOS免越狱_iOS免硬件_高级功能_热更新
hide_title: false
hide_table_of_contents: false
sidebar_label: 热更新
description: EasyClick 自动化脚本 iOS免越狱 高级功能 代码热更新
keywords:
  - EasyClick自动化脚本
  - iOS脚本
  - iOS免越狱
  - iOS免硬件
  - 高级功能
  - 热更新
  - iOS
  - EC
  - hotupdater
  - update
  - tip
  - json
  - version
  - EasyClick
  - UI
  - updateReq
  - 手机自动化
  - 自动化测试
---

# 代码热更新

## 官方热更新服务

- [点我进入官方热更新](/docs/advance/netcard#热更新管理)
  :::tip
    - 官方热更新服务支持加密链接可以放心使用
    - 官方热更新安卓和iOS 脱机版本是通用的,使用方式也是一样
    - 官方热更新服务对接阿里云的oss服务，更加方便
      :::

## 什么是热更新

- 热更新一般用于不用安装程序即可将关键的代码进行更新
- EC的热更新主要用于更新打包后的自动化测试脚本
- 注意: 一定要保持update.json文件和服务端接口返回的版本好一直，否则可能导致异常情况

## EC如何热更新

- 打开工程下面的update.json，内容如下:

:::tip

- 注意:热更的是编译的iec文件,不是apk
  ::

```json showLineNumbers
{
  "update_url": "http://baidu.com/update",
  "version": "100",
  "appendDeviceInfo": true,
  "timeout": 10000
}
```

- 参数解析
    - update_url: 代表服务端的更新接口，需要自己编写服务端接口
    - version: 代表当前脚本的版本号，- version写整数，app会自动做版本大小对比，大于的版本才会提示升级
    - appendDeviceInfo: true代表请求的时候附加上基础设备信息
    - timeout: 单位是毫秒，代表请求更新地址的超时时间
- 3.15新增参数
  :::tip
  3.15.0+版本新增了更多参数<br/>
  如果update_url配置了 http://baidu.com/update，请求地址将会变成
  http://baidu.com/update?ecid=000c109803a&systemVersion=15.2&time=1707107343187&deviceId=6acc090e33f76e&model=iPhone&serialNo=F4GSPUAZHG6W&version=100&deviceName=iPhone7&pkgName=xxx
    - ecid: 设备的ecid，一般改机也不会变化
    - systemVersion: 系统的版本
    - time: 请求时间 毫秒
    - deviceId: 设备的udid
    - model: 机型
    - serialNo: 设备序号
    - deviceName: 设备名称
    - version: 当前的脚本的版本，update.json文件中配置的
    - pkgName: 当前重新的包名，也就是bundleId
      :::

## EC 加载新包

### 客户端请求

- 以上配置好后，打包运行，程序会自动使用GET方式请求 update_url对应的地址并且会带上参数；
  例如 ：http://baidu.com/update?version=100 可以自行在服务端做版本比较，客户端自己也做了版本大小比较

### 服务端返回

- 服务端返回格式如下：
- 普通更新
  :::tip
- 热更的是编译的iec文件
- 如无需更新,服务器直接返回空字符串即可,不要返回json
  :::

```json showLineNumbers
{
  "download_url": "http://baidu.com/aaa.iec",
  "version": "101",
  "download_timeout": 120,
  "dialog": true,
  "msg": "优化部分问题",
  "force": false
}
```

- 严格方法,校验md5,防止更新失败

```json showLineNumbers
{
  "download_url": "http://baidu.com/aaa.iec",
  "version": "101",
  "download_timeout": 120,
  "dialog": true,
  "msg": "优化部分问题",
  "force": false,
  "md5": "服务器自行校验的iec文件的md5值"
}
```

- download_url： 代表新包的下载地址
- download_timeout: 代表下载iec文件超时时间
- version：代表新包的版本号
- md5: iec文件的MD5，如果有这个值会强制校验文件的准确性，保证文件一定是完整的
- 返回这样的格式的JSON后，EC会下载最新的IEC包并加载使用。
- dialog: 代表是否用对话框的形式展示，true 代表是对话框，false代表不是
- msg: 对话框中要显示的消息
- force : 代码对话框模式下是否强制更新，true 代表强制更新，无法取消，false 代表不是强制更新

## UI启动更新

- 如果上述配置无误，打开界面会自动更新

## 脚本内热更新

- 脚本执行期间可以做热更新操作，需要配合代码来进行执行

### hotupdater.updateReq 请求更新

* 请求热更新接口，如果是false，也有可能是无需更新，可以使用getErrorMsg查看具体得信息
* 适用版本(EC 脱机 3.19.0+)
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
* 适用版本(EC 脱机 3.19.0+)
* @return `{string}` 下载后热更新文件得路径，如果为空，也有可能是无需更新
* 详细代码看[例子](/iostjdocs/advance/hotupdate#hotupdaterupdatereq-请求更新)


### hotupdater.getUpdateResp 获取请求结果

* 获取热更新得请求结果
* 适用版本(EC 脱机 3.19.0+)
* @return `{string}` 字符串
* 详细代码看[例子](/iostjdocs/advance/hotupdate#hotupdaterupdatereq-请求更新)


### hotupdater.getErrorMsg 获取错误信息

* 获取热更新重新的错误
* 适用版本(EC 脱机 3.19.0+)
* @return `{string}` 字符串
* 详细代码看[例子](/iostjdocs/advance/hotupdate#hotupdaterupdatereq-请求更新)


