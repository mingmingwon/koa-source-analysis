# on-finished <sup>[![Version Badge](http://versionbadg.es/jshttp/on-finished.svg)](https://www.npmjs.com/package/on-finished)</sup>

HTTP 请求关闭、完成或出错时执行回调。

## 安装

这是一个 [Node.js](https://nodejs.org/en/) 模块，可以通过 [npm registry](https://www.npmjs.com/) 获取。使用 [`npm install`](https://docs.npmjs.com/getting-started/installing-npm-packages-locally) 完成安装：

```sh
$ npm install on-finished
```

## API

```js
var onFinished = require('on-finished')
```

### onFinished(res, listener)

添加一个监听器来监听响应完成。当响应完成时，监听器会被调用一次。如果响应结束出错，第一个参数将包含报错信息。如果响应已经完成，监听器将被调用。

监听一个响应的结束将被用来关闭和响应相关的事物，比如打开的文件。 

监听器被调用为 `listener(err, res)`。

```js
onFinished(res, function (err, res) {
  // clean up open fds, etc.
  // err contains the error if request error'd
})
```

### onFinished(req, listener)

添加一个监听器来监听请求完成。当请求完成时，监听器会被调用一次。如果请求结束出错，第一个参数将包含报错信息。如果请求已经完成，监听器将被调用。

监听一个请求的结束将被用来知道在读取数据之后何时继续。 

监听器被调用为 `listener(err, req)`。

```js
var data = ''

req.setEncoding('utf8')
req.on('data', function (str) {
  data += str
})

onFinished(req, function (err, req) {
  // data is read unless there is err
})
```

### onFinished.isFinished(res)

判断 `res` 是否已经完成。如果响应已经完成，这将有助于检查和不启动某些操作。

### onFinished.isFinished(req)

判断 `req` 是否已经完成。如果请求已经完成，这将有助于检查和不启动某些操作。

## 特殊的 Node.js 请求

### HTTP CONNECT 方法

RFC 7231，第 4.3.6 节中 `CONNECT` 方法的含义：

> CONNECT 方法请求接收方建立到由请求目标标识的目标源服务器的隧道，并且如果成功，则此后将其行为限制为在两个方向上盲目转发分组，直到隧道关闭。 隧道通常用于通过一个或多个代理创建端到端虚拟连接，然后可以使用 TLS（传输层安全性，[RFC5246]）来保护隧道。

在 Node.js 中，这些请求对象来自 HTTP 服务的 `'connect'` 事件。

当此模块用于 HTTP `CONNECT` 请求时，**由于 Node.js 接口的限制**，该请求立即被视为 “已完成”。这意味着如果 `CONNECT` 请求包含请求实体，则该请求即使在读取之前也将被视为 “已完成”。

Node.js 中的 `CONNECT` 请求没有响应对象，所以不支持。

### HTTP Upgrade 请求

RFC 7230，第 6.1 节中 `Upgrade` 请求头的含义：

> "Upgrade" 请求头字段旨在提供一种简单的机制，用于在同一连接上从 HTTP/1.1 转换到某些其他协议。

在 Node.js 中，这些请求对象来自 HTTP 服务的 `'upgrade'` 事件。

当此模块用于带有 `'upgrade'` 请求头的 HTTP 请求时，**由于 Node.js 接口的限制**，该请求立即被视为 “已完成”。这意味着如果 `Upgrade` 请求包含请求实体，则该请求即使在读取之前也将被视为 “已完成”。

Node.js 中没有 `Upgrade` 请求的响应对象，因此不支持。

## 示例

以下代码确保一旦响应完成，文件描述符将被关闭。

```js
var destroy = require('destroy')
var fs = require('fs')
var http = require('http')
var onFinished = require('on-finished')

http.createServer(function onRequest (req, res) {
  var stream = fs.createReadStream('package.json')
  stream.pipe(res)
  onFinished(res, function () {
    destroy(stream)
  })
})
```

## 译者

[Jordan Wang](https://github.com/mingmingwon/)
