---
keywords:
 - EC
 - http
 - 192.168.1.1
 - EasyClick
 - img
 - src
 - iosimg
 - remote
 - png
 - width
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
 - remote screen mirroring
 - OCR
---

# Remote Debugging

## Install EC Debug Build on the Phone First

- Menu bar - EasyClick Development Tools - Device Connection - Scan QR Code to Install APK
- Download and install, or install via QR code scan

## Enable Remote Debugging
- Menu bar - EasyClick Development Tools - Device Connection - Remote Debugging
- In the dialog, enter the local port (default 10825), click OK to open the local port, and watch EasyClick Run Log

## Expose Port on Router

- The default router address is usually [http://192.168.1.1](http://192.168.1.1). This doc uses a TP-LINK router as an example; open [http://192.168.1.1](http://192.168.1.1) in a browser
- Find Virtual Server

<img src='/iosimg/remote_1.png' width='300' />

- Find Virtual Server

<img src='/iosimg/remote_2.png' width='300' />

- Find public IP

<img src='/iosimg/remote_3.png' width='300' />

## Connect from EC on the Phone
- Open the EC debug app on the phone

- Go to the remote connection page

<img src='/iosimg/remote_5.png' width='300' />



- Enter connection details

<img src='/iosimg/remote_6.png' width='300' />


## FRP Reverse Proxy
- Use FRP to reverse-proxy: map the computer port to a server, then connect the phone to the server IP so you do not need the router's public IP
