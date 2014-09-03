---
layout: crawler.cn
section: rules
toc: article
title: 爬虫规则
---


## robots.txt

微搜索遵循 [robots.txt][robots]。如果你不希望微搜索爬某些页面，如 `/sample-directory` 下面的页面，只需在`robots.txt`里声明：

```
User-agent: Tinysoubot
Disallow: /sample-directory/
```


## Sitemap

微搜索支持 [Sitemap][sitemap]。`Sitemap`能帮助微搜索更好地进行收录。推荐在 robots.txt 中指定。

```
Sitemap: http://www.example.com/sitemap.xml
```


## 白名单和黑名单

白名单用来限定爬虫只收录 URL 符合一系列规则的网页。

黑名单限定爬虫不收录 URL 符合一系列规则的网页。

规则采用正则表达式语法。

若同时指定白名单和黑名单，将优先判定白名单，若不在白名单中则不收录，再判定黑名单，若在黑名单中则不收录，否则才收录。

**注意**即使指定了黑白名单，爬虫依旧会访问页面，但是只用来发现新的 URL，而不索引内容。若不想让爬虫访问，请在 robots.txt 中指定。

黑、白名单的设置位置在 控制台 -> 你的引擎 -> 管理。请确保你已经[注册][signup]，并添加了至少一个域名。

以下是一些黑白名单的使用示例：

### 过滤子站

白名单

```
http://blog.example.com
http://doc.example.com
http://example.com
```

### 过滤重复页面

黑名单

```
http://example.com/tags
http://example.com/calendar
```

### 过滤特定页面

黑名单

```
http://example.com/calendar/2013/\d+
```

[signup]:http://dashboard.tinysou.com/signup
[robots]:http://www.robotstxt.org/
[sitemap]:http://www.sitemaps.org/protocol.html
