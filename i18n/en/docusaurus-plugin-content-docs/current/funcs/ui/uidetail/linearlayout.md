---
title: LinearLayout
description: EasyClick automation scripts — Android no root — LinearLayout
keywords:
 - EasyClick automation scripts Android no root LinearLayout
 - orientation
 - LinearLayout
 - android
 - vertical
 - horizontal
 - ui
 - weight
 - en
 - funcs
 - EasyClick
 - mobile automation
 - automation testing
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview
LinearLayout is a commonly used layout in development. It arranges controls horizontally or vertically.
The linear layout manager allows each child view to specify a weight attribute to control how much space each child occupies.
The `orientation` attribute controls the arrangement direction.
- `android:orientation="vertical"` — vertical linear arrangement
- `android:orientation="horizontal"` — horizontal linear arrangement

## Example
```xml showLineNumbers
<LinearLayout android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:orientation="vertical">
</LinearLayout>
```

## Properties

### Common Properties
See [Common Properties](/docs/funcs/ui/ui-native-view#common-properties)

### Private Properties

| Property | Description | Values |
| :------: | :------: | :------: |
| orientation | Direction | vertical: vertical <br/> horizontal: horizontal |
