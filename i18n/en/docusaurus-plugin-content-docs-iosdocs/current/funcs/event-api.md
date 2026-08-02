---
title: Proxy Events
description: EasyClick automation scripts — iOS no jailbreak proxy events
keywords:
 - EasyClick automation scripts iOS no jailbreak proxy events
 - param
 - return
 - 'true'
 - 'false'
 - setAgentCallParam
 - touchDown
 - touchMove
 - touchUp
 - agentEvent
 - lockNodeFromXml
 - clickPoint
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview

- The proxy event module uses the `agentEvent` prefix, e.g. `agentEvent.clickPoint()`
- Functions listed here are specific to proxy mode; use global functions for other calls

## Settings

### setAgentCallParam Set Proxy Global Communication Timeout

* Set proxy mode parameters
* @param data Parameter map
* Example: ```{"remoteCallTimeout":10000}```
* remoteCallTimeout: Call timeout in milliseconds; default 10 seconds
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 var result = agentEvent.setAgentCallParam({"remoteCallTimeout": 10000});
 if (result) {
 logd("yes");
 } else {
 logd("no");
 }
}

main();
```

## Gesture and Input Events

### touchDown Execute Touch Down [Not Implemented]

* Execute touch-down input event
* @param x X coordinate
* @param y Y coordinate
* @return boolean true on success, false on failure

```javascript showLineNumbers
function main() {
 var result = agentEvent.touchDown(10, 10);
 if (result) {
 logd("success");
 } else {
 logd("failure");
 }
}

main();
```

### touchMove Execute Touch Move [Not Implemented]

* Execute touch-move input event
* @param x X coordinate
* @param y Y coordinate
* @return boolean true on success, false on failure

```javascript showLineNumbers
function main() {
 var result = agentEvent.touchMove(10, 10);
 if (result) {
 logd("success");
 } else {
 logd("failure");
 }
}

main();
```

### touchUp Execute Touch Up [Not Implemented]

* Execute touch-up input event
* @param x X coordinate
* @param y Y coordinate
* @return boolean true on success, false on failure

```javascript showLineNumbers
function main() {
 var result = agentEvent.touchUp(10, 10);
 if (result) {
 logd("success");
 } else {
 logd("failure");
 }
}

main();
```

## Nodes

### agentEvent.lockNodeFromXml Lock Node from XML {#agenteventlocknodefromxml-从-xml-锁定节点}

* Inject external XML into the PC-side node cache and lock it; equivalent to global `lockNodeFromXml(xml)`
* Typical use: with `auxEvent.agentDumpXml()` in WiFi scenarios and [Node functions](./node-api) selectors
* Requires EC iOS control center 9.39.0+
* @param xml XML string
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 auxEvent.ensureWifiSession("");
 auxEvent.agentStartEnv();
 auxEvent.agentSetFetchNodeParam({
 labelFilter: "2",
 maxDepth: "20",
 visibleFilter: "2",
 excludedAttributes:"visible,selected,enable,accessible"
 });

 let xml = auxEvent.agentDumpXml();
 logd("xml length: {}", xml ? xml.length : 0);
 if (!xml) {
 logd("agentDumpXml failed");
 return;
 }

 agentEvent.releaseNode();
 let ok = agentEvent.lockNodeFromXml(xml);
 logd("lockNodeFromXml: {}", ok);
 if (!ok) {
 return;
 }

 let nd = label("设置").getOneNodeInfo(0);
 logd("node: {}", nd ? JSON.stringify(nd) : "null");
}

main();
```

See full documentation at [Node functions — lockNodeFromXml](./node-api#locknodefromxml-从-xml-锁定节点); related Aux API: [agentDumpXml](./aux-event-api#agentdumpxml).
