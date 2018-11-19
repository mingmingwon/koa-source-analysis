# error-inject <sup>[![Version Badge](http://versionbadg.es/stream-utils/error-inject.svg)](https://www.npmjs.com/package/error-inject)</sup>

注册流错误监听器。

## 安装

```
npm install error-inject
```

## 使用


```js
var inject = require('error-inject');

var rs = fs.createReadStream('index.js');
inject(rs, error);

function error(err) {
  console.error(err);
}
```

## 译者

[Jordan Wang](https://github.com/mingmingwon/)
