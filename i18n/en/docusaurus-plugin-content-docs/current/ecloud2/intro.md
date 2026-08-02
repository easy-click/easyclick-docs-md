---
title: EasyClick Android — Cloud control platform overview
hide_title: false
hide_table_of_contents: false
sidebar_label: Cloud control platform overview
description: 'EasyClick cloud control — web platform for scripts, tasks, and data; remote mirroring, cross-site networking, remote device control'
keywords:
 - EasyClick
 - mobile automation scripts
 - automation software
 - cloud control platform
 - Douyin cloud control
 - Kuaishou cloud control
 - game cloud control
 - EC
 - tip
 - 3.11.0
 - 9.4.0
 - ecloud
 - getTaskInfo
 - mobile automation
 - automation testing
 - script development
 - Android automation
 - iOS automation
---

# Cloud control platform overview

:::tip
Cloud control runs on the server — manage many devices from the web. This is different from on-device group control.
:::

## Cloud control platform
- In the EC product line, cloud control is the **cloud testing platform** for multi-device, multi-resource workflows
- EC includes built-in cloud communication — call the APIs directly
- This doc applies to cloud control **3.11.0+** and EC client **9.4.0+**

## Core concepts

### Devices

- Hardware that runs tasks; each device has a unique ID

### Scripts

- Code that automates a test or workflow


### Tasks

- A task defines what to run: device group, schedule, state, etc.
- **Parameters**: base inputs shipped with the task into the script



### Data

- Predefined data sets scripts consume

- Data produced on devices can be stored in cloud control
- CRUD, append, and remove via API


## Execution flow

- 1. After device registration, the device connects to the cloud
- 2. The cloud pushes tasks to EC
- 3. EC loads script files and starts execution
- 4. Scripts read task info and parameters via `ecloud.getTaskInfo()`
- 5. During execution, scripts can read/write cloud data
- 6. Developers focus on device IDs and steps 4–5; EC handles the rest

## Usage notes

- Only **debug** and **enterprise** builds can talk to cloud control
