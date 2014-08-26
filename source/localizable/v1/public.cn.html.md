---
layout: api.cn
section: public
toc: article
title: 公开(public) API
---

## 总述

Public API 是一组用于暴露在公网上的 API。这组 API，无需权限认证，采用 `engine_key` 参数来标识 `engine`。Public API 均为"只读(Readonly)" API，即：不会改变`资源`。Public API 包含`搜索`和`自动补全` 两部分。

## 搜索

```
GET /public/search
```

```
POST /public/search
```

### 说明

无需 `auth_token`进行认证，通过 `engine_key` 参数指定要进行搜索的 `engine`。

参数和返回格式和[搜索 API][search-api]完全一样。

### 示例

```
curl -XPOST 'http://api.tinysou.com/v1/public/search' \
  -H 'Content-Type: application/json' \
  -d '{
        "engine_key": "97eaafba26b04d3cdeb9",
        "c": "page",
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

## 自动补全

```
GET /public/autocomplete
```

```
POST /public/autocomplete
```

### 说明

无需 `auth_token`进行认证，通过 `engine_key` 参数指定要进行搜索的 `engine`。

参数和返回格式和[自动补全 API][autocomplete-api]完全一样。

### 示例

```
curl -XPOST 'http://api.tinysou.com/v1/public/autocomplete' \
  -H 'Content-Type: application/json' \
  -d '{
        "engine_key": "97eaafba26b04d3cdeb9",
        "c": "page",
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
[autocomplete-api]:/v1/autocomplete.html
