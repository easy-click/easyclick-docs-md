---
keywords:
  - openapi
  - changeTask
  - POST
  - http
  - 192.168.1.182
  - Content
  - Type
  - application
  - json
  - EasyClick
  - 手机自动化
  - 自动化测试
  - 脚本开发
  - 安卓自动化
  - iOS自动化
  - 鸿蒙Next
  - 远程投屏
  - OCR
---


## 更改云控任务状态
- 可以执行任务
- 可以停止任务
- 可以移出、增加执行设备
- 可以移出、增加参数等

POST : 云控地址/openapi/changeTask
例如: http://192.168.1.182:8099/openapi/changeTask
Content-Type: application/json

```json showLineNumbers

{
  "dataSecret":"后台配置的API云控通信密钥", // 配置在config.toml文件中的appkey选项，默认是 test123
  "taskId":"1",// 任务ID，在云控建好的
	"status":"0",// 1 停止本地，2 停止远程 0 开启
	"addDevices":"001,002,003",//需要增加执行的设备
    "removeDevices":"007,009",//需要减少执行的设备
  // 要增加的参数解释
  //key = 代表参数名称
  //type = 参数类型，1代表是文本 2 代表是单选框  3代表是多选框
  // value = 代表是参数值，如果是文本情况下，直接就是字符串
  // 如果是单选框，也填写字符串
  // 如果是多选框，填写json数组，因为是多选，必须是数组
  // options = 代表是候选选择，如果是文本，请忽略
  // 如果是单选框，请填写json数组
  // 如果是多选框，请填写json数组
  "addParamEx":[
    {
      "key":"我是文本",
      "type":"1",
      "value":"我是文本的值"
    },
    {
      "key":"我是单选框",
      "type":"2",
      "value":"2222",
      "options":["我是选中","2222"]
    },
    {
      "key":"我是多选框",
      "type":"3",
      "value":["111","222"],
      "options":["111","2222","333"]
    }
  ],
  "removeParam":["ke1"]
}


```



```sequence
title: 下单自动执行流程
participant 客户
participant 下单系统
participant 云控系统
participant 设备

客户->下单系统: 客户下单并支付完成
下单系统->云控系统: 调用(/api/changeTask) 执行任务某个任务，\n并加入某写设备和参数
云控系统->云控系统: 改变任务状态\n参数、设备等
云控系统->设备: 下发任务到设备
设备->设备: 循环执行脚本
设备->云控系统: 调用(/api/changeTask)达到条件\n将自己从任务的设备中移出


```



