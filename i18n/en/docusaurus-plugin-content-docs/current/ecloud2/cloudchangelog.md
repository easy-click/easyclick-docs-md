---
title: EasyClick Android — Cloud control changelog
hide_title: false
hide_table_of_contents: false
sidebar_label: Cloud control changelog
description: 'EasyClick cloud control — web platform for scripts, tasks, and data; remote mirroring, cross-site networking, remote device control'
keywords:
 - EasyClick
 - mobile automation scripts
 - automation software
 - cloud control platform
 - Douyin cloud control
 - Kuaishou cloud control
 - game cloud control
 - 5.16.0
 - 5.15.0
 - 5.13.0
 - 5.12.0
 - 5.11.0
 - 5.9.0
 - 5.8.0
 - 5.7.0
 - 5.5.0
 - mobile automation
 - automation testing
---

# Changelog

## Latest release

### 5.18.0
Released: 2026-04-30

```text
- Added webhook callbacks
    - Push heartbeat data and script logs to external systems via HTTP or WebSocket
    - See the Open API docs
- General improvements
```

## Previous releases

### 5.17.0
Released: 2025-10-18

```text
# 5.17.0
- User management → sub-user management: manage all devices and data under a sub-user
- Per-feature CRUD ownership selection and list filters by user
- General improvements
- Upgrading from below 5.6: see https://www.ieasyclick.com/docs/ecloud2/installcloud upgrade notes
```
### 5.16.0
Released: 2025-8-12

```text
# 5.16.0
- Device groups show available device count
- Task publish: select all devices in one click
- AI LLM MCP protocol: start and stop tasks
- General improvements
- Upgrading from below 5.6: see https://www.ieasyclick.com/docs/ecloud2/installcloud upgrade notes
```

### 5.15.0
Released: 2025-6-6

```text
# 5.15.0
- Device monitor: unlink device from task
- Improved device list display
- General improvements
- Upgrading from below 5.6: see https://www.ieasyclick.com/docs/ecloud2/installcloud upgrade notes
```
### 5.13.0
Released: 2025-5-27

```text
# 5.13.0
- Admin can share scripts to the script library
- Admin can share tasks to the task library
- Tenants can copy scripts from the script library
- Tenants can copy tasks from the task library
- Aliyun OSS can bind a custom domain
- UI improvements
- Upgrading from below 5.6: see https://www.ieasyclick.com/docs/ecloud2/installcloud upgrade notes
```
### 5.12.0
Released: 2025-5-20

```text
# 5.12.0
- Admin can transfer devices to sub-users
- Configurable per-device expiry for sub-users
- Fixed dynamic table create/edit issues
- Clearer UI and messages
- Upgrading from below 5.6: see https://www.ieasyclick.com/docs/ecloud2/installcloud upgrade notes
```

### 5.11.0
Released: 2025-3-26

```text
# 5.11.0
- Fixed saving the “daily” option for scheduled runs
- General improvements
- Upgrading from below 5.6: see https://www.ieasyclick.com/docs/ecloud2/installcloud upgrade notes
```

### 5.9.0
Released: 2025-3-18

```text
# 5.9.0
- Monitor page: filter by whether a task is assigned
- Improved online filter on monitor page
- Improved device assignment page
- General improvements
- Upgrading from below 5.6 to 5.8: see https://www.ieasyclick.com/docs/ecloud2/installcloud upgrade notes
```

### 5.8.0
Released: 2025-3-3

```text
# 5.8.0
- dynamicQuery: order field for sorting
- Fixed menu issues
- General improvements
- Upgrading from below 5.6 to 5.8: see https://www.ieasyclick.com/docs/ecloud2/installcloud upgrade notes
```

### 5.7.0
Released: 2025-2-28

```text
# 5.7.0
- Upgraded underlying framework for stability and concurrency
- General improvements
- Upgrading to 5.7: see https://www.ieasyclick.com/docs/ecloud2/installcloud upgrade notes
```
### 5.5.0
Released: 2025-2-15

```text
# 5.5.0
- iOS USB edition cloud control support
- Optimized command dispatch (less memory/CPU)
- General improvements
```
### 5.3.0
Released: 2024-12-06

```text
# 5.3.0
- Script upload progress
- Fixed some table encoding issues
- Fixed dynamic table create failures caused by field length
- General improvements
```

### 5.0.0
Released: 2024-11-08

```text
# 5.0.0
- Monitor page: view screen
- Mirroring text input
- Improved cloud mirroring monitor page
- General improvements
```
### 4.7.0
Released: 2024-06-26

```text
# 4.7.0
- Remote pause / resume scripts
- Improved task stop
- General improvements
```


### 4.6.0
Released: 2024-06-13

```text
# 4.6.0
- Edit script: book/import/export parameter templates
- Edit task: load script parameter templates as global params
- Edit task: export/import parameters
- General improvements
```

### 4.5.0
Released: 2024-06-01

```text
# 4.5.0
- Upload scripts to Aliyun OSS
- Fixed cloud screenshot mirroring quality
- Fixed high-concurrency connection not released promptly
- General improvements
```


### 4.4.0
Released: 2024-04-14

```text
# 4.4.0
- Fixed data upload UI
- Fixed crashes from some exceptions
- General improvements
```
### 4.3.0
Released: 2024-03-27

```text
# 4.3.0
- Fixed crash when task parameters were incomplete
- General improvements
```

### 4.2.0
Released: 2023-12-24

```text
# 4.2.0
- Start task button can clear history
- General improvements
```

### 4.1.0
Released: 2023-12-18

```text
# 4.1.0
- Lowered memory use in some edge cases
- Added config.toml no_limit_token for full-site Open API request token
```
:::tip
- Unlimited-access token for highest full-site API privilege
- Empty by default (disabled). Set a complex alphanumeric string longer than 16 characters
- Send as HTTP header, e.g. `header["no_limit_token"] = "xxxxx"`
:::

### 4.0.0
Released: 2023-11-15

```text
# 4.0.0
- Rebuilt cloud control architecture
- iOS mirroring with remote control
- Dedicated cloud mirroring control page
- Per-device parameter config
- Improved UI flows

```
### 3.13.0
Released: 2023-09-16

```text
# 3.13.0
- Improved data export
- Improved device monitor page
- Improved load under concurrency

```

### 3.10.0


```text
# 3.10.0
1. Batch import for data lists and dynamic forms
2. Batch export for data lists and dynamic forms
3. Import templates for data lists and dynamic forms
4. Warn against special characters in device names
5. Device list export
6. Fixed brand search on device list
7. Fixed pause when only one task exists
8. Performance improvements

```
### 3.6.0

```text
# 3.6.0
- Device list filter by service status
- More menus in tenant user management
- Faster task cache refresh
```
