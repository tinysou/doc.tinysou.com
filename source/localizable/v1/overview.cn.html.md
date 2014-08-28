---
layout: api.cn
section: overview
toc: article
title: API 概述
---

微搜索提供一套完整的 API，涵盖从创建引擎，索引资源到搜索的方方面面。

## 搜索流程

搜索的流程简单来说分为两部分：索引和搜索。

在索引环节，需要将待搜索的`资源`提交到搜索引擎中，搜索引擎对其做相应的处理。例如要对博客里的文章提供搜索功能，需要将博客里的文章提交到搜索引擎中。我们把** "将待搜索的`资源`提交到搜索引擎中"** 的过程称为 `索引资源`，简称`索引`(`indexing`)

在搜索环节，向搜索引擎提交要搜索的内容，比如一些关键词，搜索引擎会在前一步中索引的资源里进行搜索，返回符合搜索要求的`资源`，比如返回一系列文章。值得注意的是，`自动补全`是一种特殊的`搜索`，在不引起歧义的情况下，我们所说的 `搜索` 包含 `自动补全`。

## 资源

微搜索的资源组织结构如下图所示：

![资源组织结构][resource_structure]

### engine

`engine`是资源组织结构中最顶层的概念，类似于 mysql 中的 database，mongodb 中的 db, s3中的 bucket。

每个`engine` 有1个`name`，为全局唯一，可由字母，数字和 ‘-’ 构成。另外每个`engine`有一个`engine_key`，可用于调用 [搜索 API][search_api] 时指定 `engine`。

### collection

`collection` 定义了 `document` 的 ‘schema’，包含了拥有类似 ‘schema’ 的一系列 `document`，类似于 mysql 中的 table, mongodb 中的 collection。

每个`collection` 有1个`name`，在同一个`engine`中唯一，可由字母，数字和 ‘-’ 构成。另外每个`collection`还有一个`field_types`，描述了所包含`document`的 ‘schema’。例如：

```json
{
  "title": "string",
  "tags": "string",
  "published_date": "date",
  "url": "enum",
  "body": "text"
}
```

表示 `document` 由 ‘title’, ‘tags’, ‘published_date’, ‘url’, ‘body’ 这些 `field` 组成，每个 `field` 的类型(`type`) 分别是：‘string’, ‘string’, ‘date’, ‘enum’, ‘text’。

`field` `type` 的详细列表，参见下面 [Field Types][field_types] 小节。

### document

微搜索中， `document` 对应于具体的一个个资源，类似于 mysql 中的 record, mongodb 中的 document。每个`document`拥有一个id，以及符合所属 `collection` 中规定的 ‘schema’ 内容。例如：

```json
{
  "title": "微搜索上线内测",
  "tags": ["公告"],
  "published_date": "2014-08-16T00:00:00Z",
  "url": "http://blog.tinysou.com/cn/2014/08/09/%E5%BE%AE%E6%90%9C%E7%B4%A2%E4%B8%8A%E7%BA%BF%E5%86%85%E6%B5%8B.html",
  "body": "我们很高兴地在这里宣布，今天，微搜索正式开始上线内测了！"
}
```


## Field Types

在介绍 `collection` 时，我们提到每个 `collection` 包含一个 `field_types`属性，用于定义所包含 `document` 的 **schema**。

`field_types`是一个 `hash`，其中 **key** 表示 `field`的 **name**, **value** 表示 `field`的 **type**。`field` `type` 的详细列表如下：

### field types 列表

| Type | 描述 |
| ------- | ------ |
| string  | 较短的字符串。例如博客的标题，章节标题等等。本类型可被用于"全文搜索"(`full text search`)，并且会为自动补全做优化。例如："che", "cxy" 会匹配 "程序员"(chengxuyuan)。 |
| text    | 长文本。例如书的章节内容，博客的正文。本类型可被用于"全文搜索" |
| enum    | 特殊的字符串，用于精确匹配。例如 id, url 等等。本类型可被用于精确匹配搜索，过滤，排序等等。 |
| integer | 整数。本类型可被用于精确匹配搜索，过滤，排序等等。 |
| float   | 浮点数。本类型可被用于精确匹配搜索，过滤，排序等等。 |
| date    | ISO 8601 格式的时间字符串 或者 以毫秒为单位的 Unix 时间戳。例如 `"2014-08-16T00:00:00Z"` 或者 `1409208887122`。本类型可被用于精确匹配搜索，过滤，排序等等。 |
| location| 地理位置。由经纬度表示。可以是`hash`: { lon: 经度值, lat: 纬度值}，也可以是`array`: [经度值,纬度值]。 |

### field types 适用场景

| Type       | 可被搜索    | 为自动补全做优化 |  支持 `Functional Boosts` | 可过滤 `Filter` | 可排序 `Sort` |
| ---------- | ------ | ----------------- | ------------------------ | ----------- | --------------- |
| string    | Y | Y | N | Y | Y |
| text      | Y | N | N | N | N |
| enum      | Y | N | N | Y | Y |
| integer   | Y | N | Y | Y | Y |
| float     | Y | N | Y | Y | Y |
| date      | N | N | N | Y | Y |
| location  | N | N | N | Y | N |

### 数组(array)类型数值

除了 `location`类型外，其余任何类型的 `field` 的值都可以是由该类型值组成的数组。例如 `field` "title" 的类型是 `string`，则 "title" 的值可以为 ["第一章", "第二章"]。

## score

在搜索结果中，每个`document`都有一个`score`。默认情况下，`score`表示该`document`与搜索内容的相符程度。

## 调用约定

### URI 前缀

API 服务域名地址为：`api.tinysou.com`，当前 API 版本为 v1。

### 请求的表示

以

```
GET /engines/:engine_name
```

为例：

`GET` 表示 HTTP 动词, 本例中 HTTP 请求的动词需要使用 `GET`。

`:engine_name` 表示 URI 中需要替换成特定值的变量部分。本例中，如果我们操作的是`name`为 'demo' 的`engine`，则最终实际的 URI 应该为 `http://api.tinysou.com/v1/engines/demo`。

### 响应的格式

所有响应均包含下述的 `Header`， 且`body`均以 `json` 编码。

```
$ curl -i http://api.tinysou.com/v1

HTTP/1.1 200 OK
Server: nginx
Date: Thu, 21 Aug 2014 14:54:51 GMT
Content-Type: application/json
Content-Length: 15
Connection: keep-alive
Status: 200 OK

{"version":"1"}
```

## 权限验证

除了 公开(Public) API，所有的 API 调用都需要通过权限验证。

微搜索支持两种携带认证信息的方式：通过 `Authorization` Header 和通过 `auth_token` 参数携带。

### `Authorization` Header 方式

```
$ curl -H "Authorization: token YOUR_AUTH_TOKEN" http://api.tinysou.com/v1
```

### `auth_token` 参数

```
$ curl http://api.tinysou.com/v1?auth_token=YOUR_AUTH_TOKEN
```

[resource_structure]:/images/resource_structure.png
[field_types]:/v1/overview.html#3-Field-Types
[search_api]:/v1/searching.html
