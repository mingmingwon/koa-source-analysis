# vary <sup>[![Version Badge](http://versionbadg.es/jshttp/vary.svg)](https://www.npmjs.com/package/vary)</sup>

操作 HTTP vary 标头

## 安装

这是一个 [Node.js](https://nodejs.org/en/) 模块，通过 [npm registry](https://www.npmjs.com/) 获取。使用 [`npm install`](https://docs.npmjs.com/getting-started/installing-npm-packages-locally) 完成安装：

```sh
$ npm install vary
```

## API

```js
var vary = require('vary')
```

### vary(res, field)

将给定的标头 `field` 添加到 `res` 的 `vary` 响应头。它可以是单个字段的字符串，也可以是有效的 `Vary` 的字符串标头或多个字段的数组。

如果未列出，将追加标头，否则保持其当前位置不管。

```js
// 将 "Origin" 添加到响应的 Vary 标头
vary(res, 'Origin')
```

### vary.append(header, field)

将给定的标头 `field` 添加到 `res` 的 `vary` 响应头。它可以是单个字段的字符串，也可以是有效的 `Vary` 的字符串标头或多个字段的数组。

如果未列出，将追加标头，否则保持其当前位置不管。返回新的标头字符串。

```js
// 获取标头字符串将 "Origin" 添加到 "Accept, User-Agent"
vary.append('Accept, User-Agent', 'Origin')
```

## 示例

当内容依赖它时更新 Vary 标头

```js
var http = require('http')
var vary = require('vary')

http.createServer(function onRequest (req, res) {
  // about to user-agent sniff
  vary(res, 'User-Agent')

  var ua = req.headers['user-agent'] || ''
  var isMobile = /mobi|android|touch|mini/i.test(ua)

  // serve site, depending on isMobile
  res.setHeader('Content-Type', 'text/html')
  res.end('You are (probably) ' + (isMobile ? '' : 'not ') + 'a mobile user')
})
```

## 测试

```sh
$ npm test
```

## 译者

[Jordan Wang](https://github.com/mingmingwon/)
