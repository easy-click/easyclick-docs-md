---
title: FlexboxLayout
description: EasyClick automation scripts — Android no root — FlexboxLayout
keywords:
 - EasyClick automation scripts Android no root FlexboxLayout
 - FlexboxLayout
 - row
 - br
 - flex
 - ui
 - reverse
 - column
 - Flex
 - Flexible
 - Box
 - EasyClick
 - mobile automation
 - automation testing
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview
Flex is short for Flexible Box, meaning "flexible layout." It is widely used in frontend CSS. Having worked with React Native and WeChat Mini Programs, page layouts often use flex for consistent structure across different screen sizes, enabling simple, complete, and responsive layouts. This section introduces FlexboxLayout for Android — tags, flow layouts, and similar UI patterns can all be implemented with FlexboxLayout.


## Example
```xml showLineNumbers
<?xml version="1.0" encoding="utf-8"?>
<ScrollView xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xmlns:android="http://schemas.android.com/apk/res/android"
            xsi:noNamespaceSchemaLocation="layout.xsd"
            android:background="#333333"
            android:layout_height="match_parent"
            android:layout_width="match_parent">
    <FlexboxLayout android:layout_height="200dp"
                   android:layout_width="match_parent"
                   android:background="#888888"
                   android:flexDirection="row"
                   android:flexWrap="wrap">

        <Button android:layout_width="wrap_content"
                android:layout_weight="1"
                android:layout_order="1"
                android:layout_flexGrow="20.0"
                android:layout_flexShrink="20.0"
                android:layout_alignSelf="auto"
                android:layout_height="wrap_content"
                  android:text="1"/>

        <Button android:layout_width="wrap_content"
                  android:layout_weight="1"
                  android:layout_height="wrap_content"
                  android:text="2"/>

        <Button android:layout_width="wrap_content"
                android:layout_weight="1"
                android:layout_height="wrap_content"
                  android:text="3"/>

        <Button android:layout_width="wrap_content"
                android:layout_weight="1"
                android:layout_height="wrap_content"
                android:text="4"/>

        <Button android:layout_width="wrap_content"
                android:layout_weight="1"
                android:layout_height="wrap_content"
                android:text="5"/>
        <Button android:layout_width="wrap_content"
                android:layout_weight="1"
                android:layout_height="wrap_content"
                android:text="6"/>
    </FlexboxLayout>
</ScrollView>

```

## Properties

### Common Properties
See [Common Properties](/docs/funcs/ui/ui-native-view#common-properties)

### Private Properties

| Property | Description | Values |
| :------: | :------: | :------: |
| flexDirection | Main axis direction (arrangement direction of child elements) | row (default): horizontal, start on the left<br/>row_reverse: horizontal, start on the right, opposite of row<br/>column: vertical, start at the top<br/>column_reverse: vertical, start at the bottom, opposite of column<br/>In XML use app:flexDirection="row"; in code use flexboxLayout.setFlexDirection(FlexDirection.ROW)|
| flexWrap | Whether to wrap lines | nowrap (default): no wrap<br/>wrap: wrap normally, first line on top<br/>wrap_reverse: wrap in reverse, first line on bottom / |
| justifyContent | Alignment along the main axis | flex_start (default): align to main axis start<br/>flex_end: align to main axis end<br/>center: center on main axis<br/>space_between: space evenly between elements<br/>space_around: equal space on both sides of each element (gaps between elements are twice the gap to container edges) / |
| More properties | See https://www.cnblogs.com/taixiang/p/9177215.html | / |
