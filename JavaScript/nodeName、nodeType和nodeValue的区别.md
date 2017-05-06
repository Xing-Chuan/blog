---
title: nodeName、nodeType和nodeValue的区别
tags:
  - nodeName
  - nodeType
  - nodeValue
categories: JavaScript
date: 2017-05-02 22:36:43
---

### nodeName

nodeName 包含节点的名称

- 元素节点 -> 标签名称
- 属性节点 -> 属性名称
- 文本节点 -> #text
- 文档节点 -> #document

PS: nodeName 所包含的 XML 元素的标签名称永远是大写的.

### nodeType

nodeType 属性可返回节点的类型

常用的节点类型:

|    元素类型   | 节点类型 |
|---------------|----------|
| 元素 element  |        1 |
| 属性 attr     |        2 |
| 文本 text     |        3 |
| 注释 comments |        8 |
| 文档 document |        9 |

### nodeValue

nodeValue 属性对于文档节点和元素节点是不可用的.

- 对于文本节点 -> nodeValue 为文本
- 对于属性节点 -> nodeValue 为属性值
