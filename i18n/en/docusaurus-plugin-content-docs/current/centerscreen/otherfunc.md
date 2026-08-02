---
title: EasyClick Android — Control center scripts & other features
hide_title: false
hide_table_of_contents: false
sidebar_label: Scripts & other features
description: EasyClick control center screen mirroring — LAN mirroring, script management, synchronized control
keywords:
 - EasyClick
 - mobile automation scripts
 - automation software
 - control center screen mirroring
 - Douyin group control
 - Kuaishou group control
 - game group control
 - apk
 - usb
 - br
 - img
 - src
 - andqk
 - png
 - style
 - zoom
 - mobile automation
 - automation testing
---


# Scripts & other features

## Scripts
:::tip
- Before running scripts, install the EC debug build, or package your own APK with IDEA (Android plugin 9.27.0+) and install it on the phone
- Before connecting via APK, connect once over USB (ADB mode — USB debugging on) to the control center or mirroring app so the device serial is written into the APK. Without this, the APK cannot connect
- The steps below are the same for control center and mirroring; the control center has a dedicated script log monitor
 :::

### Connect APK to control center
- Install the APK: click **Scan** — scan the QR code with the phone, or download and install
 <br/>
 <img src='/andqk/n_aqk1.png' style={{zoom:'20%'}}/>
- Left toolbar → **Scan** → **Network scan** → **Broadcast scan** (phone and PC must be on the same LAN)
 <br/>
 <img src='/andqk/n_aqk2.png' style={{zoom:'30%'}}/>
 - After the APK receives the scan, it saves the control center address and connects automatically
 - If scanning fails, set the correct NIC (left toolbar → Set NIC) and scan again
 - If still failing, enter the control center address manually, or enter the phone’s subnet and use **Subnet scan**

- Manual connect
 - Open the APK → top-left hamburger → System settings
 - Find control center / mirroring settings — enter the PC IP, or `IP:8178`
 - Open the log floating window and tap Test connection
 - If there is no device serial, enable ADB debugging and USB-connect once to the control center / mirroring app. **You may also enter any unique serial**
 <br/>
 <img src='/andqk/auth6.png' style={{zoom:'30%'}}/>
 - **Note: if your PC has a public IP, control center and mirroring support WAN access**
- Connected
 - On success, mirroring or control center shows the phone IP and that scripts can run
 <br/>
 <img src='/andqk/auth7.png' width='500' />

### Run scripts
- Select device → left toolbar → Stop script
 <br/>
 <img src='/andqk/auth8.png' width='500' />
### Script parameters
- Left toolbar → Script parameters → add or edit
 <br/>
 <img src='/andqk/auth9.png' width='500' />
- When running a script, pick the parameter name; use `getCenterTaskInfo` in the script to read them
 <br/>
 <img src='/andqk/auth10.png' width='500' />


## Data management
:::tip
- Manage shared data from the control center / mirroring UI; scripts call it via [Script API — control center module](/docs/funcs/center-api)
- Helps avoid dirty/phantom reads under concurrency
 :::

### Create / edit data
- Control center / mirroring → left toolbar → Data → add or edit
 <br/>
 <img src='/andqk/zkshuju1.png' width='500' />
- Enter one record per line, or upload a file and deduplicate
 <br/>
 <img src='/andqk/zkshuju2.png' width='500' />
### Export data
- Left toolbar → Data → select a dataset → export (others can import it)
### Deduplicate
- When creating or editing, click Deduplicate to remove duplicates in the current content
### Import TXT
- When creating or editing, choose a TXT file to import




## Other features
### Activate proxy mode {#激活代理模式}
- Activates proxy mode on the device for running scripts (replaces the older EC activator)
- Left menu → Device list — **Proxy environment** shows **Activated** when done; otherwise click Activate proxy environment
 <br/>
 <img src='/andqk/auth18.png' width='500' />

## Grant accessibility
- Grants accessibility auto-start for scripts (not available on every device)
- Left menu → Device list — enter the EC app package name (from EC system settings), click Grant accessibility, then check **Accessibility auto-start** in EC system settings
 <br/>
 <img src='/andqk/auth18.png' style={{zoom:'30%'}}/>
## Grant screenshot auto-allow
- Grants **auto-allow screenshot** for scripts (not available on every phone)
- Left menu → Device list — enter the EC app package name, click **Grant screenshot auto-allow**, then check **Screenshot auto-allow** in EC system settings
 <br/>
 <img src='/andqk/auth19.png' style={{zoom:'30%'}}/>


## Software license
- Licenses are bound to the PC; each machine has a different product serial
- In control center or mirroring → Common → License info

### Add license
- Find the red row for this machine → Authorize on the right
 <br/>
 <img src='/andqk/auth1.png' width='500' />
 <br/>
 [Enter the card number from official channels or resellers, then Authorize]
 <br/>
 <img src='/andqk/auth2.png' width='500' />
### Rebind license
- Find the red row → Rebind — enter the phone number used at registration and the target product serial
 <br/>
 <img src='/andqk/auth3.png' width='500' />

### Transfer license
- Find the red row → Transfer — enter your registration phone number, the recipient’s EC username/phone, and their product serial
 <br/>
 <img src='/andqk/auth4.png' width='500' />
