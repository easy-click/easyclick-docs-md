---
title: 脱机插件编写
description: EasyClick iOS脚本免越狱免硬件，脱机插件编写
keywords:
  - EasyClick
  - iOS脚本
  - 自动化脚本
  - iOS免越狱
  - iOS脚本教程
  - iOS免硬件脚本
  - 脱机插件编写
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
  - 手机自动化
---
:::tip
- 插件编写需要使用xcode，本案例使用的xcode版本是 13.1 (13A1030d)，其他的暂未测试
- 插件支持swift，objective-c等语言编写，最终编译为framework即可
- 插件编写的时候需要使用EC官方提交的接口库 pluginhost.framework，[下载框架](/iosdocs/pluginhost.zip)，参考helloworldplugin 和helloworldplugin2插件代码下载地址, [点我下载](/iosdocs/tjplugindemo.zip)
:::
## 创建插件工程
- 选择Xcode，选择New - Project选项<br/>
<img src="/iostjimg/plugin/tj-plugin-1.png" alt="Project选项" style={{zoom:'30%'}} />
- 选择iOS的Framework类型，然后选择Next<br/>
<img src="/iostjimg/plugin/tj-plugin-2.png" alt="选择iOS的Framework类型" style={{zoom:'30%'}} />
- 输入插件名称，**不要用中文**，录入这里使用**hplugin**，其他选项如图所示，Bundle Identifier改成自己想要的<br/>
  <img src="/iostjimg/plugin/tj-plugin-3.png" alt="输入名称" style={{zoom:'30%'}} />
- 工程结构如下:<br/>
  <img src="/iostjimg/plugin/tj-plugin-4.png" alt="工程结构" style={{zoom:'30%'}} />


## 加入框架
- 将下载的**pluginhost.framework**复制到工程目录下<br/>
  <img src="/iostjimg/plugin/tj-plugin-5.png" alt="pluginhost" style={{zoom:'30%'}} />
- 在xcode中增加工程引用 **pluginhost.framework**<br/>
  <img src="/iostjimg/plugin/tj-plugin-6.png" alt="pluginhost" style={{zoom:'30%'}} />
- 选择 + 号后，选择 Add Files，然后选择 **pluginhost.framework**<br/>
  <img src="/iostjimg/plugin/tj-plugin-7.png" alt="pluginhost" style={{zoom:'30%'}} />
  <img src="/iostjimg/plugin/tj-plugin-8.png" alt="pluginhost" style={{zoom:'30%'}} />
- 更改pluginhost.framework为 **Do Not Embed**<br/>
  <img src="/iostjimg/plugin/tj-plugin-9.png" alt="pluginhost" style={{zoom:'30%'}} />

## 创建插件类
- 在xcode工程上，右键菜单，选择New File<br/>
  <img src="/iostjimg/plugin/tj-plugin-10.png" alt="pluginhost" style={{zoom:'30%'}} />
- 选择 Swift File<br/>
  <img src="/iostjimg/plugin/tj-plugin-11.png" alt="pluginhost" style={{zoom:'30%'}} />
- 命名为Plugin1，你可以使用其他名称，具体类内容如下<br/>
  <img src="/iostjimg/plugin/tj-plugin-12.png" alt="pluginhost" style={{zoom:'30%'}} />

:::tip
到这里工程已经创建好了！
:::

## 函数重载说明

```swift showLineNumbers
// 这里用demo中的代码做注释解释
import Foundation
import pluginhost
import JavaScriptCore
import UIKit
// 继承 ECPlugin 代表接口是ECPlugin，作为反射的基础
// 继承 JSExport 代表可以这个类里面的函数方法都可以给js调用
public class Plugin1 : NSObject, ECPlugin,JSExport{
    // 对应的是 pluginLoader.callMethodAny 函数
    public func callMethodAny(_ methodName: String, _ data: Any) -> Any? {
        print("callMethodAny --- ",data)
        if(data is String){
            print("any is string...")
        }else if (data is UIImage){
            print("any is ui image ...")
        }
        return ""
    }
    // 对应的是 pluginLoader.callMethodStr 函数
    public func callMethodStr(_ methodName: String, _ args: String) -> String {
      
        return " callMethodStr "+methodName + "  "+args ;
    }
    // 对应的是 pluginLoader.callMethodData 函数
    public func callMethodData(_ methodName: String, _ data: Data) -> String {
        let a = String(data: data, encoding: .utf8)
        return " callMethodData " + methodName + "  "+(a ?? "");
    }
    // 对应的是 pluginLoader.callMethodReturnData 函数
    public func callMethodReturnData(_ methodName: String, _ data: String) -> Data {
        let a = "callMethodDataReturnData "+methodName;
        return a.data(using: .utf8)!;
    }
    // 脚本结束的时候会调用这个函数进行 销毁资源
    public func disposed() {
        print("disposed plugin...")
    }
    
    // 这个是构造函数，实例化的时候使用
    public required override init(){
        
    }
    
}

```


## 打包插件
### xcode编译
- 使用xcode顶部菜单的Product编译插件<br/>
  <img src="/iostjimg/plugin/tj-plugin-13.png" alt="pluginhost" style={{zoom:'30%'}} />
- 点击`Build`会编译插件，点击`Show Build Folder in Finder`，进入插件编译的目录，复制.framework文件出来即可<br/>
  <img src="/iostjimg/plugin/tj-plugin-14.png" alt="pluginhost" style={{zoom:'30%'}} />

### 命令编译
- 已经写好的命令，将`helloworldplugin`替换为你的插件名称,编译完成后在你当前工程下
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

## 使用插件
- 写代码的时候使用
  - 在EC iOS开发插件中 新建一个工程，选择打一个调试包，然后选择`新增三方插件`,选择framework文件
    <img src="/iostjimg/plugin/tj-plugin-15.png" alt="pluginhost" style={{zoom:'30%'}} />
  - 打包完成后，把ipa签名后，安装到手机，就可以进行调试写代码了
- 发布加入插件
  - 操作和上面写代码一样选择插件文件，发布的时候选择`普通版本打包或者企业版本打包`
  
