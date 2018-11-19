## cache-content-type <sup>[![Version Badge](http://versionbadg.es/node-modules/cache-content-type.svg)](https://www.npmjs.com/package/cache-content-type)</sup>

和 [mime-types](https://github.com/jshttp/mime-types) 的 contentType 方法相同，但缓存结果。

## 安装

```sh
npm i cache-content-type
```

## 使用

```js
const getType = require('cache-content-type');
const contentType = getType('html');
assert(contentType === 'text/html; charset=utf-8');
```

## 译者

[Jordan Wang](https://github.com/mingmingwon/)
