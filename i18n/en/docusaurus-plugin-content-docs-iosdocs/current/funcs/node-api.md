---
title: Node Functions
description: EasyClick automation scripts — iOS no jailbreak node functions
keywords:
 - EasyClick automation scripts iOS no jailbreak node functions
 - label
 - id
 - setFetchNodeParam
 - lockNode
 - lockNodeFromXml
 - releaseNode
 - name
 - visible
 - 'true'
 - getNodeInfo
 - getOneNodeInfo
 - xpath
 - idMatch
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
- Due to iOS limitations, **node fetch may be slow on some devices**. After fetching nodes, lock them before searching
- You can also use setFetchNodeParam to tune node fetching
- **Some pages may be unsuitable (e.g. video playback pages)**

**Three ways to lock nodes:**

| Function | Scenario | Description |
|------|----------|------|
| `lockNode` | USB direct | Pull XML from phone over USB and lock into PC cache |
| `lockNodeFromXml` | WiFi + Aux | Inject external XML (e.g. `auxEvent.agentDumpXml()`) into PC cache and lock (9.39.0+) |
| `releaseNode` | General | Release lock; next fetch uses refreshed UI |

For the full WiFi workflow see [lockNodeFromXml](#locknodefromxml-从-xml-锁定节点); object form: `agentEvent.lockNodeFromXml(xml)` — see [Proxy Events](./event-api).

###

## setFetchNodeParam Set Node Fetch Parameters

* Set base parameters for node fetching to reduce node count and time cost
* Supported: EC iOS control center 3.0.0+
* @param ext A map, e.g. ```{"visibleFilter":1}```
* visibleFilter: 1 = fetch regardless of visible; 2 = only visible=true nodes
* labelFilter: 1 = fetch regardless of label; 2 = only nodes with a label value
* boundsFilter: 1 = no filter; 2 = filter nodes whose bounds x,y,w,h are all < 0
* maxDepth: Node tree depth to fetch; lower is faster; recommended 1–500
* maxChildCount: Max child nodes to fetch; 0 = unlimited
* excludedAttributes: Comma-separated attributes to exclude for faster fetching, e.g. visible,selected,enable
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 // Run once at the start of the script
 var data = setFetchNodeParam({"labelFilter": "2", "maxDepth": "20", "visibleFilter": "2", "excludedAttributes": "visible,selected,enable,accessible"})
 logd(data);
}

main();
```

## lockNode Lock Nodes

* Pull node XML from phone over USB and lock into PC cache; UI refresh won't update cached tree until `releaseNode`
* Supported: EC iOS control center 3.0.0+
* Use with `setFetchNodeParam` to narrow fetch scope and improve speed

```javascript showLineNumbers
function main() {
 setFetchNodeParam({"labelFilter": "2", "maxDepth": "20", "visibleFilter": "2"});
 releaseNode();
 lockNode();

 let nd = label("Settings").getOneNodeInfo(1000);
 logd("node: {}", nd ? JSON.stringify(nd) : "null");
}

main();
```

## lockNodeFromXml Lock Node from XML {#locknodefromxml-从-xml-锁定节点}

* Inject external XML into PC node cache and lock; use with [auxEvent.agentDumpXml](./aux-event-api#agentdumpxml), then continue with selectors (`label().getNodeInfo()`, etc.)
* Requires EC iOS control center 9.39.0+
* Object form: `agentEvent.lockNodeFromXml(xml)` — see [Proxy Events — lockNodeFromXml](./event-api#agenteventlocknodefromxml-从-xml-锁定节点)
* @param xml XML string (e.g. return value of `auxEvent.agentDumpXml()`)
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 auxEvent.ensureWifiSession("");
 auxEvent.agentStartEnv();
 auxEvent.agentSetFetchNodeParam({
 labelFilter: "2",
 maxDepth: "20",
 visibleFilter: "2"
 });

 let xml = auxEvent.agentDumpXml();
 logd("xml length: {}", xml ? xml.length : 0);
 if (!xml) {
 logd("agentDumpXml failed");
 return;
 }

 releaseNode();
 let ok = lockNodeFromXml(xml);
 logd("lockNodeFromXml: {}", ok);
 if (!ok) {
 return;
 }

 let nd = label("Settings").getOneNodeInfo(0);
 logd("node: {}", nd ? JSON.stringify(nd) : "null");
}

main();
```

## releaseNode Release Nodes

* Release node lock; after release, `getNodeInfo` etc. re-fetch XML from current UI
* Supported: EC iOS control center 3.0.0+
* Use together with `lockNode` and `lockNodeFromXml`

```javascript showLineNumbers
function main() {
 setFetchNodeParam({"labelFilter": "2", "maxDepth": "20", "visibleFilter": "2"});
 releaseNode();
 lockNode();

 let nd = label("Settings").getOneNodeInfo(1000);
 logd("locked: {}", nd ? JSON.stringify(nd) : "null");

 releaseNode();
 logd("Node lock released");
}

main();
```

## getNodeInfo Get Node Collection

* timeout Timeout in milliseconds
* Supported: EC iOS control center 3.0.0+
* @return `{array}` Array of node objects

```javascript showLineNumbers
function main() {
 var data = label("aaa").getNodeInfo(1000);
 logd(JSON.stringify(data));
}

main();
```

## getOneNodeInfo Get Single Node

* timeout Timeout in milliseconds
* Supported: EC iOS control center 3.0.0+
* @return `{NodeInfo}` Node object

```javascript showLineNumbers
function main() {
 var data = label("aaa").getOneNodeInfo(1000);
 logd(JSON.stringify(data));
}

main();
```

## Selector Functions

## id id Exact Attribute Match

* Exact match on id attribute
* Supported: EC iOS control center 3.0.0+

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
 // Set node fetch parameters; see setFetchNodeParam for details
 // Run once at the start of the script
 setFetchNodeParam({"labelFilter": "2", "maxDepth": "20", "visibleFilter": "2", "excludedAttributes": ""})
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

## xpath Selection

* XPath selector; see https://www.jianshu.com/p/c205334122b3 and https://www.runoob.com/xpath/xpath-syntax.html
* Use the IDE node panel to inspect xpath attributes and test xpath
* Requires EC 7.7.0+
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

## idMatch id Regex Attribute Match

* Regex match on id attribute
* Supported: EC iOS control center 3.0.0+

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
 // Set node fetch parameters; see setFetchNodeParam for details
 setFetchNodeParam({"labelFilter": "2", "maxDepth": "20", "visibleFilter": "2", "excludedAttributes": ""})
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

## label label Exact Attribute Match

* Exact match on label attribute
* Supported: EC iOS control center 3.0.0+

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
 // Set node fetch parameters; see setFetchNodeParam for details
 setFetchNodeParam({"labelFilter": "2", "maxDepth": "20", "visibleFilter": "2", "excludedAttributes": ""})
 // Release previous lock first
 releaseNode();
 // Lock new node data
 lockNode();
 // Begin node search
 // Search by label
 let nd = label("Settings").getOneNodeInfo(1000)
 if (nd) {
 console.log("label search node info {} ", JSON.stringify(nd))
 // Click if found
 let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
 logd("click it: {}", c)
 } else {
 console.log("label search: node not found ")
 }
 // Search by regex
 nd = labelMatch(".*Settings.*").getNodeInfo(1000)
 if (nd) {
 console.log("labelMatch search node info {} ", JSON.stringify(nd))
 } else {
 console.log("labelMatch search: node not found ")
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

## labelMatch label Regex Attribute Match

* Regex match on label attribute
* Supported: EC iOS control center 3.0.0+

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
 // Set node fetch parameters; see setFetchNodeParam for details
 setFetchNodeParam({"labelFilter": "2", "maxDepth": "20", "visibleFilter": "2", "excludedAttributes": ""})
 // Release previous lock first
 releaseNode();
 // Lock new node data
 lockNode();
 // Begin node search
 // Search by label
 let nd = label("Settings").getOneNodeInfo(1000)
 if (nd) {
 console.log("label search node info {} ", JSON.stringify(nd))
 // Click if found
 let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
 logd("click it: {}", c)
 } else {
 console.log("label search: node not found ")
 }
 // Search by regex
 nd = labelMatch(".*Settings.*").getNodeInfo(1000)
 if (nd) {
 console.log("labelMatch search node info {} ", JSON.stringify(nd))
 } else {
 console.log("labelMatch search: node not found ")
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

## name name Exact Attribute Match

* Exact match on name attribute
* Supported: EC iOS control center 3.0.0+

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
 // Set node fetch parameters; see setFetchNodeParam for details
 setFetchNodeParam({"labelFilter": "2", "maxDepth": "20", "visibleFilter": "2", "excludedAttributes": ""})
 // Release previous lock first
 releaseNode();
 // Lock new node data
 lockNode();
 // Begin node search
 // Search by name
 let nd = name("Settings").getOneNodeInfo(1000)
 if (nd) {
 console.log("name search node info {} ", JSON.stringify(nd))
 // Click if found
 let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
 logd("click it: {}", c)
 } else {
 console.log("name search: node not found ")
 }
 // Search by name regex
 nd = nameMatch(".*Settings.*").getNodeInfo(1000)
 if (nd) {
 console.log("nameMatch search node info {} ", JSON.stringify(nd))
 } else {
 console.log("nameMatch search: node not found ")
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

## nameMatch name Regex Attribute Match

* Regex match on name attribute
* Supported: EC iOS control center 3.0.0+

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
 // Set node fetch parameters; see setFetchNodeParam for details
 setFetchNodeParam({"labelFilter": "2", "maxDepth": "20", "visibleFilter": "2", "excludedAttributes": ""})
 // Release previous lock first
 releaseNode();
 // Lock new node data
 lockNode();
 // Begin node search
 // Search by name
 let nd = name("Settings").getOneNodeInfo(1000)
 if (nd) {
 console.log("name search node info {} ", JSON.stringify(nd))
 // Click if found
 let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
 logd("click it: {}", c)
 } else {
 console.log("name search: node not found ")
 }
 // Search by name regex
 nd = nameMatch(".*Settings.*").getNodeInfo(1000)
 if (nd) {
 console.log("nameMatch search node info {} ", JSON.stringify(nd))
 } else {
 console.log("nameMatch search: node not found ")
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

## type type Exact Attribute Match

* Exact match on type attribute
* Supported: EC iOS control center 3.0.0+

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
 // Set node fetch parameters; see setFetchNodeParam for details
 setFetchNodeParam({"labelFilter": "2", "maxDepth": "20", "visibleFilter": "2", "excludedAttributes": ""})
 // Release previous lock first
 releaseNode();
 // Lock new node data
 lockNode();
 // Begin node search
 // Search by type
 let nd = type("XCUIElementTypeOther").getOneNodeInfo(1000)
 if (nd) {
 console.log("type search node info {} ", JSON.stringify(nd))
 // Click if found
 let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
 logd("click it: {}", c)
 } else {
 console.log("label search: node not found ")
 }
 // Search by type regex
 nd = typeMatch(".*XCUIElement.*").getNodeInfo(1000)
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

## typeMatch type Regex Attribute Match

* Regex match on type attribute
* Supported: EC iOS control center 3.0.0+

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
 // Set node fetch parameters; see setFetchNodeParam for details
 setFetchNodeParam({"labelFilter": "2", "maxDepth": "20", "visibleFilter": "2", "excludedAttributes": ""})
 // Release previous lock first
 releaseNode();
 // Lock new node data
 lockNode();
 // Begin node search
 // Search by type
 let nd = type("XCUIElementTypeOther").getOneNodeInfo(1000)
 if (nd) {
 console.log("type search node info {} ", JSON.stringify(nd))
 // Click if found
 let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
 logd("click it: {}", c)
 } else {
 console.log("label search: node not found ")
 }
 // Search by type regex
 nd = typeMatch(".*XCUIElementTypeOther.*").getNodeInfo(1000)
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

## value value Exact Attribute Match

* Exact match on value attribute
* Supported: EC iOS control center 3.0.0+

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
 // Set node fetch parameters; see setFetchNodeParam for details
 setFetchNodeParam({"labelFilter": "2", "maxDepth": "20", "visibleFilter": "2", "excludedAttributes": ""})
 // Release previous lock first
 releaseNode();
 // Lock new node data
 lockNode();
 // Begin node search
 // Search by value
 let nd = value("XCUIElementTypeOther").getOneNodeInfo(1000)
 if (nd) {
 console.log("value search node info {} ", JSON.stringify(nd))
 // Click if found
 let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
 logd("click it: {}", c)
 } else {
 console.log("value search: node not found ")
 }
 // Search by type regex
 nd = valueMatch(".*XCUIElementTypeOther.*").getNodeInfo(1000)
 if (nd) {
 console.log("valueMatch search node info {} ", JSON.stringify(nd))
 } else {
 console.log("valueMatch search: node not found ")
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

## valueMatch value Regex Attribute Match

* Regex match on value attribute
* Supported: EC iOS control center 3.0.0+

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
 // Set node fetch parameters; see setFetchNodeParam for details
 setFetchNodeParam({"labelFilter": "2", "maxDepth": "20", "visibleFilter": "2", "excludedAttributes": ""})
 // Release previous lock first
 releaseNode();
 // Lock new node data
 lockNode();
 // Begin node search
 // Search by value
 let nd = value("XCUIElementTypeOther").getOneNodeInfo(1000)
 if (nd) {
 console.log("value search node info {} ", JSON.stringify(nd))
 // Click if found
 let c = clickPoint(nd.bounds.centerX(), nd.bounds.centerY());
 logd("click it: {}", c)
 } else {
 console.log("value search: node not found ")
 }
 // Search by type regex
 nd = valueMatch(".*XCUIElementTypeOther.*").getNodeInfo(1000)
 if (nd) {
 console.log("valueMatch search node info {} ", JSON.stringify(nd))
 } else {
 console.log("valueMatch search: node not found ")
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

## enable enable Exact Attribute Match

* Exact match on enable attribute
* Supported: EC iOS control center 3.0.0+

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
 // Set node fetch parameters; see setFetchNodeParam for details
 setFetchNodeParam({"labelFilter": "2", "maxDepth": "20", "visibleFilter": "2", "excludedAttributes": ""})
 // Release previous lock first
 releaseNode();
 // Lock new node data
 lockNode();
 // Begin node search
 // Search by enable and accessible
 let nd = enable(true).accessible(true).getOneNodeInfo(1000)
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

## accessible accessible Exact Attribute Match

* Exact match on accessible attribute

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
 // Set node fetch parameters; see setFetchNodeParam for details
 setFetchNodeParam({"labelFilter": "2", "maxDepth": "20", "visibleFilter": "2", "excludedAttributes": ""})
 // Release previous lock first
 releaseNode();
 // Lock new node data
 lockNode();
 // Begin node search
 // Search by enabled and accessible
 let nd = enabled(true).accessible(true).getOneNodeInfo(1000)
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

## visible visible Exact Attribute Match

* Exact match on visible attribute
* Supported: EC iOS control center 3.0.0+

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
 // Set node fetch parameters; see setFetchNodeParam for details
 setFetchNodeParam({"labelFilter": "2", "maxDepth": "20", "visibleFilter": "2", "excludedAttributes": ""})
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

## index index Exact Attribute Match

* Exact match on index attribute
* Supported: EC iOS control center 3.0.0+

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
 // Set node fetch parameters; see setFetchNodeParam for details
 setFetchNodeParam({"labelFilter": "2", "maxDepth": "20", "visibleFilter": "2", "excludedAttributes": ""})
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

## depth depth Exact Attribute Match

* Exact match on depth attribute
* Supported: EC iOS control center 3.0.0+

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
 // Set node fetch parameters; see setFetchNodeParam for details
 setFetchNodeParam({"labelFilter": "2", "maxDepth": "20", "visibleFilter": "2", "excludedAttributes": ""})
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

## selected selected Exact Attribute Match

* Exact match on selected attribute
* Supported: EC iOS control center 3.0.0+

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
 // Set node fetch parameters; see setFetchNodeParam for details
 setFetchNodeParam({"labelFilter": "2", "maxDepth": "20", "visibleFilter": "2", "excludedAttributes": ""})
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

## childcount childcount Exact Attribute Match

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
 // Set node fetch parameters; see setFetchNodeParam for details
 setFetchNodeParam({"labelFilter": "2", "maxDepth": "20", "visibleFilter": "2", "excludedAttributes": ""})
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

## bounds bounds Bounds Range Match

* Bounds range match on bounds attribute
* Supported: EC iOS control center 3.0.0+

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
 // Set node fetch parameters; see setFetchNodeParam for details
 setFetchNodeParam({"labelFilter": "2", "maxDepth": "20", "visibleFilter": "2", "excludedAttributes": ""})
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

## clickCenter Click Node Center

* Click the center of the node
* Supported: EC iOS control center 3.0.0+
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 setFetchNodeParam({
 "labelFilter": "2",
 "maxDepth": "70",
 "visibleFilter": "2",
 "excludedAttributes": "visible,2"
 })
 let node = name("Maps").getOneNodeInfo(10000)
 logd(JSON.stringify(node))
 if (node) {
 logd(node.clickCenter())
 }

}

main()
```

## clickRandom Click Node Randomly

* Click a random point within the node bounds
* Supported: EC iOS control center 3.0.0+
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 setFetchNodeParam({
 "labelFilter": "2",
 "maxDepth": "70",
 "visibleFilter": "2",
 "excludedAttributes": "visible,2"
 })
 let node = name("Maps").getOneNodeInfo(10000)
 logd(JSON.stringify(node))
 if (node) {
 logd(node.clickRandom())
 }

}

main()
```

## Multi-Attribute Cascaded Query

Supported: EC iOS control center 3.0.0+

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
 // Set node fetch parameters; see setFetchNodeParam for details
 setFetchNodeParam({"labelFilter": "2", "maxDepth": "20", "visibleFilter": "2", "excludedAttributes": ""})
 // Release previous lock first
 releaseNode();
 // Lock new node data
 lockNode();
 // Begin node search
 // Cascaded attribute search
 let nd = labelMatch(".*1.*").enabled(true).accessible(true).bounds(100, 100, 300, 300).getOneNodeInfo(1000)
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

## parent Query Node Parent

* Query the node's parent
* Supported: EC iOS control center 3.0.0+
* @return `{NodeInfo}` Node object

```javascript showLineNumbers
function main() {
 // Begin node search
 let nd = labelMatch(".*1.*").getOneNodeInfo(1000)
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

## child Get Single Child Node

* Get a single child node
* Supported: EC iOS control center 3.0.0+
* @param index Child node index
* @return `{NodeInfo}` NodeInfo object or null

```javascript showLineNumbers
function main() {
 // Begin node search
 let nd = labelMatch(".*1.*").getOneNodeInfo(1000)
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

## allChildren Get All Child Nodes

* Get all child nodes
* Supported: EC iOS control center 3.0.0+
* @return `{array}` Array of NodeInfo nodes

```javascript showLineNumbers
function main() {
 // Begin node search
 let nd = labelMatch(".*1.*").getOneNodeInfo(1000)
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

## siblings Get All Sibling Nodes

* All sibling nodes of the current node
* Supported: EC iOS control center 3.0.0+
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

## previousSiblings Get Previous Sibling Nodes

* Sibling nodes before the current node
* Supported: EC iOS control center 3.0.0+
* @return `{array}` Array of NodeInfo nodes

```javascript showLineNumbers
function main() {
 // Begin node search
 let nd = labelMatch(".*1.*").getOneNodeInfo(1000)
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

## nextSiblings Get Next Sibling Nodes

* Sibling nodes after the current node
* Supported: EC iOS control center 3.0.0+
* @return `{array}` Array of NodeInfo nodes

```javascript showLineNumbers
function main() {
 // Begin node search
 let nd = labelMatch(".*1.*").getOneNodeInfo(1000)
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
