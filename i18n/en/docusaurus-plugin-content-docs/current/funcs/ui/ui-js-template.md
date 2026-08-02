---
title: Template-Based UI
description: EasyClick automation scripts — Android no root — template-based UI
keywords:
 - EasyClick automation scripts Android no root template-based UI
 - UI
 - tab
 - layout
 - JS
 - xml
 - XML
 - ui
 - js
 - EasyClick
 - mobile automation
 - automation testing
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
 - remote screen mirroring
 - OCR
---


## Creating Templates
- If you do not need to interact with JS, you can create UI directly using templates
- Template creation only requires creating an XML file in the project's `layout` folder and writing the corresponding XML code
- For example:

```xml showLineNumbers
<?xml version="1.0" encoding="UTF-8" ?>
<LinearLayout
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns:android="http://schemas.android.com/apk/res/android"
        xsi:noNamespaceSchemaLocation="layout.xsd"
        android:layout_height="match_parent"
        android:layout_width="match_parent">
    <TextView android:layout_width="wrap_content"
              android:layout_height="wrap_content"
              android:text="helloworld "/>


</LinearLayout>
 ```

## Multi-Tab Support
Simply create a `ui.js` file in the project's `layout` folder with the following content:

```javascript showLineNumbers
function main() {
    ui.layout("Parameter Settings", "main.html");
    ui.layout("Register & Use", "reg.html");
    ui.layout("Usage Instructions", "intr.html");
}

main();
```

- See the Native UI Controls section for supported native views
