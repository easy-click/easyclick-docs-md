---
title: CardView
description: EasyClick automation scripts — Android no root — CardView
keywords:
 - EasyClick automation scripts Android no root CardView
 - br
 - dp
 - CardView
 - ui
 - en
 - funcs
 - native
 - view
 - md
 - EasyClick
 - mobile automation
 - automation testing
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview
CardView card layout

## Example
```xml showLineNumbers
<CardView>
            <LinearLayout android:orientation="vertical">
                <TextView android:layout_width="match_parent" android:layout_height="wrap_content"
                          android:padding="20dp" android:text="CardView Demo"/>
                <ImageView
                        android:src="https://upload.jianshu.io/users/upload_avatars/4321745/406ef6d9-28c1-4f35-8cee-37818cc404af.jpg"
                        android:layout_width="200dp" android:layout_height="200dp" android:scaleType="CENTER_CROP"/>
            </LinearLayout>
        </CardView>
```

## Properties

### Common Properties
See [Common Properties](/docs/funcs/ui/ui-native-view#common-properties)

### Private Properties

| Property | Description | Values |
| :------: | :------: | :------: |
| layout_weight | Child weight | Number<br/>When the parent is LinearLayout, child controls can set weight|
| cardBackgroundColor | Background color | Number <br/>e.g. #888888|
| cardCornerRadius | Corner radius | Number <br/>e.g. 20dp|
| cardElevation | Z-axis shadow | Number <br/>e.g. 20dp|
| cardMaxElevation | Maximum Z-axis elevation | Number <br/>e.g. 20dp|
