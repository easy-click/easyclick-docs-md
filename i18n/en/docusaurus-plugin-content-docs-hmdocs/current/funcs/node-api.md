---
title: Node Functions
description: EasyClick automation scripts — HarmonyOS Next node functions
keywords:
 - EasyClick automation scripts HarmonyOS Next node functions
 - xpath
 - id
 - key
 - type
 - getNodeInfo
 - getOneNodeInfo
 - idMatch
 - hint
 - timeout
 - return
 - EasyClick
 - mobile automation
 - automation testing
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---
## Overview

- Node module functions are used for node operations
- This module has no prefix; call functions directly
- After fetching nodes, lock them before searching to avoid issues with parent/child functions

## getNodeInfo Get Node Collection

* timeout Timeout in milliseconds
* @return `{array}` Array of node objects

```javascript showLineNumbers
function main() {
    let nodes = text("aaa").getNodeInfo(0)
    if (nodes){
        logd(JSON.stringify(nodes))
        for (let i = 0; i < nodes.length; i++) {
            logd(JSON.stringify(nodes[i]))
            logd(nodes[i].text)
            logd(nodes[i].bounds.top)
            logd(nodes[i].bounds.centerX())
        }
    }
}

main();
```

## getOneNodeInfo Get Single Node

* timeout Timeout in milliseconds
* @return `{NodeInfo}` Node object

```javascript showLineNumbers
function main() {
    let node = text("aaa").getOneNodeInfo(0);
    if (node){
        logd(JSON.stringify(node));
        logd(node.text)
        logd(node.bounds.top)
        logd(node.bounds.centerX())
    }
}

main();
```


## Selector Functions

### xpath Selection

* XPath selector; see https://www.jianshu.com/p/c205334122b3 and https://www.runoob.com/xpath/xpath-syntax.html
* Use the IDE node panel to inspect xpath attributes and test xpath
* @param value e.g. //node[@value='易点云测'] selects nodes whose value equals 易点云测 (EasyClick Cloud Test)

```javascript showLineNumbers
function main() {
    // Get selector object
    var selector = xpath("//node[@value='易点云测']");
    let n = selector.getNodeInfo(1000);
    logd(JSON.stringify(n))
}

main();
```

### id id Exact Attribute Match

* Exact match on id attribute

```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    // Search by id
    let nd = id("Settings").getOneNodeInfo(1000)
    if (nd) {
        console.log("id search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log("id search: node not found ")
    }
    // Search by regex
    nd = idMatch(".*Settings.*").getNodeInfo(1000)
    if (nd) {
        console.log("idMatch search node info {} ", JSON.stringify(nd))
    } else {
        console.log("idMatch search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}

function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main()
```


### idMatch id Regex Attribute Match

* Regex match on id attribute

```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    // Search by id
    let nd = id("Settings").getOneNodeInfo(1000)
    if (nd) {
        console.log("id search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log("id search: node not found ")
    }
    // Search by regex
    nd = idMatch(".*Settings.*").getNodeInfo(1000)
    if (nd) {
        console.log("idMatch search node info {} ", JSON.stringify(nd))
    } else {
        console.log("idMatch search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}

function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main()
```

### key key Exact Attribute Match
* Exact match on key attribute
```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    // key
    let nd = key("Settings").getOneNodeInfo(1000)
    if (nd) {
        console.log("key search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log("key search: node not found ")
    }
    // Search by regex
    nd = keyMatch(".*Settings.*").getNodeInfo(1000)
    if (nd) {
        console.log("keyMatch search node info {} ", JSON.stringify(nd))
    } else {
        console.log("keyMatch search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}

function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main()
```

### keyMatch key

* Regex match on key attribute

```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    let nd = keyMatch("Settings").getOneNodeInfo(1000)
    if (nd) {
        console.log("key search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log("keyMatch search: node not found ")
    }
    // Search by regex
    nd = keyMatch(".*Settings.*").getNodeInfo(1000)
    if (nd) {
        console.log("keyMatch search node info {} ", JSON.stringify(nd))
    } else {
        console.log("keyMatch search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}

function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main()
```


### type type Exact Attribute Match

* Exact match on type attribute

```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    // Search by type
    let nd = type("Item").getOneNodeInfo(1000)
    if (nd) {
        console.log("type search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log("label search: node not found ")
    }
    // Search by type regex
    nd = typeMatch(".*Item.*").getNodeInfo(1000)
    if (nd) {
        console.log("typeMatch search node info {} ", JSON.stringify(nd))
    } else {
        console.log("typeMatch search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}

function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main()
```

### typeMatch type Regex Attribute Match

* Regex match on type attribute

```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    // Search by type
    let nd = type("Item").getOneNodeInfo(1000)
    if (nd) {
        console.log("type search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log("label search: node not found ")
    }
    // Search by type regex
    nd = typeMatch(".*Item.*").getNodeInfo(1000)
    if (nd) {
        console.log("typeMatch search node info {} ", JSON.stringify(nd))
    } else {
        console.log("ltypeMatch search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}

function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main()
```

### hint hint Exact Attribute Match
* Exact match on hint attribute
```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    let nd = hint("123").getOneNodeInfo(1000)
    if (nd) {
        console.log("value search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log("value search: node not found ")
    }
    nd = textMatch(".*123.*").getNodeInfo(1000)
    if (nd) {
        console.log("textMatch search node info {} ", JSON.stringify(nd))
    } else {
        console.log("textMatch search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}

function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main()
```

### hintMatch hint Regex Attribute Match
* Regex match on hint attribute
```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    let nd = hintMatch("123").getOneNodeInfo(1000)
    if (nd) {
        console.log(" search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log(" search: node not found ")
    }
    nd = textMatch(".*123.*").getNodeInfo(1000)
    if (nd) {
        console.log("textMatch search node info {} ", JSON.stringify(nd))
    } else {
        console.log("textMatch search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}
function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main()
```


### text text Exact Attribute Match
* Exact match on text attribute
```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    let nd = text("123").getOneNodeInfo(1000)
    if (nd) {
        console.log("value search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log("value search: node not found ")
    }
    nd = textMatch(".*123.*").getNodeInfo(1000)
    if (nd) {
        console.log("textMatch search node info {} ", JSON.stringify(nd))
    } else {
        console.log("textMatch search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}

function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main()
```

### textMatch text Regex Attribute Match
* Regex match on text attribute
```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    let nd = text("123").getOneNodeInfo(1000)
    if (nd) {
        console.log(" search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log(" search: node not found ")
    }
    nd = textMatch(".*123.*").getNodeInfo(1000)
    if (nd) {
        console.log("textMatch search node info {} ", JSON.stringify(nd))
    } else {
        console.log("textMatch search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}
function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main()
```



### hostWindowId hostWindowId Exact Attribute Match
* Exact match on hostWindowId attribute
```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    let nd = hostWindowId("123").getOneNodeInfo(1000)
    if (nd) {
        console.log("value search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log("value search: node not found ")
    }
    nd = hostWindowIdMatch(".*123.*").getNodeInfo(1000)
    if (nd) {
        console.log("hostWindowIdMatch search node info {} ", JSON.stringify(nd))
    } else {
        console.log("hostWindowIdMatch search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}

function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main()
```

### hostWindowIdMatch hostWindowId
* Regex match on hostWindowId attribute
```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    let nd = hostWindowId("123").getOneNodeInfo(1000)
    if (nd) {
        console.log(" search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log(" search: node not found ")
    }
    nd = hostWindowIdMatch(".*123.*").getNodeInfo(1000)
    if (nd) {
        console.log("textMatch search node info {} ", JSON.stringify(nd))
    } else {
        console.log("textMatch search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}
function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main()
```




### description description Exact Attribute Match
* Exact match on description attribute
```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    let nd = description("123").getOneNodeInfo(1000)
    if (nd) {
        console.log("value search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log("value search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}

function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main()
```

### descriptionMatch description
* Regex match on description attribute
```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    let nd = description("123").getOneNodeInfo(1000)
    if (nd) {
        console.log(" search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log(" search: node not found ")
    }
    nd = descriptionMatch(".*123.*").getNodeInfo(1000)
    if (nd) {
        console.log("descriptionMatch search node info {} ", JSON.stringify(nd))
    } else {
        console.log("descriptionMatch search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}
function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main()
```





### bundleName bundleName Exact Attribute Match
* Exact match on bundleName attribute
```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    let nd = bundleName("123").getOneNodeInfo(1000)
    if (nd) {
        console.log("value search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log("value search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}

function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main()
```

### bundleNameMatch bundleName
* Regex match on bundleName attribute
```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    let nd = bundleName("123").getOneNodeInfo(1000)
    if (nd) {
        console.log(" search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log(" search: node not found ")
    }
    nd = bundleNameMatch(".*123.*").getNodeInfo(1000)
    if (nd) {
        console.log("bundleNameMatch search node info {} ", JSON.stringify(nd))
    } else {
        console.log("bundleNameMatch search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}
function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main()
```



### pagePath pagePath Exact Attribute Match
* Exact match on pagePath attribute
```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    let nd = pagePath("123").getOneNodeInfo(1000)
    if (nd) {
        console.log("value search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log("value search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}

function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main()
```

### pagePathMatch pagePath
* Regex match on pagePath attribute
```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    let nd = pagePath("123").getOneNodeInfo(1000)
    if (nd) {
        console.log(" search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log(" search: node not found ")
    }
    nd = pagePathMatch(".*123.*").getNodeInfo(1000)
    if (nd) {
        console.log("pagePathMatch search node info {} ", JSON.stringify(nd))
    } else {
        console.log("pagePathMatch search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}
function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main()
```




### longClickable longClickable Exact Attribute Match
* Exact match on longClickable attribute
```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    // Search by longClickable
    let nd = longClickable(true).getOneNodeInfo(1000)
    if (nd) {
        console.log(" search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log(" search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}

function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main()
```


### enable enable Exact Attribute Match
* Exact match on enable attribute
```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    // Search by enable
    let nd = enable(true).getOneNodeInfo(1000)
    if (nd) {
        console.log(" search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log(" search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}

function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main()
```


### visible visible Exact Attribute Match

* Exact match on visible attribute

```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    // Search by visible
    let nd = visible(true).getOneNodeInfo(1000)
    if (nd) {
        console.log(" search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log(" search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}

function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main()
```


### zIndex zIndex Exact Attribute Match

* Exact match on zIndex attribute

```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    // Search by zIndex
    let nd = zIndex(1).getOneNodeInfo(1000)
    if (nd) {
        console.log(" search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log(" search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}

function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main()
```

### index index Exact Attribute Match

* Exact match on index attribute

```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    // Search by index
    let nd = index(1).getOneNodeInfo(1000)
    if (nd) {
        console.log(" search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log(" search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}

function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main()
```

### depth depth Exact Attribute Match

* Exact match on depth attribute

```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    // Search by depth
    let nd = depth(1).getOneNodeInfo(1000)
    if (nd) {
        console.log(" search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log(" search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}

function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main()
```



### focused focused Exact Attribute Match
* Exact match on focused attribute
```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    let nd = focused(true).getOneNodeInfo(1000)
    if (nd) {
        console.log(" search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log(" search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}

function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main()
```


### enabled enabled Exact Attribute Match
* Exact match on enabled attribute
```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    let nd = enabled(true).getOneNodeInfo(1000)
    if (nd) {
        console.log(" search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log(" search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}

function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main()
```

### selected selected Exact Attribute Match
* Exact match on selected attribute
```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    // Search by selected
    let nd = selected(true).getOneNodeInfo(1000)
    if (nd) {
        console.log(" search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log(" search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}
function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}
main()
```


### checkable checkable Exact Attribute Match
* Exact match on checkable attribute
```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    // Search by checkable
    let nd = checkable(true).getOneNodeInfo(1000)
    if (nd) {
        console.log(" search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log(" search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}
function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}
main()
```


### checked checked Exact Attribute Match
* Exact match on checked attribute
```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    // Search by checked
    let nd = checked(true).getOneNodeInfo(1000)
    if (nd) {
        console.log(" search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log(" search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}
function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}
main()
```

### childcount childcount Exact Attribute Match

* Exact match on childcount attribute

```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    // Search by childCount
    let nd = childCount(2).getOneNodeInfo(1000)
    if (nd) {
        console.log(" search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log(" search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}

function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main()
```

### bounds bounds Bounds Range Match

* Bounds range match on bounds attribute

```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    // Search by bounds
    let nd = bounds(100, 100, 300, 300).getOneNodeInfo(1000)
    if (nd) {
        console.log(" search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log(" search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}

function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main()
```

## Click Node

### clickCenter Click Node Center

* Click the center of the node
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    let node = text("Maps").getOneNodeInfo(10000)
    logd(JSON.stringify(node))
    if (node) {
        logd(node.clickCenter())
    }

}

main()
```

### clickRandom Click Node Randomly

* Click a random point within the node bounds
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    let node = text("Maps").getOneNodeInfo(10000)
    logd(JSON.stringify(node))
    if (node) {
        logd(node.clickRandom())
    }

}

main()
```

## Multi-Attribute Cascaded Query


```javascript showLineNumbers
function main() {
    // Write your code here
    logd("Checking automation environment...")
    // If automation service is OK
    if (!autoServiceStart(3)) {
        logd("automation service failed to start; cannot run script")
        exit();
        return;
    }
    logd("Starting script...")
    // Release previous lock first
    releaseNode();
    // Lock new node data
    lockNode();
    // Begin node search
    // Cascaded attribute search
    let nd = textMatch(".*1.*").enabled(true).bounds(100, 100, 300, 300).getOneNodeInfo(1000)
    if (nd) {
        console.log(" search node info {} ", JSON.stringify(nd))
        // Click if found
        let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
        logd("click it: {}", c)
    } else {
        console.log(" search: node not found ")
    }
    // Release previous lock first
    releaseNode();
}

function autoServiceStart(time) {
    for (let i = 0; i < time; i++) {
        if (isServiceOk()) {
            return true;
        }
        let started = startEnv();
        logd("Service start attempt " + (i + 1) + " result: " + started);
        if (isServiceOk()) {
            return true;
        }
    }
    return isServiceOk();
}

main()
```

## Node Cascading Operations


### Cascaded Get Single Node getOneNodeInfo

* Get the first node via selector
* @param timeout Wait time in ms; 0=no wait
* @return NodeInfo object or null

```javascript showLineNumbers
function main() {
    // Get selector object
    var node =type("Text").getOneNodeInfo(10000);
    if (node) {
        // Get one child node
        node = node.getOneNodeInfo(text("Ad"), 10000);
        if (!node) {
            toast("no child nodes");
            return;
        }
        var x = node.click();
        logd(x);
    } else {
        toast("no nodes");
    }
}

main();
```

### Cascaded Get Multiple Nodes getNodeInfo

* Get node information
* @param timeout Wait time in ms; 0=no wait
* @return Collection of NodeInfo nodes

```javascript showLineNumbers
function main() {
    // Get selector object
    // Select nodes with type=Text
    var node = type("Text").getOneNodeInfo(10000);
    if (node) {
        // Get all child nodes
        node = node.getNodeInfo(text("Ad").type("Text"), 10000);
        if (!node) {
            toast("no child nodes");
            return;
        }
        var x = node[0].click();
        logd(x);
    } else {
        toast("no nodes");
    }
}

main();
```

### parent Query Node Parent

* Query the node's parent
* @return `{NodeInfo}` Node object

```javascript showLineNumbers
function main() {
    // Begin node search
    let nd = textMatch(".*1.*").getOneNodeInfo(1000)
    if (nd) {
        console.log(" search node info {} ", JSON.stringify(nd))
        let parent = nd.parent()
        console.log(" search parent {} ", JSON.stringify(parent))
    } else {
        console.log(" search: node not found ")
    }
// Release previous lock first
    releaseNode();
}


main()
```

### child Get Single Child Node

* Get a single child node
* @param index Child node index
* @return `{NodeInfo}` NodeInfo object or null

```javascript showLineNumbers
function main() {
    // Begin node search
    let nd = textMatch(".*1.*").getOneNodeInfo(1000)
    if (nd) {
        console.log(" search node info {} ", JSON.stringify(nd))
        let child1 = nd.child(0)
        console.log(" search child {} ", JSON.stringify(child1))
    } else {
        console.log(" search: node not found ")
    }
// Release previous lock first
    releaseNode();
}


main()
```

### allChildren Get All Child Nodes

* Get all child nodes
* @return `{array}` Array of NodeInfo nodes

```javascript showLineNumbers
function main() {
    // Begin node search
    let nd = textMatch(".*1.*").getOneNodeInfo(1000)
    if (nd) {
        console.log(" search node info {} ", JSON.stringify(nd))
        let allChildren = nd.allChildren()
        console.log(" search allChildren {} ", JSON.stringify(allChildren))
    } else {
        console.log(" search: node not found ")
    }
// Release previous lock first
    releaseNode();
}


main()
```

### siblings Get All Sibling Nodes

* All sibling nodes of the current node
* @return `{array}` Array of NodeInfo nodes

```javascript showLineNumbers
function main() {
    // Begin node search
    let nd = labelMatch(".*1.*").getOneNodeInfo(1000)
    if (nd) {
        console.log(" search node info {} ", JSON.stringify(nd))
        let siblings = nd.siblings()
        console.log(" search siblings {} ", JSON.stringify(siblings))
    } else {
        console.log(" search: node not found ")
    }
// Release previous lock first
    releaseNode();
}

main()
```

### previousSiblings Get Previous Sibling Nodes

* Sibling nodes before the current node
* Supported: EC HarmonyOS Next 1.0.0+
* @return `{array}` Array of NodeInfo nodes

```javascript showLineNumbers
function main() {
    // Begin node search
    let nd = textMatch(".*1.*").getOneNodeInfo(1000)
    if (nd) {
        console.log(" search node info {} ", JSON.stringify(nd))
        let previousSiblings = nd.previousSiblings()
        console.log(" search previousSiblings {} ", JSON.stringify(previousSiblings))
    } else {
        console.log(" search: node not found ")
    }
// Release previous lock first
    releaseNode();
}

main()
```

### nextSiblings Get Next Sibling Nodes

* Sibling nodes after the current node
* @return `{array}` Array of NodeInfo nodes

```javascript showLineNumbers
function main() {
    // Begin node search
    let nd = textMatch(".*1.*").getOneNodeInfo(1000)
    if (nd) {
        console.log(" search node info {} ", JSON.stringify(nd))
        let nextSiblings = nd.nextSiblings()
        console.log(" search nextSiblings {} ", JSON.stringify(nextSiblings))
    } else {
        console.log(" search: node not found ")
    }
// Release previous lock first
    releaseNode();
}

main()
```
