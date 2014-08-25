---
layout: guides.cn
section: api-quickstart
toc: article
title: API 快速上手
---

微搜索除了提供基于爬虫的方式来构建搜索资源，还有一套完整的 API，涵盖从创建引擎，索引资源到搜索的方方面面。

下面通过简单3步，快速了解微搜索API。

开始之前，你需要复制你的 `auth_token`，如下图所示：

![复制auth_token][copy_auth_token]

## 1. 创建搜索引擎

首先，创建一个`engine`

```
curl -XPOST 'http://api.tinysou.com/v1/engines' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: token YOUR_AUTH_TOKEN' \
  -d '{
        "name": "blog",
        "display_name": "My Blog"
      }'
```

其次，在刚刚创建`engine` 'blog'中,创建一个名为'post'的`collection`

```
curl -XPOST 'http://api.tinysou.com/v1/engines/blog/collections' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: token YOUR_AUTH_TOKEN' \
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

## 2. 索引资源

现在我们有了一个名为'blog'的`engine`，并在其中有一个名为'post'的`collection`

接下来我们可以向该'post'中添加资源，即用于搜索的目标，我们称之为`document`

#### 使用 `POST` 添加 `document`，微搜索会自动为该`document`分配一个`id`

```
curl -XPOST 'http://api.tinysou.com/v1/engines/blog/collections/post/documents' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: token YOUR_AUTH_TOKEN' \
  -d '{
        "title": "微搜索上线内测",
        "tags": ["公告"],
        "author": "Michael Ding",
        "date": "2014-08-09T00:00:00Z",
        "body": "我们很高兴地在这里宣布，今天，微搜索正式开始上线内测了！微搜索致力于为大家提供一个易于安装又特性丰富的站内搜索引擎。本次的内测版本，跨站点全文搜索，并提供搜索框的下拉式自动补全。而整个过程只需要简单的三步：创建引擎，添加站点地址，复制代码到 html。内测阶段需要注册码进行注册，想试用的朋友欢迎发邮件给我们索取注册码。邮件请发到support@tinysou.com免费帐号可创建1个引擎，添加两个域名。如需更多，同样请发邮件到以上邮箱联系我们。"
      }'
```

返回 201 的状态号，表示创建成功

#### 也可以使用 `PUT` 添加 `document`，直接指定自定义的 `id`，例如 mongodb 中对应博文的 'objectid'

```
curl -XPUT 'http://api.tinysou.com/v1/engines/blog/collections/post/documents/53c028da70726f7e2d010000' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: token YOUR_AUTH_TOKEN' \
  -d '{
        "title": "支持拼音搜索",
        "tags": ["公告", "特性"],
        "author": "Michael Ding",
        "date": "2014-08-16T00:00:00Z",
        "body": "我们很高兴地在这里宣布，今天开始，微搜索支持拼音搜索了。微搜索的拼音搜索包括：拼音补全，首字母搜索及补全，例如：如果你的文本中包含 程序员，输入 cxy, chen, chengxu均可匹配到程序员效果如下图所示："
      }'
```

返回 201 的状态号，表示创建成功。如果返回 200，表示之前已经存在此 `id` 的 `document`，并且成功以现在的内容替换了之前的内容。即一次 `upsert`。

## 3. 搜索/自动补全

现在就可以执行搜索或自动补全了。

### 搜索

```
curl -XPOST 'http://api.tinysou.com/v1/engines/blog/collections/post/search' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: token YOUR_AUTH_TOKEN' \
  -d '{
        "q": "微搜索拼音补全"
      }' | json_reformat
```

返回

```json
{
    "info": {
        "query": "微搜索拼音补全",
        "page": 0,
        "per_page": 20,
        "total": 2,
        "max_score": 0.048643537
    },
    "records": [
        {
            "collection": "post",
            "score": 0.048643537,
            "highlight": {
                "body": [
                    "我们很高兴地在这里宣布，今天开始，<em>微搜索</em>支持<em>拼音</em><em>搜索</em>了。<em>微</em><em>搜索</em>的<em>拼音</em><em>搜索</em>包括：<em>拼音</em><em>补全</em>，首字母<em>搜索</em>及<em>补全</em>，例如：如果你的文本中包含 程序员，输入 cxy, chen, chengxu均可匹配到程序员效果如下图所示："
                ],
                "title": [
                    "支持<em>拼音</em><em>搜索</em>"
                ]
            },
            "document": {
                "id": "53c028da70726f7e2d010000",
                "title": "支持拼音搜索",
                "tags": [
                    "公告",
                    "特性"
                ],
                "author": "Michael Ding",
                "date": "2014-08-16T00:00:00.000Z",
                "body": "我们很高兴地在这里宣布，今天开始，微搜索支持拼音搜索了。微搜索的拼音搜索包括：拼音补全，首字母搜索及补全，例如：如果你的文本中包含 程序员，输入 cxy, chen, chengxu均可匹配到程序员效果如下图所示："
            }
        },
        {
            "collection": "post",
            "score": 0.020018604,
            "highlight": {
                "body": [
                    "我们很高兴地在这里宣布，今天，<em>微</em><em>搜索</em>正式开始上线内测了！<em>微</em><em>搜索</em>致力于为大家提供一个易于安装又特性丰富的站内<em>搜索</em>引擎。本次的内测版本，跨站点全文<em>搜索</em>，并提供<em>搜索</em>框的下拉式自动<em>补全</em>。而整个过程只需要简单的三步：创建引擎，添加站点地址，复制代码到",
                    "引擎，添加站点地址，复制代码到 html。内测阶段需要注册码进行注册，想试用的朋友欢迎发邮件给我们<em>索</em>取注册码。邮件请发到support@tinysou.com免费帐号可创建1个引擎，添加两个域名。如需更多，同样请发邮件到以上邮箱联系我们。"
                ],
                "title": [
                    "<em>微</em><em>搜索</em>上线内测"
                ]
            },
            "document": {
                "id": "wY-BwU2YQJ2m45bT358Tng",
                "title": "微搜索上线内测",
                "tags": [
                    "公告"
                ],
                "author": "Michael Ding",
                "date": "2014-08-09T00:00:00.000Z",
                "body": "我们很高兴地在这里宣布，今天，微搜索正式开始上线内测了！微搜索致力于为大家提供一个易于安装又特性丰富的站内搜索引擎。本次的内测版本，跨站点全文搜索，并提供搜索框的下拉式自动补全。而整个过程只需要简单的三步：创建引擎，添加站点地址，复制代码到 html。内测阶段需要注册码进行注册，想试用的朋友欢迎发邮件给我们索取注册码。邮件请发到support@tinysou.com免费帐号可创建1个引擎，添加两个域名。如需更多，同样请发邮件到以上邮箱联系我们。"
            }
        }
    ],
    "errors": {}
}
```

### 自动补全

```
curl -XPOST 'http://api.tinysou.com/v1/engines/blog/collections/post/autocomplete' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: token YOUR_AUTH_TOKEN' \
  -d '{
        "q": "piny"
      }' | json_reformat
```

返回

```json
{
    "info": {
        "query": "piny",
        "page": 0,
        "per_page": 20,
        "total": 1,
        "max_score": 0.04119441
    },
    "records": [
        {
            "collection": "post",
            "score": 0.04119441,
            "highlight": null,
            "document": {
                "id": "53c028da70726f7e2d010000",
                "title": "支持拼音搜索",
                "tags": [
                    "公告",
                    "特性"
                ],
                "author": "Michael Ding",
                "date": "2014-08-16T00:00:00.000Z",
                "body": "我们很高兴地在这里宣布，今天开始，微搜索支持拼音搜索了。微搜索的拼音搜索包括：拼音补全，首字母搜索及补全，例如：如果你的文本中包含 程序员，输入 cxy, chen, chengxu均可匹配到程序员效果如下图所示："
            }
        }
    ],
    "errors": {}
}
```

[copy_auth_token]:/images/copy_auth_token.jpg
[copy_engine_key]:/images/copy_engine_key.png
