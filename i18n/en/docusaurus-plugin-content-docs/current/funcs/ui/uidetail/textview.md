---
title: TextView
description: EasyClick automation scripts — Android no root — TextView
keywords:
 - EasyClick automation scripts Android no root TextView
 - br
 - center
 - vertical
 - fill
 - horizontal
 - ui
 - clip
 - TextView
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
Text view
## Example
```xml showLineNumbers
<TextView android:layout_height="wrap_parent"
            android:layout_width="match_parent"
            android:tag="btn"
            android:text="Button"
            android:textColor="#669999"
            android:textSize="14dp"
            android:gravity="center"
            android:maxLength="12"
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
| text | Text content | String |
| textColor | Text color | Hex, e.g. #FFFFFF |
| textSize | Text size | specific number + dp |
| maxLength | Maximum text length | specific number |
| lines | Number of lines | specific number |
| maxLines | Maximum lines | specific number |
| ellipsize | Truncation when text exceeds width | start: ellipsis at start<br/>end: ellipsis at end<br/>middle: ellipsis in middle<br/>marquee: horizontal scroll (requires focus)<br/>none: no truncation |
