---
title: EasyClick自动化脚本_鸿蒙Next自动化_高级功能_代码混淆
hide_title: false
hide_table_of_contents: false
sidebar_label: 代码混淆
description: 'EasyClick 代码混淆,使用nodejs对js代码进行混淆后，在进行二次编译，大大在增加安全性，防止破解卡密等'
keywords:
  - EasyClick
  - 手机自动化脚本
  - nodejs混淆
  - js代码混淆
  - 防破解
  - 免root脚本防破解
  - obfuscator
  - javascript
  - https
  - js
  - Node
  - npm
  - install
  - json
  - blog
  - csdn
  - 手机自动化
  - 自动化测试
---

# 代码混淆

## 什么是混淆
- 混淆是指在js编译期间，对代码进行花指令、流程更改等转换，目的是为了保护代码

## 如何开始混淆的工作
- IDEA开发工具会在工程模块下面创建一个 obfuscator.json文件
- 默认是不开启混淆的，需要正确设置json配置中的binPath的路径才能支持工作

## 安装和设置混淆器
### 安装Node.js
- 参考: [https://blog.csdn.net/qq_48485223/article/details/122709354](https://blog.csdn.net/qq_48485223/article/details/122709354)
### 安装 npm install -g javascript-obfuscator
- windows在cmd 或者powershell中运行以下命令
- 参考[https://www.npmjs.com/package/javascript-obfuscator](https://www.npmjs.com/package/javascript-obfuscator)
- 也可以百度搜索 javascript-obfuscator 的安装
```shell showLineNumbers
npm install -g javascript-obfuscator
npm install -g class-validator
```

## 配置说明
- 这里的默认配置是测试过的，可以运行的，更多配置可以参考工具的官网[[https://www.npmjs.com/package/javascript-obfuscator](https://www.npmjs.com/package/javascript-obfuscator)]
- nodeBinPath属性，nodejs的的node.exe文件路径，请看实例
- obfuscatorBinPath属性，javascript-obfuscator 的可执行文件的路径,
    - 安装NodeJs后，默认全局安装路径为：C:\Users\Administrator\AppData\Roaming\npm\node_modules（可通过npm命令npm root -g查看。Administrator用户名各人一般不同。）
    - node_modules 找到 javascript-obfuscator全路径复制过来

```json showLineNumbers
{
  "nodeBinPath": "D:\\programe\\nodejs\\node.exe",
  "obfuscatorBinPath": "C:\\Users\\Administrator\\AppData\\Roaming\\npm\\node_modules\\javascript-obfuscator\\bin\\javascript-obfuscator",
  "target": "node",
  "compact": false, // 是否压缩代码
  "log": true, // 是否展示log
  "optionsPreset": "high-obfuscation", // 混淆程度
  "deadCodeInjection": false,
  "simplify": false,
  "seed": 10, // 随机种子数量
  "controlFlowFlattening": true,
  "controlFlowFlatteningThreshold": 1,
  "unicodeEscapeSequence": false,
  "stringArray": true,
  "stringArrayRotate": false,
  "stringArrayShuffle": false,
  "stringArrayThreshold": 1,
  "stringArrayWrappersCount": 5,
  "stringArrayEncoding": ["rc4"],
  "stringArrayCallsTransform": false,
  "selfDefending": false,
  "splitStrings": false,
  "splitStringsChunkLength": 1
}
```

## 特别说明
- 配置文件中 obfuscatorBinPath 文件路径配置了，并且文件存在，就会默认进行混淆，包括EC编译的js模式和dex模式
- 删除 obfuscatorBinPath 属性值，或者直接删除obfuscator.json,都会导致js代表编译期间不进行混淆
- 混淆会增加代码的体积，不建议一个文件中有很长的代码，建议分开文件进行模块化
- 在自行改动混淆配置属性的时候，可以会导致混淆后无法运行脚本的情况，这个时候只要自己恢复一下默认设置即可
- log开关可以在编译期间看到混淆代码的详细信息
- 混淆是比较耗费电脑资源的的事情，建议在平时调试开发的时候，关闭混淆，在发布、或者打包的时候进行开启





