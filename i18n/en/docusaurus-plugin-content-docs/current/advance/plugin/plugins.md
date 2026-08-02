---
title: EasyClick Android docs — Plugin development
hide_title: false
hide_table_of_contents: false
sidebar_label: Plugin development
description: EasyClick Android script automation supports plugin development — extend script capabilities with Java for quick, convenient custom functionality. This chapter explains how to develop plugins.
keywords:
 - EasyClick
 - mobile automation scripts
 - Android plugin development
 - Java plugin development
 - anti-tamper
 - non-root script anti-tamper
 - libs
 - src
 - js
 - apk
 - java
 - br
 - resources
 - android
 - Android
 - mobile automation
 - test automation
 - script development
---

# Plugin development

## Overview
- A plugin is an APK package — no different from ordinary Android development
- You can use Android Studio during development; package it as an APK when done
- This chapter focuses on developing plugins with EasyClick

## Create a plugin project
- In IDEA, create a project and select **EasyClick Plugin Project**
- Click Next and finish — the corresponding Java classes and configuration are generated automatically
<br/>
<br/>
<img src='/androidimg/plugin-1.jpg' width='400'/ >

# Plugin directory structure
- `src/js/main.js` — test entry for JS calls into plugin methods
- `src/com/` — Java source; `PluginClz` is the default plugin entry class
- `libs/jarlibs` — third-party JARs, merged into the plugin
- `libs/solibs` — native `.so` libraries, compiled into the plugin `lib` folder
- `libs/resources` — resource files, compiled into the plugin `resources` folder
- `libs/jslibs` — JS libraries for testing only; not compiled into the plugin

----
- As a plugin developer, focus on writing Java code. For JNI, put `.so` files in `libs/solibs`


<br/><br/>
<img src='/androidimg/plugin-2.jpg' width='400' />

# Plugin Java class PluginClz
- `PluginClz` is the default generated plugin Java class
- `test` is the default generated plugin method
- See `main.js` for how it is invoked

# Run the plugin
- Use the menu **EasyClick Dev Tools → Run Project** (device must be connected)


# Package the plugin
- If your plugin APK is very large, you likely misconfigured something — typical size is a few KB to a few hundred KB
- Use **EasyClick Dev Tools → Package Plugin → Build plugin APK**
- Check the EasyClick run log; output is usually at `build/release.apk`


## Using a plugin

- Put the packaged APK in your script project’s `plugin` folder
- Load it with the [loadDex function](/docs/funcs/global#loaddex-载入dex或者apk)
- Instantiate with `new` and call methods



