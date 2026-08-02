---
title: EasyClick Android — USB HID host
hide_title: false
hide_table_of_contents: false
sidebar_label: USB HID host
description: EasyClick control center screen mirroring — LAN mirroring, script management, synchronized control
keywords:
 - EasyClick
 - mobile automation scripts
 - automation software
 - control center screen mirroring
 - Douyin group control
 - Kuaishou group control
 - game group control
 - HID
 - Ubuntu
 - Server
 - tip
 - '4.0'
 - windows
 - winusb
 - linux
 - EC
 - mobile automation
 - automation testing
---

# HID host

:::tip Notes
 - HID host mode: driver-free, low cost, high device density
 - Supports both HID scripts and HID screen mirroring
:::

:::tip Newer versions
- HID host 4.0+ supports Windows WinUSB — you can run without a Linux host
:::
## Hardware options
### Ascend mini PC
 - Search `升腾小主机` on Pinduoduo. Official EC purchases use Ascend C92, 8 GB RAM, 120 GB disk (~¥217). Lower configs (~¥150) also work
 - After installing Ubuntu Server, tests show ~20 phones — about ¥7.5/phone at ¥150/20. Higher density not fully tested
 - Far cheaper than MCU or Bluetooth kits — recommended
 <br/><img src='/andqk/sthost.png' style={{zoom:'10%'}}/>
### Motherboard PC conversion
 - Some motherboard chassis include a small PC — convert it to Ubuntu Server and install HID host; works well in practice
### Desktop PC
 - Unused desktops can run Ubuntu Server as the HID host
### Other
 - Any host you can convert to Ubuntu Server works
## Install Ubuntu Server
 - Many Ubuntu Server guides exist online; we tested **24.04**
 - References:
 - https://zhuanlan.zhihu.com/p/698434939
 - https://www.bilibili.com/video/BV1ZAxqedEJy/?vd_source=58a0fd7e5e5cdf152718d0faed99c04f
 - https://blog.csdn.net/ziqibit/article/details/129932038
## Install HID host
 - After Ubuntu is ready, in the mirroring app open **HID host → Install HID host**
 - Enter the Ubuntu IP range, username, and password, then install
 <img src='/andqk/n_aqk5.png' style={{zoom:'20%'}}/>
 - On success, the HID service auto-starts after reboot
 <img src='/andqk/n_aqk6.png' style={{zoom:'20%'}}/>
## HID intranet penetration
 - For cross-network access, the mirroring app provides HID intranet penetration
 - See [Set up intranet penetration](/docs/centerscreen/openscreen#搭建内网穿透)
 - After mirroring penetration succeeds, HID is included — open `public-IP:8989` to browse the local HID host

## HID networking
 - Open HID host → Advanced networking → **Broadcast scan** or enter an IP range to scan
 <br/><img src='/andqk/n_aqk7.png' style={{zoom:'20%'}}/>
 - After scanning, HID hosts report data to this machine
 <br/><img src='/andqk/n_aqk8.png' style={{zoom:'20%'}}/>
 - Click **Activate HID mode** on the host to start activation
 <br/><img src='/andqk/n_aqk9.png' style={{zoom:'20%'}}/>
## Use HID for mirroring
 - After host setup and networking, you can mirror
 - Details: [HID mirroring](/docs/centerscreen/openscreen#hid投屏)
## Use HID in scripts
 - On LAN, use [setHidCenter](/docs/funcs/hid-event-api#sethidcenter-设置hid主控地址) to point at this machine’s LAN IP
 - Over WAN via penetration, set the address to `public-IP:8989`

