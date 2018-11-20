# escape-html <sup>[![Version Badge](http://versionbadg.es/component/escape-html.svg)](https://www.npmjs.com/package/escape-html)</sup>

转义 HTML 字符串。

该模块导出一个用于转义字符串的函数 `escapeHtml` ，以便可以在 HTML 内容中进行插值。

该模块将转义以下字符：`"`，`'`，`&`，`<` 和 `>`。

**注意** 转义值仅适用于被插入 HTML 作为元素的文本内容，其内的标签转义机制也相同（它不能放在 `<style>` 或 `<script>` 中，因为这些内容主体不是 HTML，而分别是 CSS 和 JavaScript；这些在 HTML 标准中被称为“原始文本元素”）。

**注意** 在标签内使用转义值时，仅适用于属性的值，其值使用双引号字符（`"`）或单引号字符（`'`）引住。

## 示例

```js
var escapeHtml = require('escape-html');
var html = escapeHtml('foo & bar');
// -> foo &amp; bar
```

## Benchmark 

```
$ npm run-script bench

> escape-html@1.0.3 bench nodejs-escape-html
> node benchmark/index.js


  http_parser@1.0
  node@0.10.33
  v8@3.14.5.9
  ares@1.9.0-DEV
  uv@0.10.29
  zlib@1.2.3
  modules@11
  openssl@1.0.1j

  1 test completed.
  2 tests completed.
  3 tests completed.

  no special characters    x 19,435,271 ops/sec ±0.85% (187 runs sampled)
  single special character x  6,132,421 ops/sec ±0.67% (194 runs sampled)
  many special characters  x  3,175,826 ops/sec ±0.65% (193 runs sampled)
```

## 译者

[Jordan Wang](https://github.com/mingmingwon/)
