---
title: HorizontalScrollView
description: EasyClick automation scripts — Android no root — HorizontalScrollView
keywords:
 - EasyClick automation scripts Android no root HorizontalScrollView
 - ui
 - HorizontalScrollView
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
A horizontally scrollable layout control; can have only one direct child element
## Example
```xml showLineNumbers
<HorizontalScrollView
            android:fillViewport="true"
            android:layout_width="match_parent"
            android:layout_height="match_parent">
        <LinearLayout android:layout_height="match_parent"
                      android:orientation="vertical"
                      android:layout_width="match_parent">
            <TextView android:layout_width="match_parent"
                      android:layout_height="match_parent"
                      android:tag="sctest"
                      />
        </LinearLayout>

</HorizontalScrollView>

```

## Properties

### Common Properties
See [Common Properties](/docs/funcs/ui/ui-native-view#common-properties)

### Private Properties

| Property | Description | Values |
| :------: | :------: | :------: |
| layout_weight | Child weight | Number<br/>When the parent is LinearLayout, child controls can set weight|
| fillViewport | Fill entire viewport | true: yes false: no |
