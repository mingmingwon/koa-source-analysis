# http-assert <sup>[![Version Badge](http://versionbadg.es/jshttp/http-assert.svg)](https://www.npmjs.com/package/http-assert)</sup>

HTTP 状态码断言，类似于 Koa 中的 ctx.throw()，但有守护。

## 安装

这是一个 [Node.js](https://nodejs.org/en/) 模块，通过 [npm registry](https://www.npmjs.com/) 获取。使用 [`npm install`](https://docs.npmjs.com/getting-started/installing-npm-packages-locally) 完成安装：

```bash
$ npm install http-assert
```

## 示例
```js
var assert = require('http-assert')
var ok = require('assert')

var username = 'foobar' // username from request

try {
  assert(username === 'fjodor', 401, 'authentication failed')
} catch (err) {
  ok(err.status === 401)
  ok(err.message === 'authentication failed')
  ok(err.expose)
}
```

## API

此模块的 API 类似于 Node.js [`assert`](https://nodejs.org/dist/latest/docs/api/assert.html) 模块。

当断言失败时，每个函数都会从 [`http-errors`](https://www.npmjs.com/package/http-errors) 模块抛出一个 `HttpError` 实例。

### assert(value, [status], [message], [properties])

测试 `value` 是否为真。如果 `value` 非真，则抛出由给定的 `status`，`message` 和 `properties` 构造的 `HttpError`。

### assert.deepEqual(a, b, [status], [message], [properties])

通过相等比较（`==`）测试 `a` 和 `b` 是否深度相等。如果 `a` 和 `b` 不相等，则抛出由给定的 `status`，`message` 和 `properties` 构造的 `HttpError`。

### assert.equal(a, b, [status], [message], [properties])

通过相等比较（`==`）测试 `a` 和 `b` 是否相等。如果 `a` 和 `b` 不相等，则抛出由给定的 `status`，`message` 和 `properties` 构造的 `HttpError`。

### assert.notDeepEqual(a, b, [status], [message], [properties])

通过相等比较（`==`）测试 `a` 和 `b` 是否深度相等。如果 `a` 和 `b` 相等，则抛出由给定的 `status`，`message` 和 `properties` 构造的 `HttpError`。

### assert.notEqual(a, b, [status], [message], [properties])


通过相等比较（`==`）测试 `a` 和 `b` 是否相等。如果 `a` 和 `b` 相等，则抛出由给定的 `status`，`message` 和 `properties` 构造的 `HttpError`。

### assert.notStrictEqual(a, b, [status], [message], [properties])


通过全等比较（`===`）测试 `a` 和 `b` 是否严格相等。如果 `a` 和 `b` 相等，则抛出由给定的 `status`，`message` 和 `properties` 构造的 `HttpError`。

### assert.ok(value, [status], [message], [properties])

测试 `value` 是否为真。如果 `value` 非真，则抛出由给定的 `status`，`message` 和 `properties` 构造的 `HttpError`。

### assert.strictEqual(a, b, [status], [message], [properties])

通过全等比较（`===`）测试 `a` 和 `b` 是否严格相等。如果 `a` 和 `b` 不相等，则抛出由给定的 `status`，`message` 和 `properties` 构造的 `HttpError`。

## 译者

[Jordan Wang](https://github.com/mingmingwon/)
