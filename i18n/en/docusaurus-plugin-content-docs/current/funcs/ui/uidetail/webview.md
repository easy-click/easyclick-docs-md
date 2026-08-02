---
title: WebView
description: EasyClick automation scripts — Android no root — WebView embedded browser
keywords:
 - EasyClick automation scripts Android no root WebView embedded browser
 - html
 - WebView
 - layout
 - ui
 - H5
 - JS
 - en
 - funcs
 - native
 - EasyClick
 - mobile automation
 - automation testing
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview
WebView embedded browser — supports H5 and JS operations
## Example
- Option 1: Load HTML from the layout folder
```xml showLineNumbers
<WebView android:layout_height="wrap_parent"
            android:layout_width="match_parent"
            android:url="main.html"
    />
```

- Option 2: Load HTML from the network
```xml showLineNumbers
<WebView android:layout_height="wrap_parent"
            android:layout_width="match_parent"
            android:url="http://jd.com"
    />
```


## Properties

### Common Properties
See [Common Properties](/docs/funcs/ui/ui-native-view#common-properties)

### Private Properties

| Property | Description | Values |
| :------: | :------: | :------: |
| url | Web page URL | Supports HTML in layout folder and HTTP URLs |
