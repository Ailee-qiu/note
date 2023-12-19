classNames

[classnames中文文档|classnames js中文教程|解析 | npm中文文档 (npmdoc.org)](http://www.npmdoc.org/classnameszhongwenwendangclassnames-jszhongwenjiaochengjiexi.html)

[React使用 classnames 让你的代码更优雅 - 掘金 (juejin.cn)](https://juejin.cn/post/7194289762209890361)

````bash
# via npm
npm install classnames

# via Bower
bower install classnames

yarn add classnames
````

`classnames` 简单的说就是一个把多个className链接起来的工具



该`classNames`函数接受任意数量的参数，可以是字符串或对象。该参数`'foo'`是 的缩写`{ foo: true }`。如果与给定键关联的值是假的，则该键将不会包含在输出中。

````js
classNames('foo', 'bar'); // => 'foo bar'
classNames('foo', { bar: true }); // => 'foo bar'
classNames({ 'foo-bar': true }); // => 'foo-bar'
classNames({ 'foo-bar': false }); // => ''
classNames({ foo: true }, { bar: true }); // => 'foo bar'
classNames({ foo: true, bar: true }); // => 'foo bar'

// lots of arguments of various types
classNames('foo', { bar: true, duck: false }, 'baz', { quux: true }); // => 'foo bar baz quux'

// other falsy values are just ignored
classNames(null, false, 'bar', undefined, 0, 1, { baz: null }, ''); // => 'bar 1'
````

数组将按照上述规则递归展平：

```js
var arr = ['b', { c: true, d: false }];
classNames('a', arr); // => 'a b c'
```

如果你的开发环境支持 `ES5`，类名也可以动态化：

````js
let buttonType = 'primary';
classNames({ [`btn-${buttonType}`]: true });
````

