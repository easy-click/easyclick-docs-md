---
title: Network Functions
description: EasyClick automation scripts — Android no root network functions
keywords:
 - EasyClick automation scripts Android no root network functions
 - http
 - br
 - GET
 - POST
 - JSON
 - HTTP
 - 'true'
 - 'false'
 - request
 - downloadFile
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview

- Network module functions handle HTTP requests
- The network module uses the `http` prefix, e.g. `http.downloadFile()`

## http.request Universal Request Function

* Universal HTTP request
* @param param map parameter with the following fields:<br/>
 * url: string, request URL<br/>
 * timeout: integer milliseconds, timeout duration<br/>
 * method: string — POST, GET, or PUT<br/>
 * proxy: proxy address as a map with host and port, e.g. ```{"host":"11","port":111}```
    * followRedirects: whether to follow redirects — true or false<br/>
    * requestBody: request body; use a JSON string for JSON payloads<br/>
    * userAgent: string, HTTP User-Agent<br/>
    * ignoreContentType: whether to ignore content type — true or false<br/>
    * ignoreHttpErrors: whether to ignore HTTP errors — true or false<br/>
    * maxBodySize: integer, maximum HTTP body size<br/>
    * referrer: string, request referrer<br/>
    * header: HTTP request headers as a map, e.g. ```{"UA":"test"}```
    * cookie: HTTP request cookies as a map, e.g. ```{"a":1}```
    * data: HTTP POST data as a map, e.g. ```{"a":1}```
    * file: files to upload as a collection, e.g.
    * ```[{"key":"a1","fileName":"a.txt","filePath":"/sdcard/"},{"key":"a1","fileName":"a.jpg","filePath":"/sdcard/","contentType":"image/jpg"}]```
    - contentType is optional
        * responseCharset: string, force response content charset
* @return Response1 object or null
:::tip
- If the request body is a plain string, put it in `requestBody`
  :::
```javascript showLineNumbers
function main() {
 http_request();
}

function http_request() {
 //url:string
 //timeout:int ms
 //method: post ,get
 //proxy: {"host":"11","port":111}
 //followRedirects:true false
 //requestBody: string
 //userAgent:string
 //ignoreContentType:true false
 //ignoreHttpErrors:true false
 //maxBodySize : int
 //referrer:string
 //header:{"UA":"test"}
 //cookie:{"a":1}
 //data:{"a":1}
 //file:[{}]
 //responseCharset: string
 var md = utils.dataMd5("12345");
 var md2 = utils.fileMd5("/sdcard/sb.png");
 var url = "http://192.168.0.5:8081/api/request";
 var proxy = {"host": "192.168.0.5", "port": "100"};
 var userAgent = "xxx";
 var followRedirects = false;
 var requestBody = JSON.stringify({"A": 111});
 var ignoreContentType = true;
 var ignoreHttpErrors = true;
 var referrer = "xxx";
 var header = {
 "Content-Type": " application/json; charset=UTF-8",
 "User-Agent": "from test",
 "ddd": md,
 "dd2": md2,
 "imei": device.getIMEI()
 };
 var cookie = {
 "cookie1": "tst1",
 "cookie2": "tst2"
 };
 var data = {
 "a1": "aaa",
 "pwd2": md,
 "md2": md2
 };
 var file = [
 {
 "key": "file",
 "fileName": "f.png",
 "filePath": "/sdcard/sb.png"
 },
 {
 "key": "file",
 "fileName": "f2.png",
 "filePath": "/sdcard/sde.png",
 "contentType": "image/png"
 }
 ];
 var params = {
 "url": url,
 "method": "POST",
 "userAgent": userAgent,
 "referrer": "baidu.com",
 "cookie": cookie,
 "data": data,
 "file": file
 };
 var x = http.request(params);
 if (x) {
 logd("header=> " + JSON.stringify(x.header));
 logd("cookie=> " + JSON.stringify(x.cookie));
 logd("statusCode=" + x.statusCode);
 logd("statusMessage=" + x.statusMessage);
 logd("charset=" + x.charset);
 logd("contentType=" + x.contentType);
 logd("body=" + x.body);
 } else {
 loge("No result");
 }
}

main();
```

## http.requestEx Universal Request Function (Extended)

* Universal HTTP request
* @param param map parameter with the following fields:<br/>
    * url: string, request URL<br/>
    * timeout: integer milliseconds, timeout duration<br/>
    * method: string — POST, GET, or PUT<br/>
    * proxy: proxy address as a map with host and port, e.g. ```{"host":"11","port":111}```
    * followRedirects: whether to follow redirects — true or false<br/>
    * requestBody: request body; use a JSON string for JSON payloads<br/>
    * userAgent: string, HTTP User-Agent<br/>
    * ignoreContentType: whether to ignore content type — true or false<br/>
    * ignoreHttpErrors: whether to ignore HTTP errors — true or false<br/>
    * maxBodySize: integer, maximum HTTP body size<br/>
    * referrer: string, request referrer<br/>
    * header: HTTP request headers as a map, e.g. ```{"UA":"test"}```
    * cookie: HTTP request cookies as a map, e.g. ```{"a":1}```
    * data: HTTP POST data as a map, e.g. ```{"a":1}```
    * file: files to upload as a collection, e.g.
    * ```[{"key":"a1","fileName":"a.txt","filePath":"/sdcard/"},{"key":"a1","fileName":"a.jpg","filePath":"/sdcard/","contentType":"image/jpg"}]```
    - contentType is optional
        * responseCharset: string, force response content charset
* @return Response1 object or null
:::tip
- If the request body is a plain string, put it in `requestBody`
  :::
```javascript showLineNumbers
function main() {
 http_request();
}

function http_request() {
 //url:string
 //timeout:int ms
 //method: post ,get
 //proxy: {"host":"11","port":111}
 //followRedirects:true false
 //requestBody: string
 //userAgent:string
 //ignoreContentType:true false
 //ignoreHttpErrors:true false
 //maxBodySize : int
 //referrer:string
 //header:{"UA":"test"}
 //cookie:{"a":1}
 //data:{"a":1}
 //file:[{}]
 //responseCharset: string
 var md = utils.dataMd5("12345");
 var md2 = utils.fileMd5("/sdcard/sb.png");
 var url = "http://192.168.0.5:8081/api/request";
 var proxy = {"host": "192.168.0.5", "port": "100"};
 var userAgent = "xxx";
 var followRedirects = false;
 var requestBody = JSON.stringify({"A": 111});
 var ignoreContentType = true;
 var ignoreHttpErrors = true;
 var referrer = "xxx";
 var header = {
 "Content-Type": " application/json; charset=UTF-8",
 "User-Agent": "from test",
 "ddd": md,
 "dd2": md2,
 "imei": device.getIMEI()
 };
 var cookie = {
 "cookie1": "tst1",
 "cookie2": "tst2"
 };
 var data = {
 "a1": "aaa",
 "pwd2": md,
 "md2": md2
 };
 var file = [
 {
 "key": "file",
 "fileName": "f.png",
 "filePath": "/sdcard/sb.png"
 },
 {
 "key": "file",
 "fileName": "f2.png",
 "filePath": "/sdcard/sde.png",
 "contentType": "image/png"
 }
 ];
 var params = {
 "url": url,
 "method": "POST",
 "userAgent": userAgent,
 "referrer": "baidu.com",
 "cookie": cookie,
 "data": data,
 "file": file
 };
 var x = http.requestEx(params);
 if (x) {
 logd("header=" + x.header);
 // Direct access
 logd("header=" + x.header["Location"]);
 for (var d in x.header) {
 logd("header key " + d + " " + x.header[d]);
 }
 logd("cookie=" + x.cookie);
 for (var d in x.cookie) {
 logd("cookie key " + d + " " + x.cookie[d]);
 }
 logd("cookie=" + x.cookie["aa"]);
 logd("statusCode=" + x.statusCode);
 logd("statusMessage=" + x.statusMessage);
 logd("charset=" + x.charset);
 logd("contentType=" + x.contentType);
 logd("body=" + x.body);
 } else {
 loge("No result");
 }
}

main();
```

## http.downloadFile Download File

* Download a remote file locally; supports resumable downloads
* @param remoteUrl remote file URL
* @param file local file path to save to
* @param timeout download timeout in milliseconds
* @param headers header map, e.g.```{"a":"11"}```
* @return true on success, false on failure

```javascript showLineNumbers
function main() {
 var url = "https://imtt.dd.qq.com/16891/apk/DF4FD15AF9A9B51BA74D2710CF738EEF.apk?fsname=com.ishugui_3.9.2.3068_3923068.apk&csr=1bbd";
 var x = http.downloadFile(url, "/sdcard/ss.apk", 10 * 1000, {"User-Agent": "test"});
 toast("download result- " + x);
}

main();
```

## http.downloadFileDefault Download File

* Download a remote file locally with resumable downloads; default timeout is 30 seconds
* @param remoteUrl remote file URL
* @param file local file path to save to
* @param headers header map, e.g.```{"a":"11"}```
* @return true on success, false on failure

```javascript showLineNumbers
function main() {
 var url = "https://imtt.dd.qq.com/16891/apk/DF4FD15AF9A9B51BA74D2710CF738EEF.apk?fsname=com.ishugui_3.9.2.3068_3923068.apk&csr=1bbd";
 var x = http.downloadFileDefault(url, "/sdcard/ss.apk", {"User-Agent": "test"});
 toast("download result- " + x);
}

main();
```

## http.httpGetDefault GET Request

* HTTP GET request
* @param url request URL
* @param timeout timeout in milliseconds
* @param headers header map, e.g.```{"a":"11"}```
* @return string response body

```javascript showLineNumbers
function main() {
 var url = "http://192.168.0.5:8081/api/httpGet?a=1";
 var x = http.httpGetDefault(url, 10 * 1000, {"User-Agent": "test"});
 toast(" result- " + x);
 loge("result - " + x);
}

main();
```

## http.httpGet GET Request

* HTTP GET request
* @param url request URL
* @param params parameter map, e.g. ```{"a":"1"}``` or a plain string
* @param timeout timeout in milliseconds
* @param headers header map, e.g.```{"a":"1"}```
* @return string response body

```javascript showLineNumbers
function main() {
 var url = "http://192.168.0.5:8081/api/httpGet?a=1";
 var pa = {"b": "22"};
 var x = http.httpGet(url, pa, 10 * 1000, {"User-Agent": "test"});
 toast(" result- " + x);
 loge("result - " + x);
}

main();
```

## http.httpPost POST Request

* HTTP POST request
* @param url request URL
* @param params parameters, e.g. ```{"a":"1"}``` or a plain string
* @param files files to upload, e.g. ```{"file1":"/sdcard/1.png"}```
* @param timeout timeout in milliseconds
* @param headers header map, e.g. ```{"a":"1"}```
* @return string response body

```javascript showLineNumbers
function main() {
 // Request without files
 var url = "http://192.168.0.5:8081/api/httpPost";
 var pa = {"b": "我是b的值"};
 var x = http.httpPost(url, pa, null, 10 * 1000, {"User-Agent": "test"});
 toast(" result- " + x);
 loge("result - " + x);
}

main();
```

```javascript showLineNumbers
function main() {
 // Request with file upload
 var url = "http://192.168.0.5:8081/api/httpPost";
 var pa = {"b": "我是b的值"};
 var files = {"file1": "/sdcard/p.json", "file2": "/sdcard/z.xml"};
 var x = http.httpPost(url, pa, files, 10 * 1000, {"User-Agent": "test"});
 toast(" result- " + x);
 loge("result - " + x);
}

main();
```

## http.httpPostEx POST Request

* HTTP POST request; compatible with E2EE server file uploads
* Supported EC 9.6.0+
* @param url request URL
* @param params parameters, e.g. ```{"a":"1"}``` or a plain string
* @param files files to upload, e.g. ```{"file1":"/sdcard/1.png"}```
* @param timeout timeout in milliseconds
* @param headers header map, e.g.```{"a":"1"}```
* @return string response body

```javascript showLineNumbers
function main() {
 // Request without files
 var url = "http://192.168.0.5:8081/api/httpPost";
 var pa = {"b": "我是b的值"};
 var x = http.httpPostEx(url, pa, null, 10 * 1000, {"User-Agent": "test"});
 toast(" result- " + x);
 loge("result - " + x);
}

main();
```

```javascript showLineNumbers
function main() {
 // Request with file upload
 var url = "http://192.168.0.5:8081/api/httpPost";
 var pa = {"b": "我是b的值"};
 var files = {"file1": "/sdcard/p.json", "file2": "/sdcard/z.xml"};
 var x = http.httpPostEx(url, pa, files, 10 * 1000, {"User-Agent": "test"});
 toast(" result- " + x);
 loge("result - " + x);
}

main();
```

## http.postJSON Send JSON

* HTTP POST JSON data
* @param url request URL
* @param json JSON data
* @param timeout timeout in milliseconds
* @param headers header map, e.g.```{"a":"1"}```
* @return string response body

```javascript showLineNumbers
function main() {
 var url = "http://192.168.0.5:8081/api/postJSON";
 var pa = {"b": "我是b的值"};
 var x = http.postJSON(url, pa, 10 * 1000, {"User-Agent": "test"});
 toast(" result- " + x);
 loge("result - " + x);
}

main();
```

## http.newWebsocket WebSocket Communication

* Create a WebSocket connection
* @param url WebSocket URL to connect to
* @param header header map
* @param type library type — 1 = okhttp3, 2 = javawebsocket
* @return ```{@link WebSocket1}``` WebSocket1 object

```javascript showLineNumbers
function main() {
 let result = [];
 // Create a new WebSocket connection
 var ws = http.newWebsocket("ws://192.168.1.76:8081/websocket/device/call?deviceNo=111", null, 2);
 // Connection parameters when type=1
 ws.setCallTimeout(5);
 ws.setReadTimeout(5);
 ws.setWriteTimeout(5);
 // Heartbeat: recommended 0 because some servers do not support ping
 ws.setPingInterval(0)

 // Heartbeat timeout when type=2; can be set to 0
 ws.setConnectionLostTimeout(5)
 // Set listener for connection open
 ws.onOpen(function (ws1, code, msg) {
 logi("onOpen code " + code + " msg:" + msg);
 })
 // Set text message listener
 ws.onText(function (ws1, text) {
 logi(" onText " + text);
 })
 // Set close listener
 ws.onClose(function (ws1, code, reason) {
 logi(" onClose " + code + " reason : " + reason + " remote:");
 })
 ws.onError(function (ws1, msg) {
 logi(" onError " + msg);
 result[0] = "error";
 })
 // bytes is a Java byte array object
 ws.onBinary(function (ws1, bytes) {
 // Convert to Java string
 logi(" onBinary " + new java.lang.String(bytes));
 })

 // Send text heartbeat every 3000 ms
 ws.startHeartBeat(function () {
 return null;
 }, function () {
 return new Date().toISOString();
 }, 3000, true);


 ws.startHeartBeat(function () {
 return new java.lang.String("testXXX").getBytes();
 }, function () {
 return null;
 }, 3000, true);

 // Stop sending heartbeat
 //ws.stopHeartBeat()

 // Start connection (blocking)
 let r = ws.connect(10000);
 // Enable auto reconnect
 ws.setAutoReconnect(true);
 logd("connect {} rr = {}", result[0], r);

 while (true) {
 logd("isconnect " + ws.isConnected());
 sleep(1000)
 if (ws.isConnected()) {
 b = ws.sendText("new Date-" + new Date())
 logd("send =" + b);
 sleep(1000)
 // Convert Java string to bytes
 ws.sendBinary(new java.lang.String("test").getBytes());
 } else {
 // Reset connection
 // let reset = ws.reset();
 // logd("reset {}",reset)
 // if (reset) {
 // logd("Reconnecting...");
 // let rc = ws.connect(10000);
 // logd("Reconnect--"+rc);
 // }
 }
 }
 logd("isClosed " + ws.isClosed())
 sleep(1000)
 // Close connection
 ws.close();
}

main();
```

### WebSocket Object Methods

#### connect Synchronous Connection

* Start synchronous connection
* See [example](/docs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### startHeartBeat Send Heartbeat

* Call once; internally sends at the configured interval
* @param heartDataBinCallback binary heartbeat data; must return a byte[] array
* @param heartDataStrCallback text heartbeat data; return a string directly
* @param period heartbeat interval in milliseconds
* @param cancelOld whether to cancel the previous heartbeat — true or false
* See [example](/docs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### stopHeartBeat Stop Heartbeat

* Stop sending heartbeat
* See [example]

#### reset Reset Connection

* Reset connection
* @return `{bool}` true on success, false on failure
* See [example](/docs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### isClosed Check Whether Closed

* Check whether the connection is closed
* @return true if closed, false if not closed
* See [example](/docs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### isConnected Check Whether Connected

* Check whether the connection is established
* @return true if connected, false if not connected
* See [example](/docs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### close Close Connection

* Close the connection
* See [example](/docs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### sendText Send Text

* Send a text message
* @param text text message
* See [example](/docs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### sendBinary Send Binary

* Send binary data
* @param bin Java byte array object
* See [example](/docs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### onOpen Open Callback

* Callback when the connection opens
* @param callback callback function
* See [example](/docs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### onText Text Callback

* Callback when a text message is received
* @param callback callback function
* See [example](/docs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### onClose Close Callback

* Callback when the connection closes
* @param callback callback function
* See [example](/docs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### onError Error Callback

* Callback when an error occurs
* @param callback callback function
* See [example](/docs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### onBinary Binary Message Callback

* Callback when binary data is received
* @param callback callback function
* See [example](/docs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### setReadTimeout Set Read Timeout

* Supported EC 6.17.0+
* Use when WebSocket library type = 1
* Set read timeout
* @param timeout timeout in seconds
* See [example](/docs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### setWriteTimeout Set Write Timeout

* Supported EC 6.17.0+
* Use when WebSocket library type = 1
* Set write timeout
* @param timeout timeout in seconds
* See [example](/docs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### setPingInterval Set Ping Interval

* Supported EC 6.17.0+
* Use when WebSocket library type = 1
* Set heartbeat timeout
* @param timeout timeout in seconds
* See [example](/docs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### setConnectionLostTimeout Set Connection Lost Timeout

* Supported EC 6.17.0+
* Use when WebSocket library type = 2
* Set heartbeat timeout
* @param timeout timeout in seconds
* See [example](/docs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### setAutoReconnect Enable Auto Reconnect

* Supported EC 6.17.0+
* Enable auto reconnect
* See [example](/docs/funcs/http-api#httpnewwebsocket-websocket-communication)

