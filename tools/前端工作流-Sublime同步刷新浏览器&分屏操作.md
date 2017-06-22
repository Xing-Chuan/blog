---
title: 前端工作流-Sublime同步刷新浏览器&分屏操作
tags:
  - sublime
  - tools
  - 自动化
categories: tools
date: 2017-06-22 12:54:38
---

### 电脑环境

- Mac pro 10.12.5
- chrome 浏览器

最近更换了 Mac 本, 在尝试一些提高工作效率的流程和工具, 今天分享一下 sublime 的使用技巧.

### Sublime 保存浏览器同步刷新

安装 Sublime 插件, 不通过 package control 安装, 需要通过 git 克隆, 网友说直接安装的有问题, 具体没有细究.

插件官方地址: https://github.com/alepez/LiveReload-sublimetext3



Mac 安装方法:

1. 执行以下命令:

```bash
cd ~/Library/Application\ Support/Sublime\ Text\ 3/Packages/
rm -rf LiveReload
git clone https://github.com/alepez/LiveReload-sublimetext3 LiveReload
```

安装插件成功.

2. commond + shift + p 启动 sublime 命令行 (LiveReload:enable/disable plug-ins => Enable - simple reload)

3. 在谷歌应用商店安装 livereload 插件

4. 在 chrome 点击 livereload 按钮, 图标中间小圆点变成实心即成功.(点击需在你想要同步刷新的页面完成)

### Mac 同窗口分屏操作

win 窗口左右分屏有很方便的 win + 左箭头/右箭头

Mac 其实也有类似功能, 点击窗口左上角最大化按钮, 持续 2s 以上, 即可拖动到左边窗口/右边窗口, 点击现存的窗口即可填充另一边的窗口.