---
title: 前端构建mock-server的几种方式
tags:
  - mock
categories: works
date: 2017-05-02 22:47:29
---

这里收集一些用来构建 mock-server 的方式:

### 什么是 mock-server

目前流行的前后端分离开发方式, 前后端并行开发, 前端部分前期是没有 API 接口可用的, 所以需要按照约定的数据格式构建 mock-server, 尽量保持真实的开发环境.

### mock-server 实现方式

- Mock.js，仅用于单一数据模拟，没有生成文档的功能
- Swagger Editor npm install 时候的几个包弄不定, 文档编写难度大
- FMS - FMS，轻量级， 使用数据模拟时还有些别扭
- RAP v0.11，结合了文档、Mock.js、可视化、Rest、接口过渡、文档修改提醒、支持本地部署
- json-server 基于 nodejs 的 mock-server, 支持 CRUD, 支持动态参数, 2分钟创建

博主只使用过 json-server , 作为一个前端, 我认为这款 GitHub 上 21K star 的工具已经足够用了 :happy .
等有时间翻译出来一篇 json-server 的中文文档.
