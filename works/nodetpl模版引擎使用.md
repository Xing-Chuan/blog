---
title: nodetpl模版引擎使用
tags:
  - 模版引擎
  - nodetpl
categories: works
date: 2017-05-09 22:23:46
---

## nodetpl API

- nodetpl.config 基本配置, 一般使用默认
- nodetpl.get 获取模版或者编译后的模版文件
- nodetpl.render 渲染某个文件
- nodetpl.exec 执行模版里面的 js 代码

### API 参数

1. nodetpl.config(options)

```js
nodetpl.config({
  base: 'http://www.nodetpl.com/static/tpl/',   // 强制指定模板根目录
  openTag: '<?',
  closeTag: '?>',
  strict: true,
  map: function(str) {
    return str;
  },
  beforeCompile: function(html) {
    return html;
  },
  afterCompile: function(html) {
    return html;
  }
});
```
- base：模板所在的根路径，可以省略。若配置该参数，则使用 .get() 方法填写的路径会相对于该路径进行查找
- openTag：模板定界符开始标签，默认为：<?
- closeTag：模板定界符结束标签，默认为：?>
- strict：是否开启严格模式，默认开启
- map：传递一个解析函数（转换器），用来解析其他语法，例如 php、jsp 等
- beforeCompile：编译前的处理函数
- afterCompile：编译后的处理函数

2. nodetpl.get(url, [data, ] callback)

- url 模版文件地址
- data 传入的数据, 值为 false 则暂缓渲染, 传入 NodeTplClass 实例作为 callback 的参数, 需要渲染时, 使用 tpl.render(data).
- callback 接收渲染后的 html 作为参数

3. nodetpl.render(tplcontent[, data][, callback]);

- tplcontent 需要渲染的 HTML 结构
- data 传入的数据
- callback 接收渲染后的 HTML 结构为参数

4. nodetpl.exec(content);

- 执行 HTML 结构中的 JavaScript 代码.

## nodetpl 预编译

需要安装 node 模块: 

```bash
npm i -g nodetpl
```

使用语法:

```bash
nodetpl <path> --extname <extname> --encoding <encoding> --nostrict --watch
```

- `<path>`：必选，模板文件的路径，或模板所在的目录路径
- `--extname`：可选，模板文件的扩展名，默认为 .tpl
- `--encoding`：可选，模板的编码格式，默认为 utf-8，可选值：utf-8、gbk
- `--nostrict`：可选，启用非 strict 模式，不推荐传递该参数
- `--watch`：可选，是否监控文件变化，默认为 false，当传递该参数时，修改模板内容会同步自动编译

