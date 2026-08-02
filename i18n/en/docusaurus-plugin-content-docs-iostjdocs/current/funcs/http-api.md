---
title: Network Functions
description: EasyClick automation scripts — iOS no jailbreak — network functions
keywords:
 - EasyClick automation scripts — iOS no jailbreak — network functions
 - http
 - br
 - JSON
 - 'true'
 - 'false'
 - request
 - downloadFile
 - GET
 - POST
 - HTTP
 - EasyClick
 - mobile automation
 - test automation
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---

## Overview

- Network module functions are related to network requests
- The network module uses the `http` prefix, e.g. `http.downloadFile()`
- `ignoreSSLErrors` is optional; when omitted or `false`, behavior matches older versions; when `true`, sites with expired or self-signed HTTPS certificates can be accessed
- `http.request` / `http.requestEx` support `method` values such as `GET`, `POST`, `PUT`, `DELETE`, etc.; for `DELETE`, `data` can be appended to URL query parameters, or a request body can be sent via `requestBody`

## http.request Universal Request Function

* Universal HTTP request
* @param param map parameters, including:<br/>
 * url: string — request URL<br/>
 * timeout: integer milliseconds — timeout<br/>
 * method: HTTP method string such as GET, POST, PUT, DELETE<br/>
 * proxy: proxy address, map with host and port, e.g. ```{"host":"11","port":111}```<br/>
    * followRedirects: whether to follow redirects — `true` or `false`<br/>
    * ignoreSSLErrors: whether to ignore HTTPS certificate errors (expired, self-signed, etc.) — `true` or `false`, default `false`<br/>
    * requestBody: request body; for JSON, pass a JSON string<br/>
    * userAgent: string — HTTP User-Agent<br/>
    * ignoreContentType: whether to ignore content type — `true` or `false`<br/>
    * ignoreHttpErrors: whether to ignore HTTP errors — `true` or `false`<br/>
    * maxBodySize: integer — maximum HTTP body size<br/>
    * referrer: string — request referrer<br/>
    * header: HTTP request headers, map, e.g. ```{"UA":"test"}```
      * For `application/x-www-form-urlencoded` content-type, parameters are separated by `&`; set `requestBody` to query-style data such as `a=1&b=2`, or upgrade to the latest version and use the `data` map parameter
      * If content-type is not set and method is POST or PUT, content-type is `multipart/form-data`
    * cookie: HTTP request cookies, map, e.g. ```{"a":1}```
    * data: HTTP POST data, map, e.g. ```{"a":1}```
    * file: files to upload, e.g.
    * ```{"file1":"a1.png","file2":"a2.png"}```
    * responseCharset: string — force response content charset
* @return Response object or null

:::tip
  - If the request content is a plain string, put it in `requestBody`
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
 //file:{}
 //responseCharset: string
 let md = utils.dataMd5("12345");
 let md2 = utils.fileMd5(file.getSandBoxFilePath("a.txt"));
 let url = "http://192.168.0.5:8081/api/request";
 let proxy = {"host": "192.168.0.5", "port": "100"};
 let userAgent = "xxx";
 let followRedirects = false;
 let requestBody = JSON.stringify({"A": 111});
 let ignoreContentType = true;
 let ignoreHttpErrors = true;
 let referrer = "xxx";
 let header = {
 "Content-Type": " application/json; charset=UTF-8",
 "User-Agent": "from test",
 "ddd": md,
 "dd2": md2,
 "imei": device.getDeviceId()
 };
 let cookie = {
 "cookie1": "tst1",
 "cookie2": "tst2"
 };
 let data = {
 "a1": "aaa",
 "pwd2": md,
 "md2": md2
 };
 let file =
 {
 "file1": "a.png",
 "file2": "f.png"
 }
 let params = {
 "url": url,
 "method": "POST",
 "userAgent": userAgent,
 "referrer": "baidu.com",
 "cookie": cookie,
 "data": data,
 "file": file
 };
 let x = http.request(params);
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
* @param param map parameters, including:<br/>
    * url: string — request URL<br/>
    * timeout: integer milliseconds — timeout<br/>
    * method: HTTP method string such as GET, POST, PUT, DELETE<br/>
    * proxy: proxy address, map with host and port, e.g. ```{"host":"11","port":111}```<br/>
    * followRedirects: whether to follow redirects — `true` or `false`<br/>
    * ignoreSSLErrors: whether to ignore HTTPS certificate errors (expired, self-signed, etc.) — `true` or `false`, default `false`<br/>
    * requestBody: request body; for JSON, pass a JSON string<br/>
    * userAgent: string — HTTP User-Agent<br/>
    * ignoreContentType: whether to ignore content type — `true` or `false`<br/>
    * ignoreHttpErrors: whether to ignore HTTP errors — `true` or `false`<br/>
    * maxBodySize: integer — maximum HTTP body size<br/>
    * referrer: string — request referrer<br/>
    * header: HTTP request headers, map, e.g. ```{"UA":"test"}```
    * cookie: HTTP request cookies, map, e.g. ```{"a":1}```
    * data: HTTP POST data, map, e.g. ```{"a":1}```
    * file: files to upload, e.g.
    * ```{"file1":"a1.png","file2":"a2.png"}```
    * responseCharset: string — force response content charset
* @return Response object or null
  :::tip
- If the request content is a plain string, put it in `requestBody`
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
 //file:{}
 //responseCharset: string
 let md = utils.dataMd5("12345");
 let md2 = utils.fileMd5(file.getSandBoxFilePath("a.txt"));
 let url = "http://192.168.0.5:8081/api/request";
 let proxy = {"host": "192.168.0.5", "port": "100"};
 let userAgent = "xxx";
 let followRedirects = false;
 let requestBody = JSON.stringify({"A": 111});
 let ignoreContentType = true;
 let ignoreHttpErrors = true;
 let referrer = "xxx";
 let header = {
 "Content-Type": " application/json; charset=UTF-8",
 "User-Agent": "from test",
 "ddd": md,
 "dd2": md2,
 "imei": device.getDeviceId()
 };
 let cookie = {
 "cookie1": "tst1",
 "cookie2": "tst2"
 };
 let data = {
 "a1": "aaa",
 "pwd2": md,
 "md2": md2
 };
 let file = {
 "file1": "a.png",
 "file2": "b.png"
 }
 let params = {
 "url": url,
 "method": "POST",
 "userAgent": userAgent,
 "referrer": "baidu.com",
 "cookie": cookie,
 "data": data,
 "file": file
 };
 let x = http.requestEx(params);
 if (x) {
 logd("header=" + x.header);
 // Direct value access
 logd("header=" + x.header["Location"]);
 for (let d in x.header) {
 logd("header key " + d + " " + x.header[d]);
 }
 logd("cookie=" + x.cookie);
 for (let d in x.cookie) {
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

* Download a remote file locally; resume is not supported
* @param remoteUrl remote file URL
* @param file local file path to save to
* @param timeout download timeout in milliseconds
* @param headers — header map, e.g. ```{"a":1}```
* @param ignoreSSLErrors whether to ignore HTTPS certificate errors (expired, self-signed, etc.); optional, default `false`
* @return `true` on success, `false` on failure

```javascript showLineNumbers
function main() {
 let url = "https://imtt.dd.qq.com/16891/apk/DF4FD15AF9A9B51BA74D2710CF738EEF.apk?fsname=com.ishugui_3.9.2.3068_3923068.apk&csr=1bbd";
 let x = http.downloadFile(url, file.getSandBoxFilePath("a.apk"), 10 * 1000, {"User-Agent": "test"});
 logd("download result- " + x);
}

main();
```

## http.downloadFile2 Resumable Download

* Download a remote file locally with resume support
* Available in EC iOS standalone 3.2.0+
* @param remoteUrl remote file URL
* @param file local file path to save to
* @param timeout download timeout in milliseconds
* @param headers — header map, e.g. ```{"a":"11"}```
* @param ignoreSSLErrors whether to ignore HTTPS certificate errors (expired, self-signed, etc.); optional, default `false`
* @return `true` on success, `false` on failure

```javascript showLineNumbers
function main() {
 for (let i = 0; i < 10; i++) {
 let url = "http://192.168.2.19/1.mp4";
 let f = file.getSandBoxFilePath("1.mp4")
 logd("download filepath: {}", f)
 logd("file.exists() " + file.exists(f));
 // If the file is deleted, download starts from the beginning (no resume)
 //file.deleteAllFile(f)
 let x = http.downloadFile2(url, f, 10 * 1000, {"User-Agent": "test"});
 logd("download result- " + x);
 if (x) {
 let save = utils.saveVideoToAlbumPath(f)
 logd("save " + save)
 break
 }

 }
}

main();
```

## http.httpGet GET Request

* HTTP GET request
* @param url request URL
* @param params parameter map, e.g. ```{"a":"11"}```, or a string
* @param timeout timeout in milliseconds
* @param headers — header map, e.g. ```{"a":"11"}```
* @param ignoreSSLErrors whether to ignore HTTPS certificate errors (expired, self-signed, etc.); optional, default `false`
* @return string — response body

```javascript showLineNumbers
function main() {
 let url = "http://192.168.0.5:8081/api/httpGet?a=1";
 let pa = {"b": "22"};
 let x = http.httpGet(url, pa, 10 * 1000, {"User-Agent": "test"});
 logd(" result- " + x);

 // For expired/self-signed HTTPS certificates, pass true as the last argument
 // let x2 = http.httpGet("https://expired.example.com/api", pa, 10 * 1000, {"User-Agent": "test"}, true);
}

main();
```

## http.httpPost POST Request

* HTTP POST request
* @param url request URL
* @param params parameters, e.g. ```{"a":"11"}``` or a string
* @param files files to upload, e.g. ```{"file1":file.getSandBoxFilePath("a.txt")}```
* @param timeout timeout in milliseconds
* @param headers — header map, e.g. ```{"a":"11"}```
* @param ignoreSSLErrors whether to ignore HTTPS certificate errors (expired, self-signed, etc.); optional, default `false`
* @return string — response body

```javascript showLineNumbers
function main() {
 // Request without file upload
 let url = "http://192.168.0.5:8081/api/httpPost";
 let pa = {"b": "value of b"};
 let x = http.httpPost(url, pa, null, 10 * 1000, {"User-Agent": "test"});
 logd(" result- " + x);

}

main();
```

```javascript showLineNumbers
function main() {
 // Request with file upload
 let url = "http://192.168.0.5:8081/api/httpPost";
 let pa = {"b": "value of b"};
 let files = {"file1": file.getSandBoxFilePath("a.txt"), "file2": file.getSandBoxFilePath("b.txt")};
 let x = http.httpPost(url, pa, files, 10 * 1000, {"User-Agent": "test"});
 logd(" result- " + x);

}

main();
```

## http.postJSON Send JSON

* HTTP POST JSON data
* @param url request URL
* @param json JSON data
* @param timeout — timeout in milliseconds
* @param headers — header map, e.g. ```{"a":"11"}```
* @param ignoreSSLErrors whether to ignore HTTPS certificate errors (expired, self-signed, etc.); optional, default `false`
* @return string — response body

```javascript showLineNumbers
function main() {
 let url = "http://192.168.0.5:8081/api/postJSON";
 let pa = {"b": "value of b"};
 let x = http.postJSON(url, pa, 10 * 1000, {"User-Agent": "test"});
 logd(" result- " + x);
 loge("result - " + x);

 // For expired/self-signed HTTPS certificates, pass true as the last argument
 // let x2 = http.postJSON("https://expired.example.com/api", pa, 10 * 1000, {"User-Agent": "test"}, true);
}

main();
```




## http.getLanIp Get LAN IP

* Get LAN IP addresses, including IPv4 and IPv6
* Available in EC 4.6.0+
* @returns `{JSON String}`

```javascript showLineNumbers
function main() {
 let x = http.getLanIp();
 logd(" result- " + x);

}

main();
```

## http.newWebsocket WebSocket Communication

* Create a WebSocket connection
* @param url connection URL
* @param header request headers
* @return ```{@link WebSocket }``` WebSocket object

```javascript showLineNumbers
function testwebsocket() {
 let result = [];
 // Create a new WebSocket; do not use 127.0.0.1 or localhost — the script runs on the phone, so 127 is the phone IP, not the PC
 let ws = http.newWebsocket("ws://192.168.2.13:8120/api/ws/device?deviceId=111", {"t1": "100"});
 ws.setWriteTimeout(5);
 // Heartbeat interval; 5 is recommended
 ws.setPingInterval(5)
 ws.setConnectionTimeout(5)
 // Listener when connection opens
 ws.onOpen(function (ws1) {
 logi("onOpen ");
 })
 // Listener for text messages
 ws.onText(function (ws1, text) {
 logi(" onText " + text);
 })
 // Listener when connection closes
 ws.onClose(function (ws1, reason) {
 logi(" onClose " + " reason : " + reason);
 })
 ws.onError(function (ws1, msg) {
 logi(" onError " + msg);
 result[0] = "error";
 })
 ws.onBinary(function (ws1, bytes) {
 logi(" onBinary " + (bytes));
 })

 // Start connecting
 let r = ws.connect();
 // Enable auto-reconnect
 ws.setAutoReconnect(true);
 logd("connect {} rr = {}", result[0], r);
 // Custom heartbeat data, sent every 5 seconds
 ws.startHeartbeatInterval(5, function () {
 return "heartbeat data"
 })
 // Stop heartbeat
 //ws.stopHeartbeatInterval()

 while (true) {
 if (isScriptExit()) {
 return
 }
 logd("isconnect " + ws.isConnected());
 sleep(1000)
 if (ws.isConnected()) {
 b = ws.sendText("new Date-> " + new Date())
 logd("send => " + b);
 sleep(1000)
 } else {
 // Reset connection
 let reset = ws.reset();
 logd("reset {}", reset)
 if (reset) {
 logd("Reconnecting...");
 let rc = ws.connect();
 logd("Reconnect --> " + rc);
 }
 }
 }
 logd("isClosed " + ws.isClosed())
 sleep(1000)
 // Close connection
 ws.close();
}

testwebsocket()

```
:::tip
- WebSocket connections work best in a child thread; store received data in shared storage. Requires version 5.20.0+
- Use shared storage read/write so JSVMs do not interfere with each other
- Example below
:::
```javascript showLineNumbers
// js/websocket.js
function testwebsocket() {
 let result = [];
 // Create a new WebSocket; do not use 127.0.0.1 or localhost — the script runs on the phone, so 127 is the phone IP, not the PC
 let ws = http.newWebsocket("ws://192.168.2.12:8199/wstest?deviceId=111", {"t1": "100"});
 ws.setWriteTimeout(5);
 // Heartbeat interval; 5 is recommended
 ws.setPingInterval(5)
 ws.setConnectionTimeout(5)
 // Listener when connection opens
 ws.onOpen(function (ws1) {
 logi("onOpen ");
 })
 // Listener for text messages
 ws.onText(function (ws1, text) {
 logi(" onText " + text);
 thread.putShareValue(text)
 })
 // Listener when connection closes
 ws.onClose(function (ws1, reason) {
 logi(" onClose " + " reason : " + reason);
 })
 ws.onError(function (ws1, msg) {
 logi(" onError " + msg);
 result[0] = "error";
 })
 ws.onBinary(function (ws1, bytes) {
 logi(" onBinary " + (bytes));
 })

 // Start connecting
 let r = ws.connect();
 // Enable auto-reconnect
 ws.setAutoReconnect(true);
 logd("connect {} rr = {}", result[0], r);
 // Custom heartbeat data, sent every 5 seconds
 ws.startHeartbeatInterval(5, function () {
 return "heartbeat data"
 })
 // Stop heartbeat
 //ws.stopHeartbeatInterval()

 while (true) {
 if (isScriptExit()) {
 return
 }
 logd("isconnect " + ws.isConnected());
 sleep(1000)
 if (ws.isConnected()) {
 b = ws.sendText("new Date-> " + new Date())
 logd("send => " + b);
 sleep(1000)
 } else {
 // Reset connection
 let reset = ws.reset();
 logd("reset {}", reset)
 if (reset) {
 logd("Reconnecting...");
 let rc = ws.connect();
 logd("Reconnect --> " + rc);
 }
 }
 }
 logd("isClosed " + ws.isClosed())
 sleep(1000)
 // Close connection
 ws.close();
}

```

```javascript showLineNumbers
// js/main.js
// Run WebSocket in a child thread; main thread reads data

function threadTest() {

 // Child threads cannot access outer variables.
 // Best practice: write business logic in a JS file and call it from the thread.
 let name = "thread2";
 let th = thread.newThread(name)
 // Run asynchronously
 th.execAsync(function () {
 var testData = readIECFileAsString("js/websocket.js");
 // testwebsocket is defined in websocket.js; append the call
 testData = (testData+" ;testwebsocket();")
 // Start execution
 let dx = execScript(2, testData);
 });

 while (!isScriptExit()) {
 sleep(100)
 // Read from KV shared storage
 let kvValue = thread.getShareKeyValue("currentTime")
 if (kvValue != "") {
 logd("Get current time: currentTime " + kvValue)
 }
 // Read from array shared storage
 let arValue = thread.getShareValue()
 if (arValue != "") {
 logd("After receiving data, run main-thread task... arValue = " + arValue)
 sleep(1000)
 logd("Task finished, waiting for next command")
 }
 }

}

threadTest();

```


### WebSocket Object Methods

#### connect Start Async Connection

* Start asynchronous connection
* See [example](/iostjdocs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### reset Reset Connection

* Reset connection
* @return `{bool}` `true` on success, `false` on failure
* See [example](/iostjdocs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### isClosed Is Closed

* Whether the connection is closed
* @return `true` if closed, `false` if not closed
* See [example](/iostjdocs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### isConnected Is Connected

* Whether the connection is established
* @return `true` if connected, `false` if not connected
* See [example](/iostjdocs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### close Close

* Close the connection
* See [example](/iostjdocs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### sendText Send Text

* Send a text message
* @param text text content
* See [example](/iostjdocs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### sendBinary Send Binary

* Send binary data
* @param bin Swift Data object
* See [example](/iostjdocs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### onOpen Open Callback

* Callback when the connection opens
* @param callback callback function
* See [example](/iostjdocs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### onText Text Callback

* Callback when a text message is received
* @param callback callback function
* See [example](/iostjdocs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### onClose Close Callback

* Callback when the connection closes
* @param callback callback function
* See [example](/iostjdocs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### onError Error Callback

* Callback when an error occurs
* @param callback callback function
* See [example](/iostjdocs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### onBinary Binary Message Callback

* Callback when binary data is received
* @param callback callback function
* See [example](/iostjdocs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### setConnectionTimeout Set Connection Timeout

* Set connection timeout when creating the WebSocket
* @param timeout seconds
* See [example](/iostjdocs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### setWriteTimeout Set Write Timeout

* Set write timeout
* @param timeout seconds
* See [example](/iostjdocs/funcs/http-api#httpnewwebsocket-websocket-communication)

*

#### setPingInterval Set Heartbeat Timeout

* Set heartbeat timeout (currently unused)
* @param timeout seconds
* See [example](/iostjdocs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### startHeartbeatInterval Start Heartbeat

* After connection succeeds, sends periodic heartbeat data
* @param timeInterval interval in seconds
* @param callback callback that returns heartbeat data
* See [example](/iostjdocs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### stopHeartbeatInterval Stop Heartbeat Timer

* Stop the periodic heartbeat timer
* See [example](/iostjdocs/funcs/http-api#httpnewwebsocket-websocket-communication)

#### setAutoReconnect Enable Auto-Reconnect

* Enable auto-reconnect
* See [example](/iostjdocs/funcs/http-api#httpnewwebsocket-websocket-communication)
