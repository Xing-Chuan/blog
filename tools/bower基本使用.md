---
title: bower基本使用
tags: [bower,包管理工具]
date: 2016-4-29 13:25:26
categories: tools
---



### bower是什么?

bower是基于nodejs的静态资源管理工具,由twitter公司开发、维护,使用它可以方便的安装、更新、卸载前端类库,同时解决类库之前的依赖关系.

### 依赖环境

bower依赖于nodejs和git,bower是通过内置的映射表从GitHub上下载类库到本地

### 安装

打开Window命令条窗口,输入以下命令,完成全局安装,`-g`是全局(global)安装的意思

```
npm install -g bower
```

显示版本号则安装成功

```
bower -v
```

输入上面的指令查看bower当前版本号,我的是"1.8.0".

### 映射表查询

如果我们想下载jquery-cookie,但不知道bower内置的映射表对应的名称,可以通过以下git指令查询

```
$bower search jquery-cookie
```

git窗口会显示以下信息,这就是bower内部设置的映射关系

```
Search results:

    jquery.cookie https://github.com/carhartl/jquery-cookie.git
    jquery-cookie https://github.com/carhartl/jquery-cookie.git
    hg-jquery-cookie https://github.com/hackergaucho/hg-jquery-cookie.git
    jquery-cookie-consent https://github.com/smichaelsen/jquery-cookie-consent.git
    jquery-cookie-banner https://github.com/alberon/jquery-cookie-banner.git
    jquery-cookie-alerter https://github.com/siliconsalad/jquery-cookie-alerter.git
    jaaulde-jquery-cookies https://github.com/JAAulde/jquery-cookies.git
    jquery-cookie-disclaimer https://github.com/Gix075/cookieDisclaimer.git

```

### 类库本地安装

知道了映射关系,我们就可以安装类库了

```
$ bower install jquery-cookie
```

在打开git的路径下,自动创建bower_components文件夹,下载jquery-cookie和它依赖的库jquery

### 类库卸载

通过uninstall命令卸载

```
$ bower uninstall jquery-cookie
```

### 查询类库历史版本

```
$ bower info jquery
```

git窗口会显示类库详细的历史版本号,默认安装是最新版本

```
{
  name: 'jquery',
  main: 'dist/jquery.js',
  license: 'MIT',
  ignore: [
    'package.json'
  ],
  keywords: [
    'jquery',
    'javascript',
    'browser',
    'library'
  ],
  homepage: 'https://github.com/jquery/jquery-dist',
  version: '3.2.1'
}

Available versions:
  - 3.2.1
  - 3.2.0
  - 3.1.1
  - 3.1.0
  - 3.0.0
  - 2.2.4
  - 2.2.3
  - 2.2.2
  - 2.2.1
  - 2.2.0
  - 2.1.4
  - 2.1.3
  - 2.1.2
  - 2.1.1
  - 2.1.0
  - 2.0.3
  - 2.0.2
  - 2.0.1
  - 2.0.0
  - 1.12.4
  - 1.12.3
  - 1.12.2
  - 1.12.1
  - 1.12.0
  - 1.11.3
  - 1.11.2
  - 1.11.1
  - 1.11.0
  - 1.10.2
  - 1.10.1
  - 1.10.0
  - 1.9.1
  - 1.9.0
  - 1.8.3
  - 1.8.2
  - 1.8.1
  - 1.8.0
  - 1.7.2
  - 1.7.1
  - 1.7.0
  - 1.6.4
  - 1.6.3
  - 1.6.2
  - 1.6.1
  - 1.6.0
  - 1.5.2
  - 1.5.1
  - 1.5.0
  - 1.4.4
  - 1.4.3
  - 1.4.2
  - 1.4.1
  - 1.4.0
  - 1.3.2
  - 1.3.1
  - 1.3.0
  - 1.2.6
  - 1.2.5
  - 1.2.4
  - 1.2.3
  - 1.2.2
  - 1.2.1
  - 1.1.4
  - 1.1.3
  - 1.1.2
  - 1.1.1
  - 1.0.4
  - 1.0.3
  - 1.0.2
  - 1.0.1
```

我们也可以安装指定版本的类库

```
$ bower install jquery#1.1.2
```

### 小结

有了bower后,我们不用费心去找各种版本的类库了