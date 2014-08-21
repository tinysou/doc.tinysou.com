---
layout: guides.cn
section: opensearch
toc: article
title: 添加 opensearch 支持
---

## 什么是 Opensearch
用户访问某个网站，如果该网站具备搜索功能，并提供遵循 OpenSearch 规范的代码，那么支持 OpenSearch 功能的浏览器就能够进行快捷搜索。
比如，Chrome 浏览器地址栏输入 github，再按 tab 键即可输入搜索内容。

![search github](search-github.png)

## 如何为你的网站添加 Opensearch

### 1. 确认网站具有搜索功能

通过添加微搜索插件，使网站具有搜索功能，并提供一个搜索的页面,如

```
http://doc.tinysou.com/search.html#tsq={searchTerms}&tsp=0
```

### 2.添加 XML 描述文件

在网站的根目录下，添加 xml 文件，比如 `opensearch.xml` 。

```
<OpenSearchDescription xmlns="http://a9.com/-/spec/opensearch/1.1/" xmlns:moz="http://www.mozilla.org/2006/browser/search/">
  <ShortName> Tinysou Doc</ShortName>
  <Description>Search 微搜索文档</Description>
  <InputEncoding>UTF-8</InputEncoding>
  <Url type="text/html" method="get" template="http://doc.tinysou.com/search.html#tsq={searchTerms}&amp;tsp=0"/>
</OpenSearchDescription>
```

### 3.引用 XML 描述文件

在 HTML 的 head 标签中添加如下代码。

```
<link type="application/opensearchdescription+xml" href="opensearch.xml" title="Tinysou Doc" rel="search" />
```

按照上面三步，网站就成功添加了 Opensearch 功能。
