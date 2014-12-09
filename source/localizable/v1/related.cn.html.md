---
layout: api.cn
section: related
toc: article
title: 相关文档 API
---

## 总述

相关文档 API 与[搜索 API][search-api] 使用相同的权限验证和返回格式。

## 参数

| 名称    | 类型    | 说明 |
| ------ | ------ | ------------------------------------------------------ |
| page   | number | 分页参数，指定返回结果的起始页数，默认从第 1 页开始。**可选** |
| per_page   | number | 分页参数，指定每页显示条目的数据量，默认每页 20 条。**可选** |
| fetch_fields   | array(of string) | 搜索结果中，`document`需要包含的`field`。 **默认值**：所有`field`。**可选** |
| filter | hash | 过滤器。**可选** |
| sort   | array | 排序方式。 **可选** |

## Endpoint

```
GET /engines/:engine_name/collections/:collection_name/documents/:documents_id/related
```

```
POST /engines/:engine_name/collections/:collection_name/documents/:documents_id/related
```

## 示例

```
curl 'http://api.tinysou.com/v1/engines/blog/collections/post/documents/97889abb211b29fac05b04162c935319/related \
  -H 'Authorization: token YOUR_AUTH_TOKEN'
```

[search-api]:/v1/searching.html
