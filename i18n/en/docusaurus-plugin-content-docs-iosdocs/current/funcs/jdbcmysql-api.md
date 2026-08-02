---
title: JDBC MySQL Functions
description: EasyClick automation scripts — iOS no jailbreak JDBC MySQL functions
keywords:
 - EasyClick automation scripts iOS no jailbreak JDBC MySQL functions
 - jdbc
 - JDBC
 - param
 - init
 - return
 - getLastError
 - connect
 - query
 - mysql
 - 'true'
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview

- The JDBC module is used to operate MySQL, Oracle, and other databases
- The JDBC module uses the `jdbc` prefix, e.g. `jdbc.init()`

## jdbc.init Initialize JDBC Connection

* Initialize JDBC connection
* @param jdbcDriver JDBC driver; for MySQL use: com.mysql.jdbc.Driver
* @param dbUrl JDBC connection URL, e.g.
 `jdbc:mysql://{ip}:{port}/{db}?characterEncoding=utf8&useSSL=false&serverTimezone=UTC&rewriteBatchedStatements=true`
* @param user database username
* @param password database password
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    // MySQL address ip:port/database name
    let mysqlUrl = "jdbc:mysql://192.168.0.3:3306/test?characterEncoding=utf8&autoReconnect=true"
    let inited = jdbc.init("com.mysql.cj.jdbc.Driver", mysqlUrl, "root", "root123456");
    logd("inited " + inited);
    let conn = jdbc.connect()
    logd("connect " + conn);
    if (!conn) {
        logd(jdbc.getLastError());
        exit()
    }
    // Query statement
    let q = "Select * from table1 where id=1"
    let qur = jdbc.query(q)
    logd(qur);
    jdbc.connectionClose()
}

main();
 ```

## jdbc.getLastError Get Last Error

* Get the most recent error
* @return `{string}` error string; null means no error

```javascript showLineNumbers
function main() {
    // MySQL address ip:port/database name
    let mysqlUrl = "jdbc:mysql://192.168.0.3:3306/test?characterEncoding=utf8&autoReconnect=true"
    let inited = jdbc.init("com.mysql.cj.jdbc.Driver", mysqlUrl, "root", "root123456");
    logd("inited " + inited);
    let conn = jdbc.connect()
    logd("connect " + conn);
    if (!conn) {
        logd(jdbc.getLastError());
        exit()
    }
    // Query statement
    let q = "Select * from table1 where id=1"
    let qur = jdbc.query(q)
    logd(qur);
    jdbc.connectionClose()
}

main();
```

## jdbc.connect Connect to Database

* Connect to the database; call after `init()`
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    // MySQL address ip:port/database name
    let mysqlUrl = "jdbc:mysql://192.168.0.3:3306/test?characterEncoding=utf8&autoReconnect=true"
    let inited = jdbc.init("com.mysql.cj.jdbc.Driver", mysqlUrl, "root", "root123456");
    logd("inited " + inited);
    // Connect to database
    let conn = jdbc.connect()
    logd("connect " + conn);
    if (!conn) {
        logd(jdbc.getLastError());
        exit()
    }
    // Query statement
    let q = "Select * from table1 where id=1"
    let qur = jdbc.query(q)
    logd(qur);
    jdbc.connectionClose()
}

main();
```

## jdbc.query Query Data

* Query data
* @param sql SQL statement
* @return JSON | data collection object

```javascript showLineNumbers
function main() {
    // MySQL address ip:port/database name
    let mysqlUrl = "jdbc:mysql://192.168.0.3:3306/test?characterEncoding=utf8&autoReconnect=true"
    let inited = jdbc.init("com.mysql.cj.jdbc.Driver", mysqlUrl, "root", "root123456");
    logd("inited " + inited);
    let conn = jdbc.connect()
    logd("connect " + conn);
    if (!conn) {
        logd(jdbc.getLastError());
        exit()
    }
    // Query statement
    let q = "select * from table1 where id=1"
    let qur = jdbc.query(q)
    logd(qur);
    jdbc.connectionClose()
}

main();
```

## jdbc.createPreparedStatement Create Prepared Statement

* Create a prepared SQL statement
* @param sql prepared statement SQL
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    // MySQL address ip:port/database name
    let mysqlUrl = "jdbc:mysql://192.168.0.3:3306/test?characterEncoding=utf8&autoReconnect=true"
    let inited = jdbc.init("com.mysql.cj.jdbc.Driver", mysqlUrl, "root", "root123456");
    logd("inited " + inited);

    let conn = jdbc.connect()
    logd("connect " + conn);
    if (!conn) {
        logd(jdbc.getLastError());
        exit()
    }

    // Query statement
    let q = "select * from table1 where id=? or uname=?"
    // Create a query
    let qur = jdbc.createPreparedStatement(q)
    if (qur) {
        // Set the first indexed parameter
        jdbc.psqlSetInt(1, 1)
        // Set the second indexed parameter
        jdbc.psqlSetString(2, 'test')

    }
    // Prepared statement query
    let data = jdbc.psqlQuery()
    logd(data);

    // Close prepared statement
    jdbc.psqlClose()

    // Insert data
    q = "insert table1(`uname`,`ucontent`,`create_time`)values(?,?,?);"
    qur = jdbc.createPreparedStatement(q)
    if (qur) {
        // Set the first indexed parameter
        jdbc.psqlSetString(1, "我是名称")
        // Set the second indexed parameter
        jdbc.psqlSetString(2, '我是内容')
        // Set timestamp
        jdbc.psqlSetTimestamp(3, "yyyy-MM-dd hh:mm:ss", "2020-10-02 12:02:11")
    }
    rowcount = jdbc.psqlExecuteUpdate();
    logi("Insert affected row count -" + rowcount);
    if (rowcount <= 0) {
        loge("Insert error: " + jdbc.getLastError())
    }
    jdbc.connectionClose()
}

main();
```

## jdbc.psqlQuery Execute Prepared Statement

* Execute the previously created prepared statement
* @return `{string}` JSON string

```javascript showLineNumbers
function main() {
    // MySQL address ip:port/database name
    let mysqlUrl = "jdbc:mysql://192.168.0.3:3306/test?characterEncoding=utf8&autoReconnect=true"
    let inited = jdbc.init("com.mysql.cj.jdbc.Driver", mysqlUrl, "root", "root123456");
    logd("inited " + inited);

    let conn = jdbc.connect()
    logd("connect " + conn);
    if (!conn) {
        logd(jdbc.getLastError());
        exit()
    }

    // Query statement
    let q = "select * from table1 where id=? or uname=?"
    // Create a query
    let qur = jdbc.createPreparedStatement(q)
    if (qur) {
        // Set the first indexed parameter
        jdbc.psqlSetInt(1, 1)
        // Set the second indexed parameter
        jdbc.psqlSetString(2, 'test')

    }
    // Prepared statement query
    let data = jdbc.psqlQuery()
    logd(data);

    // Close prepared statement
    jdbc.psqlClose()
    // Insert data
    q = "insert table1(`uname`,`ucontent`,`create_time`)values(?,?,?);"
    qur = jdbc.createPreparedStatement(q)
    if (qur) {
        // Set the first indexed parameter
        jdbc.psqlSetString(1, "我是名称")
        // Set the second indexed parameter
        jdbc.psqlSetString(2, '我是内容')
        // Set timestamp
        jdbc.psqlSetTimestamp(3, "yyyy-MM-dd hh:mm:ss", "2020-10-02 12:02:11")
    }
    rowcount = jdbc.psqlExecuteUpdate();
    logi("Insert affected row count -" + rowcount);
    if (rowcount <= 0) {
        loge("Insert error: " + jdbc.getLastError())
    }
    jdbc.connectionClose()
}

main();
```

## jdbc.psqlSetString Set String Parameter

* Set a string parameter on the prepared statement
* @param index parameter index
* @param input string value
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    // MySQL address ip:port/database name
    let mysqlUrl = "jdbc:mysql://192.168.0.3:3306/test?characterEncoding=utf8&autoReconnect=true"
    let inited = jdbc.init("com.mysql.cj.jdbc.Driver", mysqlUrl, "root", "root123456");
    logd("inited " + inited);

    let conn = jdbc.connect()
    logd("connect " + conn);
    if (!conn) {
        logd(jdbc.getLastError());
        exit()
    }

    // Query statement
    let q = "select * from table1 where id=? or uname=?"
    // Create a query
    let qur = jdbc.createPreparedStatement(q)
    if (qur) {
        // Set the first indexed parameter
        jdbc.psqlSetInt(1, 1)
        // Set the second indexed parameter
        jdbc.psqlSetString(2, 'test')

    }
    // Prepared statement query
    let data = jdbc.psqlQuery()
    logd(data);

    // Close prepared statement
    jdbc.psqlClose()

    // Insert data
    q = "insert table1(`uname`,`ucontent`,`create_time`)values(?,?,?);"
    qur = jdbc.createPreparedStatement(q)
    if (qur) {
        // Set the first indexed parameter
        jdbc.psqlSetString(1, "我是名称")
        // Set the second indexed parameter
        jdbc.psqlSetString(2, '我是内容')
        // Set timestamp
        jdbc.psqlSetTimestamp(3, "yyyy-MM-dd hh:mm:ss", "2020-10-02 12:02:11")
    }
    rowcount = jdbc.psqlExecuteUpdate();
    logi("Insert affected row count -" + rowcount);
    if (rowcount <= 0) {
        loge("Insert error: " + jdbc.getLastError())
    }
    jdbc.connectionClose()
}

main();
```

## jdbc.psqlSetLong Set long Parameter

* Set a long parameter on the prepared statement
* @param index parameter index
* @param input long value
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    jdbc.psqlSetLong(1, 1000);
}

main();
```

## jdbc.psqlSetInt Set int Parameter

* @param index parameter index
* @param input int value
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    // MySQL address ip:port/database name
    let mysqlUrl = "jdbc:mysql://192.168.0.3:3306/test?characterEncoding=utf8&autoReconnect=true"
    let inited = jdbc.init("com.mysql.cj.jdbc.Driver", mysqlUrl, "root", "root123456");
    logd("inited " + inited);

    let conn = jdbc.connect()
    logd("connect " + conn);
    if (!conn) {
        logd(jdbc.getLastError());
        exit()
    }

    // Query statement
    let q = "select * from table1 where id=? or uname=?"
    // Create a query
    let qur = jdbc.createPreparedStatement(q)
    if (qur) {
        // Set the first indexed parameter
        jdbc.psqlSetInt(1, 1)
        // Set the second indexed parameter
        jdbc.psqlSetString(2, 'test')

    }
    // Prepared statement query
    let data = jdbc.psqlQuery()
    logd(data);

    // Close prepared statement
    jdbc.psqlClose()

    // Insert data
    q = "insert table1(`uname`,`ucontent`,`create_time`)values(?,?,?);"
    qur = jdbc.createPreparedStatement(q)
    if (qur) {
        // Set the first indexed parameter
        jdbc.psqlSetString(1, "我是名称")
        // Set the second indexed parameter
        jdbc.psqlSetString(2, '我是内容')
        // Set timestamp
        jdbc.psqlSetTimestamp(3, "yyyy-MM-dd hh:mm:ss", "2020-10-02 12:02:11")
    }
    rowcount = jdbc.psqlExecuteUpdate();
    logi("Insert affected row count -" + rowcount);
    if (rowcount <= 0) {
        loge("Insert error: " + jdbc.getLastError())
    }
    jdbc.connectionClose()
}

main();
```

## jdbc.psqlSetFloat Set float Parameter

* Set a float parameter on the prepared statement
* @param index parameter index
* @param input float value
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    jdbc.psqlSetFloat(1, 1.0)
}

main();
```

## jdbc.psqlSetBoolean Set boolean Parameter

* Set a boolean parameter
* @param index parameter index
* @param input boolean value
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    jdbc.psqlSetBoolean(1, true)
}

main();
```

## jdbc.psqlSetDate Set Date Parameter

* Set a date parameter on the prepared statement
* @param index parameter index
* @param dataFormat date format, e.g. yyyy-MM-dd
* @param input date string
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    // MySQL address ip:port/database name
    let mysqlUrl = "jdbc:mysql://192.168.0.3:3306/test?characterEncoding=utf8&autoReconnect=true"
    let inited = jdbc.init("com.mysql.cj.jdbc.Driver", mysqlUrl, "root", "root123456");
    logd("inited " + inited);

    let conn = jdbc.connect()
    logd("connect " + conn);
    if (!conn) {
        logd(jdbc.getLastError());
        exit()
    }

    // Query statement
    let q = "select * from table1 where id=? or uname=?"
    // Create a query
    let qur = jdbc.createPreparedStatement(q)
    if (qur) {
        // Set the first indexed parameter
        jdbc.psqlSetInt(1, 1)
        // Set the second indexed parameter
        jdbc.psqlSetString(2, 'test')

    }
    // Prepared statement query
    let data = jdbc.psqlQuery()
    logd(data);

    // Close prepared statement
    jdbc.psqlClose()

    // Insert data
    q = "insert table1(`uname`,`ucontent`,`create_time`)values(?,?,?);"
    qur = jdbc.createPreparedStatement(q)
    if (qur) {
        // Set the first indexed parameter
        jdbc.psqlSetString(1, "我是名称")
        // Set the second indexed parameter
        jdbc.psqlSetString(2, '我是内容')
        // Set date
        jdbc.psqlSetDate(3, "yyyy-MM-dd hh:mm:ss", "2020-10-02 12:02:11")
    }
    rowcount = jdbc.psqlExecuteUpdate();
    logi("Insert affected row count -" + rowcount);
    if (rowcount <= 0) {
        loge("Insert error: " + jdbc.getLastError())
    }
    jdbc.connectionClose()
}

main();
```

## jdbc.psqlSetTimestamp Set Timestamp Parameter

* Set a timestamp parameter on the prepared statement
* @param index parameter index
* @param dataFormat date format, e.g. yyyy-MM-dd
* @param input date string
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    // MySQL address ip:port/database name
    let mysqlUrl = "jdbc:mysql://192.168.0.3:3306/test?characterEncoding=utf8&autoReconnect=true"
    let inited = jdbc.init("com.mysql.cj.jdbc.Driver", mysqlUrl, "root", "root123456");
    logd("inited " + inited);

    let conn = jdbc.connect()
    logd("connect " + conn);
    if (!conn) {
        logd(jdbc.getLastError());
        exit()
    }

    // Query statement
    let q = "select * from table1 where id=? or uname=?"
    // Create a query
    let qur = jdbc.createPreparedStatement(q)
    if (qur) {
        // Set the first indexed parameter
        jdbc.psqlSetInt(1, 1)
        // Set the second indexed parameter
        jdbc.psqlSetString(2, 'test')

    }
    // Prepared statement query
    let data = jdbc.psqlQuery()
    logd(data);

    // Close prepared statement
    jdbc.psqlClose()

    // Insert data
    q = "insert table1(`uname`,`ucontent`,`create_time`)values(?,?,?);"
    qur = jdbc.createPreparedStatement(q)
    if (qur) {
        // Set the first indexed parameter
        jdbc.psqlSetString(1, "我是名称")
        // Set the second indexed parameter
        jdbc.psqlSetString(2, '我是内容')
        // Set timestamp
        jdbc.psqlSetTimestamp(3, "yyyy-MM-dd hh:mm:ss", "2020-10-02 12:02:11")
    }
    rowcount = jdbc.psqlExecuteUpdate();
    logi("Insert affected row count -" + rowcount);
    if (rowcount <= 0) {
        loge("Insert error: " + jdbc.getLastError())
    }
    jdbc.psqlClose()
    jdbc.connectionClose()
}

main();
```

## jdbc.psqlAddBatch Batch Submit

* Add current parameters to batch submission
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    // MySQL address ip:port/database name
    let mysqlUrl = "jdbc:mysql://192.168.0.3:3306/test?characterEncoding=utf8&autoReconnect=true"
    let inited = jdbc.init("com.mysql.cj.jdbc.Driver", mysqlUrl, "root", "root123456");
    logd("inited " + inited);

    let conn = jdbc.connect()
    logd("connect " + conn);
    if (!conn) {
        logd(jdbc.getLastError());
        exit()
    }


    // Insert data
    let q = "insert table1(`uname`,`ucontent`,`create_time`)values(?,?,?);"
    let qur = jdbc.createPreparedStatement(q)


    if (qur) {

        for (var i = 0; i < 10; i++) {
            // Set the first indexed parameter
            jdbc.psqlSetString(1, "我是名称")
            // Set the second indexed parameter
            jdbc.psqlSetString(2, '我是内容')
            // Set timestamp
            jdbc.psqlSetTimestamp(3, "yyyy-MM-dd hh:mm:ss", "2020-10-02 12:02:11")
            r = jdbc.psqlAddBatch()
            if (!r) {
                logd(jdbc.getLastError());
            }
        }
    }

    rowcount = jdbc.psqlExecuteUpdate();
    logi("Insert affected row count -" + rowcount);
    if (rowcount <= 0) {
        loge("Insert error: " + jdbc.getLastError())
    }
    jdbc.psqlClose()
    jdbc.connectionClose()

}

main();
```

## jdbc.psqlExecuteUpdate Execute Update

* Execute an update operation
* @return `{int}` number of affected rows

```javascript showLineNumbers
function main() {
    // MySQL address ip:port/database name
    let mysqlUrl = "jdbc:mysql://192.168.0.3:3306/test?characterEncoding=utf8&autoReconnect=true"
    let inited = jdbc.init("com.mysql.cj.jdbc.Driver", mysqlUrl, "root", "root123456");
    logd("inited " + inited);

    let conn = jdbc.connect()
    logd("connect " + conn);
    if (!conn) {
        logd(jdbc.getLastError());
        exit()
    }

    // Update data
    let q = "update table1 set uname=?,ucontent=?,create_time=? where id=?;"
    let qur = jdbc.createPreparedStatement(q)
    if (qur) {
        // Set the first indexed parameter
        jdbc.psqlSetString(1, "我是名称")
        // Set the second indexed parameter
        jdbc.psqlSetString(2, '我是内容')
        // Set timestamp
        jdbc.psqlSetTimestamp(3, "yyyy-MM-dd hh:mm:ss", "2020-10-02 12:02:11")
        // Set id
        jdbc.psqlSetInt(4, 1)
    }
    let rowcount = jdbc.psqlExecuteUpdate();
    logi("Insert affected row count -" + rowcount);
    if (rowcount <= 0) {
        loge("Insert error: " + jdbc.getLastError())
    }
    // Insert data
    q = "insert table1(`uname`,`ucontent`,`create_time`)values(?,?,?);"
    qur = jdbc.createPreparedStatement(q)
    if (qur) {
        // Set the first indexed parameter
        jdbc.psqlSetString(1, "我是名称")
        // Set the second indexed parameter
        jdbc.psqlSetString(2, '我是内容')
        // Set timestamp
        jdbc.psqlSetTimestamp(3, "yyyy-MM-dd hh:mm:ss", "2020-10-02 12:02:11")
    }
    rowcount = jdbc.psqlExecuteUpdate();
    logi("Insert affected row count -" + rowcount);
    if (rowcount <= 0) {
        loge("Insert error: " + jdbc.getLastError())
    }
    jdbc.psqlClose()
    jdbc.connectionClose()
}

main();
```

## jdbc.psqlClose Close Prepared Statement

* Close the prepared statement
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    jdbc.psqlClose()
}

main();
```

## jdbc.connectionClose Close Database Connection

* Close the database connection
* @return `{bool}` true on success, false on failure

```javascript showLineNumbers
function main() {
    // MySQL address ip:port/database name
    let mysqlUrl = "jdbc:mysql://192.168.0.3:3306/test?characterEncoding=utf8&autoReconnect=true"
    let inited = jdbc.init("com.mysql.cj.jdbc.Driver", mysqlUrl, "root", "root123456");
    logd("inited " + inited);

    let conn = jdbc.connect()
    logd("connect " + conn);
    if (!conn) {
        logd(jdbc.getLastError());
        exit()
    }

    // Query statement
    let q = "select * from table1 where id=? or uname=?"
    // Create a query
    let qur = jdbc.createPreparedStatement(q)
    if (qur) {
        // Set the first indexed parameter
        jdbc.psqlSetInt(1, 1)
        // Set the second indexed parameter
        jdbc.psqlSetString(2, 'test')

    }
    // Prepared statement query
    let data = jdbc.psqlQuery()
    logd(data);

    // Close prepared statement
    jdbc.psqlClose()

    // Insert data
    q = "insert table1(`uname`,`ucontent`,`create_time`)values(?,?,?);"
    qur = jdbc.createPreparedStatement(q)
    if (qur) {
        // Set the first indexed parameter
        jdbc.psqlSetString(1, "我是名称")
        // Set the second indexed parameter
        jdbc.psqlSetString(2, '我是内容')
        // Set timestamp
        jdbc.psqlSetTimestamp(3, "yyyy-MM-dd hh:mm:ss", "2020-10-02 12:02:11")
    }
    rowcount = jdbc.psqlExecuteUpdate();
    logi("Insert affected row count -" + rowcount);
    if (rowcount <= 0) {
        loge("Insert error: " + jdbc.getLastError())
    }
    jdbc.psqlClose()
    jdbc.connectionClose()
}

main();
```



