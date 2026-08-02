---
title: Spinner
description: EasyClick automation scripts — Android no root — Spinner
keywords:
 - EasyClick automation scripts Android no root Spinner
 - br
 - center
 - vertical
 - fill
 - horizontal
 - ui
 - clip
 - Spinner
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
Drop-down selection box
## Example
```xml showLineNumbers
<Spinner android:layout_height="wrap_parent"
            android:layout_width="match_parent"
            android:tag="btn"
            android:text="Option 1|Option 2"
            android:textColor="#669999"
            android:mode="dialog"
            android:textSize="14dp"
            android:defaultText="Option 2"
            android:gravity="center"
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
| text | Drop-down options | String<br/>Separate multiple options with pipe `\|`, e.g.: Option 1&#124;Option 2|
| textColor | Text color | Hex, e.g. #FFFFFF |
| textSize | Text size | specific number + dp |
| defaultText | Default selected item | One of the items in text |
| mode | Display mode | dialog — dialog box; dropdown — drop-down mode |
| popupHeight | Popup height | specific number + dp |
