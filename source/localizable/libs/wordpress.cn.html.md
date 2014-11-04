---
layout: libs.cn
section: wordpress
toc: article
title: WordPress 插件
---

## 总述

微搜索的 WordPress 插件—— TinySou Search 能帮助用户将博客内的文章数据提交到微搜索服务器，让用户可以快速使用微搜索来为自己的网站添加搜索功能。下载可到WordPress官网下载 [TinySou Search](https://wordpress.org/plugins/tinysou-search/)，也可以直接点击[下载](https://downloads.wordpress.org/plugin/tinysou-search.zip)。所有源码托管在 GitHub， 开发者们可前往查看[最新版本][github]。

## 准备

您需要首先登录微搜索（没有帐号的用户需要先[注册][signup]），进入控制台界面。请点击左上角，显示设置的按钮，点击进入设置页面能找到用户自己的 Authen Token，这个是使用插件的必要条件。

## 安装

1. 在线安装或将插件上传至 WordPress 的插件目录中并在后台激活。

2. 接着，在TinySou插件配置页面 输入你的 Authen Token.

3. 在创建engine页面输入 engine name.

4. 点击提交按钮,将文章提交至 TinySou 服务器即可.

[signup]:http://dashboard.tinysou.com/signup
[github]:https://github.com/tinysou/tinysou-wordpress