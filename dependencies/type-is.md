# type-is <sup>[![Version Badge](http://versionbadg.es/jshttp/type-is.svg)](https://www.npmjs.com/package/type-is)</sup>

推断请求的 Content-Type。

### 安装

这是一个 [Node.js](https://nodejs.org/en/) 模块，通过 [npm registry](https://www.npmjs.com/) 获取。使用 [`npm install`](https://docs.npmjs.com/getting-started/installing-npm-packages-locally) 完成安装：

```sh
$ npm install type-is
```

## API

```js
var http = require('http')
var typeis = require('type-is')

http.createServer(function (req, res) {
  var istext = typeis(req, ['text/*'])
  res.end('you ' + (istext ? 'sent' : 'did not send') + ' me text')
})
```

### type = typeis(request, types)

`request` 是 node HTTP 请求，`types` 是类型组成的数组。

```js
// req.headers.content-type = 'application/json'

typeis(req, ['json'])             // 'json'
typeis(req, ['html', 'json'])     // 'json'
typeis(req, ['application/*'])    // 'application/json'
typeis(req, ['application/json']) // 'application/json'

typeis(req, ['html']) // false
```

### typeis.hasBody(request)

如果给定的 `request` 有请求体，则返回布尔值，而不管 `Content-Type` 标头是什么。

有请求体与请求体的大小无关（可能是 0 字节）。这与文件存在的工作方式类似。如果请求体确实存在，那么就表示存在要从 Node.js 请求流中读取的数据。

```js
if (typeis.hasBody(req)) {
  // read the body, since there is one

  req.on('data', function (chunk) {
    // ...
  })
}
```

### type = typeis.is(mediaType, types)

`mediaType` 是 [media type](https://tools.ietf.org/html/rfc6838) 字符串。`types` 是类型组成的数组。

```js
var mediaType = 'application/json'

typeis.is(mediaType, ['json'])             // 'json'
typeis.is(mediaType, ['html', 'json'])     // 'json'
typeis.is(mediaType, ['application/*'])    // 'application/json'
typeis.is(mediaType, ['application/json']) // 'application/json'

typeis.is(mediaType, ['html']) // false
```

每种类型可以是:

- 扩展名，例如 `json`。如果匹配，将返回此名称。
- mime 类型，例如 `application / json`。
- 带有通配符的 mime 类型，例如 `* / *` 或 `* / json` 或 `application / *`。如果匹配，将返回完整的 mime 类型。
- 后缀例如 `+ json`。可以与通配符结合使用，例如 `* / vnd + json` 或 `application / * + json`。如果匹配，将返回完整的 mime 类型。

如果未匹配到类型或 Content-Type 无效返回 `false` 。

如果请求不包含请求体返回 `null` 。

## 示例

### 请求体解析

```js
var express = require('express')
var typeis = require('type-is')

var app = express()

app.use(function bodyParser (req, res, next) {
  if (!typeis.hasBody(req)) {
    return next()
  }

  switch (typeis(req, ['urlencoded', 'json', 'multipart'])) {
    case 'urlencoded':
      // parse urlencoded body
      throw new Error('implement urlencoded body parsing')
    case 'json':
      // parse json body
      throw new Error('implement json body parsing')
    case 'multipart':
      // parse multipart body
      throw new Error('implement multipart body parsing')
    default:
      // 415 error code
      res.statusCode = 415
      res.end()
      break
  }
})
``

## 译者

[Jordan Wang](https://github.com/mingmingwon/)
