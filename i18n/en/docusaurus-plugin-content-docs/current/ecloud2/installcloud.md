---
title: EasyClick Android — Cloud control system · Install
hide_title: false
hide_table_of_contents: false
sidebar_label: Install cloud control
description: >-
 EasyClick cloud control system manages scripts and tasks.
 Install is simple: use BT Panel to install MySQL and Redis, copy the cloud control program into the site directory, update MySQL config, and start the ecloud binary
keywords:
 - EasyClick
 - mobile automation scripts
 - automation software
 - cloud control system
 - cloud control install
 - Douyin cloud control
 - Kuaishou cloud control
 - game cloud control
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
 - mobile automation
---

# Install cloud control
- Android, standalone iOS, and iOS USB share the same cloud control system — install steps are identical

:::tip
- Cloud control **5.5.0+** supports Android, standalone iOS, and USB iOS
:::

:::danger Upgrade note
- Upgrading from older versions to **5.7.0+** requires updating `[database.link]` in `config/config.toml`
- Append `?loc=Local&parseTime=true`, e.g. `link = "mysql:ecloudgo:ecloudgo@tcp(localhost:3306)/ecloudgo?loc=Local&parseTime=true"`
- Prevents incorrect timestamps when writing data

:::

## Install

### Requirements

- Linux (Ubuntu 18.04, CentOS, etc.); Windows also works
- MySQL 5.7
- Redis 4
- Supervisor process manager
- You can one-click install MySQL and Redis with BT Panel — the guide below uses BT Panel



### Install MySQL, Nginx, Redis
- 1. Open BT Panel; if prompted, choose MySQL 5.7 and install
<img src="/ecloudimages/image-20211203144143477.png" alt="image-20211203144143477" style={{zoom:'33%'}} />

- 2. Or open App Store and search for Nginx, MySQL, phpMyAdmin
- 3. Install Redis from App Store
- 4. Install Supervisor from App Store
<img src="/ecloudimages/image-20211203144321514.png" alt="image-20211203144321514" style={{zoom:'33%'}} />

- After install, pin them to the home page



<img src="/ecloudimages/image-20211203144420239.png" alt="image-20211203144420239" style={{zoom:'33%'}} />

###

### Download cloud control

- 1. Download the zip starting with `ecloud` from the group files or cloud drive

### Upload cloud control

- 1. In BT File Manager, create folder `ecloudgo`

<img src="/ecloudimages/image-20211203144601138.png" alt="image-20211203144601138" style={{zoom:'33%'}} />





> 2. Upload the zip via BT File Manager

<img src="/ecloudimages/image-20211203144634238.png" alt="image-20211203144634238" style={{zoom:'33%'}} />



> 3. Extract the binary

<img src="/ecloudimages/image-20211203145105395.png" alt="image-20211203145105395" style={{zoom:'33%'}} />

<img src="/ecloudimages/image-20211203145114564.png" alt="image-20211203145114564" style={{zoom:'33%'}} />



### Create database

```text showLineNtumbers
MySQL and Redis must be installed first! Orange zeros in the top-left of BT Panel mean install is done
```

<img src="/ecloudimages/image-20211203145436463.png" alt="image-20211203145436463" style={{zoom:'25%'}} />


- Create a database instance with username, password, and database name all `ecloudgo`

<img src="/ecloudimages/image-20211203145825106.png" alt="image-20211203145825106" style={{zoom:'33%'}} />



- Grant database privileges
- Sign in to phpMyAdmin as root (phpMyAdmin button)



<img src="/ecloudimages//image-20211203151414051.png" alt="image-20211203151414051" style={{zoom:'33%'}} />



- Open the Users tab in phpMyAdmin

<img src="/ecloudimages/image-20211203151451822.png" alt="image-20211203151451822" style={{zoom:'33%'}} />



- Click Edit privileges in the red box, then Select all


<img src="/ecloudimages/image-20211203151534953.png" alt="image-20211203151534953" style={{zoom:'33%'}} />

- Click Go

<img src="/ecloudimages/image-20211203151554359.png" alt="image-20211203151554359" style={{zoom:'33%'}} />



- When it looks like below, privileges are granted

<img src="/ecloudimages/image-20211203151718322.png" alt="image-20211203151718322" style={{zoom:'33%'}} />



### Edit database config

- In File Manager, edit `config.toml`



<img src="/ecloudimages/image-20211203145911611.png" alt="image-20211203145911611" style={{zoom:'33%'}} />


- Set the three options below to `ecloudgo`

<img src="/ecloudimages/image-20211203150103562.png" alt="image-20211203150103562" style={{zoom:'33%'}} />



### Configure process startup

- On the home page, open Supervisor

<img src="/ecloudimages/image-20211203150553998.png" alt="image-20211203150553998" style={{zoom:'33%'}} />

- Add a process daemon


<img src="/ecloudimages/image-20211203150616509.png" alt="image-20211203150616509" style={{zoom:'33%'}} />


- Choose the directory and executable

<img src="/ecloudimages/image-20211203150723547.png" alt="image-20211203150723547" style={{zoom:'33%'}} />

- Started successfully


<img src="/ecloudimages/image-20211203150742524.png" alt="image-20211203150742524" style={{zoom:'33%'}} />

- Check logs for success
<img src="/ecloudimages//image-20211203150820652.png" alt="image-20211203150820652" style={{zoom:'33%'}} />

<img src="/ecloudimages//image-20211203150827519.png" alt="image-20211203150827519" style={{zoom:'33%'}} />



### Open firewall ports

- New cloud control port is **8098**

- In BT Security, allow port 8098
- If prompted to upgrade Nginx firewall, install it from App Store

<img src="/ecloudimages/image-20211203151152772.png" alt="image-20211203151152772" style={{zoom:'33%'}} />

<img src="/ecloudimages/image-20211203151226538.png" alt="image-20211203151226538" style={{zoom:'33%'}} />



- Alibaba Cloud security group: https://help.aliyun.com/document_detail/25471.html?spm=a2c6h.13066369.0.0.1eec1ecfVVJqw6&source=5176.11533457&userCode=28kqeewo&type=copy

- Tencent Cloud: https://cloud.tencent.com/developer/article/1841261?from=15425



### Open cloud control

- Browse to `your-ip:8098` — default user `admin`, password `admin123`

## Licensing
### Site license

- Cloud control is licensed by device count — contact your local reseller or QQ: 2557945562

- Product serial is under Site config → Site license

### APK connect & license

- With the debug APK, open Enterprise cloud settings, enter remote IP or domain, click Test link

<img src="/ecloudimages//image-20211203153342198.png" alt="image-20211203153342198" style={{zoom:'33%'}} />

- When packaging with the IDE, you do not fill the domain in the APK UI
- You must enter the domain when packaging an enterprise build — see the cloud control domain chapter
- Device license
 - Device management → Device list → Unauthorized devices → Assign



<img src="/ecloudimages//image-20211203153609841.png" alt="image-20211203153609841" style={{zoom:'33%'}} />



## Optimize & upgrade

### Upgrade
:::danger Upgrade note
- Upgrading from older versions to **5.6.0+** requires updating `[database.link]` in `config/config.toml`
- Append `?loc=Local&parseTime=true`, e.g. `link = "mysql:ecloudgo:ecloudgo@tcp(localhost:3306)/ecloudgo?loc=Local&parseTime=true"`
- Prevents incorrect timestamps when writing data
:::
- Backup the database and files before upgrading!!!

- 1. Upload the standalone `ecloudgo` binary to the site root

<img src="/ecloudimages//image-20211203152131132.png" alt="image-20211203152131132" style={{zoom:'33%'}} />

- 2. Upload `config` contents into the site `config` folder

- 3. Edit database settings as in **Edit database config** above

- 4. Update Supervisor to the new binary


<img src="/ecloudimages//image-20211203152321572.png" alt="image-20211203152321572" style={{zoom:'33%'}} />



- Click Restart



<img src="/ecloudimages//image-20211203152346117.png" alt="image-20211203152346117" style={{zoom:'33%'}} />






### Server tuning

- Some servers are not tuned for high traffic

- 1. Run `./opt.sh` in the console
- 2. In Supervisor config, add `minfds=65535` (main and child configs), then restart cloud control

<img src="/ecloudimages//image-20211203152851539.png" alt="image-20211203152851539" style={{zoom:'33%'}} />

<img src="/ecloudimages//image-20211203152918388.png" alt="image-20211203152918388" style={{zoom:'33%'}} />



## Device connection
### Android
- In debug APK: open EC APP → top-left hamburger → **Enterprise cloud config**
- **Remote IP or domain**: your cloud control address
- **Device number**: your own ID, matching the cloud control device list
- **Device ID**: ignore
- Tap Test link; on success, kill the app and reopen
:::tip
- For production, package an enterprise APK and set the cloud control address in the cloud control packaging options; assign device numbers after install
- Debug builds: if you get a digital signature error, connect IDEA and run a script or preview UI once
- More domain options: [Packaging settings](/docs/ecloud2/pkgset)
:::



### Standalone iOS
- Open the EC standalone app → Settings → **Cloud control** — enter address and device number matching the backend
- Tap Test; on success, restart the app to auto-connect
### iOS USB
- Open the EC iOS USB control center → **Control center settings**
- Set **Cloud control address**, save, then restart the control center
:::tip
- There is no device-number field in the control center — by default **ecid** is used. If you set cloud device number to **Alias**, the alias is used
- Use digits + English for aliases (no Chinese) to avoid connection issues
:::

### HarmonyOS Next USB
- Open the EC HarmonyOS Next USB control center → **Control center settings**
- Set **Cloud control address**, save, then restart the control center
:::tip
- There is no device-number field — by default **device serial** is used. If you set cloud device number to **Alias**, the alias is used
- Use digits + English for aliases (no Chinese) to avoid connection issues
:::

## MySQL 8.0
### Monitor page fails to load data
```sql
## Erroring SQL
device_monitor.go:77: SELECT COUNT(1) FROM `t_device` AS a LEFT JOIN `t_device_group` as b on a.group_id=b.id LEFT JOIN `sys_user` as c on c.id = a.tenant_id LEFT JOIN `t_device_online` as ol on a.device_no=ol.device_no WHERE `a`.`status`='0': Error 1267 (HY000): Illegal mix of collations (utf8mb4_0900_ai_ci,IMPLICIT) and (utf8mb4_general_ci,IMPLICIT) for operation '='
```
- Cause:
 - After upgrading MySQL 5.7.34 → 8.0.32, some queries fail with:
 - ERROR 1267 (HY000): Illegal mix of collations (utf8mb4_general_ci,IMPLICIT) and (utf8mb4_0900_ai_ci,IMPLICIT) for operation '='
- Fixes: https://zhuanlan.zhihu.com/p/705783937 https://blog.csdn.net/wangchange/article/details/139231166

## Cloud control does not dispatch tasks
```text
Tasks may not dispatch when:

1. Device is offline
2. Task is one-shot and already ran
3. Task is looping but the run count is exhausted
4. Script failed to download
5. User list → Cloud settings → single-device expiry is on, but the device number has no correct expiry or is expired
For 2 and 3, clear execution history (Clean in the action buttons)

- If a tenant is assigned, also set device quota and expiry under User list → Cloud settings — missing this can block tenant task dispatch

Monitor list → click device number → black window shows that device’s task logs

```
