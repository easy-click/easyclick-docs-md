---
title: Remote IPA Installation
description: EasyClick automation scripts — iOS no jailbreak — remote IPA installation
keywords:
 - EasyClick automation scripts iOS no jailbreak remote IPA installation
 - xcode
 - image
 - Xcode
 - img
 - src
 - iosimg
 - png
 - alt
 - https
 - com
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---


:::tip
- This document is obsolete. Certificates are now purchased directly to sign IPAs.
:::

## GUI Environment

### Device Side Startup

- The device side refers to the computer with the connected iPhone
- Download the control center, reinstall and open it, and go to the Xcode Remote Debugging option
- <img src="/iosimg/image-20220416095051551.png" alt="image-20220416095051551" style={{zoom:'50%'}} />
- Set the port and save, click Start Program, and wait until the running status becomes Normal
- The current port (e.g. 8999) must be exposed to the public internet. You can use PeanutHull to expose it for Xcode to connect
- PeanutHull tutorial: https://www.bilibili.com/video/BV1F34y147Ky?p=6
- If you have your own server, you can use frp for intranet penetration: https://github.com/fatedier/frp/releases



### Xcode Side Startup

- Download and install the control center program. This must be on macOS
- Go to the Xcode Remote Debugging option
- <img src="/iosimg/image-20220416094437335.png" alt="image-20220320213338751" style={{zoom:'50%'}} />
- Enter the device-side port address — the address exposed by PeanutHull. Domain or IP both work, e.g. 122.22.33.22:8999
- Enter your Mac password, then click Save
- Click Start Program and wait until the running status becomes Normal



### Xcode Debugging

- After the above steps, go to the [Install Agent IPA](/iosdocs/tools/signagent) workflow. You will see the remote phone and can install the IPA remotely following the normal installation flow



## Terminal Mode Startup

### Device Side Startup

- Download the forwarder from the cloud drive
- <img src="/iosimg/image-20220416095855116.png" alt="image-20220416095855116" style={{zoom:'50%'}} />
- Extract it to a path without Chinese characters. Open Terminal and use `cd` to enter the iosforward folder
- In Terminal, run: `ios-forward -mode=dc -p=8992`. On Mac; on Windows use `ios-forward.exe -mode=dc -p=8992`
- <img src="/iosimg/image-20220416100124057.png" alt="image-20220416100124057" style={{zoom:'50%'}} />
- `-mode=dc` means device client; `-p=8992` sets the port to 8992 (port can be changed)



### Xcode Side Startup

- Download and extract the forwarder from the cloud drive to a path without Chinese characters. This must be on macOS
- Open Terminal and use `cd` to enter the iosforward/macos folder
- In Terminal, run: `sudo ios-forward -mode=xc -p=8988 -address=122.111.111.111:8999`
- <img src="/iosimg/image-20220416100705989.png" alt="image-20220416100705989" style={{zoom:'50%'}} />
- `-mode=xc` means Xcode client; `-p` is the runtime port; `-address` is the device-side address
- If the remote connection fails, a connection failure message will appear
- After all sides are started, proceed to the [Install Agent IPA](/iosdocs/tools/signagent) workflow. You will see the remote phone and can install the IPA remotely following the normal installation flow


