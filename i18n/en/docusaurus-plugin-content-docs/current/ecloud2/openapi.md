---
title: EasyClick cloud control system — Open API
hide_title: false
hide_table_of_contents: false
sidebar_label: Cloud Open API
description: >-
 EasyClick cloud control system Open API for scripts, tasks, and device integration.
 Install is simple: use BT Panel for MySQL/Redis, copy the program into the site directory, update MySQL config, and start the ecloud binary
keywords:
 - EasyClick
 - mobile automation scripts
 - automation software
 - cloud control system
 - cloud control install
 - Douyin cloud control
 - Kuaishou cloud control
 - game cloud control
 - API
 - tip
 - config
 - toml
 - Google
 - Firefox
 - 'no'
 - verify
 - code
 - mobile automation
 - automation testing
---
# Cloud Open API

:::tip
- For full-site API access:
- 1. Under `config/`, open `config.toml`, set `no_limit_token` to a high-privilege request secret (any string you choose), then restart cloud control
- 2. In Chrome or Firefox DevTools → **Network**, capture requests
- 3. When calling cloud APIs, add HTTP header `no_limit_token` with the value from step 1
:::
:::tip
- Per-user auth
- 1. If you prefer per-user API auth instead of full-site: simulate login to get a token. Also set `no_verify_code` in `config.toml` to a random string, e.g. `aaabbbccc`
- 2. On the login request, besides `username` and `password`, add `ignoreCode` equal to `no_verify_code`
- 3. Find the login endpoint via Chrome/Firefox DevTools → **Network** capture
:::

## Get task info

POST: `cloud-control-url/openapi/getTask`
Example: `http://192.168.1.182:8099/openapi/getTask`
Content-Type: `application/json`

```json showLineNumbers
{
  "dataSecret": "API cloud communication secret from backend"
  // config.toml → appkey; default is test123
}
```
- Response:
```json showLineNumbers
[
  {
    "cacheScript": 1,
    "delaySecond": 0,
    "execNumber": 9999,
    "cronText": "* * * * * ?",
    "deviceNos": "006,001,003x,test222", // devices that run this task
    "downloadUrl": "http://192.168.2.7:8098/pub_upload/2022-10-07/cnfdy1l2biiwpl8qcm.iec",
    "execType": 1,
    "scheduleTime": null,
    "scheduleType": 0,
    "execStatus": 1,
    "endTime": null,
    "scriptId": 16,
    "scriptMd5": "b7ec603dffd9527c98e505bdd0eea137",
    "scriptName": "html5",
    "scriptVersion": "1",
    "sort": 1,
    "taskId": 11, // task ID — usually all you need
    "taskName": "23", // task name
    "tenantId": 1, // tenant id
    // parameter config
    "valueConfig": "[{\"id\":3,\"key\":\"我是多选框\",\"options\":[\"111\",\"2222\",\"333\"],\"type\":\"3\",\"value\":[\"111\",\"222\",\"333\"]},{\"id\":2,\"type\":\"2\",\"key\":\"b\",\"value\":\"111\",\"options\":[\"111\",\"2\"]},{\"id\":1,\"type\":\"\",\"key\":\"a\",\"value\":\"111333\",\"options\":\"111\"},{\"id\":1,\"key\":\"我是文本\",\"options\":\"\",\"type\":\"\",\"value\":\"我是文本的值\"},{\"id\":2,\"key\":\"我是单选框\",\"options\":[\"我是选中\",\"2222\"],\"type\":\"2\",\"value\":\"我是选中\"}]",
    // formatted parameter values
    "valueJson": {
      "a": "111333",
      "b": "111",
      "我是单选框": "我是选中",
      "我是多选框": [
        "111",
        "222",
        "333"
      ],
      "我是文本": "我是文本的值"
    }
  },
  {
    "cacheScript": 1,
    "delaySecond": 222,
    "execNumber": 133,
    "cronText": "* * * * * ?",
    "deviceNos": "003,test222,001,002",
    "downloadUrl": "http://192.168.2.7:8098/pub_upload/2022-10-07/cnfdy1l2biiwpl8qcm.iec",
    "execType": 2,
    "scheduleTime": null,
    "scheduleType": 3,
    "execStatus": 1,
    "endTime": null,
    "scriptId": 16,
    "scriptMd5": "b7ec603dffd9527c98e505bdd0eea137",
    "scriptName": "html5",
    "scriptVersion": "1",
    "sort": 1,
    "taskId": 10,
    "taskName": "33",
    "tenantId": 1,
    "valueConfig": "[{\"id\":3,\"key\":\"我是多选框\",\"options\":[\"111\",\"2222\",\"333\"],\"type\":\"3\",\"value\":[\"111\",\"222\",\"333\"]},{\"id\":2,\"type\":\"2\",\"key\":\"b\",\"value\":\"111\",\"options\":[\"111\",\"2\"]},{\"id\":1,\"type\":\"\",\"key\":\"a\",\"value\":\"111333\",\"options\":\"111\"},{\"id\":1,\"key\":\"我是文本\",\"options\":\"\",\"type\":\"\",\"value\":\"我是文本的值\"},{\"id\":2,\"key\":\"我是单选框\",\"options\":[\"我是选中\",\"2222\"],\"type\":\"2\",\"value\":\"我是选中\"}]",
    "valueJson": {
      "a": "111333",
      "b": "111",
      "我是单选框": "我是选中",
      "我是多选框": [
        "111",
        "222",
        "333"
      ],
      "我是文本": "我是文本的值"
    }
  }
]

```


## Task control

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



## WebHook heartbeat / log push
- Pushes heartbeat and script logs to an external system
- HTTP or WebSocket (pick one)
- Configure in `config/config.toml`
- Set `mode`, then `httpUrl` or `wsUrl`, and restart cloud control
```markdown
# Heartbeat forward (push heartbeat data and script logs to an external system)
[heartbeatForward]
# Enable
enabled = true
# Mode: http or websocket
mode = "http"

# HTTP mode (POST JSON)
# e.g. http://192.168.2.26:8199/heartbeat
httpUrl = ""
httpTimeoutMs = 3000
# Optional custom header (e.g. Authorization)
httpHeaderKey = "ecloud_token"
httpHeaderValue = "90284kdf9i4k"

# WebSocket mode (TextMessage JSON)
# e.g. ws://192.168.2.26:8199/wstest
wsUrl = ""
wsDialTimeoutMs = 3000
wsWriteTimeoutMs = 1500
wsReconnectWaitMs = 2000
# Header auth
wsHeaderKey = "ecloud_token"
wsHeaderValue = "90284kdf9i4k"

# Queue (async send, does not block main flow)
queueSize = 10240
# When full: true = drop + warn; false = block RecordHeartBeat
dropWhenFull = true

```
