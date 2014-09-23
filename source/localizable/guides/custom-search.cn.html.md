---
layout: guides.cn
section: custom-search
toc: article
title: 使用 jQuery 定制微搜索搜索结果
---

文档中的engineKey来自于[微搜索官方网站](http://tinysou.com/)，可以直接试用。

## 总述

微搜索的所有搜索和自动补全行为，由 `jquery.tinysou.js` 完成，其初始化方式如下：

```javascript
var option = {
  engineKey: '97eaafba26b04d3cdeb9',
  searchInputElement: '#search-input'
};
(function(w,d,t,u,n,s,e){
  s = d.createElement(t);
  s.src = u;
  s.async = 1;
  w[n] = function(r){
    w[n].opts = r;
  };
  e = d.getElementsByTagName(t)[0];
  e.parentNode.insertBefore(s, e);
})(window,document,'script','//tinysou-cdn.b0.upaiyun.com/ts.js','_ts');
_ts(option);
```

其中 `engineKey` 为一个配置选项，之后的匿名函数会加载搜索插件需要的文件。tinysouSearch 还包含众多其他的配置选项，例如`perPage`，`sort`等等。不指定`searchInputElement`，默认值为`#ts-search-input`，如果你想使用别的`id`，可以通过指定`searchInputElement`。

下文中将为大家介绍几个主要的选项：

## 定制自动补全的显示行为

默认的自动补全显示函数如下：

```javascript
var defaultRenderActFunction = function(item) {
  return '<p class="title">' + TinySou.htmlEscape(item['document']['title']) + '</p>';
};
```

其效果如下：

![default-autocomplete-display](default-autocomplete-display.png)

其中`item`变量的结构如下:

```json
{
  "collection":"page",
  "score":0.18271253,
  "highlight":{
    "title":["微搜索上线<em>内测</em>"],
    "sections":["微搜索上线<em>内测</em>"],
    "body":["     微搜索上线<em>内测</em>            2014 / 8 / 9    Michael Ding    公告     我们很高兴地在这里宣布，今天，微搜索正式开始上线<em>内测</em>了！  微搜索致力于为","微搜索致力于为大家提供一个易于安装又特性丰富的站内搜索引擎。本次的<em>内测</em>版本，跨站点全文搜索，并提供搜索框的下拉式自动补全。而整个过程只需要简单的三步：创建引擎，添加站点地址，复制代码到 html。  <em>内测</em>阶段需要注册码进行注册，想试用的朋友欢迎发邮件给我
们索取注册码。"]
  },
  "document":{
    "id":"bd44321fc4d76c6a01b36ec89d51fd58",
    "title":"微搜索上线内测",
    "sections":["微搜索官方博客","微搜索官方博客","微搜索上线内测"],
    "url":"http://blog.tinysou.com/cn/2014/08/09/%E5%BE%AE%E6%90%9C%E7%B4%A2%E4%B8%8A%E7%BA%BF%E5%86%85%E6%B5%8B.html"
  }
}
```

如果需要不同的自动补全显示行为(例如选择显示不同的`field`)，可以利用`renderActFunction`选项覆盖此函数，方法如下:

```javascript
var customRenderActFunction = function(item) {
  var out = '<p class="title">' + item['document']['title'] + '</p>';
  out = out.concat('<p class="url">' + item['document']['sections'][0] + '</p>');
  return out;
};
var option = {
  engineKey: '97eaafba26b04d3cdeb9',
  renderActFunction: customRenderActFunction
};
```

## 定制搜索的显示行为

默认的搜索显示函数如下：

```javascript
var defaultRenderFunction = function (item) {
  var resultTemplate = Hogan.compile('<div class="ts-result"><h3 class="title"><a href="{{url}}" class="ts-search-result-link">{{title}}</a></h3><div class="ts-metadata"><span class="ts-snippet">{{{body}}}</span></div></div>');
  var data = {
    title: item['document']['title'],
    url: item['document']['url'],
    body: (item.highlight && item.highlight['body']) || item['document']['sections'].join(',')
  };
  return resultTemplate.render(data);
};
```

其效果如下：

![default-search-display](default-search-display.png)

其中`item`变量的结构和上一节中的一致。

如果需要不同的搜索显示行为(例如选择显示不同的`field`)，可以利用`renderFunction`选项覆盖此函数，方法如下:

```javascript
var customRenderFunction = function(item) {
  var out = '<p class="title">' + item['document']['title'] + '</p>';
  out = out.concat('<p class="url">' + item['document']['sections'][0] + '</p>');
  return out;
};
var option = {
  engineKey: '97eaafba26b04d3cdeb9',
  renderFunction: customRenderFunction
};
```

## 定制搜索和自动补全行为

### 启用或禁用自动补全

`disableAutocomplete` 指定是否禁用自动补全。禁用为`true`，启用为`false`。默认值为`false`。

### 设定分页选项

`perPage`指定搜索时每页返回的结果个数，默认值为`10`，最大值为`100`。

`resultLimit`指定自动补全时每页返回的结果个数，默认值为`5`，最大值为`100`。

### 设定搜索目标`collection`

关于`collection`的概念，可参见'[基本概念][concept]'

可以通过`collection`选项指定搜索的目标`collection`，可以指定多个collection，方法是多个`collection` 名之间用 ‘,’ 分隔，例如:

```
var option = {
  engineKey: '97eaafba26b04d3cdeb9',
  collection: 'page,book'
};
```

`collection`选项默认值为`'page'`

### 返回结果包含的`field`

`fetchFields`指定返回结果中，每条记录包含的 `field`。即上文 `item` 变量中 `document` 包含的属性值。

默认值为`['title', 'url', 'sections']`

例如

```javascript
var option = {
  engineKey: '97eaafba26b04d3cdeb9',
  fetchFields: ['title', 'url', 'sections', 'body']
};
```

`item`变量则变为：

```json
{
  "collection":"page",
  "score":0.18271253,
  "highlight":{
    "title":["微搜索上线<em>内测</em>"],
    "sections":["微搜索上线<em>内测</em>"],
    "body":["     微搜索上线<em>内测</em>            2014 / 8 / 9    Michael Ding    公告     我们很高兴地在这里宣布，今天，微搜索正式开始上线<em>内测</em>了！  微搜索致力于为","微搜索致力于为大家提供一个易于安装又特性丰富的站内搜索引擎。本次的<em>内测</em>版本，跨站点全文搜索，并提供搜索框的下拉式自动补全。而整个过程只需要简单的三步：创建引擎，添加站点地址，复制代码到 html。  <em>内测</em>阶段需要注册码进行注册，想试用的朋友欢迎发邮件给我
们索取注册码。"]
  },
  "document":{
    "id":"bd44321fc4d76c6a01b36ec89d51fd58",
    "title":"微搜索上线内测",
    "sections":["微搜索官方博客","微搜索官方博客","微搜索上线内测"],
    "url":"http://blog.tinysou.com/cn/2014/08/09/%E5%BE%AE%E6%90%9C%E7%B4%A2%E4%B8%8A%E7%BA%BF%E5%86%85%E6%B5%8B.html",
    "body": "     微搜索上线内测            2014 / 8 / 9    Michael Ding    公告     我们很高兴地在这里宣布，今天，微搜索正式开始上线内测了！  微搜索致力于为","微搜索致力于为大家提供一个易于安装又特性丰富的站内搜索引擎。本次的内测版本，跨站点全文搜索，并提供搜索框的下拉式自动补全。而整个过程只需要简单的三步：创建引擎，添加站点地址，复制代码到 html。  内测阶段需要注册码进行注册，想试用的朋友欢迎发邮件给我
们索取注册码。"
  }
}
```


[concept]:/guides/concept.html
