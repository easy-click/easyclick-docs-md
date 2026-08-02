---
title: Install main app & agent IPA
description: EasyClick iOS standalone — sign and install main/agent IPAs
sidebar_label: Install main & agent IPA
keywords:
 - EasyClick
 - iOS standalone
 - IPA
 - signing
 - TrollStore
---

:::tip
- Standalone edition needs **iOS 15+** — older versions are not supported
- Third-party IPAs must be signed before install
:::

:::tip
- 7.0.0+ supports **single-app** and **dual-app** automation engines
- Single-app: install the main app only → Settings → Automation → Single app → save → start automation
- Dual-app: also sign and install the agent IPA
- Why both?
 - Single-app is faster and needs one signed IPA (legacy-friendly)
 - Dual-app runs in a separate process and can self-reactivate for unattended runs
:::

## Download the main package

- From [**cloud downloads**](/iostjdocs/tools/download_resources)
- **EC package → iOS resources → Standalone edition → latest version (e.g. v6.4.0)**
 Contains main IPA, agent IPA, and optional agent Xcode source<br/>
 <img src="/iostjimg/download_main_zip.png" alt="download package" style={{zoom:'80%'}} />
 - `easyclick-tj-main-….ipa` — standalone main app
 - `easyclick-tj-agent-….ipa` — standalone agent
 - `easyclick-tj-agent-source-….zip` — agent Xcode source (optional)

## Sign & install the main app

- Supports personal free / paid developer / enterprise / TrollStore and most common signers
- Guide → [**IPA signing**](http://laoleng.vip/docs/tools/easyclick/ipasign/)
- After signing, install with iTools / similar tools
- Phone shows **EasyClick Cloud Test** (易点云测) — open the app:<br/>
 <img src="/iostjimg/tj-index.jpg" alt="main app" style={{zoom:'30%'}} />

### Keep-alive

- Settings (top-right) → Keep-alive → Save; when the float window appears, drag it to a screen edge to hide
 <img src="/iostjimg/tj-index-1.png" alt="keepalive" /><br/>
 <img src="/iostjimg/tj-index-2.png" alt="float" />
- Allow location while using (first launch)
- Or iOS Settings → EasyClick Cloud Test (易点云测)<br/>
 <img src="/iostjimg/tj-sys-setting.jpg" alt="ios settings" style={{zoom:'30%'}} />
- Location: Always; Background App Refresh: On; Cellular + Wi‑Fi; enable Siri & Search as needed<br/>
 <img src="/iostjimg/tj-ec-auth.jpg" alt="permissions" style={{zoom:'30%'}} />

### License init

- Standalone is paid per device
- Text → [**standalone center docs**](/iostjdocs/advance/tjcenter)
- Video → [**standalone course**](http://laoleng.vip/docs/tools/easyclick/ios-qk/tj/)

## Sign & install the agent

- Agent IPA is special — personal free accounts and many tools (iTools / Sideloadly) often fail; search **签名** on the EC forum
- Guide → [**IPA signing**](http://laoleng.vip/docs/tools/easyclick/ipasign/)
- On macOS you can build agent source in Xcode (personal free account OK) — see USB [**macOS source build**](/iosdocs/tools/signagent#macos-source-build)
- After install, open **EasyClick-Runner** — system should show white **Automation Running**
- If not → [**mount developer image**](http://laoleng.vip/docs/tools/easyclick/ios-qk/tj/#%E5%88%B7%E5%85%A5%E5%BC%80%E5%8F%91%E8%80%85%E9%95%9C%E5%83%8F)
