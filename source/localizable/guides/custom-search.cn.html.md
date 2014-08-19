---
layout: guides.cn
section: custom-styles
toc: article
title: 使用jQuery定制微搜索搜索结果
---

# 使用jQuery定制微搜索搜索结果

文档中的引擎key来自于[微搜索官方网站](http://tinysou.com/)，可以直接试用。

## 定制搜索返回结果的字段

通过添加 `fetchFields` 字段，可以指定返回结果包含哪些字段。如果不指定这一字段，返回结果默认包含 `title`， `url`， `section`。

示例一

```
$("#ts-search-input"). tinysouSearch({
   engineKey: '97eaafba26b04d3cdeb9',
   resultContainingElement: '#st-results-container'
});
```

示例一结果

```
{
  "info":{
    "query":"搜索",
    "page":0,
    "per_page":5,
    "total":11,
    "max_score":0.41762865
  },
  "records":[
    {
      "collection":"page",
      "score":0.41762865,
      "highlight": {
        "body":[
          "搜索引擎博客设置登出微搜索搜索引擎博客设置登出",
          "© 2014 微搜索苏ICP备12071270号南京自由风暴网络科技有限公司"],
        "sections":[
          "微搜索"],
        "title":["微搜索"]
      },
      "document":{
        "id":"bfa8ab471a86e40055d34585cf89f07d",
        "title":"微搜索",
        "sections":["微搜索"],
        "url":"http://dashboard.tinysou.com/"
        }
    }, {
      "collection":"page",
      "score":0.17096238,
      "highlight": {
        "body":[
          "微搜索首页博客控制台微搜索首页博客控制台微搜索最好用的站内搜索引擎",
          "跨站搜索自动补全安装简单简单3步，给你的站点添加微搜索功能1. 创建搜索引擎2. 添加域名",
          "3. 安装1. 创建搜索擎2. 添加域名3. 安装1 创建搜索引擎2 添加域名3"
          ],
        "sections":[
          "微搜索",
          "微搜索",
          "简单3步，给你的站点添加微搜索功能",
          "创建搜索引擎"
          ],
        "title":["微搜索"]
        },
      "document": {
        "id":"91f676ba1b05116e557cdae67f47c609",
        给你的站点添加微搜索功能1.创建搜索引擎2. 添加域名3. 安装",
        "title":"微搜索",
        "sections": ["微搜索","微搜索","简单3步，给你的站点添加微搜索功能","创建搜索引擎","添加域名","安装","1","2","3"],
        "url":"http://tinysou.com/"
      }
    }
  ],
  "errors": {}
}
```

在 `fetchFields` 中指定返回结果的字段包含 `title`， `url`。

示例二

```
$("#ts-search-input").tinysouSearch({
 engineKey: '97eaafba26b04d3cdeb9',
 fetchFields: ['title', 'url']
});
```

示例二结果

```
{
  "info":{
    "query":"搜索",
    "page":0,
    "per_page":10,
    "total":11,
    "max_score":0.41762865
  },
  "records":[
    {
      "collection":"page",
      "score":0.41762865,
      "highlight":{
        "body":["微搜索搜索引擎博客设置登出微搜索搜索引擎博客设置登出",
        "© 2014 微搜索苏ICP备12071270号南京自由风暴网络科技有限公司"
        ],
        "sections":[
          "微搜索"
        ],
        "title":["微搜索"]
      },
      "document":{
        "id":"bfa8ab471a86e40055d34585cf89f07d",
        "title":"微搜索",
        "url":"http://dashboard.tinysou.com/"
      }
    },{
      "collection":"page",
      "score":0.17096238,
      "highlight":{
        "body":[
          "微搜索首页博客控制台微搜索首页博客控制台微搜索最好用的站内搜索引擎",
          "跨站搜索自动补全安装简单简单3步，给你的站点添加微搜索功能1. 创建搜索引擎2. 添加域名",
          "3. 安装1. 创建搜索擎2. 添加域名3. 安装1 创建搜索引擎2 添加域名3"
        ],
        "title":["微搜索"]
      },
      "document":{
        "id":"91f676ba1b05116e557cdae67f47c609",
        "title":"微搜索",
        "url":"http://tinysou.com/"
      }
    }
  ],
  "errors": {}
}
```

示例二结果的返回结果中只有 `title` 和 `url` 字段（返回结果中都会包含id字段）。

## 指定搜索字段

如果不指定 `searchFields`，默认将会在所有字段中进行搜索。通过指定 `searchFields`，搜索将会限定在这些字段中。

示例三

```
$("#ts-search-input").tinysouSearch({
 engineKey: '97eaafba26b04d3cdeb9',
 fetchFields: ['title', 'url'],
 searchFields: ['title']
});
```

示例三指定了仅在 `title` 字段中进行搜索，只有标题中有搜索词的条目才会呈现在返回结果中。

## 排序

设定 `sort` 可以进行结果排序。如果需要按照 `updated_at` 升序排列，按照示例四指定。

示例四

```
$("#ts-search-input").tinysouSearch({
 engineKey: '97eaafba26b04d3cdeb9',
 fetchFields: ['title', 'url', 'updated_at'],
 sort: {
   field: 'updated_at',
   order: 'asc'
  }
});
```

示例四结果

```
{
  "info":{
    "query":"搜索",
    "page":0,
    "per_page":10,
    "total":11,
    "max_score":0.41762865
  },
  "records":[
    {
      "collection":"page",
      "score":0.41762865,
      "highlight":{
        "body":[
          "搜索引擎博客设置登出微搜索搜索引擎博客设置登出",
          "© 2014 微搜索苏ICP备12071270号南京自由风暴网络科技有限公司"],
        "sections":[
          "微搜索"],
        "title":["微搜索"]
      },
      "document":{
        "id":"bfa8ab471a86e40055d34585cf89f07d",
        "title":"微搜索",
        "updated_at":"2014-08-19T03:00:06.776Z",
        "url":"http://dashboard.tinysou.com/"
      }
    },{
      "collection":"page",
      "score":0.17096238,
      "highlight":{
        "body":[
          "微搜索首页博客控制台微搜索首页博客控制台微搜索最好用的站内搜索引擎",
          "跨站搜索自动补全安装简单简单3步，给你的站点添加微搜索功能1. 创建搜索引擎2. 添加域名",
          "3. 安装1. 创建搜索擎2. 添加域名3. 安装1 创建搜索引擎2 添加域名3"
          ],
        "sections":[
          "微搜索",
          "微搜索",
          "简单3步，给你的站点添加微搜索功能",
          "创建搜索引擎"
          ],
        "title":["微搜索"]},
      "document":{
        "id":"91f676ba1b05116e557cdae67f47c609",
        "title":"微搜索",
        "updated_at":"2014-08-19T03:00:06.620Z",
        "url":"http://tinysou.com/"
      }
    }
  ],
  "errors":{}
}
```
