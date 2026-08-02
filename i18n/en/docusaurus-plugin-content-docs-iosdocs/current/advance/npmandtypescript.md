---
title: EasyClick iOS docs — NPM and TypeScript
hide_title: false
hide_table_of_contents: false
sidebar_label: NPM and TypeScript
description: 'EasyClick — npm and TypeScript support'
keywords:
 - EasyClick
 - mobile automation scripts
 - nodejs obfuscation
 - js code obfuscation
 - anti-tamper
 - npm and TypeScript support
 - npm
 - TypeScript
 - js
 - NPM
 - ts
 - https
 - www
 - com
 - ios
 - EC
 - mobile automation
 - test automation
---

# NPM and TypeScript support

:::tip
- This chapter applies to EC USB edition 8.0+
:::

## NPM support

### Install npm

- Many online tutorials exist. Examples:
 - https://www.bilibili.com/video/BV1wBDTYZECJ/
 - https://www.cnblogs.com/yang37/p/18423524
- Or search for "install npm"

### Enable npm in a project

- Right-click the project → **Add NPM support**
- A `package.json` is created in the project root when done
- Run `npm install <package-name>` in the project directory, e.g. `npm install md5` — follow the package's official usage

### Notes

- npm packages target different runtimes (web, Node, etc.). EC's JS runtime is pure JS — choose pure-JS packages only
- Not every npm package is supported

## TypeScript support

### Install TypeScript

- Examples:
 - https://www.runoob.com/typescript/ts-install.html
 - https://blog.csdn.net/cdns_1/article/details/140401513
 - Or search for "install TypeScript"

### Enable TypeScript in a project

- Right-click the project → **Add TypeScript support**
- A `tsconfig.json` is created and `.d.ts` references are generated for JS under `libs`
-

### TypeScript in the `js` folder

- JS under `js` is compiled to Java bytecode — no `require`; do not use `export`, but you can reference other JS files
- Write `.ts` in `js`, e.g. `Card.ts` — saving generates `Car.d.ts` and `Card.js` for use in other JS files
 ```typescript showLineNumbers
  // File: js/Car.ts
  class Car{
      test():void{
          logd("i am car.")
      }
  }
  ```
- Call from other JS:
 ```javascript showLineNumbers
  // File: js/main.js
  function main(){
      let car = new Car();
      car.test()
  }
  ```

### TypeScript in the `ts` folder

- Files under `ts` must `export` and be loaded with `require`
 ```typescript showLineNumbers
  // File: ts/Car1.ts
  class Car1{
      test1():void{
          logd("i am car1.")
      }
  }
  export ={Car1}
  ```
- Call from JS:
 ```javascript showLineNumbers
  // File: js/main.js
  function main(){
      let car = require("ts/Car1")
      let car1 = new car.Car1();
      car1.test1()
  }
  ```

## JS and TS mixed development

:::tip
- For security, put script JS/TS under `js`
- For security, put UI JS/TS under `subjs`
- For project references, files under `js` (JS and TS) are directly usable; TS outside `js` cannot be referenced from other projects
- The above avoids `require`. If you use `require`, put JS/TS outside `js` and obfuscate JS files
- Compiled JS from TS participates in IEC build; TS sources do not
:::
