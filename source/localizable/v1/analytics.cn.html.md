---
layout: api.cn
section: analytics
toc: article
title: 分析 API
---

## 总述

分析 API，均需要提供`auth_token`以通过权限验证。验证方式请参考[权限验证][auth]一节。

分析 API 的每个接口均提供两种形式：

### 作用于单个 `collection`

```
GET /engines/:engine_name/collections/:collection_name/analytics/:method
```

### 作用于多个 `collection`

```
GET /engines/:engine_name/analytics/:method
```

## 参数

| 名称    | 类型    | 说明 |
| ------ | ------ | ------------------------------------------------------ |
| collections | string | 在 [形式 2][form2]中，指定的多个 `collection`。在 [形式 2][form2] 默认为**全部**，在 [形式 1][form1] 中**无效**。 |
| start  | string/number | 起始时间，可以是 ISO8601 格式的字符串，也可以是 UNIX 毫秒数。 默认为 **1天前**。|
| end    | string/number | 终止时间，可以是 ISO8601 格式的字符串，也可以是 UNIX 毫秒数。 默认为**当前时间**。|
| interval | string/number | 分段的时间间隔，可以是 `year`，`quarter`，`month`，`week`,`day`，`hour`，`minute`，`second`，也可以是 UNIX 毫秒数。 默认为 **0**，表示不进行分段。|
| function | string | 分析函数，可选 `count`，`top`, 默认为 **count**|

### `collections`

以','分隔多个`collection`的`name`。例如：`"post,comment"`表示：作用于“posts”,“comments”两个`collection`。

## Endpoint

### 搜索

```
GET /engines/:engine_name/analytics/search
```

示例

```
curl http://api.tinysou.com/v1/engines/blog/collections/posts/analytics/search?interval=hour \
  -H 'Authorization: token YOUR_AUTH_TOKEN'
```

响应

```json
[
  ["2014-08-28T00:02:00Z", 2],
  ["2014-08-28T00:03:00Z", 0],
  ["2014-08-28T00:04:00Z", 1],
  ["2014-08-28T00:05:00Z", 5],
  ["2014-08-28T00:06:00Z", 6],
  ["2014-08-28T00:07:00Z", 7],
  ["2014-08-28T00:08:00Z", 8],
  ["2014-08-28T00:09:00Z", 9],
  ["2014-08-28T00:10:00Z", 1],
  ["2014-08-28T00:11:00Z", 2],
  ["2014-08-28T00:12:00Z", 3],
  ["2014-08-28T00:13:00Z", 4],
  ["2014-08-28T00:14:00Z", 5],
  ["2014-08-28T00:15:00Z", 6],
  ["2014-08-28T00:16:00Z", 7],
  ["2014-08-28T00:17:00Z", 8],
  ["2014-08-28T00:18:00Z", 9],
  ["2014-08-28T00:19:00Z", 0],
  ["2014-08-28T00:20:00Z", 1],
  ["2014-08-28T00:21:00Z", 2],
  ["2014-08-28T00:22:00Z", 3],
  ["2014-08-28T00:23:00Z", 4],
  ["2014-08-29T00:00:00Z", 6],
  ["2014-08-29T00:01:00Z", 4]
]
```

### 自动补全

```
GET /engines/:engine_name/analytics/autocomplete
```

示例

```
curl http://api.tinysou.com/v1/engines/blog/collections/posts/analytics/autocomplete?function=top \
  -H 'Authorization: token YOUR_AUTH_TOKEN'
```

```json
[["tinysou", 100], ["search", 50]]
```

### 搜索点击

```
GET /engines/:engine_name/analytics/search_click
```

示例

```
curl http://api.tinysou.com/v1/engines/blog/collections/posts/analytics/search_click?function=top \
  -H 'Authorization: token YOUR_AUTH_TOKEN'
```

响应

```json
[
  [{"collection_name": "posts", "document_id": "53c028da70726f7e2d010000"}, 100],
  [{"collection_name": "posts", "document_id": "53c028da70726f7e2d010001"}, 50]
]
```

### 自动补全点击

```
GET /engines/:engine_name/analytics/autocomplete_click
```

示例

```
curl http://api.tinysou.com/v1/engines/blog/collections/posts/analytics/autocomplete_click?interval=hour \
  -H 'Authorization: token YOUR_AUTH_TOKEN'
```

响应

```json
[
  ["2014-08-28T00:02:00Z", 2],
  ["2014-08-28T00:03:00Z", 0],
  ["2014-08-28T00:04:00Z", 1],
  ["2014-08-28T00:05:00Z", 5],
  ["2014-08-28T00:06:00Z", 6],
  ["2014-08-28T00:07:00Z", 7],
  ["2014-08-28T00:08:00Z", 8],
  ["2014-08-28T00:09:00Z", 9],
  ["2014-08-28T00:10:00Z", 1],
  ["2014-08-28T00:11:00Z", 2],
  ["2014-08-28T00:12:00Z", 3],
  ["2014-08-28T00:13:00Z", 4],
  ["2014-08-28T00:14:00Z", 5],
  ["2014-08-28T00:15:00Z", 6],
  ["2014-08-28T00:16:00Z", 7],
  ["2014-08-28T00:17:00Z", 8],
  ["2014-08-28T00:18:00Z", 9],
  ["2014-08-28T00:19:00Z", 0],
  ["2014-08-28T00:20:00Z", 1],
  ["2014-08-28T00:21:00Z", 2],
  ["2014-08-28T00:22:00Z", 3],
  ["2014-08-28T00:23:00Z", 4],
  ["2014-08-29T00:00:00Z", 6],
  ["2014-08-29T00:01:00Z", 4]
]
```


[form1]:/v1/analytics.html#1-1-作用于单个-collection
[form2]:/v1/analytics.html#1-2-作用于多个-collection
[auth]:/v1/overview.html#6-权限验证
