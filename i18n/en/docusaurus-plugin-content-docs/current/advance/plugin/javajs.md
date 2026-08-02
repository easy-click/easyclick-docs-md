---
title: EasyClick Android docs — Java-JS hybrid development
hide_title: false
hide_table_of_contents: false
sidebar_label: Java-JS hybrid development
description: EasyClick Android script automation supports Java and JS mixed development. This chapter explains the Java-JS hybrid script development model.
keywords:
 - EasyClick
 - mobile automation scripts
 - Android plugin development
 - Java plugin development
 - JS calling Java
 - Java calling JS
 - Java
 - java
 - js
 - JS
 - jar
 - src
 - libs
 - PluginClz
 - main
 - mobile automation
 - test automation
 - script development
---

# Java-JS hybrid development

## Overview
- Hybrid development combines Java and JS in one project — JS calls Java methods
- After compilation, Java code and JARs become a `defaultplugin.apk` plugin
- This chapter focuses on EasyClick hybrid development

## Create a Java-JS hybrid project
- In IDEA, create a project and select **EasyClick Java-JS Hybrid Project**; use JDK 1.8 (minor version does not matter)
- Click Next and finish — the corresponding Java classes and configuration are generated automatically
<br/>
<br/>
<img src='/androidimg/javajs.png' width='400' />

## Directory structure
- `src/js/main.js` — test entry for JS calls into plugin methods
- `src/com/` — Java source; `PluginClz` is the default plugin entry class
- `libs/jarlibs` — third-party JARs, merged into the plugin
- `libs/solibs` — native `.so` libraries, compiled into the plugin `lib` folder
- `libs/resources` — resource files, compiled into the plugin `resources` folder
- `libs/jslibs` — JS libraries for testing only; not compiled into the plugin

----
- For JNI, put `.so` files in `libs/solibs`


<br/><br/>
<img src='/androidimg/javajs-2.png' width='400' />

## Configure JDK and output class directory
- Right-click the project
<br/>
<img src='/androidimg/minx-1.png' width='400' />
<br/>
<img src='/androidimg/minx-2.png' width='400' />
<br/>
<img src='/androidimg/minx-3.png' width='400' />


## Add android.jar
- Right-click `android.jar` and choose **Add as Library**
<br/>
<img src='/androidimg/addlib-1.png' width='300' /><br/><br/>
- Click OK
<br/><br/>
<img src='/androidimg/addlib-2.png' width='300' /><br/>


## Default Java class PluginClz
- `PluginClz` is the default generated Java class
- `test` is the default generated method
- See `main.js` for how it is invoked

## Using Java from main.js
- After Java compilation, the plugin is named `defaultplugin.apk` — call `loadDex("defaultplugin.apk")` when the script runs
- Load the plugin with the [loadDex function](/docs/funcs/global#loaddex-载入dex或者apk)
- Then use `importClass` or `importPackage` to import Java classes
- Instantiate with `new` and call methods



# Run
- Use the menu **EasyClick Dev Tools → Run Project** (device must be connected)


# Package

- Use **EasyClick Dev Tools → Package Project**; check the EasyClick run log for output



