---
title: encodeURI和encodeURIComponent
tags:
  - 编码
categories: JavaScript
date: 2017-06-22 15:45:35
---

URL是统一命名的网络资源, 是为了支持网络所有协议使用而诞生的.

需要满足以下 3 个特点:

- 移植性 (支持各种网络协议)
- 完整性 (不会丢失数据)
- 阅读性 (语义化结构)

为了满足以上特点, 设计者引入了转义序列, 以 ACSII 的有限子集转换对任意数据和字符进行编码, 如此的话, 只要支持 ACSII 码的设备都可以根据映射表来使用.

JavaScript 中进行编码和解码工作的是 encodeURI/decodeURI 和 encodeURIComponent/decodeURIComponent

### encodeURI/decodeURI

语法:

```bash
encodeURI(URIString);
```

返回值:

返回以转义序列替换的 URISting 副本

说明:

该方法不会替换 ACSII 中的字母和数字, 也不会替换 ACSII 的标点符号 `- _ . ! ~ * ' ( )`

编码效果展示:

```js
<script type="text/javascript">
console.log(encodeURI("http://www.w3school.com.cn")+ "<br />");
console.log(encodeURI("http://www.w3school.com.cn/My first/"));
console.log(encodeURI(",/?:@&=+$#"));
</script>
```

```
http://www.w3school.com.cn
http://www.w3school.com.cn/My%20first/
,/?:@&=+$#
```

解码:

```
decodeURI
```

### encodeURIComponent/decodeURIComponent

语法:

```bash
encodeURIComponent(URIstring)
```

返回值:

返回以转义序列替换的 URISting 副本

说明:

该方法不会替换 ACSII 中的字母和数字, 也不会替换 ACSII 的标点符号 `- _ . ! ~ * ' ( )`.

`;/?:@&=+$,#` 这些用于分割 URL 的标点符号, 会被转义.

编码效果展示:

```js
<script type="text/javascript">
console.log(encodeURIComponent("http://www.w3school.com.cn"));
console.log(encodeURIComponent("http://www.w3school.com.cn/p 1/"));
console.log(encodeURIComponent(",/?:@&=+$#"));
</script>
```

```
http%3A%2F%2Fwww.w3school.com.cn
http%3A%2F%2Fwww.w3school.com.cn%2Fp%201%2F
%2C%2F%3F%3A%40%26%3D%2B%24%23
```

解码:

```
decodeURIComponent
```


### 参考

- HTTP 权威指南
- w3cschool
