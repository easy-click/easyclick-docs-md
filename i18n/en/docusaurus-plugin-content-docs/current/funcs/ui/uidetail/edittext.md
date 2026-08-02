---
title: EditText
description: EasyClick automation scripts — Android no root — EditText
keywords:
 - EasyClick automation scripts Android no root EditText
 - br
 - center
 - vertical
 - fill
 - horizontal
 - ui
 - clip
 - EditText
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
Text input field
## Example
```xml showLineNumbers
<EditText android:layout_height="wrap_parent"
            android:layout_width="match_parent"
            android:tag="btn"
            android:text="Button"
            android:textColor="#669999"
            android:textSize="14dp"
            android:gravity="center"
            android:maxLength="12"
            android:hint="I am a hint"
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
| hit | Input hint | String |
| text | Text | String |
| textColor | Text color | Hex, e.g. #FFFFFF |
| textSize | Text size | specific number + dp |
| maxLength | Maximum text length | specific number |
| lines | Number of lines | specific number |
| maxLines | Maximum lines | specific number |
| ellipsize | Truncation when text exceeds width | start: ellipsis at start<br/>end: ellipsis at end<br/>middle: ellipsis in middle<br/>marquee: horizontal scroll (requires focus)<br/>none: no truncation |
| inputType | Input type | text: text<br/>phone: phone number<br/>number: number<br/>textPassword: text password<br/>numberPassword: numeric password |
