---

title: Storage Functions
description: EasyClick automation scripts — iOS no jailbreak storage functions
keywords:
 - EasyClick automation scripts iOS no jailbreak storage functions
 - storage
 - key
 - param
 - return
 - storages
 - create
 - value
 - keys
 - all
 - putString
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview

- Storage module functions store key-value data
- The utility module uses the `storages` prefix, e.g. `storages.create()`

## Create Storage Object

### storages.create Create Storage

* Create a storage object
* Supported version (EC 5.15.0+)
* @param name Storage object name
* @return `{StorageApiWrapper}` storage object instance

```javascript showLineNumbers
function main() {
    let storage = storages.create("123");
    logd(storage);
}

main();
```

## Store Data

### storage.keys Get All Keys

* Get all keys
* Supported version (EC 5.16.0+)
* @return `{string}` JSON string

```javascript showLineNumbers
function main() {
    let storage = storages.create("123");
    logd(storage);
    // Store data
    let r = storage.putString("key", "sdfasfd");
    logd(r);
    logd(storage.keys());
}

main();
```

### storage.all Get All Keys and Values

* Get all keys and values
* Supported version (EC 5.16.0+)
* @return `{string}` JSON string

```javascript showLineNumbers
function main() {
    let storage = storages.create("123");
    logd(storage);
    // Store data
    let r = storage.putString("key", "sdfasfd");
    logd(r);
    logd(storage.all());
}

main();
```

### storage.putString Store String

* Store a string
* Supported version (EC 5.15.0+)
* @param key Key
* @param value String value
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    let storage = storages.create("123");
    logd(storage);
    // Store data
    let r = storage.putString("key", "sdfasfd");
    logd(r);
    // Get data; temporary bug workaround: append +""
    logd(storage.getString("key", "") + "");
}

main();
```

### storage.putInt Store Integer

* Store an integer
* Supported version (EC 5.15.0+)
* @param key Key
* @param value Integer value
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    let storage = storages.create("123");
    logd(storage);
    // Store data
    let r = storage.putInt("key", 1);
    logd(r);
    // Get data
    logd(storage.getInt("key", 0));
}

main();
```

### storage.putBoolean Store Boolean

* Store a boolean
* Supported version (EC 5.15.0+)
* @param key Key
* @param value Boolean value
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    let storage = storages.create("123");
    logd(storage);
    // Store data
    let r = storage.putBoolean("key", true);
    logd(r);
    // Get data
    logd(storage.getBoolean("key", false));
}

main();
```

### storage.putFloat Store Float

* Store a float
* Supported version (EC 5.15.0+)
* @param key Key
* @param value Float value
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    let storage = storages.create("123");
    logd(storage);
    // Store data
    let r = storage.putFloat("key", 1.0);
    logd(r);
    // Get data
    logd(storage.getFloat("key", 0));
}

main();
```

### storage.putEncrypt Store Encrypted String

* Store and encrypt a string
* Supported version (EC 5.15.0+)
* @param key Key
* @param value String value
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    let storage = storages.create("123");
    logd(storage);
    // Store data
    let r = storage.putEncrypt("key", "3232");
    logd(r);
    // Get data
    logd(storage.getDecryptString("key") + "");
}

main();
```

## Get Data

### storage.getString Get String

* Get a string value
* Supported version (EC 5.15.0+)
* @param key Key
* @return `{string}` string

```javascript showLineNumbers
function main() {
    let storage = storages.create("123");
    logd(storage);
    // Store data
    let r = storage.putString("key", "sdfasfd");
    logd(r);
    // Get data; temporary bug workaround: append +""
    logd(storage.getString("key", "") + "");
}

main();
```

### storage.getInt Get Integer

* Get an integer value
* Supported version (EC 5.15.0+)
* @param key Key
* @return `{string}` integer

```javascript showLineNumbers
function main() {
    let storage = storages.create("123");
    logd(storage);
    // Store data
    let r = storage.putInt("key", 1);
    logd(r);
    // Get data
    logd(storage.getInt("key", 0));
}

main();
```

### storage.getBoolean Get Boolean

* Get a boolean value
* Supported version (EC 5.15.0+)
* @param key Key
* @return `{string}` boolean

```javascript showLineNumbers
function main() {
    let storage = storages.create("123");
    logd(storage);
    // Store data
    let r = storage.putBoolean("key", true);
    logd(r);
    // Get data
    logd(storage.getBoolean("key", false));
}

main();
```

### storage.getFloat Get Float

* Get a float value
* Supported version (EC 5.15.0+)
* @param key Key
* @return `{string}` float

```javascript showLineNumbers
function main() {
 let storage = storages.create("123");
 logd(storage);
 // Store data
 let r = storage.putFloat("key", 1.0);
 logd(r);
 // Get data
 logd(storage.getFloat("key", 0));
}

main();
```

### storage.getDecryptString Get Decrypted String

* Get a decrypted string value
* Supported version (EC 5.15.0+)
* @param key Key
* @return `{string}` decrypted string

```javascript showLineNumbers
function main() {
 let storage = storages.create("123");
 logd(storage);
 // Store data
 let r = storage.putEncrypt("key", "3232");
 logd(r);
 // Get data
 logd(storage.getDecryptString("key") + "");
}

main();
```

## Clear and Other Operations

### storage.clear Clear Storage

* Clear all stored data
* Supported version (EC 5.15.0+)
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 let storage = storages.create("123");
 logd(storage);
 // Store data
 let r = storage.putEncrypt("key", "3232");
 logd(r);
 // Get data
 logd(storage.getDecryptString("key") + "");
 logd(storage.clear());
 logd(storage.getDecryptString("key") + "");
}

main();
```

### storage.remove Remove Key

* Remove the value for a key
* Supported version (EC 5.15.0+)
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
 let storage = storages.create("123");
 logd(storage);
 // Store data
 let r = storage.putEncrypt("key", "3232");
 logd(r);
 // Get data
 logd(storage.getDecryptString("key")) + "";
 logd(storage.remove("key"));
 logd(storage.getDecryptString("key") + "");
}

main();
```

### storage.contains Check Key

* Check whether a key exists
* Supported version (EC 5.15.0+)
* @return `{bool}` true if the key exists, false otherwise

```javascript showLineNumbers
function main() {
 let storage = storages.create("123");
 logd(storage);
 // Store data
 let r = storage.putEncrypt("key", "3232");
 logd(r);
 // Get data
 logd(storage.getDecryptString("key") + "");
 logd(storage.contains("key"));
 logd(storage.getDecryptString("key") + "");
}

main();
```
