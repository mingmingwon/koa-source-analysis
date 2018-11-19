# http-errors <sup>[![Version Badge](http://versionbadg.es/jshttp/http-errors.svg)](https://www.npmjs.com/package/http-errors)</sup>

轻松为 Express，Koa，Connect 等创建 HTTP 错误。

## 安装

这是一个 [Node.js](https://nodejs.org/en/) 模块，通过 [npm registry](https://www.npmjs.com/) 获取。使用 [`npm install`](https://docs.npmjs.com/getting-started/installing-npm-packages-locally) 完成安装：

```sh
$ npm install http-errors
```

## 示例

```js
var createError = require('http-errors')
var express = require('express')
var app = express()

app.use(function (req, res, next) {
  if (!req.user) return next(createError(401, 'Please login to view this page.'))
  next()
})
```

## API

这是当前的 API，目前是从 Koa 中提取，可能会发生变化。

### 错误属性

- `expose` - 用来表示 `message` 是否应该被发送到客户端，当 `status` >= 500 时默认为 `false`
- `headers` - 可以是发送到客户端的头信息 name/value 对象，默认为 `undefined`。定义时，键名都应该为小写
- `message` - 传统错误信息，应该保持简短和一行概括
- `status` - 错误的状态码，`statusCode` 镜像，为了兼容性
- `statusCode` - 错误的状态码，默认为 `500`

### createError([status], [message], [properties])

使用给定的信息 `message` 创建错误对象。此错误对象继承自 `createError.HttpError`。

```js
var err = createError(404, 'This video does not exist!')
```

- `status` - 状态码为数字
- `message` - 错误信息，默认为状态码的 node 文本
- `properties` - 要附加到对象的自定义属性

### createError([status], [error], [properties])

使用 `createError.HttpError` 属性扩展给定的 `error` 对象。不会改变给定 `error` 对象的继承，并且修改后的 `error` 对象为返回值。

```js
fs.readFile('foo.txt', function (err, buf) {
  if (err) {
    if (err.code === 'ENOENT') {
      var httpError = createError(404, err, { expose: false })
    } else {
      var httpError = createError(500, err)
    }
  }
})
```

- `status` - 状态码为数字
- `error` - 待扩展的错误对象
- `properties` - 要附加到对象的自定义属性

### new createError\[code || name\](\[msg]\))

使用给定的信息 `msg` 创建错误对象。此错误对象继承自 `createError.HttpError`。

```js
var err = new createError.NotFound()
```

- `code` - 状态码为数字
- `name` - "bumpy case" 模式的错误名字，例如：`NotFound` 或 `InternalServerError`。

#### 所有构造函数列表

|状态码     |构造函数名                   |
|-----------|-----------------------------|
|400        |BadRequest                   |
|401        |Unauthorized                 |
|402        |PaymentRequired              |
|403        |Forbidden                    |
|404        |NotFound                     |
|405        |MethodNotAllowed             |
|406        |NotAcceptable                |
|407        |ProxyAuthenticationRequired  |
|408        |RequestTimeout               |
|409        |Conflict                     |
|410        |Gone                         |
|411        |LengthRequired               |
|412        |PreconditionFailed           |
|413        |PayloadTooLarge              |
|414        |URITooLong                   |
|415        |UnsupportedMediaType         |
|416        |RangeNotSatisfiable          |
|417        |ExpectationFailed            |
|418        |ImATeapot                    |
|421        |MisdirectedRequest           |
|422        |UnprocessableEntity          |
|423        |Locked                       |
|424        |FailedDependency             |
|425        |UnorderedCollection          |
|426        |UpgradeRequired              |
|428        |PreconditionRequired         |
|429        |TooManyRequests              |
|431        |RequestHeaderFieldsTooLarge  |
|451        |UnavailableForLegalReasons   |
|500        |InternalServerError          |
|501        |NotImplemented               |
|502        |BadGateway                   |
|503        |ServiceUnavailable           |
|504        |GatewayTimeout               |
|505        |HTTPVersionNotSupported      |
|506        |VariantAlsoNegotiates        |
|507        |InsufficientStorage          |
|508        |LoopDetected                 |
|509        |BandwidthLimitExceeded       |
|510        |NotExtended                  |
|511        |NetworkAuthenticationRequired|

## 译者

[Jordan Wang](https://github.com/mingmingwon/)
