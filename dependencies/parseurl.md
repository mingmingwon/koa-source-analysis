# parseurl <sup>[![Version Badge](http://versionbadg.es/pillarjs/parseurl.svg)](https://www.npmjs.com/package/parseurl)</sup>

解析 URL 并缓存。

## 安装

这是一个 [Node.js](https://nodejs.org/en/) 模块，通过 [npm registry](https://www.npmjs.com/) 获取。使用 [`npm install`](https://docs.npmjs.com/getting-started/installing-npm-packages-locally) 完成安装：

```sh
$ npm install parseurl
```

## API

```js
var parseurl = require('parseurl')
```

### parseurl(req)

解析给定请求对象的 URL（`req.url` 属性）并返回结果。结果与 Node.js 核心的 `url.parse` 相同。在 `req.url` 保持不变的同一 `req` 上多次调用此函数将返回缓存的解析对象，而不是再次解析。

### parseurl.original(req)

解析给定请求对象的原始 URL 并返回结果。如果 `req.originalUrl` 为字符串将尝试解析它，否则解析 `req.url`。结果与 Node.js 核心的 `url.parse` 相同。在 `req.originalUrl` 保持不变的同一 `req` 上多次调用此函数将返回缓存的解析对象，而不是再次解析。

## Benchmark

```bash
$ npm run-script bench

> parseurl@1.3.2 bench nodejs-parseurl
> node benchmark/index.js

  http_parser@2.7.0
  node@4.8.4
  v8@4.5.103.47
  uv@1.9.1
  zlib@1.2.11
  ares@1.10.1-DEV
  icu@56.1
  modules@46
  openssl@1.0.2k

> node benchmark/fullurl.js

  Parsing URL "http://localhost:8888/foo/bar?user=tj&pet=fluffy"

  3 tests completed.

  fasturl   x 1,246,766 ops/sec ±0.74% (188 runs sampled)
  nativeurl x    91,536 ops/sec ±0.54% (189 runs sampled)
  parseurl  x    90,645 ops/sec ±0.38% (189 runs sampled)

> node benchmark/pathquery.js

  Parsing URL "/foo/bar?user=tj&pet=fluffy"

  3 tests completed.

  fasturl   x 2,077,650 ops/sec ±0.69% (186 runs sampled)
  nativeurl x   638,669 ops/sec ±0.67% (189 runs sampled)
  parseurl  x 2,431,842 ops/sec ±0.71% (189 runs sampled)

> node benchmark/samerequest.js

  Parsing URL "/foo/bar?user=tj&pet=fluffy" on same request object

  3 tests completed.

  fasturl   x  2,135,391 ops/sec ±0.69% (188 runs sampled)
  nativeurl x    672,809 ops/sec ±3.83% (186 runs sampled)
  parseurl  x 11,604,947 ops/sec ±0.70% (189 runs sampled)

> node benchmark/simplepath.js

  Parsing URL "/foo/bar"

  3 tests completed.

  fasturl   x 4,961,391 ops/sec ±0.97% (186 runs sampled)
  nativeurl x   914,931 ops/sec ±0.83% (186 runs sampled)
  parseurl  x 7,559,196 ops/sec ±0.66% (188 runs sampled)

> node benchmark/slash.js

  Parsing URL "/"

  3 tests completed.

  fasturl   x  4,053,379 ops/sec ±0.91% (187 runs sampled)
  nativeurl x    963,999 ops/sec ±0.58% (189 runs sampled)
  parseurl  x 11,516,143 ops/sec ±0.58% (188 runs sampled)
```

## 译者

[Jordan Wang](https://github.com/mingmingwon/)
