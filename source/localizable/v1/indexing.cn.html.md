---
layout: api.cn
section: indexing
toc: article
title: 资源与索引 API
---

## 总述

资源与索引相关 API，均需要提供 `auth_token` 以通过权限验证。验证方式请参考[权限验证][auth]一节。

## Engine

### 罗列 Engines

```
GET /engines
```

####示例

**请求**

```
curl -H "Authorization: token YOUR_AUTH_TOKEN" 'http://api.tinysou.com/v1/engines'
```

**响应**

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
| name   | string | `engine`的唯一标识。格式：字母，数字，'-' 组成，且不以'-'开头，长度3-60字符。**必需** |
| display_name   | string | 显示名。 **可选**|

#### 示例

**请求**

```
curl -XPOST 'http://api.tinysou.com/v1/engines' \
  -H "Authorization: token YOUR_AUTH_TOKEN" \
  -d '{
        "name": "my-blog"
      }'
```

**响应**

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

**请求**

```
curl -H "Authorization: token YOUR_AUTH_TOKEN" 'http://api.tinysou.com/v1/engines/demo'
```

**响应**

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

**请求**

```
curl -XPUT 'http://api.tinysou.com/v1/engines/demo' \
  -H "Authorization: token YOUR_AUTH_TOKEN" \
  -d '{
        "display_name": "My Demo"
      }'
```

**响应**

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

**请求**

```
curl -XDELETE -H "Authorization: token YOUR_AUTH_TOKEN" 'http://api.tinysou.com/v1/engines/my-blog'
```

**响应**

```
Status: 204 No Content
```

## Collection

### 罗列 Collections

```
GET /engines/:engine_name/collections
```

####示例

**请求**

```
curl 'http://api.tinysou.com/v1/engines/demo/collections' \
  -H "Authorization: token YOUR_AUTH_TOKEN"
```

**响应**

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
| name   | string | `collection`的唯一标识。格式：字母，数字，'-' 组成，且不以'-'开头，长度3-60字符。**必需** |
| field_types   | hash | 描述包含`documents`的schema。 具体内容可参见[Field Types][field_types] 小节。**必需**|

####示例

**请求**

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

**响应**

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

**请求**

```
curl 'http://api.tinysou.com/v1/engines/demo/collections/post'
```

**响应**

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

**请求**

```
curl -XDELETE 'http://api.tinysou.com/v1/engines/demo/collections/post'
```

**响应**

```
Status: 204 No Content
```

[auth]:/v1/overview.html#5-权限验证
[field_types]:/v1/overview.html#3-Field-Types
