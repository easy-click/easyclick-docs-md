---
title: FrameLayout
description: EasyClick automation scripts — Android no root — FrameLayout
keywords:
 - EasyClick automation scripts Android no root FrameLayout
 - FrameLayout
 - ui
 - en
 - funcs
 - native
 - view
 - md
 - layout
 - weight
 - EasyClick
 - mobile automation
 - automation testing
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---


## Overview
FrameLayout is the simplest layout type. It creates a blank region (a frame) for each child control added. All controls are placed in frames and, by default, displayed in the top-left corner of the screen, stacked in the order they are added — earlier controls appear on the bottom layer, later controls on top. FrameLayout is suitable for layered designs.

## Example
```xml showLineNumbers
 <FrameLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent">
</FrameLayout>
```

## Properties

### Common Properties
See [Common Properties](/docs/funcs/ui/ui-native-view#common-properties)

### Private Properties

| Property | Description | Values |
| :------: | :------: | :------: |
| layout_weight | Child weight | Number<br/>When the parent is LinearLayout, child controls can set weight|
