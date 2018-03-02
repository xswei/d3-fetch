# d3-fetch

这个模块基于[Fetch](https://fetch.spec.whatwg.org/)之上添加了解析功能。例如加载一个 text 文件:

```js
d3.text("/path/to/file.txt").then(function(text) {
  console.log(text); // Hello, world!
});
```

加载并转换 CSV 文件:

```js
d3.csv("/path/to/file.csv").then(function(data) {
  console.log(data); // [{"Hello": "world"}, …]
});
```

这个模块内部支持[JSON](#json), [CSV](#csv), and [TSV](#tsv)，你也可以直接对文本使用其他形式的解析(这个方法替换了[d3-request](https://github.com/d3/d3-request)模块)。

## Installing

使用NPM安装:`npm install d3-fetch`. 下载[latest release](https://github.com/d3/d3-fetch/releases/latest). 直接从[d3js.org](https://d3js.org)以[standalone library](https://d3js.org/d3-fetch.v1.min.js)的形式加载. 支持 AMD, CommonJS, 和 原始标签用法。使用原始方法时会全局暴露 `d3` 变量:

```html
<script src="https://d3js.org/d3-dsv.v1.min.js"></script>
<script src="https://d3js.org/d3-fetch.v1.min.js"></script>
<script>

d3.csv("/path/to/file.csv").then(function(data) {
  console.log(data); // [{"Hello": "world"}, …]
});

</script>
```

## API Reference

<a name="blob" href="#blob">#</a> d3.<b>blob</b>(<i>input</i>[, <i>init</i>]) [<源码>](https://github.com/d3/d3-fetch/blob/master/src/blob.js "Source")

从指定的 `input` URL 以 Blob 的形式获取二进制文件。如果指定了 *init* 则会将其传递给底层的 [fetch](https://fetch.spec.whatwg.org/#fetch-method) 方法；参考 [RequestInit](https://fetch.spec.whatwg.org/#requestinit) 获取允许的字段。

<a name="buffer" href="#buffer">#</a> d3.<b>buffer</b>(<i>input</i>[, <i>init</i>]) [<源码>](https://github.com/d3/d3-fetch/blob/master/src/buffer.js "Source")

从指定的 `input` URL 以 ArrayBuffer 的形式获取二进制文件。如果指定了 *init* 则会将其传递给底层的 [fetch](https://fetch.spec.whatwg.org/#fetch-method) 方法；参考 [RequestInit](https://fetch.spec.whatwg.org/#requestinit) 获取允许的字段。

<a name="csv" href="#csv">#</a> d3.<b>csv</b>(<i>input</i>[, <i>init</i>][, <i>row</i>]) [<源码>](https://github.com/d3/d3-fetch/blob/master/src/dsv.js "Source")

等价于以逗号作为分隔符的 [d3.dsv](#dsv)。

<a name="dsv" href="#dsv">#</a> d3.<b>dsv</b>(<i>delimiter</i>, <i>input</i>[, <i>init</i>][, <i>row</i>]) [<源码>](https://github.com/d3/d3-fetch/blob/master/src/dsv.js "Source")

从指定的 `input` URL 获取 [DSV](https://github.com/d3/d3-dsv) 文件。如果指定了 *init* 则会将其传递给底层的 [fetch](https://fetch.spec.whatwg.org/#fetch-method) 方法；参考 [RequestInit](https://fetch.spec.whatwg.org/#requestinit) 获取允许的字段。可选的 *row* 转换函数可以被用来将行对象转换为更具体的形式；参考 [*dsv*.parse](https://github.com/d3/d3-dsv#dsv_parse) 获取更多细节。例如:

```js
d3.dsv(",", "test.csv", function(d) {
  return {
    year: new Date(+d.Year, 0, 1), // convert "Year" column to Date
    make: d.Make,
    model: d.Model,
    length: +d.Length // convert "Length" column to number
  };
}).then(function(data) {
  console.log(data);
});
```

如果只指定了 *init* 和 *row* 中的其中一个，如果是函数类型则将其视为 *row* 转换函数，否则将其视为 *init* 对象。

参考 [d3.csv](#csv) 和 [d3.tsv](#tsv).

<a name="html" href="#html">#</a> d3.<b>html</b>(<i>input</i>[, <i>init</i>]) [<源码>](https://github.com/d3/d3-fetch/blob/master/src/xml.js "Source")

从指定的 `input` URL 以 [text](#text) 获取文件然后 [parses it(将其转换为)](https://developer.mozilla.org/docs/Web/API/DOMParser) HTML。如果指定了 *init* 则会将其传递给底层的 [fetch](https://fetch.spec.whatwg.org/#fetch-method) 方法；参考 [RequestInit](https://fetch.spec.whatwg.org/#requestinit) 获取允许的字段。

<a name="image" href="#image">#</a> d3.<b>image</b>(<i>input</i>[, <i>init</i>]) [<源码>](https://github.com/d3/d3-fetch/blob/master/src/image.js "Source")

从指定的 `input` URL 获取图片。如果指定了 *init* 则表示可以在图片加载之前设置任何其他属性。例如如果要启用匿名 [cross-origin request(跨域请求)](https://developer.mozilla.org/en-US/docs/Web/HTML/CORS_enabled_image) 则需要执行以下操作:

```js
d3.image("https://example.com/test.png", {crossOrigin: "anonymous"}).then(function(img) {
  console.log(img);
});
```

<a name="json" href="#json">#</a> d3.<b>json</b>(<i>input</i>[, <i>init</i>]) [<源码>](https://github.com/d3/d3-fetch/blob/master/src/json.js "Source")

从指定的 `input` URL 获取 [JSON](http://json.org) 文件。如果指定了 *init* 则会将其传递给底层的 [fetch](https://fetch.spec.whatwg.org/#fetch-method) 方法；参考 [RequestInit](https://fetch.spec.whatwg.org/#requestinit) 获取允许的字段。

<a name="svg" href="#svg">#</a> d3.<b>svg</b>(<i>input</i>[, <i>init</i>]) [<源码>](https://github.com/d3/d3-fetch/blob/master/src/xml.js "Source")

从指定的 `input` URL 获取以[text](#text) 获取文件然后 [parses it(将其转换)](https://developer.mozilla.org/docs/Web/API/DOMParser) 为 SVG。 如果指定了 *init* 则会将其传递给底层的 [fetch](https://fetch.spec.whatwg.org/#fetch-method) 方法；参考 [RequestInit](https://fetch.spec.whatwg.org/#requestinit) 获取允许的字段。

<a name="text" href="#text">#</a> d3.<b>text</b>(<i>input</i>[, <i>init</i>]) [<源码>](https://github.com/d3/d3-fetch/blob/master/src/text.js "Source")

从指定的 `input` URL 获取 text 文件。如果指定了 *init* 则会将其传递给底层的 [fetch](https://fetch.spec.whatwg.org/#fetch-method) 方法；参考 [RequestInit](https://fetch.spec.whatwg.org/#requestinit) 获取允许的字段。

<a name="tsv" href="#tsv">#</a> d3.<b>tsv</b>(<i>input</i>[, <i>init</i>][, <i>row</i>]) [<源码>](https://github.com/d3/d3-fetch/blob/master/src/dsv.js "Source")

等价于以 tab 字符作为分隔符的 [d3.dsv](#dsv)。

<a name="xml" href="#xml">#</a> d3.<b>xml</b>(<i>input</i>[, <i>init</i>]) [<源码>](https://github.com/d3/d3-fetch/blob/master/src/xml.js "Source")

从指定的 `input` URL 获取以[text](#text) 获取文件然后 [parses it(将其转换)](https://developer.mozilla.org/docs/Web/API/DOMParser) 为 XML。如果指定了 *init* 则会将其传递给底层的 [fetch](https://fetch.spec.whatwg.org/#fetch-method) 方法；参考 [RequestInit](https://fetch.spec.whatwg.org/#requestinit) 获取允许的字段。
