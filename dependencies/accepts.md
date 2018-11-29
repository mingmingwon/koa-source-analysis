## accepts <sup>[![Version Badge](http://versionbadg.es/jshttp/accepts.svg)](https://www.npmjs.com/package/accepts)</sup>

基于 [negotiator](https://www.npmjs.com/package/negotiator) 库的高级内容协商。从 [koa](https://www.npmjs.com/package/koa) 中提取以供通用。

对于 negotiator 补充，它允许：

- 类型为数组和参数列表，例如 `(['text/html', 'application/json'])`，以及 `('text/html', 'application/json')`。
- 类型缩写，例如 `json`。
- 当类型匹不配时返回 `false` 。
- 将不存在的 headers 视为 `*`。

## 安装

这是一个 [Node.js](https://nodejs.org/en/) 模块，通过 [npm registry](https://www.npmjs.com/) 获取。使用 [`npm install`](https://docs.npmjs.com/getting-started/installing-npm-packages-locally) 完成安装：

```sh
$ npm install accepts
```

## API

```js
var accepts = require('accepts')
```

### accepts(req)

为给定的 `req` 创建一个新的 `Accepts` 对象。

#### .charset(charsets)

返回第一个接受的 charset。如果 `charsets` 中没有 chartset 被接受，则返回 `false`。

#### .charsets()

按照客户端的偏好顺序返回请求接受的 charset（首选第一个）。

#### .encoding(encodings)

返回第一个接受的 encoding。如果 `encodings` 中没有 encoding 被接受，则返回 `false`。

#### .encodings()

按照客户端的偏好顺序返回请求接受的 encoding（首选第一个）。

#### .language(languages)

返回第一个接受的 language。如果 `languages` 中没有 language 被接受，则返回 `false`。

#### .languages()

按照客户端的偏好顺序返回请求接受的 language（首选第一个）。

#### .type(types)

返回第一个接受的 type。如果 `types` 中没有 type 被接受，则返回 `false`。

`types` 数组可以包含完整的 MIME 类型或文件扩展名。任何一个非完整的 MIME 类型将被传递给 `require('mime-types').lookup`。

#### .types()

按照客户端的偏好顺序返回请求接受的 type（首选第一个）。

## 示例

#### 简单类型协商

这个简单的例子展示了如何使用 `accepts` 根据客户端想接收的响应体来返回不同的类型的响应体。服务端按顺序列出其偏好，返回客户端和服务端的最佳匹配。

```js
var accepts = require('accepts')
var http = require('http')

function app (req, res) {
  var accept = accepts(req)

  // the order of this list is significant; should be server preferred order
  switch (accept.type(['json', 'html'])) {
    case 'json':
      res.setHeader('Content-Type', 'application/json')
      res.write('{"hello":"world!"}')
      break
    case 'html':
      res.setHeader('Content-Type', 'text/html')
      res.write('<b>hello, world!</b>')
      break
    default:
      // the fallback is text/plain, so no need to specify it above
      res.setHeader('Content-Type', 'text/plain')
      res.write('hello, world!')
      break
  }

  res.end()
}

http.createServer(app).listen(3000)
```

可以使用 cURL 程序对此进行测试：

```sh
curl -I -H'Accept: text/html' http://localhost:3000/
```

## 译者

[Jordan Wang](https://github.com/mingmingwon/)
