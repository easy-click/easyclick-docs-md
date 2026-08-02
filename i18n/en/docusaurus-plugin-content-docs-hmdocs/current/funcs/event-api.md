---
title: Agent Events
description: EasyClick automation scripts — HarmonyOS Next automation — agent events
keywords:
 - EasyClick automation scripts HarmonyOS Next automation agent events
 - setAgentCallParam
 - agentEvent
 - clickPoint
 - param
 - data
 - remoteCallTimeout
 - return
 - 'true'
 - 'false'
 - EasyClick
 - mobile automation
 - automation testing
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
 - remote screen mirroring
---

## Overview

- The agent event module uses the `agentEvent` object prefix, e.g. `agentEvent.clickPoint`
- This section lists functions specific to agent mode; other calls can use global functions directly

## Settings

### setAgentCallParam — Set Agent Global Communication Timeout

* Set agent mode parameters
* @param data Parameter map
* Example: ```{"remoteCallTimeout":10000}```
* remoteCallTimeout: Call timeout in milliseconds; default is 10 seconds
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

