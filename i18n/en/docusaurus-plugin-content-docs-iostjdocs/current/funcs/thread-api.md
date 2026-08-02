---
title: Thread Functions
description: EasyClick automation scripts — iOS no jailbreak — thread functions — resource download
keywords:
 - EasyClick automation scripts — iOS no jailbreak — thread functions — resource download
 - js
 - thread
 - ThreadClient
 - JS
 - thread1
 - execAsyncFile
 - iOS
 - jsvm
 - worker
 - src
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview
:::tip
 - The iOS JS engine is single-threaded by nature; true multithreading is not possible. Here, `thread` uses multiple JSVM virtual machines under the hood
 - Multiple JSVMs mean several separate JS virtual machines run in the iOS process with isolated data between them
 - This module is a simplified version of the worker module — it can run code snippets directly and is easier to use than worker
 - Available from EC standalone 5.0.0+
:::



## ThreadClient.execAsyncFile Run JS File in Thread
* Run a JS file in a thread; only JS files inside IEC packages are supported
* Create `thread1/sub.js` under the project `src` directory (`thread1` is a sibling of `js`); pass `thread1/sub.js` as `path`
* Child threads do not auto-load other JS files; use `require` for libraries, e.g.
* `let lib2 = require("work1/lib1.js")`
* `logd(lib2.add(1, 2))`
* The above references `lib1.js` under `work1` in the `src` folder
* @param path file path, e.g. `thread1/sub.js`
* @returns `{ThreadClient}`

```javascript showLineNumbers
function main() {
    let name = "thread2";
    let th = thread.newThread(name)
    // Optional callback
    th.addCallback("f1", function (name, data) {
        logd("callback " + data)
        return "ok"
    });
    th.execAsyncFile("thread1/sub.js");
    while (!isScriptExit()) {
        logd("main " + new Date())
        sleep(1000)
    }
}

main();
```



## ThreadClient.execAsyncStr Run JS String in Thread
* Run a JS code string asynchronously in a thread
* No line numbers because the code is dynamic
* @param funcStr code string
* @returns `{ThreadClient}`

```javascript showLineNumbers
function main() {
    let name = "thread2";
    let th = thread.newThread(name)
    th.execAsyncStr("logd('123');");
    while (!isScriptExit()) {
        logd("main " + new Date())
        sleep(1000)
    }
}

main();
```


## thread.execCodeAsync Run Code Block Asynchronously

* Run a code block asynchronously
* Pre-wrapped helper — use directly
* @param name thread name
* @param func code block
* @param callbackName callback function name
* @param callbackFunc callback function
* @return `{string}` thread name

```javascript showLineNumbers
function main() {
  // Child threads cannot access outer variables.
  // Best practice: write business logic in a JS file and call it from the thread.
  thread.execCodeAsync("thread1", function () {
    while (!isScriptExit()) {
      sleep(1000)
      logd("sub thread " + new Date())
      var url = "http://baidu.com";
      var pa = {"b": "22"};
      var x = http.httpGet(url, pa, 10 * 1000, {"User-Agent": "test"});
      logd(" result- " + x);
      // Call f1 callback
      let backdata = thread.invokeCallback("f1", "Baidu data:" + x)
      logd("backdata " + backdata)
    }
  }, "f1", function (name, data) {
    logd("callback " + data)
    return "ok->"
  })
  let timex = 0
  while (!isScriptExit()) {
    logd("main " + new Date())
    sleep(1000)
    timex = timex + 1000
    if (timex > 8000) {
      break
    }
  }
  // Cancel thread after 5 seconds
  thread.cancelThread("thread1")
  thread.stopAll();
  sleep(1000)
  logd("thread1 cancel " + thread.isCancelled("thread1"))
  // Or use cancelThread
  // thread.cancelThread("thread1")
}

main();
```






## thread.invokeCallback Invoke Callback Function

* Invoke a function registered with `addCallback`, looked up by `funcName`
* @param funcName callback function name
* @param data callback data
* @return `{*}` return value from the callback

```javascript showLineNumbers
function main() {
    // See thread.execCodeAsync example
}

main();
```


## thread.newThread Create New Thread

* Create a new thread wrapper
* @param name thread name; auto-generated if omitted
* @return `{ThreadClient|null}`

```javascript showLineNumbers
function main() {
  let name = "thread2";
  let th = thread.newThread(name)
  // Optional callback
  th.addCallback("f1", function (name, data) {
    logd("callback " + data)
    return "ok"
  });
  // Run asynchronously
  th.execAsync(function () {
    var url = "http://baidu.com";
    var pa = { "b": "22" };
    var x = http.httpGet(url, pa, 10 * 1000, { "User-Agent": "test" });
    
    let backdata = thread.invokeCallback("f1",x)
    logd("backdata "+backdata)
    
  });
  sleep(5000);
  logd("Finished");
}

main();
```
### addCallback Add Callback Function
- Add a callback on the thread object
- @param name function name
- @param func callback function
```javascript showLineNumbers
function main() {
  // See thread.newThread example
}

main();
```

## thread.cancelThread Cancel Thread

* Cancel thread execution
* @param t thread object ID
* @return boolean

```javascript showLineNumbers
function main() {
  // See thread.execCodeAsync example
}

main();
```
## thread.stopAll Stop All Threads

* Cancel all running threads

```javascript showLineNumbers
function main() {
    // See thread.execCodeAsync example
}

main();
```
## thread.isCancelled Check Cancellation Status

* Check whether a thread was cancelled
* @param t thread object ID
* @return boolean — `true` if cancelled, `false` if not

```javascript showLineNumbers
function main() {
  // See thread.execCodeAsync example
}
main();
```



## thread.putShareKeyValue Store Shared Data

* Store shared data
* Uses a key-value shared storage area
* @param key data key (string)
* @param value data value (string)

```javascript showLineNumbers
// js/sub_thread_func.js
function subThreadFunc(){
  logd("subThreadFunc running...")
  thread.putShareKeyValue("key","vb1")
  thread.putShareValue("subThreadFunc message")
}
```


```javascript showLineNumbers
// js/main.js
function threadTest() {

  // Child threads cannot access outer variables.
  // Best practice: write business logic in a JS file and call it from the thread.
  let name = "thread2";
  let th = thread.newThread(name)
  // Run asynchronously
  th.execAsync(function () {
    var testData = readIECFileAsString("js/sub_thread_func.js");
    // subThreadFunc is defined in sub_thread_func.js; append the call
    testData = (testData+" ;subThreadFunc();")
    // Start execution
    let dx = execScript(2, testData);
  });

  while (!isScriptExit()) {
    sleep(100)
    // Read from KV shared storage
    let kvValue = thread.getShareKeyValue("key")
    if (kvValue != "") {
      logd("Get: key " + kvValue)
    }
    // Read from array shared storage
    let arValue = thread.getShareValue()
    if (arValue != "") {
      logd("After receiving data, run main-thread task... arValue = " + arValue)
      sleep(1000)
      logd("Task finished, waiting for next command")
    }
  }

}

threadTest();
```


## thread.getShareKeyValue Get Data from Shared Storage

* Get data from shared storage
* After read, the key's value is automatically removed
* Available in EC standalone 5.20.0+
* @param key key matching `putShareKeyValue`
* @return `{string}` string

```javascript showLineNumbers
    // See thread.putShareKeyValue example
```


## thread.putShareValue Store Data in Shared Area

* Store data in the shared area
* Implemented as an array under the hood
* Available in EC standalone 5.20.0+
* @param value value to store

```javascript showLineNumbers
    // See thread.putShareKeyValue example
```



## thread.getShareValue Get Value from Shared Area

* Get a value from the shared area
* Array-based FIFO — reads index 0 and removes it
* Available in EC standalone 5.20.0+
* @return `{string}` string

```javascript showLineNumbers
    // See thread.putShareKeyValue example
```
