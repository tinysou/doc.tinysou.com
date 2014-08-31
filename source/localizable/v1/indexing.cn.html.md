---
layout: api.cn
section: indexing
toc: article
title: 索引 API
---

## 总述

索引 API，均需要提供`auth_token`以通过权限验证。验证方式请参考[权限验证][auth]一节。

## Engine

### 罗列 Engines

```
GET /engines
```

####示例

> 请求

```
curl -H "Authorization: token YOUR_AUTH_TOKEN" 'http://api.tinysou.com/v1/engines'
```

> 响应

```
Status: 200 OK
```

```json
[
  {
    "name": "tinysou",
    "display_name": "微搜索",
    "key": "97eaafba26b04d3cdeb9",
    "doc_count": 22
  },
  {
    "name": "demo",
    "display_name": "Demo Engine",
    "key": "04d3cdeb9ba26b04d6b0",
    "doc_count": 108
  }
]
```

### 创建一个 Engine

```
POST /engines
```

#### 参数

| 名称    | 类型    | 说明 |
| ------ | ------ | ------------------------------------------------------ |
| name   | string | `engine`的唯一标识。格式：字母，数字，‘-’ 组成，且不以 ‘-’ 开头，长度3-60字符。**必需** |
| display_name   | string | 显示名。 **可选** |

#### 示例

> 请求

```
curl -XPOST 'http://api.tinysou.com/v1/engines' \
  -H "Authorization: token YOUR_AUTH_TOKEN" \
  -d '{
        "name": "my-blog"
      }'
```

> 响应

```
Status: 201 OK
Location: http://api.tinysou.com/v1/engines/my-blog
```

```json
{
  "name": "my-blog",
  "display_name": "my-blog",
  "key": "a1b6dfc9866cb22c120f",
  "doc_count": 0
}
```

### 获取一个 Engine

```
GET /engines/:engine_name
```

####示例

> 请求

```
curl -H "Authorization: token YOUR_AUTH_TOKEN" 'http://api.tinysou.com/v1/engines/demo'
```

> 响应

```
Status: 200 OK
```

```json
{
  "name": "demo",
  "display_name": "Demo Engine",
  "key": "04d3cdeb9ba26b04d6b0",
  "doc_count": 108
}
```

### 更新一个 Engine

```
PUT /engines/:engine_name
```

#### 参数

| 名称    | 类型    | 说明 |
| ------ | ------ | ------------------------------------------------------ |
| display_name   | string | 显示名。 **可选**|

#### 示例

> 请求

```
curl -XPUT 'http://api.tinysou.com/v1/engines/demo' \
  -H "Authorization: token YOUR_AUTH_TOKEN" \
  -d '{
        "display_name": "My Demo"
      }'
```

> 响应

```
Status: 200 OK
```

```json
{
  "name": "demo",
  "display_name": "My Demo",
  "key": "04d3cdeb9ba26b04d6b0",
  "doc_count": 108
}
```

### 删除一个 Engine

```
DELETE /engines/:engine_name
```

####示例

> 请求

```
curl -XDELETE -H "Authorization: token YOUR_AUTH_TOKEN" 'http://api.tinysou.com/v1/engines/my-blog'
```

> 响应

```
Status: 204 No Content
```

## Collection

### 罗列 Collections

```
GET /engines/:engine_name/collections
```

####示例

> 请求

```
curl 'http://api.tinysou.com/v1/engines/demo/collections' \
  -H "Authorization: token YOUR_AUTH_TOKEN"
```

> 响应

```
Status: 200 OK
```

```json
[
  {
    "name": "page",
    "field_types": {
      "updated_at": "date",
      "title": "string",
      "url": "enum",
      "sections": "string",
      "body": "text",
      "type": "enum",
      "image": "enum",
      "published_at": "date",
      "popularity": "integer",
      "info": "string"
    }
  }
]
```

### 创建一个 Collection

```
POST /engines/:engine_name/collections
```

####参数

| 名称    | 类型    | 说明 |
| ------ | ------ | ------------------------------------------------------ |
| name   | string | `collection`的唯一标识。格式：字母，数字，‘-’ 组成，且不以 ‘-’ 开头，长度3-60字符。**必需** |
| field_types   | hash | 描述包含`documents`的 schema。 具体内容可参见[Field Types][field_types] 小节。**必需**|

####示例

> 请求

```
curl -XPOST 'http://api.tinysou.com/v1/engines/demo/collections' \
  -H "Authorization: token YOUR_AUTH_TOKEN" \
  -d '{
        "name": "post",
        "field_types": {
          "title": "string",
          "tags": "string",
          "author": "enum",
          "date": "date",
          "body": "text"
        }
      }'
```

> 响应

```
Status: 201 Created
Location: http://api.tinysou.com/v1/engines/demo/collections/post
```

```json
{
  "name": "post",
  "field_types": {
    "title": "string",
    "tags": "string",
    "author": "enum",
    "date": "date",
    "body": "text"
  }
}
```

### 获取一个 Collection

```
GET /engines/:engine_name/collections/:collection_name
```

#### 示例

> 请求

```
curl 'http://api.tinysou.com/v1/engines/demo/collections/post'
```

> 响应

```
Status: 200 OK
```

```json
{
  "name": "post",
  "field_types": {
    "title": "string",
    "tags": "string",
    "author": "enum",
    "date": "date",
    "body": "text"
  }
}
```

### 删除一个 Collection

```
DELETE /engines/:engine_name/collections/:collection_name
```

#### 示例

> 请求

```
curl -XDELETE 'http://api.tinysou.com/v1/engines/demo/collections/post'
```

> 响应

```
Status: 204 No Content
```

## Document

### 罗列 Documents

```
GET /engines/:engine_name/collections/:collection_name/documents
```

####参数

| 名称    | 类型    | 说明 |
| ids   | array(of string) | `document` `id`的列表。当指定`ids`后，只罗列的`id`在此列表中的`document`。**可选** |
| page   | number | 分页参数，指定返回结果的起始页数，默认从第 0 页开始。**可选** |
| per_page   | number | 分页参数，指定每页显示条目的数据量，默认每页20条。**可选** |

####示例

> 请求

```
curl 'http://api.tinysou.com/v1/engines/demo/collections/post/documents' \
  -H "Authorization: token YOUR_AUTH_TOKEN"
```

> 响应

```
Status: 200 OK
```

```json
[
  {
    "id": "293ddf9205df9b36ba5761d61ca59a29",
    "title": "微搜索上线内测",
    "tags": ["公告"],
    "author": "Michael Ding",
    "date": "2014-08-09T00:00:00Z",
    "body": "我们很高兴地在这里宣布，今天，微搜索正式开始上线内测了！微搜索致力于为大家提供一个易于安装又特性丰富的站内搜索引擎。本次的内测版本，跨站点全文搜索，并提供搜索框的下拉式自动补全。而整个过程只需要简单的三步：创建引擎，添加站点地址，复制代码到 html。内测阶段需要注册码进行注册，想试用的朋友欢迎发邮件给我们索取注册码。邮件请发到support@tinysou.com免费帐号可创建1个引擎，添加两个域名。如需更多，同样请发邮件到以上邮箱联系我们。"
  }
]
```

### 创建一个 Document(自动生成 id 方式)

```
POST /engines/:engine_name/collections/:collection_name/documents
```

#### 参数

> 创建`document`的参数遵循`collection`的`field_types`定义。

> 参数名为`field_types`中的`key`, 参数值类型与`field_types`中的`value`一致。当参数值为`array`时，保证`array`的每个元素与`field_types`中的`value`一致。

> 关于`field_types`具体内容可参照 [Field Types][field_types] 一节。

#### 示例

> 请求

```
curl -XPOST 'http://api.tinysou.com/v1/engines/demo/collections/post/documents' \
  -H "Authorization: token YOUR_AUTH_TOKEN" \
  -d '{
        "title": "支持拼音搜索",
        "tags": ["公告", "特性"],
        "author": "Michael Ding",
        "date": "2014-08-16T00:00:00Z",
        "body": "我们很高兴地在这里宣布，今天开始，微搜索支持拼音搜索了。微搜索的拼音搜索包括：拼音补全，首字母搜索及补全，例如：如果你的文本中包含 程序员，输入 cxy, chen, chengxu均可匹配到程序员效果如下图所示："
      }'
```

> 响应

```
Status: 201 OK
Location: http://api.tinysou.com/v1/engines/demo/collections/post/documents/293ddf9205df9b36ba5761d61ca59a29
```

```json
{
  "id": "293ddf9205df9b36ba5761d61ca59a29",
  "title": "支持拼音搜索",
  "tags": ["公告", "特性"],
  "author": "Michael Ding",
  "date": "2014-08-16T00:00:00Z",
  "body": "我们很高兴地在这里宣布，今天开始，微搜索支持拼音搜索了。微搜索的拼音搜索包括：拼音补全，首字母搜索及补全，例如：如果你的文本中包含 程序员，输入 cxy, chen, chengxu均可匹配到程序员效果如下图所示："
}
```

### 创建一个 Document(指定 id 方式)

见 "[创建或更新一个 Document][upsert-a-doc]" 一节。

### 获取一个 Document

```
GET /engines/:engine_name/collections/:collection_name/documents/:document_id
```

####示例

> 请求

```
curl 'http://api.tinysou.com/v1/engines/demo/collections/post/documents/293ddf9205df9b36ba5761d61ca59a29' \
  -H "Authorization: token YOUR_AUTH_TOKEN"
```

> 响应

```
Status: 200 OK
```

```json
{
  "id": "293ddf9205df9b36ba5761d61ca59a29",
  "title": "支持拼音搜索",
  "tags": ["公告", "特性"],
  "author": "Michael Ding",
  "date": "2014-08-16T00:00:00Z",
  "body": "我们很高兴地在这里宣布，今天开始，微搜索支持拼音搜索了。微搜索的拼音搜索包括：拼音补全，首字母搜索及补全，例如：如果你的文本中包含 程序员，输入 cxy, chen, chengxu均可匹配到程序员效果如下图所示："
}
```

### 创建或更新一个 Document

```
PUT /engines/:engine_name/collections/:collection_name/documents/:document_id
```

当`id`为 ':document\_id' 的`document`不存在时，会创建一个`id`值为 ':document\_id' 的`document`；
否则会更新`id`为 ':document\_id' 的`document`。

即一次 ‘Upsert’。

#### 参数

同 [创建一个 Document][create-a-doc] 一节。

#### 示例

> 请求

```
curl -XPUT 'http://api.tinysou.com/v1/engines/demo/collections/post/documents/293ddf9205df9b36ba5761d61ca59a29' \
  -H "Authorization: token YOUR_AUTH_TOKEN" \
  -d '{
        "title": "支持拼音搜索",
        "tags": ["公告", "特性"],
        "author": "Michael Ding",
        "date": "2014-08-16T00:00:00Z",
        "body": "我们很高兴地在这里宣布，今天开始，微搜索支持拼音搜索了。微搜索的拼音搜索包括：拼音补全，首字母搜索及补全，例如：如果你的文本中包含 程序员，输入 cxy, chen, chengxu均可匹配到程序员效果如下图所示："
      }'
```

> 响应

创建

```
Status: 201 OK
```

更新

```
Status: 200 OK
```

```json
{
  "id": "293ddf9205df9b36ba5761d61ca59a29",
  "title": "支持拼音搜索",
  "tags": ["公告", "特性"],
  "author": "Michael Ding",
  "date": "2014-08-16T00:00:00Z",
  "body": "我们很高兴地在这里宣布，今天开始，微搜索支持拼音搜索了。微搜索的拼音搜索包括：拼音补全，首字母搜索及补全，例如：如果你的文本中包含 程序员，输入 cxy, chen, chengxu均可匹配到程序员效果如下图所示："
}
```

### 删除一个 Document

```
DELETE /engines/:engine_name/collections/:collection_name/documents/:document_id
```

####示例

> 请求

```
curl -XDELETE 'http://api.tinysou.com/v1/engines/demo/collections/post/documents/293ddf9205df9b36ba5761d61ca59a29' \
  -H "Authorization: token YOUR_AUTH_TOKEN"
```

> 响应

```
Status: 204 No Content
```

## 基于爬虫的 Engine

如果你不想通过 API 创建`document`，你可以为你的`engine`创建`domain`。每个`domain`包含一个起始`url`。微搜索的爬虫系统会根据这个起始`url`自动爬取这个`domain`的网页，并为这些网页创建对应的`document`。

关于爬虫规则的详细规则，请参见[crawler][crawler]

### 罗列 Domain

```
GET /engines/:engine_name/domains
```

####示例

> 请求

```
curl -H "Authorization: token YOUR_AUTH_TOKEN" 'http://api.tinysou.com/v1/engines/demo/domains'
```

> 响应

```
Status: 200 OK
```

```json
[
  {
    "id": "53e5b3423163610029010000",
    "url": "http://tinysou.com",
    "white_list": [
      "http://blog.tinysou.com/cn/",
      "http://doc.tinysou.com/",
      "http://tinysou.com/"
    ],
    "black_list": [
      "http://blog.tinysou.com/cn/tags/",
      "http://blog.tinysou.com/cn/calendar/"
    ]
  }
]
```

### 创建一个 Domain

```
POST /engines/:engine_name/domains
```

#### 参数

| 名称    | 类型    | 说明 |
| ------ | ------ | ------------------------------------------------------ |
| url   | string | 起始`url`。例如：'http://doc.tinysou.com'。**必需** |
| white_list   | array(of string) | 索引白名单。白名单中的每条规则由正则表达式描述。当白名单不为空时，只有网址符合白名单规则的网页才会被索引。**可选** |
| black_list   | array(of string) | 索引黑名单。黑名单中的每条规则由正则表达式描述。当黑名单不为空时，网址符合黑名单规则的网页不会被索引。**可选** |

> 注意：当白名单，黑名单同时存在时，先应用白名单规则，再应用黑名单规则。

#### 示例

> 请求

```
curl -XPOST 'http://api.tinysou.com/v1/engines/demo/domains' \
  -H "Authorization: token YOUR_AUTH_TOKEN" \
  -d '{
        "url": "http://tinysou.com",
        "white_list": [
            "http://blog.tinysou.com/cn/",
            "http://doc.tinysou.com/",
            "http://tinysou.com/"
        ],
        "black_list": [
            "http://blog.tinysou.com/cn/tags/",
            "http://blog.tinysou.com/cn/calendar/"
        ]
      }'
```

> 响应

```
Status: 201 OK
Location: http://api.tinysou.com/v1/engines/demo/domains/53e5b3423163610029010000
```

```json
{
  "id": "53e5b3423163610029010000",
  "url": "http://tinysou.com",
  "white_list": [
    "http://blog.tinysou.com/cn/",
    "http://doc.tinysou.com/",
    "http://tinysou.com/"
  ],
  "black_list": [
    "http://blog.tinysou.com/cn/tags/",
    "http://blog.tinysou.com/cn/calendar/"
  ]
}
```

### 获取一个 Domain

```
GET /engines/:engine_name/domains/:domain_id
```

####示例

> 请求

```
curl -H "Authorization: token YOUR_AUTH_TOKEN" 'http://api.tinysou.com/v1/engines/demo/domains/53e5b3423163610029010000'
```

> 响应

```
Status: 200 OK
```

```json
{
  "id": "53e5b3423163610029010000",
  "url": "http://tinysou.com",
  "white_list": [
    "http://blog.tinysou.com/cn/",
    "http://doc.tinysou.com/",
    "http://tinysou.com/"
  ],
  "black_list": [
    "http://blog.tinysou.com/cn/tags/",
    "http://blog.tinysou.com/cn/calendar/"
  ]
}
```

### 更新一个 Domain

```
PUT /engines/:engine_name/domains/:domain_id
```

#### 参数

| 名称    | 类型    | 说明 |
| ------ | ------ | ------------------------------------------------------ |
| url   | string | 起始`url`。例如：'http://doc.tinysou.com'。**必需** |
| white_list   | array(of string) | 索引白名单。白名单中的每条规则由正则表达式描述。当白名单不为空时，只有网址符合白名单规则的网页才会被索引。**可选** |
| black_list   | array(of string) | 索引黑名单。黑名单中的每条规则由正则表达式描述。当黑名单不为空时，网址符合黑名单规则的网页不会被索引。**可选** |

> 注意：当白名单，黑名单同时存在时，先应用白名单规则，再应用黑名单规则。

#### 示例

> 请求

```
curl -XPUT 'http://api.tinysou.com/v1/engines/demo/domains/53e5b3423163610029010000' \
  -H "Authorization: token YOUR_AUTH_TOKEN" \
  -d '{
        "url": "http://tinysou.com",
        "white_list": [
          "http://blog.tinysou.com/cn/",
          "http://doc.tinysou.com/",
          "http://tinysou.com/",
          "http://help.tinysou.com/"
        ],
        "black_list": [
          "http://blog.tinysou.com/cn/tags/",
          "http://blog.tinysou.com/cn/calendar/",
          "http://tinysou.com/en/"
        ]
      }'
```

> 响应

```
Status: 200 OK
```

```json
{
  "id": "53e5b3423163610029010000",
  "url": "http://tinysou.com",
  "white_list": [
    "http://blog.tinysou.com/cn/",
    "http://doc.tinysou.com/",
    "http://tinysou.com/",
    "http://help.tinysou.com/"
  ],
  "black_list": [
    "http://blog.tinysou.com/cn/tags/",
    "http://blog.tinysou.com/cn/calendar/",
    "http://tinysou.com/en/"
  ]
}
```

### 删除一个 Domain

```
DELETE /engines/:engine_name/domains/:domain_id
```

####示例

> 请求

```
curl -XDELETE -H "Authorization: token YOUR_AUTH_TOKEN" 'http://api.tinysou.com/v1/engines/demo/domains/53e5b3423163610029010000'
```

> 响应

```
Status: 204 No Content
```


[auth]:/v1/overview.html#6-权限验证
[field_types]:/v1/overview.html#3-Field-Types
[upsert-a-doc]:/v1/indexing.html#4-14-创建或更新一个-Document
[create-a-doc]:/v1/indexing.html#4-12-创建一个-Document(指定-id-方式)
[crawler]:/crawler/overview.html
