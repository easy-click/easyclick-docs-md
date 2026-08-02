---

title: Network Verification Functions
description: EasyClick automation scripts — HarmonyOS Next automation Network Verification Functions
keywords:
 - EasyClick automation scripts — HarmonyOS Next automation Network Verification Functions
 - ecNetCard
 - param
 - JSON
 - netCardInit
 - netCardBind
 - https
 - uc
 - ieasyclick
 - com
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

- The network verification module is an official EasyClick module. Retrieve license keys from the user center at [http://uc.ieasyclick.com](http://uc.ieasyclick.com)
- For APK versions below 9.13.0, download the network verification SDK separately and integrate it into your script manually
- For 9.13.0+, call functions directly without SDK integration
- License keys include heartbeat verification automatically — no separate call needed
- License keys also validate script integrity to increase anti-tamper difficulty, unlike many third-party verification platforms
- Both integrated and SDK versions use the same **ecNetCard** module prefix
- Platform usage guide: [View here](/docs/advance/netcard)

## License Key Operations

### ecNetCard.setDeviceId Custom Device ID

* [Network verification] Set a custom device ID
* Key management: [http://uc.ieasyclick.com]
* Requires EC Android 11.21.0+
* @param deviceId Custom device ID you generate; max 32 characters
* @return `{boolean}` true on success

```javascript showLineNumbers
function main() {
    // Start writing your code here!!
    logd("Checking automation environment...")

    logd("Starting script...")
// Example only — do not copy verbatim; use android_id or generate your own
    ecNetCard.setDeviceId("123")
    let appId = "sjfjvkpw"
    let appSecret = "ykjscxcs"
let cardNo = "cbwolrftnw"

    let inited = ecNetCard.netCardInit(appId, appSecret)
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
            loge("License key binding failed, no return value ")
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
    // Can also unbind from the admin console
    let unddd = ecNetCard.netCardUnbind(cardNo, "12323")
    loge("netCardUnbind {}", JSON.stringify(unddd))
    sleep(2000)


    while (true) {
        sleep(1000)
    }
}

main();
```

### ecNetCard.netCardInit Initialize License Key

* [Network verification] Initialize license key module
* Key management: [http://uc.ieasyclick.com]
* Requires EC Android 9.13.0+
* @param appId Application appId from the user center
* @param appSecret Application secret from the user center
* @return `{json}` null on success; JSON data on failure

```javascript showLineNumbers
function main() {
 // Start writing your code here!!
 logd("Checking automation environment...")

 logd("Starting script...")

 let appId = "sjfjvkpw"
 let appSecret = "ykjscxcs"
 let cardNo = "cbwolrftnw"

 let inited = ecNetCard.netCardInit(appId, appSecret)
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
 loge("License key binding failed, no return value ")
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
 loge("user age=> " + JSON.stringify(user_ageJson))
 loge("user age value=> " + user_ageJson['data'])

 let up = ecNetCard.netCardUpdateCloudVar("user_age", "12222");
 loge("netCardUpdateCloudVar => " + JSON.stringify(up))
 if (up['code'] == 0) {
 loge("netCardUpdateCloudVar updated successfully")
 }

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

* [Network verification] Bind a license key
* Key management: [http://uc.ieasyclick.com]
* Requires EC Android 9.13.0+
* @param cardNo License key from the user center
* @return `{null|JSON}` JSON object, e.g. `{"code":0,"msg":""}`

```javascript showLineNumbers
function main() {
 logd("Checking automation environment...")
 logd("Starting script...")
 let appId = "sjfjvkpw"
 let appSecret = "ykjscxcs"
 let cardNo = "cbwolrftnw"
 let inited = ecNetCard.netCardInit(appId, appSecret)
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
 loge("License key binding failed, no return value ")
 } else {
 loge("License key binding failed: " + bind["msg"])
 }
 }
 sleep(5000)
 if (!bindResult) { return }
 let user_ageJson = ecNetCard.netCardGetCloudVar("user_age")
 loge("user age=> " + JSON.stringify(user_ageJson))
 loge("user age value=> " + user_ageJson['data'])
 let up = ecNetCard.netCardUpdateCloudVar("user_age", "12222");
 loge("netCardUpdateCloudVar => " + JSON.stringify(up))
 if (up['code'] == 0) { loge("netCardUpdateCloudVar updated successfully") }
 let unddd = ecNetCard.netCardUnbind(cardNo, "12323")
 loge("netCardUnbind {}", JSON.stringify(unddd))
 sleep(2000)
 while (true) { sleep(1000) }
}
main();
```

### ecNetCard.netCardUnbind Unbind License Key

* [Network verification] Unbind a license key
* Key management: [http://uc.ieasyclick.com]
* Requires EC Android 9.13.0+
* @param cardNo License key from the user center
* @param password Unbind password; required if one was set
* @return `{null|JSON}` JSON object, e.g. `{"code":0,"msg":""}`

```javascript showLineNumbers
function main() {
 logd("Checking automation environment...")
 logd("Starting script...")
 let appId = "sjfjvkpw"
 let appSecret = "ykjscxcs"
 let cardNo = "cbwolrftnw"
 let inited = ecNetCard.netCardInit(appId, appSecret)
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
 loge("License key binding failed, no return value ")
 } else {
 loge("License key binding failed: " + bind["msg"])
 }
 }
 sleep(5000)
 if (!bindResult) { return }
 let user_ageJson = ecNetCard.netCardGetCloudVar("user_age")
 loge("user age=> " + JSON.stringify(user_ageJson))
 loge("user age value=> " + user_ageJson['data'])
 let up = ecNetCard.netCardUpdateCloudVar("user_age", "12222");
 loge("netCardUpdateCloudVar => " + JSON.stringify(up))
 if (up['code'] == 0) { loge("netCardUpdateCloudVar updated successfully") }
 let unddd = ecNetCard.netCardUnbind(cardNo, "12323")
 loge("netCardUnbind {}", JSON.stringify(unddd))
 sleep(2000)
 while (true) { sleep(1000) }
}
main();
```

### ecNetCard.getCardInfo Get License Key Info

* [Network verification] Get license key information
* Can be called from UI to display info on screen
* Key management: [http://uc.ieasyclick.com]
* Requires EC Android 9.13.0+
* @param appId Application appId from the user center
* @param appSecret Application secret from the user center
* @param cardNo License key
* @return `{null|JSON}` JSON object, e.g. `{"code":0,"msg":""}`

```javascript showLineNumbers
function main() {
 logd("Starting script...")
 let appId = "sjfjvkpw"
 let appSecret = "ykjscxcs"
 let cardNo = "cbwolrftnw"

 let card = ecNetCard.getCardInfo(appId, appSecret, cardNo)
 logd("card " + JSON.stringify(card))
}

main();
```

## Cloud Variables

### ecNetCard.netCardGetCloudVar Get Remote Variable

* [Network verification — remote variables] Get a remote variable
* Key management: [http://uc.ieasyclick.com]
* Requires EC Android 9.13.0+
* @param key Remote variable name
* @return `{null|JSON}` JSON object, e.g. `{"code":0,"msg":""}`

```javascript showLineNumbers
function main() {
 logd("Checking automation environment...")
 logd("Starting script...")
 let appId = "sjfjvkpw"
 let appSecret = "ykjscxcs"
 let cardNo = "cbwolrftnw"
 let inited = ecNetCard.netCardInit(appId, appSecret)
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
 loge("License key binding failed, no return value ")
 } else {
 loge("License key binding failed: " + bind["msg"])
 }
 }
 sleep(5000)
 if (!bindResult) { return }
 let user_ageJson = ecNetCard.netCardGetCloudVar("user_age")
 loge("user age=> " + JSON.stringify(user_ageJson))
 loge("user age value=> " + user_ageJson['data'])
 let up = ecNetCard.netCardUpdateCloudVar("user_age", "12222");
 loge("netCardUpdateCloudVar => " + JSON.stringify(up))
 if (up['code'] == 0) { loge("netCardUpdateCloudVar updated successfully") }
 let unddd = ecNetCard.netCardUnbind(cardNo, "12323")
 loge("netCardUnbind {}", JSON.stringify(unddd))
 sleep(2000)
 while (true) { sleep(1000) }
}
main();
```

### ecNetCard.netCardUpdateCloudVar Update Remote Variable

* [Network verification — remote variables] Update a remote variable
* Key management: [http://uc.ieasyclick.com]
* Requires EC Android 9.13.0+
* @param key Remote variable name
* @param value Remote variable content
* @return `{null|JSON}` JSON object, e.g. `{"code":0,"msg":""}`

```javascript showLineNumbers
function main() {
 logd("Checking automation environment...")
 logd("Starting script...")
 let appId = "sjfjvkpw"
 let appSecret = "ykjscxcs"
 let cardNo = "cbwolrftnw"
 let inited = ecNetCard.netCardInit(appId, appSecret)
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
 loge("License key binding failed, no return value ")
 } else {
 loge("License key binding failed: " + bind["msg"])
 }
 }
 sleep(5000)
 if (!bindResult) { return }
 let user_ageJson = ecNetCard.netCardGetCloudVar("user_age")
 loge("user age=> " + JSON.stringify(user_ageJson))
 loge("user age value=> " + user_ageJson['data'])
 let up = ecNetCard.netCardUpdateCloudVar("user_age", "12222");
 loge("netCardUpdateCloudVar => " + JSON.stringify(up))
 if (up['code'] == 0) { loge("netCardUpdateCloudVar updated successfully") }
 let unddd = ecNetCard.netCardUnbind(cardNo, "12323")
 loge("netCardUnbind {}", JSON.stringify(unddd))
 sleep(2000)
 while (true) { sleep(1000) }
}
main();
```
