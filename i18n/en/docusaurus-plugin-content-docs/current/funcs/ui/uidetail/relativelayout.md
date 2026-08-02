---
title: RelativeLayout
description: EasyClick automation scripts — Android no root — RelativeLayout
keywords:
 - EasyClick automation scripts Android no root RelativeLayout
 - br
 - ui
 - center
 - vertical
 - fill
 - RelativeLayout
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
RelativeLayout positions controls using relative positioning — placing them relative to other controls or the parent container. When designing relative layouts, follow dependency relationships between controls; controls added later depend on those added earlier.
## Example
```xml showLineNumbers
 <RelativeLayout
            android:gravity="center"
            android:layout_width="match_parent"
            android:layout_height="match_parent">
</RelativeLayout>
```

## Properties

### Common Properties
See [Common Properties](/docs/funcs/ui/ui-native-view#common-properties)

### Private Properties

| Property | Description | Values |
| :------: | :------: | :------: |
| layout_weight | Child weight | Number<br/>When the parent is LinearLayout, child controls can set weight|
| gravity | Internal control alignment |[Usage reference](https://blog.csdn.net/gaojinshan/article/details/44917205)<br/>top<br/>bottom<br/>left<br/>right<br/>center_vertical<br/>fill_vertical<br/>center_horizontal<br/>fill_horizontal<br/>center<br/>fill<br/>clip_vertical<br/>clip_horizontal<br/> |
