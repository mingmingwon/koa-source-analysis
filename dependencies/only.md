# only <sup>[![Version Badge](http://versionbadg.es/tj/node-only.svg)](https://www.npmjs.com/package/vary)</sup>

返回对象的白名单属性。

## 安装

```sh
$ npm install only
```

## API

可以使用空格分隔的字符串：

```js
var obj = {
  name: 'tobi',
  last: 'holowaychuk',
  email: 'tobi@learnboost.com',
  _id: '12345'
};

var user = only(obj, 'name last email');
```

还可以使用数组：

```js
var user = only(obj, ['name', 'last', 'email']);
```

结果：

```js
{
  name: 'tobi',
  last: 'holowaychuk',
  email: 'tobi@learnboost.com'
}
```
## 译者

[Jordan Wang](https://github.com/mingmingwon/)
