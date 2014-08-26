---
layout: api.cn
section: autocomplete
toc: article
title: 自动补全 API
---

## 总述

自动补全 API 与[搜索 API][search-api] 使用相同的权限验证，参数和返回格式，主要有如下区别：

* 不同的算法：自动补全使用会尝试"猜测"用户的不完整输入，即匹配前缀(prefix)。例如 'ch' 会匹配到 '程序员'(chengxuyuan)。
* 默认情况，返回的结果中，`document` 不包含任何 `field`。


## Endpoint

### 1.自动补全单个 `collection`

```
GET /engines/:engine_name/collections/:collection_name/autocomplete
```

```
POST /engines/:engine_name/collections/:collection_name/autocomplete
```

### 2.自动补全多个 `collection`

```
GET /engines/:engine_name/autocomplete
```

```
POST /engines/:engine_name/autocomplete
```

## 示例

```
curl -XPOST 'http://api.tinysou.com/v1/engines/blog/collections/post/autocomplete' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: token YOUR_AUTH_TOKEN' \
  -d '{
        "q": "自定义样式",
        "fetch_fields": ["title", "sections", "url"],
        "sort": {
          "field": "update_at",
          "order": "desc",
          "mode": "min"
        },
        "per_page": 100
      }' | json_reformat
```

[search-api]:/v1/searching.html
