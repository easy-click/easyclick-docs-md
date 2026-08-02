---
title: EasyClick Android — Cloud control packaging
hide_title: false
hide_table_of_contents: false
sidebar_label: Cloud packaging settings
description: EasyClick cloud control APK packaging — dynamic or static domain; always build an enterprise APK
keywords:
 - EasyClick
 - mobile automation scripts
 - automation packaging
 - cloud control
 - Douyin cloud control
 - Kuaishou cloud control
 - game cloud control
 - DNS
 - IP
 - EC
 - 7.8.0
 - apk
 - tip
 - image
 - APK
 - ecloudimages
 - mobile automation
 - automation testing
---



# Cloud packaging settings

## Overview

1. From EC 7.8.0, non-debug domains can only be set when packaging an enterprise build in the IDE — nowhere else

2. From EC 7.8.0, you can change the APK’s internal domain dynamically from the cloud

3. From EC 7.8.0, you no longer set the device name in the APK — set it in the cloud



## Domain packaging



### Direct mode
:::tip
Direct mode: the packaged APK connects to cloud control using the domain entered at pack time
:::

Package an enterprise build and enter the cloud control address

	![image-20211130151733982](/ecloudimages/image-20211130151733982.png)


### Dynamic DNS resolve mode (recommended)
- Point a domain at an IP without ICP filing
- Resolve the domain to an IP, then connect by IP — easier when the server changes
- When packaging an enterprise build, set cloud control address to `domain:port`, e.g. `ecloud.devicefarm.cn:8098`, and choose **Resolve DNS to IP then connect**

### Dynamic domain
:::tip
Dynamic domain mode: the APK first fetches the real cloud control address from a configured JSON URL, then connects using that address
:::
Package an enterprise build and set the cloud control address, e.g. `http://192.168.1.76:8099/remote.json`

!!!! The URL **must** end with `.json` !!!!

1. Host the JSON on Aliyun OSS or any reachable URL

2. Or use Cloud control → Site management → Site license → Cloud connect URL settings (see that page for details)

3. To generate JSON: Site management → Site license → Cloud connect URL settings — enter the domain, click Copy, save as `remote.json`, upload to OSS (or similar)

Format:

```json showLineNumbers
{"data":"BggHBAcEBwADUQJWAlYDAQMJAwICVQMBAwYDCAJVAwECVQMHAwYDUQMIAwADCQMJAFEGCAcEBwQHAANRAlYCVgMBAwkDAgJVAwEDBgMIAlUDAQJVAwcDBgNRAwgDAAMJAwg="}
```
