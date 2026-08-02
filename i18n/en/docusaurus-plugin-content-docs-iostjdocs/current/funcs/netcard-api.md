---
title: Network Verification Functions
description: EasyClick automation scripts — iOS no jailbreak — network verification functions
keywords:
 - EasyClick automation scripts — iOS no jailbreak — network verification functions
 - ecNetCard
 - param
 - netCardInit
 - https
 - uc
 - ieasyclick
 - com
 - 3.1.0
 - appId
 - id
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview

- The network verification module is an official module. Retrieve license keys from the user center admin at [https://uc.ieasyclick.com](https://uc.ieasyclick.com)
- For versions above 3.1.0, you can call functions directly without integration — simple and easy to use
- License keys include built-in heartbeat verification; no separate call is needed
- License keys automatically verify script security and increase anti-tampering difficulty, unlike other network verification platforms
- Whether using the integrated or SDK version, all methods use the **ecNetCard** module prefix


## License Key Functions
### ecNetCard.netCardInit Initialize License Key
* [Network verification] Initialize license key
* Available in EC iOS standalone 3.1.0+
* @param appId application appId from the user center admin
* @param appSecret application secret from the user center admin
* @param deviceIdType license authorization ID type: `1` = device ID, `2` = ECID; parameter added in 3.9.0
* @return `{json}` `null` on success, JSON data on failure
```javascript showLineNumbers
function main() {
    // Start writing your code here!!
    logd("Checking automation environment...")

    logd("Starting script...")

    let appId = "sjfjvkpw"
    let appSecret = "ykjscxcs"
    let cardNo = "cbwolrftnw"

    
    //let inited = ecNetCard.netCardInit(appId, appSecret, "2")
    // Note: if you initialize online over the network, deviceIdType must be 1, otherwise initialization may fail
    let inited = ecNetCard.netCardInit(appId, appSecret, "1")
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
            loge("License key binding failed, no return value")
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
    // Get the value
    loge("user age value=> " + user_ageJson['data'])

    // Update user_age
    let up = ecNetCard.netCardUpdateCloudVar("user_age", "12222");
    loge("netCardUpdateCloudVar => " + JSON.stringify(up))
    if (up['code'] == 0) {
        loge("netCardUpdateCloudVar updated successfully")
    }


    // Unbind (call as needed)
    // You can also unbind from the admin panel
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
* Available in EC iOS standalone 3.1.0+
* @param cardNo license key from the user center admin
* @return ```{null|JSON} JSON object, {"code":0,"msg":"",}```

```javascript showLineNumbers
function main() {
 // Start writing your code here!!
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
 loge("License key binding failed, no return value")
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
 // Get the value
 loge("user age value=> " + user_ageJson['data'])

 // Update user_age
 let up = ecNetCard.netCardUpdateCloudVar("user_age", "12222");
 loge("netCardUpdateCloudVar => " + JSON.stringify(up))
 if (up['code'] == 0) {
 loge("netCardUpdateCloudVar updated successfully")
 }


 // Unbind (call as needed)
 // You can also unbind from the admin panel
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
* Available in EC iOS standalone 3.1.0+
* @param cardNo license key from the user center admin
* @param password unbind password; required if one was set
* @return ```{null|JSON} JSON object, {"code":0,"msg":"",}```

```javascript showLineNumbers
function main() {
 // Start writing your code here!!
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
 loge("License key binding failed, no return value")
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
 // Get the value
 loge("user age value=> " + user_ageJson['data'])

 // Update user_age
 let up = ecNetCard.netCardUpdateCloudVar("user_age", "12222");
 loge("netCardUpdateCloudVar => " + JSON.stringify(up))
 if (up['code'] == 0) {
 loge("netCardUpdateCloudVar updated successfully")
 }


 // Unbind (call as needed)
 // You can also unbind from the admin panel
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
* Remote variables require an EC license key
* Available in EC iOS standalone 3.1.0+
* @param key remote variable name
* @return ```{null|JSON} JSON object, {"code":0,"msg":"",}```

```javascript showLineNumbers
function main() {
 // Start writing your code here!!
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
 loge("License key binding failed, no return value")
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
 // Get the value
 loge("user age value=> " + user_ageJson['data'])

 // Update user_age
 let up = ecNetCard.netCardUpdateCloudVar("user_age", "12222");
 loge("netCardUpdateCloudVar => " + JSON.stringify(up))
 if (up['code'] == 0) {
 loge("netCardUpdateCloudVar updated successfully")
 }


 // Unbind (call as needed)
 // You can also unbind from the admin panel
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
* Available in EC iOS standalone 3.1.0+
* @param key remote variable name
* @param value remote variable content
* @return ```{null|JSON} JSON object, {"code":0,"msg":"",}```


```javascript showLineNumbers
function main() {
 // Start writing your code here!!
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
 loge("License key binding failed, no return value")
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
 // Get the value
 loge("user age value=> " + user_ageJson['data'])

 // Update user_age
 let up = ecNetCard.netCardUpdateCloudVar("user_age", "12222");
 loge("netCardUpdateCloudVar => " + JSON.stringify(up))
 if (up['code'] == 0) {
 loge("netCardUpdateCloudVar updated successfully")
 }


 // Unbind (call as needed)
 // You can also unbind from the admin panel
 let unddd = ecNetCard.netCardUnbind(cardNo, "12323")
 loge("netCardUnbind {}", JSON.stringify(unddd))
 sleep(2000)


 while (true) {
 sleep(1000)
 }
}

main();
```
