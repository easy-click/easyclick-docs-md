---
title: EasyClick Android Docs — Mobile Automation Scripts — Remote Debugging
hide_title: false
hide_table_of_contents: false
sidebar_label: Remote Debugging
description: 'EasyClick mobile automation scripts — remotely debug EasyClick projects in IDEA; as long as the device is online, you can debug scripts remotely'
keywords:
 - EasyClick
 - mobile automation scripts
 - automation software
 - IDEA remote debugging
 - Android no root remote debugging
 - remote script debugging
 - remote
 - png
 - http
 - 192.168.1.1
 - img
 - src
 - androidimg
 - alt
 - APK
 - apk
 - mobile automation
 - test automation
---


# Remote Debugging
## Install APK
- Some phones cannot install the APK directly via ADB; use download instead
- Select `IDEA menu bar - EasyClick Android - Device Connection - Download APK File`
- Copy the downloaded APK to the phone and install it manually
- Or select `IDEA menu bar - EasyClick Android - Device Connection - Scan QR Code to Install APK`

## Enable Remote Debugging
- `Menu bar - EasyClick Android - Device Connection - Enable HTTP Remote Debugging`
- In the dialog, enter the local port (default 10826), click OK to open the local port, and watch `EasyClick Android Run Log`

## Expose Port on Router

- The default router address is usually [http://192.168.1.1](http://192.168.1.1). This doc uses a TP-LINK router as an example; open [http://192.168.1.1](http://192.168.1.1) in a browser
- Find Virtual Server

<img src='/androidimg/remote_1.png' alt="remote_1.png" style={{zoom:'30%'}} />

- Find Virtual Server

<img src='/androidimg/remote_2.png'alt="remote_2.png" style={{zoom:'30%'}} />

- Find public IP

<img src='/androidimg/remote_3.png' alt="remote_3.png" style={{zoom:'30%'}} />

## Connect from EC on the Phone
- Open the EC debug app on the phone

- Go to the remote connection page

<img src='/androidimg/remote_5.png' alt="remote_5.png" style={{zoom:'30%'}} />


- Enter connection details

<img src='/androidimg/remote_6.png' alt="remote_6.png" style={{zoom:'30%'}} />


## FRP Reverse Proxy
- Use FRP to reverse-proxy: map the computer port to a server, then connect the phone to the server IP so you do not need the router's public IP

## TCP Remote
- TCP remote debugging is similar to HTTP remote debugging; only the port differs. Without a public IP, prefer FRP for reverse proxy
