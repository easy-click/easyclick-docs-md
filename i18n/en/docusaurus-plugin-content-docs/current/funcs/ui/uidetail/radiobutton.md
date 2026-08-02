---
title: RadioButton
description: EasyClick automation scripts — Android no root — RadioButton
keywords:
 - EasyClick automation scripts Android no root RadioButton
 - br
 - center
 - vertical
 - fill
 - horizontal
 - ui
 - clip
 - RadioButton
 - en
 - EasyClick
 - mobile automation
 - automation testing
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview
Radio button
## Example
```xml showLineNumbers
<RadioButton android:layout_height="wrap_parent"
            android:layout_width="match_parent"
            android:tag="btn"
            android:text="Button"
            android:textColor="#669999"
            android:textSize="14dp"
            android:gravity="center"
            android:checked="true"
    />
```

## Properties

### Common Properties
See [Common Properties](/docs/funcs/ui/ui-native-view#common-properties)

### Private Properties

| Property | Description | Values |
| :------: | :------: | :------: |
| layout_weight | Child weight | Number<br/>When the parent is LinearLayout, child controls can set weight|
| gravity | Internal control alignment |[Usage reference](https://blog.csdn.net/gaojinshan/article/details/44917205)<br/>top<br/>bottom<br/>left<br/>right<br/>center_vertical<br/>fill_vertical<br/>center_horizontal<br/>fill_horizontal<br/>center<br/>fill<br/>clip_vertical<br/>clip_horizontal<br/> |
| checked | Selected state | true: selected false: not selected |
| text | Text | String |
| textColor | Text color | Hex, e.g. #FFFFFF |
| textSize | Text size | specific number + dp |
