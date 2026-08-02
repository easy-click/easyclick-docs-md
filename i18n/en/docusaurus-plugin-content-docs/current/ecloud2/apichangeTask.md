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
 - mobile automation
 - automation testing
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
 - remote screen mirroring
 - OCR
---


## Change cloud task status
- Start a task
- Stop a task
- Add / remove devices
- Add / remove parameters

POST: `cloud-control-url/openapi/changeTask`
Example: `http://192.168.1.182:8099/openapi/changeTask`
Content-Type: `application/json`

```json showLineNumbers

{
 "dataSecret":"API cloud communication secret from backend", // config.toml → appkey; default test123
 "taskId":"1",// task ID created in cloud control
	"status":"0",// 1 stop local, 2 stop remote, 0 start
	"addDevices":"001,002,003",// devices to add
 "removeDevices":"007,009",// devices to remove
 // addParamEx fields:
 // key = parameter name
 // type = 1 text, 2 radio, 3 checkbox
 // value = string for text/radio; JSON array for checkbox
 // options = candidates; ignore for text; JSON array for radio/checkbox
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
title: Auto-run after order flow
participant Customer
participant OrderSystem
participant CloudControl
participant Device

Customer->OrderSystem: Place order and pay
OrderSystem->CloudControl: Call (/api/changeTask) to start a task,\nadd devices and parameters
CloudControl->CloudControl: Update task status,\nparameters, devices
CloudControl->Device: Dispatch task to device
Device->Device: Loop script
Device->CloudControl: Call (/api/changeTask) when done\nto remove itself from the task devices


```
