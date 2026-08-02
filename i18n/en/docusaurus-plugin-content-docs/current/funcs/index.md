---
title: Script Functions
description: EasyClick Android automation script function API documentation index
keywords:
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
 - remote screen mirroring
 - OCR
 - PPOCR
 - YOLO
 - Cursor
 - AI coding
 - script functions
 - API documentation
---

# Script Functions

> Source: https://ieasyclick.com/docs/funcs

---

## AI Assistant Guide

If you are an AI assistant, read these Skill files first:

- [Android Automation Skill](../../skills/easyclick-android.md) — complete Android development guidelines
- [iOS Automation Skill](../../skills/easyclick-ios.md) — iOS development guidelines
- [HarmonyOS Automation Skill](../../skills/easyclick-harmony.md) — HarmonyOS Next development guidelines
- [UI Design Skill](../../skills/ui-design.md) — UI design guidelines

### Core Principles

1. **Gather evidence before coding** — obtain real node trees/screenshots before writing selectors
2. **Check docs before implementing** — confirm API usage before writing code
3. **Prefer selectors** — use property-based locators instead of coordinates when possible
4. **Log key steps** — aids debugging and troubleshooting

---

## Index Overview

This documentation is split into sibling `*-api.md` and `global/*.md` modules; filenames indicate the topic and are searchable.

**UI-related** docs are in [UI Authoring](ui/index) (not duplicated on this page).

---

## Global Module

| Document | Description |
|------|------|
| [Global Module](global/global) | Global functions, version checks, plugin loading |
| [Global Shortcut Events](global/global-shortcut) | Quick tap, swipe, system keys |
| [Selector & Nodes](global/selector-node) | Node selectors, xpath, attribute matching |

## Event Modules (choose by run mode)

| Document | Run Mode | Description |
|------|---------|------|
| [Accessibility Events](acevent-api) | Accessibility mode | acEvent object API |
| [Agent Events](event-api) | Proxy mode | agentEvent object API |
| [HID Events](hid-event-api) | HID mode | hidEvent object API |
| [Bluetooth HID Events](blehid-event-api) | Bluetooth HID | bleEvent object API |
| [OTG HID Events](otghid-event-api) | OTG HID | otgEvent object API |

## Core Function Modules

| Document | Description | Run Mode Compatibility |
|------|------|---------------|
| [Image/Color Functions](image-api) | Screenshot, find image, find color | ✅ All modes |
| [YOLO Functions](yolo-api) | Object detection | ✅ All modes |
| [OCR Recognition](ocr-api) | Text recognition | ✅ All modes |
| [Device Functions](device-api) | Device information | ✅ All modes |
| [Floaty Functions](floaty-api) | Floating window UI | ✅ All modes |
| [File Functions](file-api) | File operations | ✅ All modes |
| [Storage Functions](storage-api) | Key-value storage | ✅ All modes |
| [Network Functions](http-api) | HTTP requests | ✅ All modes |
| [Thread Functions](thread-api) | Multithreading | ✅ All modes |
| [Utility Functions](utils-api) | Common utilities | ✅ All modes |
| [Shell Command Functions](shell-api) | Shell execution | ⚠️ Root/proxy mode |
| [SQLite Functions](sqlite-api) | Database operations | ✅ All modes |
| [ADB Functions](adbClient-api) | Wireless ADB | ✅ All modes |

## Business Modules

| Document | Description |
|------|------|
| [Control Center Screen Mirroring Module](center-api) | Control center system API |
| [JDBC MySQL Functions](jdbcmysql-api) | Database connection (MySQL 5.x supported) |
| [Network Verification Functions](netcard-api) | License verification, cloud variables |

## Development Tools

| Document | Description |
|------|------|
| [Development Tools Index](devtools/dev-tools) | Development tools overview |
| [Color Tool](devtools/dev-tools-color) | Color picker |
| [Device Management](devtools/dev-tools-device) | Device connection management |
| [Project Configuration](devtools/dev-tools-project) | Project settings |
| [Remote Screen Mirroring](devtools/dev-tools-remote) | Remote control |
| [Node Viewer](devtools/dev-tools-node) | Node tree analysis |
| [Glossary](devtools/dev-tools-word) | Terminology |
| [Settings](devtools/dev-tools-settings) | Tool settings |
| [Installation](devtools/dev-tools-install) | Installation guide |

## Other Platforms

| Platform | Documentation | Description |
|------|---------|------|
| iOS | [iosdocs/funcs/](../../iosdocs/funcs/) | iOS no-jailbreak automation |
| HarmonyOS Next | [hmdocs/funcs/](../../hmdocs/funcs/) | HarmonyOS Next automation |
| iOS Standalone | [iostjdocs/funcs/](../../iostjdocs/funcs/) | iOS standalone runtime |

---

## Run Mode Reference

### Five Run Modes

| Mode | Object Prefix | Characteristics | Use Case |
|------|---------|------|---------|
| **Accessibility mode** | `acEvent` | Requires accessibility service | General automation |
| **Proxy mode** | `agentEvent` | Requires proxy service | No root, fullest feature set |
| **Root mode** | `shell` | Requires root | System-level operations |
| **Bluetooth HID** | `bleEvent` | Requires Bluetooth HID hardware | When accessibility cannot be enabled |
| **OTG HID** | `otgEvent` | Requires OTG HID hardware | When accessibility cannot be enabled |

### Mode Compatibility Quick Reference

| Feature | Accessibility | Proxy | Root | Bluetooth HID | OTG HID |
|------|--------|------|------|---------|---------|
| Node selector | ✅ | ✅ | ❌ | ❌ | ❌ |
| Image/color recognition | ✅ | ✅ | ✅ | ✅ | ✅ |
| OCR recognition | ✅ | ✅ | ✅ | ✅ | ✅ |
| YOLO detection | ✅ | ✅ | ✅ | ✅ | ✅ |
| Shell commands | ❌ | ✅ | ✅ | ❌ | ❌ |
| System keys | ✅ | ✅ | ✅ | ✅ | ✅ |

---

## Quick Start

### 1. Basic Script Template

```javascript
function main() {
    // 1. Start environment
    startEnv();
    
    // 2. Wait for page load
    sleep(1000);
    
    // 3. Locate element (prefer selectors)
    let selector = text("Settings").id("title");
    let node = selector.getNodeInfo(5000);
    
    // 4. Check and act
    if (node) {
        logd("Element found, preparing to click");
        click(selector);
    } else {
        loge("Element not found");
    }
}

main();
```

### 2. Image/Color Recognition Template (HID Mode)

```javascript
function main() {
    // 1. Request screenshot permission (type=1 for HID mode)
    image.requestScreenCapture(10000, 1);
    
    // 2. Capture screen
    let img = image.captureScreen();
    
    // 3. Find image/color/OCR
    let point = image.findColor(img, "#FF0000", {});
    
    // 4. HID tap
    if (point) {
        hidEvent.click(point.x, point.y);
    }
    
    // 5. Release resources
    image.recycle(img);
}

main();
```

### 3. UI Template

```javascript
function main() {
    // 1. Load layout
    ui.layout("Title", "main.xml");
    
    // 2. Reset UI variables
    ui.resetUIVar();
    
    // 3. Get views
    let btn = ui.btn_submit;
    
    // 4. Bind events
    ui.setEvent(btn, "click", function(view) {
        toast("Button clicked");
        ui.saveAllConfig();
    });
}

main();
```

---

## Online Resources

- **Website**: https://ieasyclick.com
- **Online docs**: https://ieasyclick.com/docs/funcs
- **Developer docs**: https://www.ieasyclick.net/docs/

---

## Documentation Version

- Applicable version: EasyClick 10.x+
- Last updated: 2025
