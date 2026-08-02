---
title: EasyClick Android docs — Hot update
hide_title: false
hide_table_of_contents: false
sidebar_label: Hot update
description: 'EasyClick hot update — update script code without rebuilding the APK; simple configuration for hot updates'
keywords:
 - EasyClick
 - mobile automation scripts
 - automation software
 - script hot update
 - code hot update
 - non-root hot update
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
 - mobile automation
 - test automation
---

# Hot update

## Official hot-update service
- [Official hot update](/docs/advance/netcard#热更新管理)

## What is hot update?

:::tip
Hot updates apply to compiled `.iec` files, not packaged script APKs
:::

- Hot update lets you refresh critical code without reinstalling the app
- EC hot update is mainly for packaged automation test scripts
- Note: keep `update.json` version in sync with your server response, or unexpected behavior may occur

## How EC hot update works
- Prefer the official service: [Official hot update](/docs/advance/netcard#热更新管理)

Open `update.json` in your project:


 ```json showLineNumbers
{
  "update_url": "http://baidu.com/update",
  "version": "1.0.0",
  "appendDeviceInfo": true,
  "timeout": 30000
}
```

- Parameters
 - `update_url`: your server update endpoint — implement this yourself
 - `version`: current script version
 - `timeout`: 9.13.0+ request timeout in ms (minimum 1000)
 - `appendDeviceInfo`: 9.23.0+ append basic device info to the request
 - When `true`, these query params are added automatically (existing URL params remain):


:::tip
version=1&deviceId=7521e5d9eeec4f58b71dea8b78c414d5&apkVersion=9.22.0&osVersion=12
&pkgName=com.gibb.easyclick&model=LNA-AL00&ecVersion=9.22.0&brand=HUAWEI&androidId=82a3b055470ebe1a
- Parameter reference:
- `version`: running IEC version
- `deviceId`: EC-generated device ID — may be lost; not always a stable device identifier
- `apkVersion`: packaged APK version
- `osVersion`: OS version
- `ecVersion`: actual EC runtime version
- `pkgName`: package name
- `model`: device model
- `brand`: brand
- `androidId`: Android ID

:::
## EC loads the new package

### Client request

- After configuration, build and run — the app sends a GET to `update_url` with parameters, e.g. `http://baidu.com/update?version=1.0.0`. Compare versions on your server.

### Server response
Expected response format:
:::tip
Hot updates apply to compiled `.iec` files, not packaged script APKs.
If no update is needed, return an empty string — not JSON.

:::

- Standard update

```json showLineNumbers
{
  "download_url": "http://baidu.com/aaa.iec",
  "version": "1.1.0",
  "dialog": true,
  "msg": "Bug fixes and improvements",
  "force": false
}
```

- Strict mode — MD5 check to prevent failed updates

```json showLineNumbers
{
  "download_url": "http://baidu.com/aaa.iec",
  "version": "1.1.0",
  "dialog": true,
  "msg": "Bug fixes and improvements",
  "force": false,
  "md5": "MD5 of the IEC file computed on your server",
  "download_timeout": 60
}
```

- `download_url`: URL of the new package
- `version`: new package version
- `md5`: IEC file MD5 — when present, EC verifies file integrity
- EC downloads and loads the new IEC when this JSON is returned
- `dialog`: show a dialog (`true`) or silent update (`false`)
- `msg`: message shown in the dialog
- `force`: in dialog mode, force update (`true` = cannot cancel)
- `download_timeout`: 7.11.0+ download timeout in seconds (default 60)



## UI startup update

- With the above configured, the UI checks for updates on launch

## In-script hot update

- You can trigger hot update during script execution using the APIs below

### hotupdater.updateReq — request update

* Calls the hot-update endpoint. `false` may mean no update is needed — use `getErrorMsg()` for details.
* @param updateUrl Update URL — omit to use `update.json`
* @param version Current version as an integer, e.g. `1`
* @param appendDeviceInfo Append device info — `true` or `false`
* @param timeout Request timeout in ms
* @return `{bool}` `true` = update available; `false` = no update

```javascript showLineNumbers
function main() {
    // Read version from update.json in the project
    let version = JSON.parse(readIECFileAsString("update.json")).version
    // Or define version manually:
    // let version = 7;
    toast("Hello World - " + version);
    // Check server for a new version
    // Using update.json:
    //let updateResult = hotupdater.updateReq("",version,true,9000);
    // Custom URL:
    let updateResult = hotupdater.updateReq("http://baidu.com", version, true, 9000);
    logd("Update check result: " + updateResult);
    if (!updateResult) {
        logw("Request failed: " + hotupdater.getErrorMsg());
    } else {
        logd("Update response: " + hotupdater.getUpdateResp());
        // Download new version when an update is available
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

### hotupdater.updateDownload — download IEC

* Downloads the IEC from the hot-update response
* EC 5.20.0+
* @return `{string}` Path to the downloaded hot-update file; empty if no update
* See [example](/docs/advance/hot-update#hotupdaterupdatereq--request-update)


### hotupdater.getUpdateResp — get response

* Returns the hot-update request response
* EC 5.20.0+
* @return `{string}` Response string
* See [example](/docs/advance/hot-update#hotupdaterupdatereq--request-update)


### hotupdater.getErrorMsg — get error message

* Returns hot-update error details
* EC 5.20.0+
* @return `{string}` Error string
* See [example](/docs/advance/hot-update#hotupdaterupdatereq--request-update)
