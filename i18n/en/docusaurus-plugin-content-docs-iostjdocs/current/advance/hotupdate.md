---
title: EasyClick Automation Scripts_iOS Scripts_iOS No Jailbreak_iOS No Hardware_Advanced Features_Hot Update
hide_title: false
hide_table_of_contents: false
sidebar_label: Hot Update
description: EasyClick automation scripts — iOS no jailbreak — advanced features — code hot update
keywords:
 - EasyClick automation scripts
 - iOS scripts
 - iOS no jailbreak
 - iOS no hardware
 - advanced features
 - hot update
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
 - mobile automation
 - test automation
---

# Code Hot Update

## Official Hot Update Service

- [Official hot update](/docs/advance/netcard#热更新管理)
 :::tip
 - Official hot update supports encrypted links — safe to use
 - Official hot update is shared between Android and iOS standalone editions; usage is the same
 - Official service uses Alibaba Cloud OSS for convenience
 :::

## What Is Hot Update

- Hot update updates critical code without reinstalling the app
- EC hot update mainly updates packaged automation test scripts
- Note: keep `update.json` version in sync with the server response version, or abnormal behavior may occur

## How EC Hot Update Works

- Open `update.json` in the project:

:::tip

- Note: hot update applies to compiled `.iec` files, not APK/IPA
 ::

```json showLineNumbers
{
  "update_url": "http://baidu.com/update",
  "version": "100",
  "appendDeviceInfo": true,
  "timeout": 10000
}
```

- Parameters
 - `update_url`: server update endpoint — you implement this
 - `version`: current script version as integer; app compares versions and prompts only for higher versions
 - `appendDeviceInfo`: `true` attaches basic device info to requests
 - `timeout`: request timeout in milliseconds
- 3.15+ parameters
 :::tip
 3.15.0+ adds more parameters<br/>
 If `update_url` is `http://baidu.com/update`, the request becomes:
 http://baidu.com/update?ecid=000c109803a&systemVersion=15.2&time=1707107343187&deviceId=6acc090e33f76e&model=iPhone&serialNo=F4GSPUAZHG6W&version=100&deviceName=iPhone7&pkgName=xxx
 - ecid: device ECID — usually unchanged even after device spoofing
 - systemVersion: OS version
 - time: request time in milliseconds
 - deviceId: device UDID
 - model: device model
 - serialNo: serial number
 - deviceName: device name
 - version: script version from update.json
 - pkgName: app bundle ID
 :::

## EC Loads New Package

### Client Request

- After configuration, packaged app auto-sends GET to `update_url` with parameters;
 e.g. `http://baidu.com/update?version=100` — server can compare versions; client also compares locally

### Server Response

- Server response format:
- Normal update
 :::tip
- Hot update applies to compiled `.iec` files
- If no update needed, return empty string — not JSON
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

- Strict mode with MD5 check to prevent failed updates

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

- `download_url`: new package download URL
- `download_timeout`: IEC download timeout
- `version`: new package version
- `md5`: IEC file MD5 — if present, file integrity is strictly verified
- EC downloads and loads the latest IEC package from this JSON
- `dialog`: show dialog — `true` = dialog, `false` = no dialog
- `msg`: message shown in dialog
- `force`: in dialog mode, force update — `true` = cannot cancel, `false` = optional

## UI Startup Update

- With correct configuration, the UI auto-updates on open

## In-Script Hot Update

- Hot update during script execution requires code

### hotupdater.updateReq — Request Update

* Request hot update endpoint. `false` may also mean no update needed — use `getErrorMsg` for details
* Version: EC standalone 3.19.0+
* @param updateUrl Update URL — omit to use update.json config
* @param version Current version as integer, e.g. 1
* @param appendDeviceInfo Append device info — true or false
* @param timeout Request timeout in milliseconds
* @return `{bool}` true = update available, false = no update

```javascript showLineNumbers
function main() {
    // Get version from project update.json
    let version = JSON.parse(readIECFileAsString("update.json")).version
    // Or set version manually
    // let version = 7;
    // Request server for new version
    // Using update.json mode
    //let updateResult = hotupdater.updateReq("",version,true,9000);
    // Custom URL mode
    let updateResult = hotupdater.updateReq("http://baidu.com", version, true, 9000);
    logd("Update available: " + updateResult);
    if (!updateResult) {
        logw("Request failed: " + hotupdater.getErrorMsg());
    } else {
        logd("Response data: " + hotupdater.getUpdateResp());
        // Download new version if update available
        let path = hotupdater.updateDownload();
        logd("Download path: " + path);
        if (!path) {
            logw("IEC download error: " + hotupdater.getErrorMsg());
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

### hotupdater.updateDownload — Download IEC

* Download IEC file from hot update request
* Version: EC standalone 3.19.0+
* @return `{string}` path to downloaded hot update file; empty may mean no update
* See [example](/iostjdocs/advance/hotupdate#hotupdaterupdatereq--request-update)


### hotupdater.getUpdateResp — Get Request Result

* Get hot update request result
* Version: EC standalone 3.19.0+
* @return `{string}` string
* See [example](/iostjdocs/advance/hotupdate#hotupdaterupdatereq--request-update)


### hotupdater.getErrorMsg — Get Error Message

* Get hot update error message
* Version: EC standalone 3.19.0+
* @return `{string}` string
* See [example](/iostjdocs/advance/hotupdate#hotupdaterupdatereq--request-update)


