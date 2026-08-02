---
title: EasyClick Android — Cloud control script functions
hide_title: false
hide_table_of_contents: false
sidebar_label: Cloud control script functions
description: >-
 EasyClick cloud control platform script function module — no integration required.
 Call functions directly to interact with cloud control and fetch cloud control data
keywords:
 - EasyClick
 - mobile automation scripts
 - automation software packaging
 - cloud control platform integration
 - cloud control data retrieval
 - Douyin cloud control
 - Kuaishou cloud control
 - game cloud control
 - ecloud
 - ID
 - URL
 - return
 - EC
 - 'null'
 - log
 - getDeviceNo
 - getDeviceId
 - getCloudUrl
---

# Cloud control script functions

## EC example code

- Cloud control example code below

```javascript showLineNumbers
function main() {
    // If the automation service is running normally
    if (!autoServiceStart(3)) {
        logd("Automation service failed to start; cannot run script");
        exit();
        return;
    }
    logd("Starting script...{} = {}", ecloud.getDeviceNo(), ecloud.getCloudUrl());
    let ts = ecloud.getTaskInfo();
    logd(ts);
    if (ts == null) {
        logd("No task info");
        return;
    }
    logd("Task info:" + JSON.stringify(ts));
    logd(ts["taskName"]);
    if (ts.valueJson) {
        // Get value for name = haoma in task parameter settings
        logd("valueJson " + ts.valueJson["haoma"]);
        // Get value for name = 账号 in task parameter settings
        logd("valueJson " + ts.valueJson['账号']);
        // Print parameters
        logd("valueJson " + ts.valueJson);

    }

    // Write your business logic here
    clickSettingTask();
    sleep(3000)
    // Demo: create, read, update, delete, append, and remove one line
    dataOptDemo()

}

/**
 * Open the settings button
 **/
function clickSettingTask() {
    // Simulate business logic
    sleep(4000);

}

function dataOptDemo() {
    // Get data by group name
    let data = ecloud.getData({
        "groupName": "账号组2"
    });
    if (data) {
        logd("账号组2 data: " + JSON.stringify(data));
    }

    // Get data by group name + data name
    data = ecloud.getData({
        "groupName": "账号组2",
        "dataName": "001-1112"
    });
    if (data) {
        logd("账号组2 data + dataName : " + JSON.stringify(data));
    }

    // Simulate uploading data
    let d = ecloud.addData({
        "groupName": "账号组2",
        "dataName": ecloud.getDeviceNo() + "-111",
        "content": "xxx" + new Date()
    });
    logd("First create 账号组2 data result - " + d)


    // Simulate updating data
    let update2 = ecloud.updateData({
        "groupName": "账号组2",
        "dataName": ecloud.getDeviceNo() + "-111",
        "content": "牛逼" + new Date()
    });
    logd("Update data result - " + d)
    sleep(3000)

    // Remove data by group + data name
    // let remove2 = ecloud.removeData({
    // "groupName": "账号组2",
    // "dataName": ecloud.getDeviceNo() + "-111"
    //
    // });
    // logd("Group + data remove result - " + remove2)
    
    // Remove data by group + data name
    let remove3 = ecloud.removeData({
        "groupName": "账号组2"

    });
    logd("Group remove result - " + remove3)

    // Demo creating data again
    let d2 = ecloud.addData({
        "groupName": "账号组2",
        "dataName": ecloud.getDeviceNo() + "-111",
        "content": "星星xxx" + new Date()
    });
    logd("Second create 账号组2 data result - " + d2)
    sleep(3000)
    // Demo appending data
    let append1 = ecloud.appendOneLineData({
        "groupName": "账号组2",
        "dataName": ecloud.getDeviceNo() + "-111",
        "content": "我是要增加的数据1"
    });
    logd("Append data result - " + append1)

    // Second append
    let append2 = ecloud.appendOneLineData({
        "groupName": "账号组2",
        "dataName": ecloud.getDeviceNo() + "-111",
        "content": "我是要增加的数据2"
    });
    logd("Append data result - " + append2)

    // First remove-one-line
    let removeline1 = ecloud.removeOneLineData({
        "groupName": "账号组2",
        "dataName": ecloud.getDeviceNo() + "-111",
        "content": "我是要增加的数据1"
    });
    logd("Remove one line result - " + removeline1)
    sleep(3000)
}


function autoServiceStart(time) {
    for (var i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        var started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main();
```

## Basic operations

### ecloud.log Send log

* Send a log message to the cloud
* @param msg Log message

 ```javascript showLineNumbers
function main() {
    ecloud.log("I am a CLOUD log")
}

main();
 ```

### ecloud.getDeviceNo Get device number

* Get the device number
* @return `{string}` Device code or null

 ```javascript showLineNumbers
function main() {
    var d = ecloud.getDeviceNo()
    logd(d)
}

main();
 ```

### ecloud.getDeviceId Get device ID

* Get the device ID
* Requires EC 11.8.0+
* @return `{string}` Device ID or null

 ```javascript showLineNumbers
function main() {
    var d = ecloud.getDeviceId()
    logd(d)
}

main();
 ```

### ecloud.getCloudUrl Get cloud control URL

* Get the cloud control URL
* @return `{string}` Cloud control URL or null

 ```javascript showLineNumbers
function main() {
    var d = ecloud.getCloudUrl()
    logd(d)
}

main();
 ```

### ecloud.getTaskInfo Get current task info

* Get info for the current task (already pushed to the device locally)
* @return `{json}` Object

 ```javascript showLineNumbers
function main() {
    var d = ecloud.getTaskInfo()
    logd(d)
}

main();
 ```

- Return value example
- taskId: Cloud main task ID
- taskName: Cloud main task name
- scriptId: Script cloud ID
- scriptName: Script name
- scriptVersion: Script version
- scriptName: Script name
- scriptUrl: Script URL
- valueJson: Extra parameters
- Values returned by the task info function

 ```json showLineNumbers
{
    "taskId": "1",
    "taskName": "X手浏览",
    "sort": 3,
    "scriptId": "1",
    "scriptName": "X手脚本",
    "scriptUrl": "http://baidu.com/a.iec",
    "scriptVersion": "1",
    "valueJson": {
        "a1": "test",
        "a2": "test"
    }
}
```

### ecloud.removeScriptFile Clear script
 * Delete the script file for security
 * Requires EC 6.11.0+
 * @return `{bool}` true on success, false on failure

 ```javascript showLineNumbers
function main() {
    var d = ecloud.removeScriptFile()
    logd(d)
}

main();
 ```

## Data sharing table operations

- Data sharing maps to **Data groups** and **Data list** under cloud control data management
- Table structure is fixed; users cannot change it

### ecloud.getData Get data

* Get data by group name and/or data name; the data must exist in cloud control
* @param map Extensible parameter map
* Example ```{"groupName":"数据组1","dataName":"key"}```
* Key definitions: groupName = data group name
* dataName = data name

 ```javascript showLineNumbers
function main() {
 var d = ecloud.getData({"groupName": "资源组1", "dataName": "111"})
 logd(d)
}

main();
 ```

- Return value example
- id: Data ID (can be ignored)
- name: Data name
- content: Data content

```json showLineNumbers
[
 {
 "id": "",
 "name": "",
 "content": ""
 }
]

```


### ecloud.getDataPop Get and delete data

* Get data by group name and/or data name; cloud control deletes it automatically after fetch. The data must exist in cloud control
* @param map Extensible parameter map
* Example ```{"groupName":"数据组1","dataName":"key","getType":1,"size":1}```
* Key definitions: groupName = data group name
* dataName = data name
* getType = fetch mode: 1 head, 2 tail, 3 random
* size = number of records to fetch
* @return `{json array}` Empty or JSON array

 ```javascript showLineNumbers
function main() {
 var d = ecloud.getDataPop({"groupName": "资源组1", "dataName": "111", "getType": 1, "size": 10})
 logd(d)
 logd(d[0])
}

main();
 ```

- Return value example

```json showLineNumbers
 ["aaa","bbb"]
```

### ecloud.addData Add data
 * Add a data record; if the group already exists, data is appended automatically
 * @param map Extensible parameter map
 * Example ```{"groupName":"数据组1","dataName":"key","content":"数据"}```
 * Key definitions:
 * groupName = data group name
 * dataName = data name
 * content = content
 * @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 var d = ecloud.addData({"groupName": "资源组1", "dataName": "111", "content": "test数据"})
 logd(d)
}

main();
```

### ecloud.updateData Update data

* Update data under a group; group name and data name are required

* @param map Extensible parameter map
* Example
  * ```{"groupName":"数据组1","dataName":"key","content":"数据"}```
  * Key definitions:
  * groupName = data group name
  * dataName = data name
  * content = content
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 var d = ecloud.updateData({"groupName": "资源组1", "dataName": "111", "content": "test数据"})
 logd(d)
}

main();
 ```

### ecloud.removeData Delete data
* Delete data under a group. If only the group name is given, all data in that group is deleted. If both group name and data name are given, only matching records are deleted
* @param map Extensible parameter map
* Example ```{"groupName":"数据组1","dataName":"key"}```
* Key definitions:
* groupName = data group name
* dataName = data name
* @return `{bool}` true on success, false on failure

 ```javascript showLineNumbers
function main() {
 var d = ecloud.removeData({"groupName": "资源组1", "dataName": "111"})
 logd(d)
}

main();
 ```

### ecloud.appendOneLineData Append one line

* Look up content for the data name under the group and append one line at the end
* @param map Extensible parameter map
* Example ```{"groupName":"数据组1","dataName":"key","content":"数据"}```
* Key definitions:
  * groupName = data group name
  * dataName = data name
  * content = content
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 var d = ecloud.appendOneLineData({"groupName": "资源组1", "dataName": "111", "content": "test数据"})
 logd(d)
}

main();
```

### ecloud.removeOneLineData Remove one line

* Look up content for the data name under the group and remove one line equal to content
* @param map Extensible parameter map
* Example ```{"groupName":"数据组1","dataName":"key","content":"数据"}```
* Key definitions:
  * groupName = data group name
  * dataName = data name
  * content = content
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 var d = ecloud.removeOneLineData({"groupName": "资源组1", "dataName": "111", "content": "test数据"})
 logd(d)
}

main();
```

## Dynamic data API

- Dynamic data maps to **Data groups** and **Data list** under cloud control data management
- Table structure is not fixed; users create tables and fill data themselves

### ecloud.dynamicCreateTable Create or update dynamic table

* Create or update a dynamic data table schema
* If columns adds new fields, they are created automatically; if a field is removed from columns, it is dropped from the table
* Use caution when altering tables to avoid data loss!!!
* Requires EC 6.16.0+
* @param param Parameters

```json showLineNumbers
 {
 	"tableName": "我是牛逼的表",
 	"tableNameEn": "niubi_table",
 	"columns": [{
 			"columnInfo": "姓名",
 			"columnName": "name",
 			"columnSize": 500
 		},
 		{
 			"columnInfo": "年龄",
 			"columnName": "age",
 			"columnSize": 500
 		},
 		{
 			"columnInfo": "性别",
 			"columnName": "sex",
 			"columnSize": 500
 		}
 	]
 }
```


* Fields:
  * tableName: Chinese display name (label only, not the actual table name)
  * tableNameEn: English table name — the real table name
  * columns: Column definitions; all column types are strings
  * columnInfo: Column comment/description
  * columnName: Column name — use English; no spaces or special characters
  * columnSize: Column length
  * @return `{string}` JSON string; parse to a JSON object
  * Success example: ```{"result":{"data":1}}``` — data is the number of affected rows
  * Failure example: ```{"result":{"msg":"我是错误信息"}}```

```javascript showLineNumbers
function main() {
 let crateTable = {
 "tableName": "我是牛逼的表",
 "tableNameEn": "niubi_table",
 "columns": [
 {
 "columnInfo": "姓名",
 "columnName": "name",
 "columnSize": 500
 },
 {
 "columnInfo": "年龄",
 "columnName": "age",
 "columnSize": 500
 },
 {
 "columnInfo": "性别",
 "columnName": "sex",
 "columnSize": 500
 }
 ]
 };

 let creae = ecloud.dynamicCreateTable(crateTable)
 logd("=== {}", creae);
}

main();
```

### ecloud.dynamicQuery Query dynamic table data

* Query dynamic data
* Requires EC 6.16.0+
* @param param Parameters
```json showLineNumbers
 {
 	"pageNumber": 1,
 	"pageSize": 4,
 	"fields": "id,name",
 	"query": "and name like '%我是%'",
 	"tableNameEn": "niubi_table",
 	"search": {
 		"id": "1",
 		"name": "我是name"
 	}
 }
```

* Fields:
  * pageNumber: Page number, starting at 1. Use -1 to disable (uses LIMIT internally)
  * pageSize: Page size. Use -1 to disable (uses LIMIT internally)
  * fields: Columns to return; optional. Comma-separated English names — see example
  * query: Custom SQL-style WHERE clause
  * order: Sort clause, e.g. `order by id desc`. If order contains LIMIT, set pageNumber to -1 to avoid duplicate LIMIT
  * tableNameEn: English table name
  * search: Equality filters; use either search or query, not both
  * search example: `"id":"1"` means id = 1
  * @return `{string}` JSON string; parse to a JSON object
  * Success example: ```{"result":{"data":[{"name":"3","id":2}]}}``` — data is the result array
  * Failure example: ```{"result":{"msg":"我是错误信息"}}```

```javascript showLineNumbers
function main() {
 let query = {
 "pageNumber": 1,
 "pageSize": 2,
 "fields": "",
 "query": " and age like '%2%'",
 "tableNameEn": "niubi_table",
 "order":"",
 "search": {}
 };
 let queryr = ecloud.dynamicQuery(query)
 logd("=== {}", queryr);

 // Random query
 let query2 = {
 "pageNumber": -1,
 "pageSize": -1,
 "fields": "",
 "query": "",
 "tableNameEn": "niubi_table",
 "order":"order by rand() limit 1",
 "search": {}
 };
 let queryr2 = ecloud.dynamicQuery(query2)
 logd("=== {}", queryr2);
}

main();
```

### ecloud.dynamicAdd Add dynamic table data

* Insert dynamic data
* Requires EC 6.16.0+

```json showLineNumbers
 {
 	"tableNameEn": "niubi_table",
 	"columns": {
 		"name": "我是牛逼",
 		"age": "niubi_table2",
 		"sex": "niubi_table2"
 	}
 }
```
* @param param Parameters
* Fields:
  * tableNameEn: English table name
  * columns: Column name → value map
  * Example: `"name": "我是牛逼的表"` inserts name = 我是牛逼的表
  * @return `{string}` JSON string; parse to a JSON object
  * Success example: ```{"result":{"data":1}}``` — data is the number of affected rows
  * Failure example: ```{"result":{"msg":"我是错误信息"}}```

```javascript showLineNumbers
function main() {
 let add = {
 "tableNameEn": "niubi_table",
 "columns": {
 "name": "我是牛逼的表",
 "age": "niubi_table2",
 "sex": "niubi_table2"
 }
 }

 let queryr = ecloud.dynamicAdd(add)
 logd("=== {}", queryr);

}

main();
```

### ecloud.dynamicUpdate Update dynamic table data

* Update dynamic data
* Requires EC 6.16.0+
* @param param Parameters

```json showLineNumbers
 {
 	"tableNameEn": "niubi_table",
 	"columns": {
 		"name": "我x是牛逼xxxx的表",
 		"age": "niubi_table2",
 		"sex": "niubi_table2"
 	},
 	"query": "and id=1",
 	"search": {
 		"id": "7"
 	}
 }
```
* query: Custom SQL-style WHERE clause
* tableNameEn: English table name
* search: Equality filters; use either search or query, not both
* search example: `"id":"1"` means id = 1
* columns: Columns and values to update
* Example: `"name": "我是牛逼的表"` sets name to 我是牛逼的表
* @return `{string}` JSON string; parse to a JSON object
* Success example: ```{"result":{"data":1}}``` — data is the number of affected rows
* Failure example: ```{"result":{"msg":"我是错误信息"}}```

```javascript showLineNumbers
function main() {
 let update = {
 "tableNameEn": "niubi_table",
 "columns": {
 "name": "1我x是牛逼xxxx的表",
 "age": "niubi_table2",
 "sex": "niubi_table2"
 },
 "query": "and name like '%我%'",
 "search": {
 "id": "7"
 }
 }

 let updater = ecloud.dynamicUpdate(update)
 logd("=== {}", updater);

}

main();
```

### ecloud.dynamicRemove Delete dynamic table data

* Delete dynamic data
* Requires EC 6.16.0+
```json showLineNumbers
 {
 	"tableNameEn": "niubi_table",
 	"query": "and name like '%我%'",
 	"search": {
 		"id": "1"
 	}
 }
```
* @param param Parameters
* Fields:
* query: Custom SQL-style WHERE clause
* tableNameEn: English table name
* search: Equality filters; use either search or query, not both
* search example: `"id":"1"` means id = 1
* @return `{string}` JSON string; parse to a JSON object
* Success example: ```{"result":{"data":1}}``` — data is the number of affected rows
* Failure example: ```{"result":{"msg":"我是错误信息"}}```

```javascript showLineNumbers
function main() {
 let del = {
 "tableNameEn": "niubi_table",
 "query": "and name like '%我%'",
 "search": {
 "id": "7"
 }
 }
 let delr = ecloud.dynamicRemove(del)
 logd("=== {}", delr);
}

main();
```

## Data cache API

- Operates Redis cache, typically as key-value pairs
- Supports TTL, strings, and set collections
- For boolean or integer values, convert to strings

### ecloud.addCache Add cache

* Add a Redis cache entry
* Requires EC 7.5.0+
* @param map Extensible parameter map
* Example ```{"cacheKey":"缓存key","expireTime":300,"dataType":1,"content":"数据"}```
* Key definitions:
* cacheKey = cache key
* expireTime = TTL in seconds; empty or ≤ 0 means no expiry
* dataType = data type: 1 string, 2 set (newline `\n` separated)
* content = payload; if dataType = 2, content is split by `\n` and stored as a Redis set
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 // String
 let stringData = {
 "cacheKey": "key1",
 "expireTime": 300,
 "dataType": 1,
 "content": "数据"
 }
 let addString = ecloud.addCache(stringData);
 logd("addString {}", addString);

 // SET collection
 let sss = "s1\ns2\ns3";
 let setData = {
 "cacheKey": "key1",
 "expireTime": 300,
 "dataType": 2,
 "content": sss
 }
 let addSet = ecloud.addCache(setData);
 logd("addSet {}", addSet);
}

main();
```

### ecloud.getCache Get cache

* Get a cache entry; returns nothing if expired
* Requires EC 7.5.0+
* @param map Extensible parameter map
* Example ```{"cacheKey":"缓存key"}```
* Key definitions:
* cacheKey = cache key
* @return `{string}` JSON string from the server; parse the result node

```javascript showLineNumbers
function main() {
 // String
 let stringData = {
 "cacheKey": "key1",
 "expireTime": 300,
 "dataType": 1,
 "content": "数据"
 }
 let addString = ecloud.addCache(stringData);
 logd("addString {}", addString);

 // SET collection
 let sss = "s1\ns2\ns3";
 let setData = {
 "cacheKey": "key1",
 "expireTime": -1,
 "dataType": 2,
 "content": sss
 }
 let addSet = ecloud.addCache(setData);
 logd("addSet {}", addSet);


 let data = ecloud.getCache({"cacheKey": "key1"});
 logd("data {}", JSON.stringify(data));
 // Print directly
 logd("data {}", data.result);
 // Split and iterate
 data.result.split("\n").forEach(function (item) {
 console.log(item)
 })

}

main();
```

### ecloud.updateCache Update cache

* Update a Redis cache entry
* Requires EC 7.5.0+
* @param map Extensible parameter map
* Example ```{"cacheKey":"缓存key","expireTime":300,"dataType":1,"content":"数据"}```
* Key definitions:
* cacheKey = cache key
* expireTime = TTL in seconds; empty or ≤ 0 means no expiry
* dataType = data type: 1 string, 2 set (newline `\n` separated)
* content = payload; if dataType = 2, content is split by `\n` and stored as a Redis set
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 // String
 let stringData = {
 "cacheKey": "key1",
 "expireTime": 300,
 "dataType": 1,
 "content": "数据"
 }
 let addString = ecloud.addCache(stringData);
 logd("addString {}", addString);
 let data = ecloud.getCache({"cacheKey": "key1"});
 logd("data {}", JSON.stringify(data));
 // Print directly
 logd("data {}", data.result);

 let up = ecloud.updateCache({"cacheKey": "key1", "content": "test123"})

 data = ecloud.getCache({"cacheKey": "key1"});
 logd("Updated data {}", JSON.stringify(data));
 // Print directly
 logd("Updated data {}", data.result);

}

main();
```

### ecloud.updateCacheExpire Update expiry

* Update Redis cache TTL
* Requires EC 7.5.0+
* @param map Extensible parameter map
* Example ```{"cacheKey":"缓存key","expireTime":300}```
* Key definitions:
* cacheKey = cache key
* expireTime = TTL in seconds; empty or ≤ 0 means no expiry
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 // String
 let stringData = {
 "cacheKey": "key1",
 "expireTime": 300,
 "dataType": 1,
 "content": "数据"
 }
 let addString = ecloud.addCache(stringData);
 logd("addString {}", addString);
 let data = ecloud.getCache({"cacheKey": "key1"});
 logd("data {}", JSON.stringify(data));
 // Print directly
 logd("data {}", data.result);
 let up = ecloud.updateCacheExpire({"cacheKey": "key1", "expireTime": 0})
 logd("up {}", up);
 let leftTime = ecloud.getCacheExpire({"cacheKey": "key1"})
 logd("Remaining TTL {}", leftTime);
}

main();
```

### ecloud.getCacheExpire Get remaining TTL

* Get remaining TTL for a Redis key
* Requires EC 7.5.0+
* @param map Extensible parameter map
* Example ```{"cacheKey":"缓存key"}```
* Key definitions:
* cacheKey = cache key
* @return `{long}` Remaining TTL; negative means no expiry

```javascript showLineNumbers
function main() {
 // String
 let stringData = {
 "cacheKey": "key1",
 "expireTime": 300,
 "dataType": 1,
 "content": "数据"
 }
 let addString = ecloud.addCache(stringData);
 logd("addString {}", addString);
 let data = ecloud.getCache({"cacheKey": "key1"});
 logd("data {}", JSON.stringify(data));
 // Print directly
 logd("data {}", data.result);
 let up = ecloud.updateCacheExpire({"cacheKey": "key1", "expireTime": 0})
 logd("up {}", up);
 let leftTime = ecloud.getCacheExpire({"cacheKey": "key1"})
 logd("Remaining TTL {}", leftTime);
}

main();


```

### ecloud.removeCache Delete cache

* Delete a Redis cache entry
* Requires EC 7.5.0+
* @param map Extensible parameter map
* Example ```{"cacheKey":"缓存key"}```
* Key definitions:
* cacheKey = cache key
* @return `{bool}` true on success, false on failure

 ```javascript showLineNumbers
function main() {
 // String
 let stringData = {
 "cacheKey": "key1",
 "expireTime": 300,
 "dataType": 1,
 "content": "数据"
 }
 let addString = ecloud.addCache(stringData);
 logd("addString {}", addString);

 let data = ecloud.getCache({"cacheKey": "key1"});
 logd("data {}", JSON.stringify(data));
 // Print directly
 logd("data {}", data.result);


 let del = ecloud.removeCache({"cacheKey": "key1"})

 logd("Delete {}", del);

 data = ecloud.getCache({"cacheKey": "key1"});
 logd("After delete data {}", JSON.stringify(data));
 // Print directly
 logd("After delete data {}", data.result);
}

main();


```

### ecloud.appendOneLineCache Append to cache

* Append one line to a Redis set cache
* Requires EC 7.5.0+
* @param map Extensible parameter map
* Example ```{"cacheKey":"缓存key","content":"数据"}```
* Key definitions:
* cacheKey = cache key
* content = one line to append to the Redis set
* @return `{string}` JSON string from the server; parse the result node

```javascript showLineNumbers
function main() {
 // String
 let stringData = {
 "cacheKey": "key1",
 "expireTime": 300,
 "dataType": 2,
 "content": "a\nb\nc"
 }
 let addString = ecloud.addCache(stringData);
 logd("addString {}", addString);

 let data = ecloud.getCache({"cacheKey": "key1"});
 logd("data {}", JSON.stringify(data));
 // Print directly
 logd("data {}", data.result);


 let inc = ecloud.appendOneLineCache({"cacheKey": "key1", "content": "test"})
 logd("==inc {}", inc);
 inc.result.split("\n").forEach(function (item) {
 console.log("append --- {}", item)
 })

}

main();
```

### ecloud.removeOneLineCache Remove one cache line

* Remove one element from a Redis set cache
* Requires EC 7.5.0+
* @param map Extensible parameter map
* Example ```{"cacheKey":"缓存key","content":"数据"}```
* Key definitions:
* cacheKey = cache key
* content = one line to remove from the Redis set
* @return `{string}` JSON string from the server; parse the result node

```javascript showLineNumbers
function main() {
 // String
 let stringData = {
 "cacheKey": "key1",
 "expireTime": 300,
 "dataType": 2,
 "content": "a\nb\nc"
 }
 let addString = ecloud.addCache(stringData);
 logd("addString {}", addString);

 let data = ecloud.getCache({"cacheKey": "key1"});
 logd("data {}", JSON.stringify(data));
 // Print directly
 logd("data {}", data.result);


 let inc = ecloud.appendOneLineCache({"cacheKey": "key1", "content": "test"})
 logd("==inc {}", inc);
 inc.result.split("\n").forEach(function (item) {
 console.log("append --- {}", item)
 })
 inc = ecloud.removeOneLineCache({"cacheKey": "key1", "content": "test"})
 logd("==inc {}", inc);
 inc.result.split("\n").forEach(function (item) {
 console.log("append --- {}", item)
 })
}

main();
```

### ecloud.incrementCache Increment cache

* Redis increment counter; each call adds 1 to the key
* Requires EC 7.5.0+
* @param map Extensible parameter map
* Example ```{"cacheKey":"缓存key"}```
* Key definitions:
* cacheKey = cache key
* @return `{long}` Value after increment

```javascript showLineNumbers
function main() {
 // String
 let stringData = {
 "cacheKey": "key1",
 "expireTime": 300,
 "dataType": 2,
 "content": "a\nb\nc"
 }
 let addString = ecloud.addCache(stringData);
 logd("addString {}", addString);

 let data = ecloud.getCache({"cacheKey": "key1"});
 logd("data {}", JSON.stringify(data));
 // Print directly
 logd("data {}", data.result);


 let inc = ecloud.incrementCache({"cacheKey": "key2"})
 logd("==inc {}", inc);
 let get = ecloud.getCache({"cacheKey": "key2"})
 // Returned as a string
 logd("==get {}", get.result);

}

main();
```

### ecloud.decrementCache Decrement cache

* Redis decrement counter; each call subtracts 1 from the key
* Requires EC 7.5.0+
* @param map Extensible parameter map
* Example ```{"cacheKey":"缓存key"}```
* Key definitions:
* cacheKey = cache key
* @return `{long}` Value after decrement

```javascript showLineNumbers
function main() {
 // String
 let stringData = {
 "cacheKey": "key1",
 "expireTime": 300,
 "dataType": 2,
 "content": "a\nb\nc"
 }
 let addString = ecloud.addCache(stringData);
 logd("addString {}", addString);
 let data = ecloud.getCache({"cacheKey": "key1"});
 logd("data {}", JSON.stringify(data));
 // Print directly
 logd("data {}", data.result);
 let inc = ecloud.incrementCache({"cacheKey": "key2"})
 logd("==inc {}", inc);
 let get = ecloud.decrementCache({"cacheKey": "key2"})
 // Returned as a string
 logd("==get {}", get);
}

main();


```

### ecloud.popPushCache push or pop command

* Redis push or pop commands
* Requires EC 7.8.0+
* @param map Extensible parameter map
* Example ```{"cacheKey":"缓存key"}```
* Key definitions:
* cmd = supports spop, rpush, lpush, lpop, rpop
* cacheKey = cache key
* content = value to push or pop
* @return `{string}` JSON string from the server; parse the result node

```javascript showLineNumbers
function main() {
 // String
 let stringData = {
 "cacheKey": "key1",
 "cmd": "lpush",
 "content": "aaa"
 }
 let popPushCachex = ecloud.popPushCache(stringData);
 logd("popPushCachex {}", popPushCachex);
 let data = ecloud.getCache({"cacheKey": "key1"});
 logd("data {}", JSON.stringify(data));

}

main();
```


