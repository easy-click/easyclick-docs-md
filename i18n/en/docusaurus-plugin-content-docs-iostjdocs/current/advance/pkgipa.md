---
title: Package IPA
description: EasyClick iOS scripts — no jailbreak, no hardware — how to package an IPA project
keywords:
 - EasyClick
 - iOS scripts
 - automation scripts
 - iOS no jailbreak
 - iOS script tutorial
 - iOS no-hardware scripts
 - iOS package IPA
 - ipa
 - img
 - src
 - iostjimg
 - tj
 - pkg
 - png
 - alt
 - style
 - zoom
 - mobile automation
---

## Select project
- You must select an EasyClick iOS standalone project — USB projects cannot be packaged as IPA
- In IDEA top menu: EasyClick iOS → Standalone packaging → Standard packaging<br/>
 <img src="/iostjimg/tj-pkg-ipa-1.png" alt="Select IPA packaging project" style={{zoom:'30%'}} />
## Packaging parameters
- Basic packaging settings<br/>
 <img src="/iostjimg/tj-pkg-ipa-2.png" alt="Basic packaging parameters" style={{zoom:'30%'}} />
- Default configuration<br/>
 <img src="/iostjimg/tj-pkg-ipa-3.png" alt="Basic packaging parameters" style={{zoom:'30%'}} />
- Cloud control configuration (use when the package connects to EasyClick cloud control; standard builds can ignore this)
 <img src="/iostjimg/tj-pkg-ipa-4.png" alt="Basic packaging parameters" style={{zoom:'30%'}} />
- When configuration is complete, click the Package button
## Packaging complete
- Results appear in the EasyClick iOS run log after packaging<br/>
 <img src="/iostjimg/tj-pkg-ipa-5.png" alt="Packaging result" style={{zoom:'30%'}} />
- After packaging, the IPA is generated. Sign it with Sideloadly, Bullfrog Sign, or i4Tools, then install on the device to run
