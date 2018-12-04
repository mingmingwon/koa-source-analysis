# fresh <sup>[![Version Badge](http://versionbadg.es/jshttp/fresh.svg)](https://www.npmjs.com/package/fresh)</sup>

HTTP 响应 freshness 测试 

## 安装

这是一个 [Node.js](https://nodejs.org/en/) 模块，通过 [npm registry](https://www.npmjs.com/) 获取。使用 [`npm install`](https://docs.npmjs.com/getting-started/installing-npm-packages-locally) 完成安装：

```
$ npm install fresh
```

## API

```js
var fresh = require('fresh')
```

### fresh(reqHeaders, resHeaders)

根据请求和响应标头检测响应的 freshness。

当客户端缓存中的响应仍然 "fresh" 时，返回 true，否则返回 false，以表示客户端缓存已过时，应该发送完整的响应。

当客户端发送 `Cache-Control: no-cache` 请求标头时，表示端到端的重载请求，该模块将返回`false` 使这些请求处理透明。

## 已知问题

此模块仅用于遵循 HTTP 规范，而不是解决所有类型的客户端错误（因为此模块通常情况下没有收到足够的信息来理解客户实际上是什么情况）。

在某些版本的 Safari 中，存在一个已知问题：即使 Safari 缓存中没有资源，Safari 仍将错误地发出请求，让此模块验证资源的 freshness。[jumanji](https://www.npmjs.com/package/jumanji) 模块可以用于 Express 应用程序来解决这个问题，且提供了进一步阅读该 Safari bug 的链接。

## 示例

### API 使用

```js
var reqHeaders = { 'if-none-match': '"foo"' }
var resHeaders = { 'etag': '"bar"' }
fresh(reqHeaders, resHeaders)
// => false

var reqHeaders = { 'if-none-match': '"foo"' }
var resHeaders = { 'etag': '"foo"' }
fresh(reqHeaders, resHeaders)
// => true
```

### 配合 Node.js http server 使用

```js
var fresh = require('fresh')
var http = require('http')

var server = http.createServer(function (req, res) {
  // perform server logic
  // ... including adding ETag / Last-Modified response headers

  if (isFresh(req, res)) {
    // client has a fresh copy of resource
    res.statusCode = 304
    res.end()
    return
  }

  // send the resource
  res.statusCode = 200
  res.end('hello, world!')
})

function isFresh (req, res) {
  return fresh(req.headers, {
    'etag': res.getHeader('ETag'),
    'last-modified': res.getHeader('Last-Modified')
  })
}

server.listen(3000)
```

## 译者

[Jordan Wang](https://github.com/mingmingwon/)
