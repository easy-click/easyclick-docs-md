---
title: SQLite Command Functions
description: EasyClick automation scripts — Android no root SQLite command functions
keywords:
 - EasyClick automation scripts Android no root SQLite command functions
 - sqlite
 - param
 - connectOrCreateDb
 - createTable
 - insert
 - close
 - return
 - boolean
 - 'true'
 - 'false'
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview

- The sqlite module functions are used to operate on SQLite databases
- The sqlite module uses the `sqlite` prefix, e.g. `sqlite.close()`
- SQLite tutorial: https://www.runoob.com/sqlite/sqlite-tutorial.html
- **Frequent reads/writes may lock SQLite; add a 50 ms sleep when reading or writing data**

## sqlite.connectOrCreateDb Connect to Database

* Create or connect to a database
* @param dbName Database path/name
* @return boolean true if the operation succeeded, false on failure

```javascript showLineNumbers
function main() {
    var create = sqlite.connectOrCreateDb("/sdcard/test.db");
    logd("create db result: " + create);
}

main();
```

## sqlite.createTable Create Table

* Create a data table
* @param tableName Table name
* @param columns Column names, e.g. `["name","pwd"]`
* @return boolean true if the operation succeeded, false on failure

```javascript showLineNumbers
function main() {
    var tableName = "tbl_user";
    var columns = ["name", "pwd"];
    var createTable = sqlite.createTable(tableName, columns);
    logd("createTable result: " + createTable);
}

main();
```

## sqlite.insert Insert Data

* Insert data
* @param tableName Table name
* @param map Data map
* @return boolean true if the operation succeeded, false on failure

```javascript showLineNumbers
function main() {
    var tableName = "tbl_user";
    var map = {
        "name": "my name",
        "pwd": "my password"
    };
    var insert = sqlite.insert(tableName, map);
    logd("insert result: " + insert);
}

main();
```

## sqlite.update Update Data

* Update data
* @param tablename Table name
* @param map Data map
* @param where WHERE clause
* @return boolean true if the operation succeeded, false on failure

```javascript showLineNumbers
function main() {
    var tableName = "tbl_user";
    var map = {
        "name": "updated name"
    };
    var where = "id>3";
    var update = sqlite.update(tableName, map, where);
    logd("update result: " + update);
}

main();
```

## sqlite.query Query Data

* Query data
* @param sql SQL statement
* @return JSON | data collection object

```javascript showLineNumbers
function main() {
    var tableName = "tbl_user";
    var sql = "select * from " + tableName;
    var data = sqlite.query(sql);
    logd("data result: " + JSON.stringify(data));
}

main();
```

## sqlite.delete Delete Data

* Delete data
* @param sql SQL statement
* @return boolean true if the operation succeeded, false on failure

```javascript showLineNumbers
function main() {
    var tableName = "tbl_user";
    var sql = "delete from " + tableName + " where id>3;";
    var result = sqlite.delete(sql);
    logd("delete result: " + result);
}

main();
```

## sqlite.execSql Execute SQL

* Execute SQL
* @param sql SQL statement
* @return boolean true if the operation succeeded, false on failure

```javascript showLineNumbers
function main() {
    var tableName = "tbl_user";
    var sql = "delete from " + tableName + " where id>3;";
    var result = sqlite.execSql(sql);
    logd("execSql result: " + result);
}

main();
```

## sqlite.dropDatabase Drop Database

* Drop the database
* @return boolean true if the operation succeeded, false on failure

```javascript showLineNumbers
function main() {
    var result = sqlite.dropDatabase();
    logd("dropDatabase result: " + result);
}

main();
```

## sqlite.dropTable Drop Table

* Drop a table @param table Table name
* @return boolean true if the operation succeeded, false on failure

```javascript showLineNumbers
function main() {
    var tableName = "tbl_user";
    var result = sqlite.dropTable(tableName);
    logd("dropTable result: " + result);
}

main();
```

## sqlite.close Close Database Connection

* Close the database connection and release resources
* @return boolean true if the operation succeeded, false on failure

```javascript showLineNumbers
function main() {
    var result = sqlite.close();
    logd("close result: " + result);
}

main();
```

## sqlite.getErrorMsg Get Error Message

* Get the error message from the last SQL execution
* @return `{string}` null means no error message

```javascript showLineNumbers
function main() {
    var tableName = "tbl_user";
    var result = sqlite.dropTable(tableName);
    logd("dropTable result: " + result);
    var result = sqlite.getErrorMsg();
    logd("getErrorMsg result: " + result);
}

main();
```
