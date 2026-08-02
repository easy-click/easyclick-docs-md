---
title: Standalone Activator Tutorial
description: EasyClick iOS standalone script activation system — tutorial covering quick start, device initialization, batch operations, and full workflow
sidebar_label: Standalone Activator
keywords:
 - EasyClick automation scripts iOS no jailbreak standalone activator tutorial
 - iOS
 - USB
 - standalone activation
 - pairing certificate
 - self-activation
 - image flashing
 - initialization
 - EasyClick
 - mobile automation
 - test automation
---

# Standalone Activator Tutorial

## Overview

The standalone activator is a standalone desktop client for batch-preparing the automation runtime environment on iOS devices. It consolidates operations that previously required switching between multiple interfaces (pairing certificate generation, image flashing, WiFi debugging, self-activation environment readiness) into one interface.

**Target users**: operations staff, hosting service providers, and users who need to manage automation environments on many iPhones in batch. No coding required.

## Opening the Activator

1. Start the EasyClick control center program (ioscenter.exe)
2. Click **Standalone Activator** in the top toolbar
3. In the dialog, select **New Interface** (recommended), then click OK
4. Wait a moment; the activator window will open

## Interface Layout

After opening, you will see:

| Area | Description |
|------|------|
| Top stats bar | Shows total device count and selected device count |
| Toolbar | Connection (USB Connect/WiFi Debug), environment (Flash Image/Start Automation Service), self-activation (batch self-activation button group) |
| BundleId config | Set bundle IDs for main app and agent app |
| Device list | Shows all connected devices; supports multi-select, sort, and filter |
| License info column | Shows license expiry status for each device |

## Quick Start

Follow these steps to prepare the device automation environment:

### Step 1: Connect Device

1. Connect the iPhone to the PC with a **USB cable**
2. Ensure i4Tools (爱思助手) is installed and can recognize the device
3. In the activator, click **Refresh Devices** in the toolbar
4. Your phone should appear in the device list with connection type USB

### Step 2: Configure BundleId

BundleId is the app package identifier. The activator needs your main app bundle ID to work.

1. Find the **BundleId Configuration** card
2. In **Main App BundleId**, enter your main app bundle ID (required)
 - Example: `com.example.automation`
 - Multiple bundle IDs can be separated by commas: `com.app1,com.app2`
3. In **Agent App BundleId**, enter the agent app bundle ID (required if using **Start Automation Service**)
4. Click **Save Configuration**

:::tip
If you don't know the bundle ID, open i4Tools → Apps & Games, right-click your app, and choose **Copy Identifier**.
:::

### Step 3: Flash Developer Image

The developer image is the bridge for phone–PC communication. Place the correct version image file in advance.

1. Select device(s) in the device list (multi-select supported)
2. Click **Environment → Flash Image** in the toolbar
3. Watch the **Image** column turn green ✓ for success
4. If flashing fails, check:
 - Whether the image file is in the correct directory
 - Whether the phone is unlocked and has trusted the PC

### Step 4: Initialize Device

Initialization writes the device ID to the phone and binds the device to the main app.

1. Ensure the main app is installed on the phone and in the foreground
2. Check the devices to initialize in the device list
3. Click **Self-Activation → Batch Operations → Batch Initialize** in the toolbar
4. Wait for **Success** in the **Message** column; if failed, see [FAQ](#faq)

### Step 5: Enable WiFi Debug (Optional)

If you need to connect over WiFi later (without USB), enable WiFi debug first.

1. Select the device
2. Click **WiFi Debug → Enable WiFi Debug** in the toolbar
3. On success, the **Debug Address** column shows the device IP

## Batch Operations

The activator supports batch operations on multiple selected devices for greater efficiency:

| Operation | Description |
|------|------|
| Batch Initialize | Initialize multiple devices at once |
| Scan Device IP Addresses | Scan WiFi IPs of all initialized devices on the LAN |
| Batch Generate Pairing Files | Batch-generate trust pairing certificates for devices and PC |
| Batch Push Pairing Files | Push pairing certificates to phones |
| Batch Push Image Files | Push developer images to phones |
| One-Click Complete Self-Activation Environment | Auto-enable WiFi, generate pairing, push image and pairing files |
| Self-Activation Test | Verify self-activation environment is ready (internal/external VPN supported) |

Steps:

1. Check the devices to operate on in the device list (select all if needed)
2. Click **Self-Activation → Batch Operations** in the toolbar
3. Choose the operation from the dropdown
4. Confirm the dialog
5. Check results in the **Message** column

## Single-Device Operations

Right-click a device row in the list, or use the **Actions** button at the end of each row:

| Operation | Description |
|------|------|
| Initialize | Initialize this device only |
| Flash Image | Flash developer image on this device only |
| One-Click Self-Activation | Auto-run WiFi enable, pairing generation, and file push |
| Self-Activation Test | Test whether self-activation works on this device |
| Restart | Restart this device |
| View Pairing Certificate | View and edit pairing certificate content |
| Copy Device ID / ECID | Copy to clipboard |

## License Management

:::tip
The activator shows a **license validity marker** on the left for each device. Initialization and other operations also verify license status. If the license has expired, the row is marked **×**.
:::

- License binding is done in **Control Center → License Center**
- After binding, return to the activator and refresh the device list for the latest status
- For rebind or transfer, see [License / Rebind / Transfer](https://www.laoleng.vip/docs/tools/easyclick/ios-qk/qa/authorize)

## FAQ

### Initialize says "Please fill in and save Main App BundleId first"

Click **Set BundleId** in the BundleId config area, enter the main app bundle ID, and save. Without BundleId config, the activator cannot write the device ID to the phone.

### "Start Automation Service" says "Please fill in and save Agent App BundleId first"

Starting automation service requires the agent app. Ensure Agent App BundleId is set in BundleId config.

### Pairing file generation failed

- Ensure the phone has a passcode (lock screen password)
- If generated before, **Batch Generate Pairing Files** will overwrite the old certificate
- The phone may show **Trust This Computer** — tap Trust

### Image flash failed

- Ensure developer image files are in `bridgebin\config\DeveloperDiskImage`
- Try i4Tools **Live Screen** to trigger an image push once

### Self-activation test failed

- WiFi debug must be enabled (**WiFi Debug → Enable WiFi Debug** in toolbar)
- Pairing certificate must exist and be pushed to the phone
- Phone must be on WiFi (mobile data does not work)
- For external VPN mode, ensure LocalDevVpn is installed

### Device list is empty

- Check USB cable connection
- Ensure i4Tools is installed and recognizes the device
- Click **Refresh Devices** in the toolbar
- Try another USB port or cable

### Confirm dialog text does not wrap

Confirm dialogs with multi-line text (e.g. **One-Click Complete Self-Activation Environment**) now support auto line wrap. If layout issues persist, update the activator to the latest version.

## Related Docs

- [Install Control Center](/iosdocs/tools/installcenter)
- [USB Screen Mirroring Tutorial](/iosdocs/advance/ios-usb-screen)
- [Self-Activation (Standalone Edition)](/iostjdocs/advance/activemyself)
