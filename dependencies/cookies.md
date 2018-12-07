# cookies  <sup>[![Version Badge](http://versionbadg.es/pillarjs/cookies.svg)](https://www.npmjs.com/package/cookies)</sup>

cookies 是一个 [node.js](http://nodejs.org/) 模块，用于获取和设置 HTTP(s) cookies。使用 [Keygrip](https://www.npmjs.com/package/keygrip) 对 cookie 进行签名可以防止篡改。它可以与 node.js 内置的 HTTP 一起使用，也可以作为 Connect/Express 中间件使用。

## 安装

这是一个 [Node.js](https://nodejs.org/en/) 模块，通过 [npm registry](https://www.npmjs.com/) 获取。使用 [`npm install`](https://docs.npmjs.com/getting-started/installing-npm-packages-locally) 完成安装：

```
$ npm install cookies
```

## 功能

* **惰性**：由于多个密钥的 cookie 验证可能消耗较大，因此 cookie 仅在访问时被惰性验证，而不是每个请求都验证。

* **安全性**：默认情况下，所有 cookie 都是 `httponly`，通过 SSL 发送的 cookie 是 `安全的`。如果通过不安全的 socket 发送安全 cookie，则会引发错误。

* **隐蔽性**：签名 cookie 的存储方式与未签名 cookie 的相同，而不是以模糊的签名格式存储。 使用标准命名约定（cookie-name`.sig`）为每个签名的 cookie 存储额外的签名 cookie。这样可以允许其他库访问原始 cookie 而无需知道签名机制。

* **扩展性**：该库专门为使用 [Keygrip](https://www.npmjs.com/package/keygrip) 而优化，但并非必须依赖它；如果愿意，可以实现自己的签名方案，并且仅使用此库来读取/写入 cookie。将签名保存到单独的库中以实现代码重用，并允许将相同的签名库用于需要签名的其他区域，例如 URL。

## API

### cookies = new Cookies( request, response, [ options ] )

这将创建一个与当前 request 和 response 相对应的 cookie 容器，额外传递一个对象选项。

可选将 [Keygrip](https://www.npmjs.com/package/keygrip) 对象或密钥数组作为 options.keys 传递，以使用轮换凭据启用基于 SHA1 HMAC 的加密签名。

可选将布尔值作为 options.secure 传递，以明确指定连接是否安全，而不是此模块检测 request。

注意，由于只保存参数而不进行任何其他处理，因此它非常轻量级。cookie 仅在访问时按需解析。

### express.createServer( Cookies.express( keys ) )

这将在 Express 应用中作为 Connect 中间件添加对 cookie 的支持，允许使用 `req.cookies.get` 读取 cookie，使用 `res.cookies.set` 设置 cookie。

### cookies.get( name, [ options ] )

这将从请求中的 `Cookie` 标头提取具有给定名称的 cookie。如果存在则返回其值。否则不返回任何内容。

`{signed：true}` 可以选择作为第二个参数 options 传递。在这种情况下，将获取签名 cookie（以附加的 `.sig` 后缀结尾的同名 cookie）。如果不存在，则不返回任何内容。

如果签名 cookie 确实存在，则提供的 [Keygrip](https://www.npmjs.com/package/keygrip) 对象用于检查 cookie-name _ = _ cookie-value 的哈希是否与任何已注册密钥的哈希匹配：
 
* 如果签名 cookie 哈希与第一个键匹配，则返回原始 cookie 值。
* 如果签名 cookie 哈希与任何其他键匹配，则返回原始 cookie 值并设置出站头以将签名 cookie 的值更新为第一个键的哈希值。这样可以自动刷新由于秘钥更新而过期的签名 cookie。
*如果签名 cookie 哈希与任何密钥都不匹配，则不返回任何内容，并使用具有过期日期的出站标头来删除 cookie。

### cookies.set( name, [ value ], [ options ] )

这将在响应中设置给定的 cookie 并返回当前上下文以允许链式调用。

如果省略 value，则使用具有过期日期的出站标头来删除 cookie。

如果提供了 options 对象，它将用于生成出站 cookie 标头，如下所示：

* `maxAge`：数字，表示从 `Date.now()` 到过期的毫秒数
* `expires`：`Date` 对象，表示 cookie 的过期日期（默认情况下在会话结束时过期）。
* `path`：表示 cookie 路径的字符串（默认为 `/`）。
* `domain`：表示 cookie 域的字符串（无默认值）。
* `secure`：布尔值，表示 cookie 是否仅通过 HTTPS 发送（HTTP 默认为 `false`，HTTPS 默认为 `true`）。阅读下面有关此选项的[更多信息](＃secure-cookies)。
* `httpOnly`：布尔值，表示 cookie 是仅通过 HTTP(s) 发送，不可用于客户端 JavaScript（默认为 `true`）。
* `sameSite`：布尔值或字符串，表示 cookie 是否是“同一站点” cookie（默认为 `false`）。 这可以设置为`'strict'`, `'lax'` 或 `true`（映射为 `'strict'`）。
* `signed`：布尔值，表示是否要对 cookie 进行签名（默认为 `false`）。如果为 true，那么另一个以 `.sig` 为后缀的同名 cookie 也将被发送，其中 27 字节的安全网址 base64 SHA1 值代表 cookie-name=cookie-value 的哈希值，对应第一个 [ Keygrip](https://www.npmjs.com/package/keygrip) 秘钥。此签名密钥用于在下次收到 cookie 时检测是否被篡改。
* `overwrite`：布尔值，表示是否覆盖以前设置的同名 cookie（默认为 `false`）。如果为 true，则在设置此 cookie 时，将在 Set-Cookie 标头中过滤掉在相同请求期间设置的所有具有相同名称（无论路径或域）的 cookie。

### 安全 cookies

要发送安全 cookie，请使用 `secure：true` 选项设置 cookie。

HTTPS 是安全 cookie 所必需的。当使用 `secure:true` 调用 `cookies.set` 而并且未检测到安全连接时，将不会设置 cookie 并且将引发错误。

该模块将通过检查以下内容测试每个请求以查看它是否安全：

* 如果请求的 `protocol` 属性设置为 `https`
* 如果请求的 `connection.encrypted` 属性设置为 `true`。

如果您的服务使用代理并且使用的是 `secure: true`，则需要将服务器配置为读取代理添加的请求标头，以确定请求是否使用安全连接。

有关在代理服务背后的更多信息，请参阅您正在使用的框架：

* Koa - [`app.proxy = true`](http://koajs.com/#settings)
* Express - [代理配置](http://expressjs.com/en/4x/api.html#trust.proxy.options.table)


如果您的 Koa 或 Express 服务配置正确，请求的 `protocol` 属性将被设置为匹配代理在 `X-Forwarded-Proto` 标头中报告的协议。

## 示例

```js
var http = require('http')
var Cookies = require('cookies')

// Optionally define keys to sign cookie values
// to prevent client tampering
var keys = ['keyboard cat']

var server = http.createServer(function (req, res) {
  // Create a cookies object
  var cookies = new Cookies(req, res, { keys: keys })

  // Get a cookie
  var lastVisit = cookies.get('LastVisit', { signed: true })

  // Set the cookie to a value
  cookies.set('LastVisit', new Date().toISOString(), { signed: true })

  if (!lastVisit) {
    res.setHeader('Content-Type', 'text/plain')
    res.end('Welcome, first time visitor!')
  } else {
    res.setHeader('Content-Type', 'text/plain')
    res.end('Welcome back! Nothing much changed since your last visit at ' + lastVisit + '.')
  }
})

server.listen(3000, function () {
  console.log('Visit us at http://127.0.0.1:3000/ !')
})
```

## 译者

[Jordan Wang](https://github.com/mingmingwon/)
