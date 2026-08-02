---
title: Node Functions — On-Device Execution
description: EasyClick automation scripts — iOS no jailbreak node functions
keywords:
 - EasyClick automation scripts iOS no jailbreak node functions
 - nodeAgent
 - setFetchNodeParam
 - visible
 - 'true'
 - label
 - getNodeInfo
 - getOneNodeInfo
 - parent
 - child
 - allChildren
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview {#说明}

- Node module functions handle node operations
- This module has no prefix; call functions directly
- Due to iOS limitations, **node fetching may be slow on some devices**. After fetching nodes, lock them before searching
- You can also tune node fetch parameters
- **Not suitable for pages such as video playback views**

:::tip
This module runs on the device; data is stored on the device
:::

## nodeAgent.setFetchNodeParam Set Node Fetch Parameters {#nodeagentsetfetchnodeparam-设置节点参数}

* Set base parameters for node fetching to reduce the number of nodes fetched and time spent
* Requires EC iOS control center 5.0.0+
* @param ext A map, e.g. ```{"visibleFilter":1}```
* visibleFilter 1 fetch nodes regardless of visible true/false; 2 fetch only visible=true nodes
* labelFilter 1 fetch nodes regardless of label value; 2 fetch only nodes with a label value
* boundsFilter 1 no filter; 2 filter nodes whose bounds x,y,w,h are all less than 0
* maxDepth Node hierarchy depth to fetch; fewer levels are faster; recommended 1–500
* maxChildCount Maximum child nodes to fetch; 0 means no limit
* excludedAttributes Attributes to exclude, comma-separated, e.g. visible,selected,enable — can speed up fetching
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 // Run once at the start of the script
 var data = nodeAgent.setFetchNodeParam({
 "labelFilter": "2",
 "maxDepth": "20",
 "visibleFilter": "2",
 "boundsFilter": "1",
 "excludedAttributes": ""
 })
 logd(data);
}

main();
```

## nodeAgent.getNodeInfo Get Node Collection {#nodeagentgetnodeinfo-获取节点集合}

* Requires EC iOS control center 5.0.0+
* @param selectors Node selector
* @param timeout Timeout in milliseconds
* @return `{array}` Collection of node objects

```javascript showLineNumbers
function main() {
 var data = nodeAgent.getNodeInfo(label("aaa"), 1000);
 logd(JSON.stringify(data));
}

main();
```

## nodeAgent.getOneNodeInfo Get Single Node {#nodeagentgetonenodeinfo-获取单节点}

* Requires EC iOS control center 5.0.0+
* @param selectors Node selector
* @param timeout Timeout in milliseconds
* @return `{NodeInfo}` Node object

```javascript showLineNumbers
function main() {
 var data = nodeAgent.getOneNodeInfo(label("aaa"), 1000);
 logd(JSON.stringify(data));
}

main();
```

## nodeAgent.parent Get Parent Node {#nodeagentparent-查询节点的父级}

* Get the parent of a node
* @param nodeInfo NodeInfo node object
* @return NodeInfo `{NodeInfo object|null}`

```javascript showLineNumbers
function main() {
 let n = nodeAgent.getOneNodeInfo(name("Other").name("文件"), 1000)
 if (n) {
 logd(JSON.stringify(n))
 let p = nodeAgent.parent(n)
 let allc = nodeAgent.getOneChildNodeInfo(p, name("小红书"), 10000);
 logd("getOneChildNodeInfo JSON " + JSON.stringify(allc))
 }
}

main()
```

## nodeAgent.child Get Single Child Node {#nodeagentchild-取得单个子节点}

* Get a single child node
* Requires EC iOS control center 5.0.0+
* @param nodeInfo NodeInfo node object
* @param index Child node index
* @return `{NodeInfo}` NodeInfo object or null

```javascript showLineNumbers
function main() {
 let n = nodeAgent.getOneNodeInfo(name("Other").name("文件"), 1000)
 if (n) {
 logd(JSON.stringify(n))
 let p = nodeAgent.parent(n)
 let allc = nodeAgent.child(p, 0);
 logd("child JSON " + JSON.stringify(allc))
 }
}


main()
```

## nodeAgent.allChildren Get All Child Nodes {#nodeagentallchildren-取得所有子节点}

* Get all child nodes
* Requires EC iOS control center 5.0.0+
* @param nodeInfo NodeInfo node object
* @return `{array}` Collection of NodeInfo nodes

```javascript showLineNumbers
function main() {
 let n = nodeAgent.getOneNodeInfo(name("Other").name("文件"), 1000)
 if (n) {
 logd(JSON.stringify(n))
 logd(n.bounds.centerX())
 logd(n.bounds.centerY())
 let parentX = nodeAgent.parent(n);
 logd("parent JSON " + JSON.stringify(parentX))
 logd(JSON.stringify(nodeAgent.allChildren(parentX)));
 }

}

main()
```

## nodeAgent.siblings Get All Sibling Nodes {#nodeagentsiblings-取得所有兄弟节点}

* Get all sibling nodes of the current node
* Requires EC iOS control center 5.0.0+
* @param nodeInfo NodeInfo node object
* @return `{array}` Collection of NodeInfo nodes

```javascript showLineNumbers
function main() {
 let n = nodeAgent.getOneNodeInfo(name("Other").name("文件"), 1000)
 if (n) {
 logd(JSON.stringify(n))
 let allc = nodeAgent.siblings(n);
 logd("siblings JSON " + JSON.stringify(allc))
 if (allc) {
 for (let i = 0; i < allc.length; i++) {
 logd("siblings " + JSON.stringify(allc[i]))
 }
 }
 }
}


main()
```

## nodeAgent.previousSiblings Get Previous Sibling Nodes {#nodeagentprevioussiblings-取得前面的兄弟节点}

* Get sibling nodes before the current node
* Requires EC iOS control center 5.0.0+
* @param nodeInfo NodeInfo node object
* @return `{array}` Collection of NodeInfo nodes

```javascript showLineNumbers
function main() {
 let n = nodeAgent.getOneNodeInfo(name("Other").name("文件"), 1000)
 if (n) {
 logd(JSON.stringify(n))
 let allc = nodeAgent.previousSiblings(n);
 logd("previousSiblings JSON " + JSON.stringify(allc))
 if (allc) {
 for (let i = 0; i < allc.length; i++) {
 logd("previousSiblings " + JSON.stringify(allc[i]))
 }
 }
 }
}

main()
```

## nodeAgent.nextSiblings Get Next Sibling Nodes {#nodeagentnextsiblings-取得后面的兄弟节点}

* Get sibling nodes after the current node
* Requires EC iOS control center 5.0.0+
* @param nodeInfo NodeInfo node object
* @return `{array}` Collection of NodeInfo nodes

```javascript showLineNumbers
function main() {
 let n = nodeAgent.getOneNodeInfo(name("Other").name("文件"), 1000)
 if (n) {
 logd(JSON.stringify(n))
 let allc = nodeAgent.nextSiblings(n);
 logd("nextSiblings JSON " + JSON.stringify(allc))
 if (allc) {
 for (let i = 0; i < allc.length; i++) {
 logd("nextSiblings " + JSON.stringify(allc[i]))
 }
 }
 }
}

main()
```

## nodeAgent.getOneChildNodeInfo Cascade Filter Single Child Node {#nodeagentgetonechildnodeinfo-级联筛选子节点}

* Cascade-filter a single child node
* Requires EC iOS control center 5.0.0+
* @param nodeInfo NodeInfo node object
* @param selectors Node selector
* @param timeout Timeout in milliseconds
* @return `{NodeInfo}` NodeInfo node object

```javascript showLineNumbers
function main() {
 let n = nodeAgent.getOneNodeInfo(name("Other").name("文件"), 1000)
 if (n) {
 logd(JSON.stringify(n))
 let p = nodeAgent.parent(n)
 let allc = nodeAgent.getOneChildNodeInfo(p, name("小红书"), 10000);
 logd("getOneChildNodeInfo JSON " + JSON.stringify(allc))
 }
}

main()
```

## nodeAgent.getChildNodeInfo Cascade Filter Multiple Child Nodes {#nodeagentgetchildnodeinfo-级联筛选多个子节点}

* Cascade-filter multiple child nodes
* Requires EC iOS control center 5.0.0+
* @param nodeInfo NodeInfo node object
* @param selectors Node selector
* @param timeout Timeout in milliseconds
* @return `{array}` Collection of NodeInfo nodes

```javascript showLineNumbers
function main() {
 let n = nodeAgent.getOneNodeInfo(name("Other").name("文件"), 1000)
 if (n) {
 logd(JSON.stringify(n))
 let p = nodeAgent.parent(n)
 let allc = nodeAgent.getChildNodeInfo(p, name("小红书"), 10000);
 logd("getChildNodeInfo JSON " + JSON.stringify(allc))
 }
}

main()
```

## nodeAgent.lockNode Lock Nodes {#nodeagentlocknode-锁定节点}

* Lock nodes
* Requires EC iOS control center 5.0.0+

```javascript showLineNumbers
function main() {
 nodeAgent.lockNode()
 for (let i = 0; i < 100; i++) {
 sleep(1000)
 let n = nodeAgent.getOneNodeInfo(name("Other").name("文件"), 1000)
 if (n) {
 logd(JSON.stringify(n))
 let p = nodeAgent.parent(n)
 let allc = nodeAgent.getChildNodeInfo(p, name("小红书"), 10000);
 logd("getOneChildNodeInfo JSON " + JSON.stringify(allc))
 } else {
 logd("no node info")
 }
 }
 nodeAgent.releaseNode()
}

main()
```

## nodeAgent.releaseNode Release Nodes {#nodeagentreleasenode-释放节点}

* Release locked nodes
* Requires EC iOS control center 5.0.0+

```javascript showLineNumbers
function main() {
 nodeAgent.lockNode()
 for (let i = 0; i < 100; i++) {
 sleep(1000)
 let n = nodeAgent.getOneNodeInfo(name("Other").name("文件"), 1000)
 if (n) {
 logd(JSON.stringify(n))
 let p = nodeAgent.parent(n)
 let allc = nodeAgent.getChildNodeInfo(p, name("小红书"), 10000);
 logd("getOneChildNodeInfo JSON " + JSON.stringify(allc))
 } else {
 logd("no node info")
 }
 }
 nodeAgent.releaseNode()
}

main()
```

## nodeAgent.dumpXml Export Nodes {#nodeagentdumpxml-导出节点}

* Export node XML (node matching runs on the device)
* Requires EC iOS control center 5.0.0+
* @return `{string}` Node XML string

:::tip Difference from lockNodeFromXml
- **nodeAgent**: `lockNode` / `getNodeInfo` run on the device; suitable for pure WiFi remote node selection
- **auxEvent + lockNodeFromXml**: `agentDumpXml` pulls XML via Aux, then PC-side `lockNodeFromXml` injects the cache — suitable when you want to use [Node Functions](/iosdocs/funcs/node-api) for selector matching on the PC
:::

```javascript showLineNumbers
function main() {
 logd(nodeAgent.dumpXml())
}

main()
```
