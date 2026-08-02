---
title: Standalone Plugin Development
description: EasyClick iOS scripts — no jailbreak, no hardware — standalone plugin development
keywords:
 - EasyClick
 - iOS scripts
 - automation scripts
 - iOS no jailbreak
 - iOS script tutorial
 - iOS no hardware scripts
 - standalone plugin development
 - plugin
 - xcode
 - framework
 - pluginhost
 - iosdocs
 - zip
 - Project
 - br
 - img
 - src
 - mobile automation
---
:::tip
- Plugin development requires Xcode. This guide uses Xcode 13.1 (13A1030d); other versions are untested
- Plugins support Swift, Objective-C, etc., compiled as frameworks
- Use EC's official interface library `pluginhost.framework` when developing plugins. [Download framework](/iosdocs/pluginhost.zip). Reference helloworldplugin and helloworldplugin2 sample code: [Download](/iosdocs/tjplugindemo.zip)
:::
## Create Plugin Project
- In Xcode, choose New → Project<br/>
<img src="/iostjimg/plugin/tj-plugin-1.png" alt="Project option" style={{zoom:'30%'}} />
- Choose iOS Framework type, then Next<br/>
<img src="/iostjimg/plugin/tj-plugin-2.png" alt="Choose iOS Framework type" style={{zoom:'30%'}} />
- Enter plugin name — **no Chinese**. Example: **hplugin**. Other options as shown. Change Bundle Identifier as needed<br/>
 <img src="/iostjimg/plugin/tj-plugin-3.png" alt="Enter name" style={{zoom:'30%'}} />
- Project structure:<br/>
 <img src="/iostjimg/plugin/tj-plugin-4.png" alt="Project structure" style={{zoom:'30%'}} />


## Add Framework
- Copy downloaded **pluginhost.framework** into the project directory<br/>
 <img src="/iostjimg/plugin/tj-plugin-5.png" alt="pluginhost" style={{zoom:'30%'}} />
- Add **pluginhost.framework** reference in Xcode<br/>
 <img src="/iostjimg/plugin/tj-plugin-6.png" alt="pluginhost" style={{zoom:'30%'}} />
- Click +, choose Add Files, then select **pluginhost.framework**<br/>
 <img src="/iostjimg/plugin/tj-plugin-7.png" alt="pluginhost" style={{zoom:'30%'}} />
 <img src="/iostjimg/plugin/tj-plugin-8.png" alt="pluginhost" style={{zoom:'30%'}} />
- Set pluginhost.framework to **Do Not Embed**<br/>
 <img src="/iostjimg/plugin/tj-plugin-9.png" alt="pluginhost" style={{zoom:'30%'}} />

## Create Plugin Class
- Right-click the Xcode project, choose New File<br/>
 <img src="/iostjimg/plugin/tj-plugin-10.png" alt="pluginhost" style={{zoom:'30%'}} />
- Choose Swift File<br/>
 <img src="/iostjimg/plugin/tj-plugin-11.png" alt="pluginhost" style={{zoom:'30%'}} />
- Name it Plugin1 (or another name). Class content:<br/>
 <img src="/iostjimg/plugin/tj-plugin-12.png" alt="pluginhost" style={{zoom:'30%'}} />

:::tip
The project is ready at this point!
:::

## Method Override Reference

```swift showLineNumbers
// Comments based on demo code
import Foundation
import pluginhost
import JavaScriptCore
import UIKit
// Inheriting ECPlugin means the interface is ECPlugin, used as reflection base
// Inheriting JSExport means methods in this class can be called from JS
public class Plugin1 : NSObject, ECPlugin,JSExport{
    // Corresponds to pluginLoader.callMethodAny
    public func callMethodAny(_ methodName: String, _ data: Any) -> Any? {
        print("callMethodAny --- ",data)
        if(data is String){
            print("any is string...")
        }else if (data is UIImage){
            print("any is ui image ...")
        }
        return ""
    }
    // Corresponds to pluginLoader.callMethodStr
    public func callMethodStr(_ methodName: String, _ args: String) -> String {
      
        return " callMethodStr "+methodName + " "+args ;
    }
    // Corresponds to pluginLoader.callMethodData
    public func callMethodData(_ methodName: String, _ data: Data) -> String {
        let a = String(data: data, encoding: .utf8)
        return " callMethodData " + methodName + " "+(a ?? "");
    }
    // Corresponds to pluginLoader.callMethodReturnData
    public func callMethodReturnData(_ methodName: String, _ data: String) -> Data {
        let a = "callMethodDataReturnData "+methodName;
        return a.data(using: .utf8)!;
    }
    // Called when script ends to release resources
    public func disposed() {
        print("disposed plugin...")
    }
    
    // Constructor used when instantiating
    public required override init(){
        
    }
    
}

```


## Package Plugin
### Xcode Build
- Use Product menu in Xcode to build the plugin<br/>
 <img src="/iostjimg/plugin/tj-plugin-13.png" alt="pluginhost" style={{zoom:'30%'}} />
- Click `Build` to compile. Click `Show Build Folder in Finder`, go to the build output directory, and copy out the `.framework` file<br/>
 <img src="/iostjimg/plugin/tj-plugin-14.png" alt="pluginhost" style={{zoom:'30%'}} />

### Command-Line Build
- Pre-written script; replace `helloworldplugin` with your plugin name. Output is in the current project directory after build
```shell showLineNumbers
#!/bin/bash
WORK_DIR= `pwd`
echo `$WORK_DIR`

build_plugin_frame(){
  rm -rf /tmp/derivedDataPath/*
  xcodebuild build -project helloworldplugin.xcodeproj -scheme helloworldplugin -sdk iphoneos -configuration Release -derivedDataPath /tmp/derivedDataPath -allowProvisioningUpdates
  cp -r /tmp/derivedDataPath/Build/Products/Release-iphoneos/helloworldplugin.framework ./
  cd $WORK_DIR
}

build_plugin_frame
```

## Use Plugin
- For development
 - In EC iOS dev plugin, create a project, build a debug package, choose `Add Third-Party Plugin`, and select the framework file
 <img src="/iostjimg/plugin/tj-plugin-15.png" alt="pluginhost" style={{zoom:'30%'}} />
 - After packaging, sign the IPA, install on phone, and debug code
- For release
 - Same steps to select the plugin file. When publishing, choose `Standard Package` or `Enterprise Package`

