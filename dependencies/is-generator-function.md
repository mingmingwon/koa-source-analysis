# is-generator-function <sup>[![Version Badge](http://versionbadg.es/ljharb/is-generator-function.svg)](https://www.npmjs.com/package/is-generator-function)</sup>

[![浏览器支持情况](https://camo.githubusercontent.com/0524cf4e0849af89f8a8710c3ffc136301388412/68747470733a2f2f63692e746573746c696e672e636f6d2f6c6a686172622f69732d67656e657261746f722d66756e6374696f6e2e706e67)](https://ci.testling.com/ljharb/is-generator-function)

判断函数是否为原生 generator 函数。

## 例子

```js
var isGeneratorFunction = require('is-generator-function');
assert(!isGeneratorFunction(function () {})); // true
assert(!isGeneratorFunction(null)); // true
assert(isGeneratorFunction(function* () { yield 42; return Infinity; })); // true
```

## 测试
克隆[本仓库](https://github.com/ljharb/is-generator-function)，执行 `npm install`，然后 `npm test`。
