---
title: 常见问题
description: EasyClick 自动化脚本 iOS免越狱 常见问题
keywords:
  - EasyClick自动化脚本
  - iOS免越狱
  - iOS脚本常见问题
  - iOS
  - IPA
  - br
  - worker
  - js
  - ec
  - isScriptExit
  - 'true'
  - for
  - while
  - EasyClick
  - 手机自动化
  - 自动化测试
  - 脚本开发
  - 安卓自动化
---

## 脚本停止问题

- iOS采用的js引擎与ec的安卓不太一样，在脚本被动停止的时候会将 isScriptExit() 返回值设置为true，需要自己的业务中判断脚本停止
- 尤其是在是使用循环（包括，for，while等）一定要进行判断脚本是否停止，然后进行退出循环
- 如果不进行判断退出循环，容易造成app的cpu直接飙升到100%，正常写代码逻辑也是需要自己判断是否结束的
- 例子:<br/>

```javascript showLineNumbers
function main() {
    while (true) {
        // 退出循环
        if (isScriptExit()) {
            break;
        }
        sleep(100)
        console.log("d " + new Date())
    }
}

main()
```

## 自动化启动问题

- iOS脱机版本支持15+的iOS系统，安装代理IPA后，点击代理IPA图标，会自动出现Automation Running白色字样即可
- 如果不出现白色字样，可以通过中控或者后续提供的激活器，激活一下代理即可

## 横屏坐标问题

- iOS点击的坐标始终都是竖屏的，在某些情况找图找节点找到的是横屏坐标，所以需要进行坐标系的转变，把横坐标转换为竖坐标才能进行点击
- 以下情况需要进行转换坐标系：
  <br/>
  1、抓取的节点实际为横屏，这个时候程序查找的坐标系就是横屏，需要转换<br/>
  <img src="/iostjimg/node-h-j.png" alt="node-h-j.png" style={{zoom:'30%'}} /><br/>
  2、抓取的图片是横屏，这个时候程序查找的坐标系就是横屏，需要转换<br/>
  <img src="/iostjimg/tj-jt-h.png" alt="t-j.png" style={{zoom:'30%'}} /><br/>
  3、其他情况一律是竖屏坐标系
- 横屏转竖屏坐标系函数是 **convertPointToClickable**，请注意转换

## 脚本被杀解决方案
- 一般情况下iOS不允许后台任务的，但是EasyClick iOS脱机版做了大量的工作避免被iOS杀进程的
- 如何避免被杀进程呢
    - 1、在手机中 易点云测 - 调试版，设置里面打开保活1 和 保活2
    - 2、代码中调用 setComputeMode(1) 函数，让耗能计算放到代理程序中进行，如果你不理解这个函数，请勿使用或者直接默认参数2就行
    - 3、死循环的代码中 要使用  isScriptExit() 判断是否退出了脚本，避免cpu飙到100%造成APP进程被杀，（包括，for，while等）一定要进行判断脚本是否停止，然后进行退出循环
    - 4、系统给app的权限要给到位，包括定位、后台刷新
    - 5、打开日志悬浮窗，然后拖动悬浮窗到手机边缘，即可将悬浮窗贴边，打开地方`在 EC 主程序的设置-悬浮窗选项，选择后保存`
    - 6、脚本不要出bug，例如undefined变量等
- 做到以上5点，可最大程度保证iOS 不杀死EasyClick iOS脱机版的进程，让脚本平安运行

## 多worker模式工作
- iOS的js引擎天生单线程，这个无法避免，具体请看worker模块的问题

## 程序崩溃日志导出
- 当程序发生崩溃的时候，可以从iOS系统设置->隐私->分析与改进里面找到当时的崩溃日志，通常你的崩溃日志格式包含程序包名，时间以及后缀ips或者beta.点击进去，右上角点击分享可以各种方式分享出去
- 如果是EasyClick 主程序，可以搜索ecauto关键字过滤
- 如果是EasyClick 代理程序，可以搜索test-Runner关键字过滤
- 右上角蓝色导出按钮，导出的时候选择文件，会出现 易点云测的程序，选择这个程序就行了，然后使用爱思电脑程序访问易点云测的Documents，即可看到刚才导出的ips.synced文件

## 日志查看
- 选择idea连接到脱机的设备
- 选择idea `脱机日志查看`，可以查看到`崩溃日志`和`普通运行日志`
- `普通运行日志`需要设置setSaveLogEx函数，才会保存到文件


## 手机重启后无法启动代理服务
- 解决方式：
    - 1、使用爱思链接一下会自动刷入开发者镜像，然后点击投屏，拔线后即可启动代理服务
    - 2、使用EasyClick iOS的USB版本，配置好对应的开发者镜像，然后在中控开启下自动化，拔线后即可启动代理服务

## 减少代理IPA包大小
- 新版本代理 ipa包自带了ocr模型文件，如果不需要可以按照下面操作
- 使用压缩文件打开ipa文件，例如360压缩打开ipa文件,(ipa本身就是压缩文件)
- 依次进入PlugIns-EasyClick.xctest-文件夹
- 删除 .bin，.param，.onnx结尾的文件
- 删除keys.txt和ppocrv5_mobile_labels.txt文件即可
- 也可以解压ipa后，删除以上文件，重新压缩为ipa文件
