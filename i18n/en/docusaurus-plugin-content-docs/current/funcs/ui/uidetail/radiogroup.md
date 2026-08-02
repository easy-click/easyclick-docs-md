---
title: RadioGroup
description: EasyClick automation scripts — Android no root — RadioGroup
keywords:
 - EasyClick automation scripts Android no root RadioGroup
 - RadioGroup
 - ui
 - RadioButton
 - LinearLayout
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
RadioGroup wraps RadioButton controls to ensure only one can be selected at a time. It extends LinearLayout and includes linear layout properties.

## Example
```xml showLineNumbers
   <RadioGroup android:layout_width="match_parent"
                    android:layout_marginTop="10dp"
                    android:layout_height="100dp"
                    android:orientation="horizontal">

            <RadioButton android:layout_height="80dp"
                            android:layout_width="50dp"
                            android:text="r1"
                            android:tag="r1"
                            android:gravity="center"/>
            <RadioButton android:layout_height="80dp"
                            android:layout_width="50dp"
                            android:text="r2"
                            android:gravity="center"
                            android:tag="r2"/>
</RadioGroup>
```

## Properties

### Common Properties
See [Common Properties](/docs/funcs/ui/ui-native-view#common-properties)

### Private Properties

| Property | Description | Values |
| :------: | :------: | :------: |
| orientation | Direction | vertical: vertical <br/> horizontal: horizontal |
