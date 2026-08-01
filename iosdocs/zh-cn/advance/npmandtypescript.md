---
title: EasyClick ios文档_ios手机手机自动化脚本_npm和typescript支持
hide_title: false
hide_table_of_contents: false
sidebar_label: NPM和TypeScript支持
description: 'EasyClick 代码混淆,使npm和typescript支持等'
keywords:
  - EasyClick
  - 手机自动化脚本
  - nodejs混淆
  - js代码混淆
  - 防破解
  - npm和typescript支持
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
  - 手机自动化
  - 自动化测试
---

# NPM和TypeScript支持

:::tip
- 本章节说明基于EC USB版本 8.0+
:::

## NPM支持

### 安装npm

- 安装npm网络上教程很多。这里简单列举几个
    - https://www.bilibili.com/video/BV1wBDTYZECJ/
    - https://www.cnblogs.com/yang37/p/18423524
- 也可以自己百度 `安装npm` 关键字

### 工程支持npm

- 在工程上-鼠标右键-选择·新增NPM支持·
- 新增完成后，会在当前工程目录下生成一个package.json文件，表示新增完成
- 剩余的只需要在工程下，执行`npm install xxx类库名称`即可完成类库的安装，例如 `npm install md5`，即可安装md5类库，使用方式直接安装md5官方说明

### 注意事项

- 由于npm类库存在不同运行环境包括web，node等，EC的js环境是纯js的，使用npm安装的类库，一定要找纯js环境的
- 并不是所有的`npm类库`都支持

## TypeScript支持

### 安装TypeScript

- 网络上教程很多。这里简单列举几个
    - https://www.runoob.com/typescript/ts-install.html
    - https://blog.csdn.net/cdns_1/article/details/140401513
    - 也可以自己百度 `安装TypeScript` 关键字

### 工程支持TypeScript

- 在工程上-鼠标右键-选择·新增TypeScript支持
- 新增完成后，会在当前工程目录下生成一个 `tsconfig.json` 文件，同时会对libs下面的js生成d.ts引用文件
-

### js文件夹下编写ts

- js文件夹下编写的js文件都会转换为java的字节码，所以无法使用require，请不要使用export关键字导出，但是可以引用其他文件的js文件
- 在js文件编写ts文件，例如Card.ts，保存文件的时候会自动生成Car.d.ts和Card.js文件，在其他js文件中直接引用
  ```typescript showLineNumbers
  // 文件名称 js/Car.ts
  class Car{
      test():void{
          logd("i am car.")
      }
  }
  ```
- 在其他js文件中调用
  ```javascript showLineNumbers
  // 文件名称 js/main.js
  function main(){
      let car = new Car();
      car.test()
  }
  ```   

### ts文件夹下编写ts文件

- 注意ts文件夹下需要export导出，在其他地方进行require引用
   ```typescript showLineNumbers
  // 文件名称 ts/Car1.ts
  class Car1{
      test1():void{
          logd("i am car1.")
      }
  }
  export ={Car1}
  ```
- 在其他js文件中调用
  ```javascript showLineNumbers
  // 文件名称 js/main.js
  function main(){
      let car = require("ts/Car1")
      let car1 = new car.Car1();
      car1.test1()
  }
  ``` 

## js和ts混编建议

:::tip
- 为了安全 建议脚本的js和ts文件都放到js文件夹下编写
- 为了安全 建议UI的js和ts文件都放到subjs文件夹下编写
- 如果存在工程引用，js文件夹下的可以直接使用，包括js和ts文件，其他文件下的ts无法被另外工程引用
- 以上的建议无需使用require即可直接使用，如果非要使用require模式，将js和ts文件放到非js文件夹，建议混淆js文件
- ts文件编译后的js会参与iec的编译，ts源文件不会
:::
