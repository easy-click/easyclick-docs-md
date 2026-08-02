---

title: Thread Functions
description: EasyClick automation scripts — HarmonyOS Next automation Thread Functions resource download
keywords:
 - EasyClick automation scripts — HarmonyOS Next automation Thread Functions resource download
 - param
 - thread
 - execSync
 - setTimeout
 - cancelTimeout
 - setInterval
 - cancelInterval
 - func
 - 'true'
 - stopAll
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview

- Thread module functions manage thread-related operations
- The thread module uses the `thread` prefix, e.g. `thread.execFuncSync()`

## setTimeout Delayed Execution

* Run a function after a delay, in a child thread
* @param func Function to execute
* @param timeout Delay in milliseconds
* Returns a thread object that can be cancelled

```javascript showLineNumbers
function main() {
    var t = setTimeout(function () {
        toast("This runs after one second");
    }, 1000);
    // Simulate script running
    while (true) {
        sleep(1000)
    }
}

main();
```

## cancelTimeout Cancel Delayed Execution

* Cancel a delayed execution
* @param t Function/thread to cancel

```javascript showLineNumbers
function main() {
    var t = setTimeout(function () {
        toast("This runs after one second");
    }, 1000);
    // After cancel, it will not run
    cancelTimeout(t);
}

main();
```

## setInterval Periodic Execution

* Run a function on an interval, in a child thread
* @param func Function
* @param interval Interval in milliseconds
* @return Thread object that can be cancelled

```javascript showLineNumbers
function main() {
    var t = setInterval(function () {
        toast("This runs every second");
    }, 1000);
    // Simulate script running
    while (true) {
        sleep(1000)
    }
}

main();
```

## cancelInterval Cancel Periodic Execution

* Cancel a periodic execution
* @param t Function/thread to cancel

```javascript showLineNumbers
function main() {
    var t = setInterval(function () {
        toast("This runs every second");
    }, 1000);
    cancelInterval(t);
}

main();
```

## execSync Synchronous Execution

* Run a function and wait until it returns true; returns immediately when the callback returns true
* @param condition Condition function
* @param timeout Timeout in milliseconds
* @return boolean

```javascript showLineNumbers
function main() {
    execSync(function () {
        logd("This runs synchronously");
    }, 1000);
}

main();
```

## thread.stopAll Stop All

* Cancel all running threads

```javascript showLineNumbers
function main() {
    execSync(function () {
        logd("This runs synchronously");
    }, 1000);
    thread.stopAll();
}

main();
```

## thread.execAsync Async Execution

* Run asynchronously; the Runnable is managed by a thread pool
* @param runnable Runnable object

```javascript showLineNumbers
function main() {
    var tid = thread.execAsync(function () {
        while (true) {
            logd("This runs asynchronously");
            sleep(1000);
            if (thread.isCancelled(tid)) {
                break;
            }
        }
    });
    logd("tid " + tid);
    // Cancel thread after 5s
    sleep(5000);
    logd("Cancel thread " + tid);
    thread.cancelThread(tid);
    sleep(5000);
    logd("Done ");
}

main();
```

## thread.execSync Synchronous Execution

* Run a function and wait until it returns true; returns immediately when the callback returns true
* @param condition Condition function
* @param timeout Timeout in milliseconds
* @return boolean

```javascript showLineNumbers
function main() {
    thread.execSync(function () {
        logd("This runs synchronously");
    }, 1000);
}

main();
```

## thread.cancelThread Cancel Thread

* Cancel thread execution
* @param t Thread ID
* @return `{boolean}`

```javascript showLineNumbers
function main() {
    var tid = thread.execAsync(function () {
        while (true) {
            logd("This runs asynchronously");
            sleep(1000);
            if (thread.isCancelled(tid)) {
                break;
            }
        }
    });
    logd("tid " + tid);
    // Cancel thread after 5s
    sleep(5000);
    logd("Cancel thread " + tid);
    thread.cancelThread(tid);
    sleep(5000);
    logd("Done ");
}

main();
```

## thread.isCancelled Cancellation Check

* Check whether a thread was cancelled
* @param t Thread ID
* @return boolean true if cancelled, false if not

```javascript showLineNumbers
function main() {
    var tid = thread.execAsync(function () {
        while (true) {
            logd("This runs asynchronously");
            sleep(1000);
            if (thread.isCancelled(tid)) {
                break;
            }
        }
    });
    logd("tid " + tid);
    // Cancel thread after 5s
    sleep(5000);
    logd("Cancel thread " + tid);
    thread.cancelThread(tid);
    sleep(5000);
    logd("Done ");
}

main();
```
