---
layout: crawler.cn
section: overview
title: 爬虫概述
---

爬虫是微搜索采集网站信息的机器人，当提交域名后，我们的机器人就会去访问该网站，采集页面信息。

通过 robots.txt、Sitemap、白名单和黑名单等方法，用户能指定爬虫爬取和收录的范围。爬虫收录的数据会自动存储到用户的`engine`下自动创建的 'page' `collection`里。每过一段时间，爬虫会重新爬取，对 'page' 进行更新。你也可以在 控制台 -> 你的引擎 -> 管理 里点击 “重新索引” 按钮，爬虫就会对你的网站进行再次索引。


## Schema

爬虫爬过的页面会被存储到用户的`engine`下自动创建的 'page' `collection`里。在 'page' 里有如下`field`。

| 名称    | 类型    | 说明 |
| ------ | ------ | ------------------------------------------------------ |
| updated_at | date | 页面上次被索引的日期。 |
| title | string | 标题，由页面中`<title>`标签指定。 |
| url   | enum | 页面的 URL。|
| sections | string | 由页面中`<h1>`，`<h2>`，`<h3>`，`<h4>`，`<h5>`和`<h6>`标签指定。 |
| body | text | 页面中的`<body>`的文字内容。 |


通过这些`field`，再加上`fetch_fields`和`filter`，你就可以定制搜索啦。详细方法请看[搜索 API][search_api]


[search_api]:http://doc.tinysou.com/v1/searching.html
