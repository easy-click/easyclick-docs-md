---
title: EasyClick Android docs — Code obfuscation
hide_title: false
hide_table_of_contents: false
sidebar_label: Code obfuscation
description: 'EasyClick code obfuscation — obfuscate JS with Node.js before a second compile to improve security and protect license keys'
keywords:
 - EasyClick
 - mobile automation scripts
 - nodejs obfuscation
 - js code obfuscation
 - anti-tamper
 - non-root script anti-tamper
 - obfuscator
 - https
 - js
 - javascript
 - Node
 - npm
 - install
 - json
 - blog
 - csdn
 - mobile automation
 - test automation
---

# Code obfuscation

## What is obfuscation?
- During JS compilation, code is transformed with junk instructions and control-flow changes to protect source code

## Getting started
- After upgrading the EC IDEA plugin to 9.4.0+, an `obfuscator.json` file is created under the project module
- Obfuscation is off by default — set `binPath` correctly in the JSON config

## Install and configure the obfuscator
### Install Node.js
- Reference: [https://blog.csdn.net/qq_48485223/article/details/122709354](https://blog.csdn.net/qq_48485223/article/details/122709354)
### Install npm install -g javascript-obfuscator
- On Windows, run in cmd or PowerShell
- Reference: [https://www.npmjs.com/package/javascript-obfuscator](https://www.npmjs.com/package/javascript-obfuscator)
- Or search for javascript-obfuscator installation guides
```shell showLineNumbers
npm install -g javascript-obfuscator
npm install -g class-validator
```

## Configuration
- Default settings below are tested and working. See the tool site for more: [[https://www.npmjs.com/package/javascript-obfuscator](https://www.npmjs.com/package/javascript-obfuscator)]
- `nodeBinPath`: path to Node.js `node.exe` — see example
- `obfuscatorBinPath`: path to the `javascript-obfuscator` executable
 - After installing Node.js, global modules are usually at: `C:\Users\Administrator\AppData\Roaming\npm\node_modules` (check with `npm root -g`; username varies)
 - Copy the full path to `javascript-obfuscator` under `node_modules`

:::tip
- Note: remove `//` comments before use — they will cause errors
 :::
```json showLineNumbers
{
  "nodeBinPath": "D:\\programe\\nodejs\\node.exe",
  "obfuscatorBinPath": "C:\\Users\\Administrator\\AppData\\Roaming\\npm\\node_modules\\javascript-obfuscator\\bin\\javascript-obfuscator",
  "target": "node",
  "compact": false, // compress code
  "log": true, // show log output
  "optionsPreset": "high-obfuscation", // obfuscation level
  "deadCodeInjection": false,
  "simplify": false,
  "seed": 10, // random seed count
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

## Notes
- If `obfuscatorBinPath` is set and the file exists, obfuscation runs by default for both JS and dex compile modes
- Clear `obfuscatorBinPath` or delete `obfuscator.json` to disable obfuscation during compile
- Obfuscation increases bundle size — avoid very long single files; split into modules
- Custom settings may break scripts after obfuscation — restore defaults if that happens
- Enable `log` to see obfuscation details during compile
- Obfuscation is CPU-heavy — disable during daily dev; enable for release or packaging




