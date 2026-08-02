---
title: Multi-Worker Functions
description: EasyClick automation scripts — iOS no jailbreak multi-worker functions
keywords:
 - EasyClick automation scripts iOS no jailbreak multi-worker functions
 - worker
 - workerThread
 - jsvm
 - Worker
 - getCurrentWorkerName
 - js
 - newWorker
 - iOS
 - isRunning
 - hasWorkerFunction
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

:::tip

- The iOS JS engine is single-threaded and cannot do true multithreading.
- We use multiple JSVM instances as workers to run different tasks asynchronously.
- Multiple JSVMs means multiple JS virtual machines running in the iOS process with isolated data between them.
- Each JSVM reloads the full script until main.js runs, so use `getCurrentWorkerName()` to route to different business logic.
- Workers can exchange data via files — one writes, another reads.
- The main script JSVM is named `mainWorker`. Name other workers freely; avoid Chinese and special characters.
 :::

## Multi-Worker Module

### getCurrentWorkerName Get Current Worker Name

* Get the current worker name
* @return `{string}` worker name string

```javascript showLineNumbers
function main() {
    var m = getCurrentWorkerName();
    logd(m);
}

main();
```

### workerThread.newWorker Create a New Worker

* Create a worker
* @param name Worker name
* @return `{string}` worker name; non-empty value means success

```javascript showLineNumbers
function startNewWorker(workName) {
 let name = workerThread.newWorker(workName);
 logd("startNewWorker " + name)
 sleep(1000)
 if (!workerThread.isRunning(workName)) {
 logd(workName + " not running");
 return false
 }
 return true;
}

function testworker() {
 logd("current worker name " + getCurrentWorkerName())
 // Main script thread
 if (getCurrentWorkerName() == "mainWorker") {
 logd("Main script thread")
 // Start worker1
 startNewWorker("worker1");
 startNewWorker("worker2");
 startNewWorker("worker3");
 // Already running
 for (let i = 0; i < 10; i++) {
 if (isScriptExit()) {
 logd("mainworker exited")
 return
 }
 sleep(1000)
 logd(getCurrentWorkerName() + " --> " + new Date())
 // Call worker1 function every 2 seconds
 if (i % 2 == 0) {
 let dsx = JSON.stringify({"date": new Date()});
 if (workerThread.hasWorkerFunction("getWorker1Data")) {
 let result = workerThread.callWorkerFunction("getWorker1Data", dsx)
 logd("worker1#getWorker1Data return --->:::: " + result)
 let removeWorker1 = workerThread.removeWorker("worker1")
 logd("removeWorker1 " + removeWorker1)
 logd("isCancelled worker1 " + workerThread.isCancelled("worker1"));
 workerThread.removeWorkerFunction("getWorker1Data")
 } else {
 logw("No getWorker1Data function, skipping call")
 }
 }
 // Call worker2 function every 4 seconds
 if (i % 4 == 0) {
 let dsx = JSON.stringify({"date": new Date()});
 let result = workerThread.callWorkerFunction("getWorker2Data", dsx)
 logd("worker2#getWorker2Data return --->:::: " + result)
 }
 // Call worker3 function every 6 seconds
 if (i % 6 == 0) {
 let dsx = JSON.stringify({"date": new Date()});
 let result = workerThread.callWorkerFunction("getWorker3Data", dsx)
 logd("worker3#getWorker3Data return --->:::: " + result)
 workerThread.stopAll()
 }
 }
 } else {
 logd("Non-main script thread ------ ")
 if (getCurrentWorkerName() == "worker1") {
 workerThread.addWorkerFunction("getWorker1Data", function (data) {
 logd("---->worker1 received parameter " + data)
 return "worker1#getWorker1Data return data " + new Date()
 })
 while (true) {
 if (isScriptExit()) {
 logd("worker1 exited")
 return
 }
 sleep(1000)
 }
 }
 if (getCurrentWorkerName() == "worker2") {
 workerThread.addWorkerFunction("getWorker2Data", function (data) {
 logd("---->worker2 received parameter " + data)
 logd("getWorker2Data getCurrentWorkerName " + getCurrentWorkerName())
 return "worker2#getWorker2Data return data " + new Date()
 })
 while (true) {
 if (isScriptExit()) {
 logd("worker2 exited")
 return
 }
 sleep(1000)
 }
 }
 if (getCurrentWorkerName() == "worker3") {
 workerThread.addWorkerFunction("getWorker3Data", function (data) {
 logd("---->worker3 received parameter " + data)
 return "worker3#getWorker3Data return data " + new Date()
 })
 while (true) {
 if (isScriptExit()) {
 logd("worker3 exited")
 return
 }
 sleep(1000)
 }
 }
 }
}

testworker()
```

### workerThread.isRunning Check if Worker Is Running

* Check whether this worker is running
* @param name Worker name
* @returns `{boolean}` true if running, false if not

```javascript showLineNumbers
// See newWorker example
```

### workerThread.hasWorkerFunction Check for Worker Function

* Check whether a worker function exists
* @param funcName Function name
* @returns `{boolean}` true if exists, false if not

```javascript showLineNumbers
// See newWorker example
```

### workerThread.addWorkerFunction Add Worker Function

* Register a function callable by other workers
* @param funcName Function name
* @param callback Implementation; parameters and return values should be strings
* @returns `{boolean}` true on success, false on failure

```javascript showLineNumbers
// See newWorker example
```

### workerThread.removeWorkerFunction Remove Worker Function

* Remove a registered worker function
* @param funcName Function name
* @returns `{boolean}` true on success, false on failure

```javascript showLineNumbers
// See newWorker example
```

### workerThread.callWorkerFunction Call Registered Worker Function

* Call a function registered by another worker
* @param funcName Function name
* @param data Parameter; string recommended
* @returns `{object}` return value from the function

```javascript showLineNumbers
// See newWorker example
```

### workerThread.removeWorker Remove Worker

* Remove a worker
* @param name Worker name
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
// See newWorker example
```

### workerThread.isCancelled Check if Worker Is Cancelled

* Check whether worker execution was cancelled
* @param name Worker name
* @return `{boolean}` true if cancelled, false if not

```javascript showLineNumbers
// See newWorker example
```

### workerThread.stopAll Stop All Workers

* Stop all running workers; cannot stop the main script thread
* @return `{boolean}` true on success, false on failure

```javascript showLineNumbers
// See newWorker example
```
