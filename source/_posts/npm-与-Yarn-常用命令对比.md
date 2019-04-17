---
title: npm 与 Yarn 常用命令对比
date: 2017-06-11 18:57:10
tags: [Yarn, npm]
categories: 前端
---

现在基本上都在用yarn代替npm，管理包比较方便，现记录常用命令。

<!-- more -->

| 作用  | npm        | Yarn           |
| -------------  | ------------- |:-------------:|
| 安装 | npm install(i)      | yarn |
| 卸载 | npm uninstall(un)      | yarn remove |
| 全局安装 | npm install xxx –-global(-g)      | yarn global add xxx |
| 安装包 | npm install xxx --save(-S)     | yarn add xxx      |
| 开发模式安装包 | npm install xxx –save-dev(-D) | yarn add xxx --dev(-D)   |
| 更新 | npm update --save | yarn upgrade   |
| 全局更新 | npm update --global| yarn global upgrade|
| 卸载 | npm uninstall [--save/--save-dev] | yarn remove xx   |
| 清除缓存 | npm cache clean | yarn cache clean   |
| 重装 | rm -rf node_modules && npm install | yarn upgrade   |


-----------

<center><img src="https://subscription-1255463026.cos.ap-guangzhou.myqcloud.com/subscription.png" width="180" ></center>

