# Statuses <sup>[![Version Badge](http://versionbadg.es/jshttp/statuses.svg)](https://www.npmjs.com/package/statuses)</sup>

Node HTTP 状态工具。

此模块提供了一份来自一些不同项目的状态码和消息列表：

  * [IANA 状态代码注册表](https://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml)
  * [Node.js 项目](https://nodejs.org/)
  * [NGINX 项目](https://www.nginx.com/)
  * [Apache HTTP Server 项目](https://httpd.apache.org/)

## 安装

这是一个 [Node.js](https://nodejs.org/en/) 模块，通过 [npm registry](https://www.npmjs.com/) 获取。使用 [`npm install`](https://docs.npmjs.com/getting-started/installing-npm-packages-locally) 完成安装：

```sh
$ npm install statuses
```

## API

```js
var status = require('statuses')
```

### var code = status(Integer || String)

如果 `Integer` 或 `String` 是有效的 HTTP 码或状态消息，将返回适当的 `code`。否则，抛出错误。

```js
status(403) // => 403
status('403') // => 403
status('forbidden') // => 403
status('Forbidden') // => 403
status(306) // throws, as it's not supported by node.js
```

### status.STATUS_CODES

返回一个对象，它将状态码映射为状态消息，格式与 [Node.js http module](https://nodejs.org/dist/latest/docs/api/http.html#http_http_status_codes) 模块相同。

### status.codes

返回各项状态码均为 `Integer` 的数组。

### var msg = status[code]

从 `code` 到 `status message` 的映射。无效 `code` 的状态消息为 `undefined`。

```js
status[404] // => 'Not Found'
```

### var code = status[msg]

从 `status message` 到 `code` 的映射。`msg` 可以是单词首字母大写，或小写的，无效 `status message` 的状态码为 `undefined`。

```js
status['not found'] // => 404
status['Not Found'] // => 404
```

### status.redirect[code]

如果状态码是有效的重定向状态，返回 `true` 。

```js
status.redirect[200] // => undefined
status.redirect[301] // => true
```

### status.empty[code]

如果状态码代表空体，返回 `true` 。

```js
status.empty[200] // => undefined
status.empty[204] // => true
status.empty[304] // => true
```

### status.retry[code]

如果状态码代表重试，返回 `true`。

```js
status.retry[501] // => undefined
status.retry[503] // => true
```

## 译者

[Jordan Wang](https://github.com/mingmingwon/)
