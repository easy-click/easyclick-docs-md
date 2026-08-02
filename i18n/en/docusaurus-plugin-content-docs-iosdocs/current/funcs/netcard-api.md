---
title: Network Verification Functions
description: EasyClick automation scripts — iOS no jailbreak network verification functions
keywords:
 - EasyClick automation scripts iOS no jailbreak network verification functions
 - ecNetCard
 - 6.12.0
 - param
 - netCardInit
 - https
 - uc
 - ieasyclick
 - com
 - SDK
 - appId
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview

- The network verification module is an official module. Retrieve license keys from the [https://uc.ieasyclick.com](https://uc.ieasyclick.com) user center admin console
- Control center versions below 6.12.0 require downloading the network verification SDK separately and integrating it manually. Download: [Click to download](/iosdocs/ec-netcard-1.0.0.zip)
- Versions 6.12.0 and above can use function calls directly without integration — simple and easy
- License keys include heartbeat verification automatically — no separate call needed
- License keys automatically verify script security, increasing anti-cracking difficulty — this differs from other network verification platforms
- Whether using the integrated version or SDK version, the API is the same — all functions start with **ecNetCard**

## License Key Functions

### ecNetCard.netCardInit Initialize License Key

* [Network verification] Initialize license key
* Supported EC iOS control center 6.12.0+
* @param appId application appId from the user center admin console
* @param appSecret application secret from the user center admin console
* @param deviceIdType license authorization ID type — 1 for device ID, 2 for ecid (added in 6.29.0)
* @return `{json}` null on success, JSON data on failure

```javascript showLineNumbers
function main() {
    // Start writing code here!!
    logd("Checking automation environment...")

    logd("Starting script...")

    let appId = "sjfjvkpw"
    let appSecret = "ykjscxcs"
    let cardNo = "cbwolrftnw"


    let inited = ecNetCard.netCardInit(appId, appSecret, "2")
    logd("inited card => " + JSON.stringify(inited));
    let bind = ecNetCard.netCardBind(cardNo)
    logd("bind " + JSON.stringify(bind))
    loge("bind {}", JSON.stringify(bind))
    let bindResult = false;
    if (bind != null && bind != undefined && bind["code"] == 0) {
        loge("License key bound successfully")
        loge("Remaining time: " + bind['data']['leftDays'] + " days")
        loge("Activation time: " + bind['data']['startTime'])
        loge("Expiration time: " + bind['data']['expireTime'])
        bindResult = true;
    } else {
        if (bind == null || bind == undefined) {
            loge("License key binding failed, no response ")
        } else {
            loge("License key binding failed: " + bind["msg"])
        }
    }

    sleep(5000)
    if (!bindResult) {
        return
    }

    // Cloud variable demo
    let user_ageJson = ecNetCard.netCardGetCloudVar("user_age")
    // Returned JSON data
    loge("user age=> " + JSON.stringify(user_ageJson))
    // Get the corresponding value
    loge("user age value=> " + user_ageJson['data'])

    // Update user_age value
    let up = ecNetCard.netCardUpdateCloudVar("user_age", "12222");
    loge("netCardUpdateCloudVar => " + JSON.stringify(up))
    if (up['code'] == 0) {
        loge("netCardUpdateCloudVar updated successfully")
    }


    // Unbind (call as needed)
    // Can also unbind in the admin console
    let unddd = ecNetCard.netCardUnbind(cardNo, "12323")
    loge("netCardUnbind {}", JSON.stringify(unddd))
    sleep(2000)


    while (true) {
        sleep(1000)
    }
}

main();
```

### ecNetCard.netCardBind Bind License Key

* [Network verification] Bind license key
* Supported EC iOS control center 6.12.0+
* @param cardNo license key from the user center admin console
* @return JSON JSON object, ```{"code":0,"msg":""}```

```javascript showLineNumbers
function main() {
 // Start writing code here!!
 logd("Checking automation environment...")

 logd("Starting script...")

 let appId = "sjfjvkpw"
 let appSecret = "ykjscxcs"
 let cardNo = "cbwolrftnw"


 let inited = ecNetCard.netCardInit(appId, appSecret, "2")
 logd("inited card => " + JSON.stringify(inited));
 let bind = ecNetCard.netCardBind(cardNo)
 logd("bind " + JSON.stringify(bind))
 loge("bind {}", JSON.stringify(bind))
 let bindResult = false;
 if (bind != null && bind != undefined && bind["code"] == 0) {
 loge("License key bound successfully")
 loge("Remaining time: " + bind['data']['leftDays'] + " days")
 loge("Activation time: " + bind['data']['startTime'])
 loge("Expiration time: " + bind['data']['expireTime'])
 bindResult = true;
 } else {
 if (bind == null || bind == undefined) {
 loge("License key binding failed, no response ")
 } else {
 loge("License key binding failed: " + bind["msg"])
 }
 }

 sleep(5000)
 if (!bindResult) {
 return
 }

 // Cloud variable demo
 let user_ageJson = ecNetCard.netCardGetCloudVar("user_age")
 // Returned JSON data
 loge("user age=> " + JSON.stringify(user_ageJson))
 // Get the corresponding value
 loge("user age value=> " + user_ageJson['data'])

 // Update user_age value
 let up = ecNetCard.netCardUpdateCloudVar("user_age", "12222");
 loge("netCardUpdateCloudVar => " + JSON.stringify(up))
 if (up['code'] == 0) {
 loge("netCardUpdateCloudVar updated successfully")
 }


 // Unbind (call as needed)
 // Can also unbind in the admin console
 let unddd = ecNetCard.netCardUnbind(cardNo, "12323")
 loge("netCardUnbind {}", JSON.stringify(unddd))
 sleep(2000)


 while (true) {
 sleep(1000)
 }
}

main();
```

### ecNetCard.netCardUnbind Unbind License Key

* [Network verification] Unbind license key
* Supported EC iOS control center 6.12.0+
* @param cardNo license key from the user center admin console
* @param password unbind password; required if one was set
* @return JSON JSON object, ```{"code":0,"msg":""}```

```javascript showLineNumbers
function main() {
 // Start writing code here!!
 logd("Checking automation environment...")

 logd("Starting script...")

 let appId = "sjfjvkpw"
 let appSecret = "ykjscxcs"
 let cardNo = "cbwolrftnw"


 let inited = ecNetCard.netCardInit(appId, appSecret, "2")
 logd("inited card => " + JSON.stringify(inited));
 let bind = ecNetCard.netCardBind(cardNo)
 logd("bind " + JSON.stringify(bind))
 loge("bind {}", JSON.stringify(bind))
 let bindResult = false;
 if (bind != null && bind != undefined && bind["code"] == 0) {
 loge("License key bound successfully")
 loge("Remaining time: " + bind['data']['leftDays'] + " days")
 loge("Activation time: " + bind['data']['startTime'])
 loge("Expiration time: " + bind['data']['expireTime'])
 bindResult = true;
 } else {
 if (bind == null || bind == undefined) {
 loge("License key binding failed, no response ")
 } else {
 loge("License key binding failed: " + bind["msg"])
 }
 }

 sleep(5000)
 if (!bindResult) {
 return
 }

 // Cloud variable demo
 let user_ageJson = ecNetCard.netCardGetCloudVar("user_age")
 // Returned JSON data
 loge("user age=> " + JSON.stringify(user_ageJson))
 // Get the corresponding value
 loge("user age value=> " + user_ageJson['data'])

 // Update user_age value
 let up = ecNetCard.netCardUpdateCloudVar("user_age", "12222");
 loge("netCardUpdateCloudVar => " + JSON.stringify(up))
 if (up['code'] == 0) {
 loge("netCardUpdateCloudVar updated successfully")
 }


 // Unbind (call as needed)
 // Can also unbind in the admin console
 let unddd = ecNetCard.netCardUnbind(cardNo, "12323")
 loge("netCardUnbind {}", JSON.stringify(unddd))
 sleep(2000)


 while (true) {
 sleep(1000)
 }
}

main();
```

## Cloud Variables

### ecNetCard.netCardGetCloudVar Get Remote Variable

* [Network verification — remote variable] Get remote variable
* EC license keys are required to use remote variable features
* Supported EC iOS control center 6.12.0+
* @param key remote variable name
* @return JSON JSON object, ```{"code":0,"msg":""}```

```javascript showLineNumbers
function main() {
 // Start writing code here!!
 logd("Checking automation environment...")

 logd("Starting script...")

 let appId = "sjfjvkpw"
 let appSecret = "ykjscxcs"
 let cardNo = "cbwolrftnw"


 let inited = ecNetCard.netCardInit(appId, appSecret, "2")
 logd("inited card => " + JSON.stringify(inited));
 let bind = ecNetCard.netCardBind(cardNo)
 logd("bind " + JSON.stringify(bind))
 loge("bind {}", JSON.stringify(bind))
 let bindResult = false;
 if (bind != null && bind != undefined && bind["code"] == 0) {
 loge("License key bound successfully")
 loge("Remaining time: " + bind['data']['leftDays'] + " days")
 loge("Activation time: " + bind['data']['startTime'])
 loge("Expiration time: " + bind['data']['expireTime'])
 bindResult = true;
 } else {
 if (bind == null || bind == undefined) {
 loge("License key binding failed, no response ")
 } else {
 loge("License key binding failed: " + bind["msg"])
 }
 }

 sleep(5000)
 if (!bindResult) {
 return
 }

 // Cloud variable demo
 let user_ageJson = ecNetCard.netCardGetCloudVar("user_age")
 // Returned JSON data
 loge("user age=> " + JSON.stringify(user_ageJson))
 // Get the corresponding value
 loge("user age value=> " + user_ageJson['data'])

 // Update user_age value
 let up = ecNetCard.netCardUpdateCloudVar("user_age", "12222");
 loge("netCardUpdateCloudVar => " + JSON.stringify(up))
 if (up['code'] == 0) {
 loge("netCardUpdateCloudVar updated successfully")
 }


 // Unbind (call as needed)
 // Can also unbind in the admin console
 let unddd = ecNetCard.netCardUnbind(cardNo, "12323")
 loge("netCardUnbind {}", JSON.stringify(unddd))
 sleep(2000)


 while (true) {
 sleep(1000)
 }
}

main();
```

### ecNetCard.netCardUpdateCloudVar Update Remote Variable

* [Network verification — remote variable] Update remote variable
* Supported EC iOS control center 6.12.0+
* @param key remote variable name
* @param value remote variable content
* @return JSON JSON object, ```{"code":0,"msg":""}```

```javascript showLineNumbers
function main() {
 // Start writing code here!!
 logd("Checking automation environment...")

 logd("Starting script...")

 let appId = "sjfjvkpw"
 let appSecret = "ykjscxcs"
 let cardNo = "cbwolrftnw"


 let inited = ecNetCard.netCardInit(appId, appSecret, "2")
 logd("inited card => " + JSON.stringify(inited));
 let bind = ecNetCard.netCardBind(cardNo)
 logd("bind " + JSON.stringify(bind))
 loge("bind {}", JSON.stringify(bind))
 let bindResult = false;
 if (bind != null && bind != undefined && bind["code"] == 0) {
 loge("License key bound successfully")
 loge("Remaining time: " + bind['data']['leftDays'] + " days")
 loge("Activation time: " + bind['data']['startTime'])
 loge("Expiration time: " + bind['data']['expireTime'])
 bindResult = true;
 } else {
 if (bind == null || bind == undefined) {
 loge("License key binding failed, no response ")
 } else {
 loge("License key binding failed: " + bind["msg"])
 }
 }

 sleep(5000)
 if (!bindResult) {
 return
 }

 // Cloud variable demo
 let user_ageJson = ecNetCard.netCardGetCloudVar("user_age")
 // Returned JSON data
 loge("user age=> " + JSON.stringify(user_ageJson))
 // Get the corresponding value
 loge("user age value=> " + user_ageJson['data'])

 // Update user_age value
 let up = ecNetCard.netCardUpdateCloudVar("user_age", "12222");
 loge("netCardUpdateCloudVar => " + JSON.stringify(up))
 if (up['code'] == 0) {
 loge("netCardUpdateCloudVar updated successfully")
 }


 // Unbind (call as needed)
 // Can also unbind in the admin console
 let unddd = ecNetCard.netCardUnbind(cardNo, "12323")
 loge("netCardUnbind {}", JSON.stringify(unddd))
 sleep(2000)


 while (true) {
 sleep(1000)
 }
}

main();
```
