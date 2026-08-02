---
title: EasyClick Android docs — Network verification
hide_title: false
hide_table_of_contents: false
sidebar_label: Network verification platform
description: 'EasyClick network verification and hot update — automation without accessibility or USB debugging'
keywords:
 - EasyClick
 - mobile automation scripts
 - automation software
 - script hot update
 - code hot update
 - automation without accessibility and USB debugging
 - http
 - uc
 - ieasyclick
 - com
 - apk
 - iec
 - oss
 - OSS
 - appid
 - mobile automation
 - test automation
 - script development
---
# Network platform guide

## Sign up and log in
- URL: [http://uc.ieasyclick.com](http://uc.ieasyclick.com)
- Register and sign in

## Software list
- Create a software entry to get `appid` and secret key — required for `ecNetCard.netCardInit`
- Fields:
 - Software type: choose Android or iOS correctly — wrong choice may break parameter matching
 - Verify APK fingerprint: validates the APK — upload the APK under Script list
 - Verify script fingerprint: validates the IEC — upload the IEC under Script list
 - Heartbeat error count: tolerance for heartbeat failures (e.g. network issues) before the script reports an error
 - License verification error count: tolerance for license check failures before reporting an error
 - Script verification error count: tolerance for script file verification failures
 - Enable verification: turn off during development
 - Status: when disabled, all licenses are invalid and scripts receive errors
<br/><img src='/androidimg/netcard-1.png' width="300" />

:::tip
During development, do not enable APK or script fingerprint verification until you upload IEC and APK files.
Keep packaged APK and IEC builds — if cracked or you need to revoke access, upload them and enable verification.
:::

## Script list
- Each entry represents a script compiled to IEC
- Fields:
 - Software version: script version, e.g. 1.0
 - Package name: Android APK package name only — when set, mismatch blocks script execution
 - APK fingerprint: when set, the APK file is verified
 - Script fingerprint: when set, the IEC file is verified
<br/><img src='/androidimg/netcard-2.png' width="300" />

:::tip
Fill at least one of package name, APK fingerprint, or script fingerprint. Uploads are not stored — only MD5 is computed.
Network verification works even without creating a script list entry.
:::

## License management
- Generate network verification licenses — one device per license or multiple devices per license
- New license fields:
 - Quantity: how many licenses to generate
 - Valid days: expiry — timer starts on first device bind; unused licenses do not count down
 - Unbind password: required to unbind from a device; leave empty for password-free unbind
 - Device kick: for multi-device licenses — when enabled, newer devices kick the oldest bound device
 - Status: disabled licenses cannot be used; scripts report errors
 <br/><img src='/androidimg/netcard-3.png' width="300" />
- Batch actions:
 - Batch extend time: add time to selected licenses without rebinding
 - Batch add online slots: increase allowed device count without rebinding
 <br/><img src='/androidimg/netcard-4.png' width="300" />


:::tip
Generating licenses and batch actions deduct balance based on options — confirm deduction and keep sufficient balance.
Top up via Secretary Sha QQ 2557945562 or support QQ 2050858539
:::

## Cloud variables
- Set variables in the cloud and read them in scripts with `ecNetCard.netCardGetCloudVar`
- Values can be plain strings or JS code executed with `eval` in the script
- Fields:
 - Variable name: passed to `ecNetCard.netCardGetCloudVar` (Chinese or English OK)
 - Variable content: the value
 - Remote edit: allow scripts to update via `ecNetCard.netCardUpdateCloudVar`
 <br/><img src='/androidimg/netcard-5.png' width="300" />

## Hot update management {#热更新管理}
- Built-in hot update backed by Alibaba Cloud OSS — upload IEC files directly

### Create Alibaba Cloud OSS credentials
- Obtain Alibaba Cloud `AccessKeyId` and `AccessKeySecret`
- Go to: https://ram.console.aliyun.com/users/create and create a user
 <br/><img src='/img/oss/1.png' width="300" />
- Choose any username; enable console access and OpenAPI access, then confirm
 <br/><img src='/img/oss/2.png' width="500" />

- After creation, copy `AccessKeyId` and `AccessKeySecret`
 - **Copy AccessKeyId and AccessKeySecret promptly**
<br/><img src='/img/oss/4.png' width="500" />
<br/><img src='/img/oss/3.png' width="500" />
- Grant permissions
- Select the user → Add permissions → check `AliyunOSSFullAccess` → confirm
 <br/><img src='/img/oss/5.png' width="500" />
 <br/><img src='/img/oss/6.png' width="500" />

### Configure OSS
- Use credentials from **Create Alibaba Cloud OSS credentials**
- OSS configuration → Add OSS storage — fill in Alibaba Cloud OSS details
 - AccessKeyId and AccessKeySecret — [How to get OSS AccessKeyId and AccessKeySecret?](https://www.aliyun.com/search?k=oss+accessKeyId&scene=all)
 - **You have no right to access this object because of bucket acl.** fixes:
 - https://blog.csdn.net/weixin_38106322/article/details/106873104
 - https://blog.csdn.net/qq_53763141/article/details/136428817
 - If permissions still fail, create the bucket manually in Alibaba Cloud OSS and enable **public read**
 <br/><img src='/androidimg/cardhotupdate-1.png' width="300" />
 - Add OSS storage example
 <br/><img src='/androidimg/cardhotupdate-2.png' width="300" />

### Upload hot-update script
- Add script — fill in details and upload IEC; version must be an integer. **Create a software entry in Software list first**
 <br/><img src='/androidimg/cardhotupdate-3.png' width="300" />
- After upload, the file is stored on OSS with download URL and MD5 generated
- Save — edit, enable, dialog options work like the standalone hot-update config

### Configure hot-update URL in script
- Software list → hover the software name or use **Copy hot-update URL** on the right
 <br/><img src='/androidimg/cardhotupdate-4.png' width="300" />
- Paste the URL into the project's `update.json`
- Note: new hot-update URLs require EC Android 9.31.0+ or iOS standalone 3.17.0+ — communication is encrypted
- Legacy URLs are unencrypted for compatibility
- To disable legacy URLs, edit the software entry and set hot-update compatibility to new version only

## Anti-tamper recommendations
:::tip
- Use network verification licenses
- Obfuscate critical code
- Enable package name, APK, and script fingerprint verification — requires uploading records after each build
- Use remote variables and remote JS for critical logic — replace remotely when cracked and stop the script
- Authors with sufficient capability may add deterrent code on device (e.g. format SD card, delete contacts) and enable via remote variables when tampering is detected
:::
