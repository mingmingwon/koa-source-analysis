# content-type <sup>[![Version Badge](http://versionbadg.es/jshttp/content-type.svg)](https://www.npmjs.com/package/content-type)</sup>

根据 RFC 7231 协议创建和解析 HTTP Content-Type 标头。

## 安装

```sh
$ npm install content-type
```

## API

```js
var contentType = require('content-type')
```

### contentType.parse(string)

```js
var obj = contentType.parse('image/svg+xml; charset=utf-8')
```

解析 `Content-Type` 标头。返回具有以下属性的对象（以字符串 `'image/svg+xml; charset=utf-8'` 为例）：

 - `type`：媒体类型（类型和子类型总是小写）。例如：`'image/svg+xml'`

 - `parameters`：媒体类型中的参数对象（参数名总是小写）。例如：`{charset: 'utf-8'}`

如果 `string` 缺失或无效，则抛出 `TypeError`。

### contentType.parse(req)

```js
var obj = contentType.parse(req)
```

从给定的 `req` 解析 `Content-Type` 标头，是 `contentType.parse(req.headers['content-type'])` 的简写。

如果 `Content-Type` 标头缺失或无效，则抛出 `TypeError`。

### contentType.parse(res)

```js
var obj = contentType.parse(res)
```

从给定的 `res` 解析 `Content-Type` 标头，是 `contentType.parse(res.getHeader['content-type'])` 的简写。

如果 `Content-Type` 标头缺失或无效，则抛出 `TypeError`。

### contentType.format(obj)

```js
var str = contentType.format({
  type: 'image/svg+xml',
  parameters: { charset: 'utf-8' }
})
```

将对象格式化为 `Content-Type` 标头。返回具有以下属性的给定对象的 `Content-Type` 字符串（以产出字符串 `'image/svg+xml; charset=utf-8'` 为例）：

 - `type`：媒体类型（将被转为小写）。例如：`'image/svg+xml'`

 - `parameters`：媒体类型中的参数对象（参数名将被转为小写）。例如：`{charset: 'utf-8'}`

如果 `object` 包含无效类型或参数名，则抛出 `TypeError`。

## 译者

[Jordan Wang](https://github.com/mingmingwon/)
