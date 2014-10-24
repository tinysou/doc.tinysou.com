---
layout: libs.cn
section: php
toc: article
title: PHP 库
---

## 总述

PHP 库提供微搜索API的 PHP 封装。直接使用该库能够帮助开发者方便快捷地使用微搜索的各项功能。所有源码托管在 github， 开发者们可前往查看[最新版本][github]。源码同时附带 [例子][examples]，方便用户学习使用。

## 使用

```php
$client = new TinySou('YOUR_TOKEN');
```

### Engine

#### 罗列`engine`

罗列出你的所有`engine`：

```php
$client->engines();
```

#### 创建`engine`

创建一个名称为 'blog'，显示名为 'Blog' 的`engine`：

```php
$client->create_engine(array(
  'name' => 'blog', 'display_name' => 'Blog'
  ));
```

> 完整参数请参见 [http://doc.tinysou.com/v1/indexing.html#2-2-创建一个-Engine](/v1/indexing.html#2-2-创建一个-Engine)

#### 读取一个`engine`

通过名称 'blog' 读取该`engine`：

```php
$client->engine('blog');
```

#### 更新一个`engine`

对 'blog' 进行更新：

```php
$client->update_engine('blog', array('display_name' => 'My Blog'));
```

#### 删除一个`engine`

删除名为`blog`的`engine`:

```php
$client->delete_engine('blog');
```

### Collection

#### 罗列`collection`

罗列出 'blog' 内包含的所有`collection`:

```php
$client->collections('blog');
```

#### 创建一个`collection`

在 'blog' 下创建一个`collection`，名为 'posts'。在`posts`里有 'title'，'tags'，'author'，'date' 和 'body' 这些`field`，并分别指定这些`field·的类型，如 'title' 类型为 'string' :

```php
$client->create_collection('blog',
  array('name' => 'posts',
       'field_types' => array(
            'title' => 'string',
            'tags' => 'string',
            'author' => 'enum',
            'date' => 'date',
            'body' => 'text'
            )
        )
);
```

> 完整参数请参见 [http://doc.tinysou/v1/indexing.html#3-7-创建一个-Collection](http://doc.tinysou/v1/indexing.html#3-7-创建一个-Collection)

#### 读取一个`collection`

返回'blog'下的 'posts'：

```php
$client->collection('blog', 'posts');
```

#### 删除一个collection

删除'blog'下的 'posts'：

```php
$client->delete_collection('blog', 'posts');
```

### Document

#### 罗列`document`

罗列出 'blog' 下、'posts'里的所有`document`，并让返回的结果从第0页开始显示，每页显示20项：

```php
$client->documents('blog', 'posts', array('page' => 0, 'per_page' => 20));
```

#### 创建一个`document`

在 'blog' 下的 'posts' 里创建一个`document`：

```php
$client->create_document('blog', 'posts', array(
    'title' => 'My First Post',
    'tags' => ['news'],
    'author' => 'Author',
    'date' => '2014-08-16T00:00:00Z',
    'body' => 'Tinysou start online today!'
    )
);
```

> 完整参数请参见 [http://doc.tinysou/v1/indexing.html#4-11-创建一个-Document(自动生成-id-方式)](http://doc.tinysou/v1/indexing.html#4-11-创建一个-Document(自动生成-id-方式))

#### 读取一个`document`

与`engine`和`collection`不同，`document`只能通过唯一的`documentId`来进行访问。在下面的例子中，`documentId`就是'293ddf9205df9b36ba5761d61ca59a29'：

```php
$client->document('blog', 'posts', '293ddf9205df9b36ba5761d61ca59a29');
```


#### 更新一个`document`

在 'blog' 中的 'posts' 里更新一个`document`：

```php
$client->update_document('blog', 'posts', '293ddf9205df9b36ba5761d61ca59a29', array(
    'title' => 'First Post',
    'tags' => ['news'],
    'author' => 'Author',
    'date' => '2014-08-16T00:00:00Z',
    'body' => 'Tinysou start online today!'
    )
);
```

> 完整参数请参见 [http://doc.tinysou/v1/indexing.html#4-14-创建或更新一个-Document](http://doc.tinysou/v1/indexing.html#4-14-创建或更新一个-Document)

#### 删除一个`document`

在 'blog' 中的 'posts' 里删除一个`document`：

```php
$client->delete_document('blog', 'posts', '293ddf9205df9b36ba5761d61ca59a29');
```

### 搜索

在 'blog' 下的 'posts' 里搜索关键词'tinysou'：

```php
$client->search('blog', array(
    'q' => 'tinysou', 'c' => 'posts',
    'page' => 0, 'per_parge' => 10,
    'filter' => array(
            'range' => array(
                'field' => "date",
                'from' => "2014-07-01T00:00:00Z",
                'to' => "2014-08-01T00:00:00Z"
            )
        ),
    'sort' => array(
        'field' => "date",
        'order' => "asc",
        'mode' => "avg"
    )
  )
);
```

> 完整参数请参见 [http://doc.tinysou/v1/searching.html#2-2-2.搜索多个collection](http://doc.tinysou/v1/searching.html#2-2-2.搜索多个collection)

### 自动补全

在 'posts' 里对关键词 'tinys' 进行补全：

```php
$client->autocomplete('blog', array('q' => 't', 'c' => 'posts'));
```

自动补全 API 与搜索 API 使用相同的权限验证，参数和返回格式。
完整参数请参考搜索的参数。

## 参与

1. Fork 我们的项目 ( [https://github.com/tinysou/tinysou-php/fork][fork] )
2. 创建你的新分支 (`git checkout -b my-new-feature`)
3. 提交你的修改 (`git commit -am 'Add some feature'`)
4. 推送到新分支 (`git push origin my-new-feature`)
5. 创建一个合并请求


[github]:https://github.com/tinysou/tinysou-php
[setup]:http://dashboard.tinysou.com/signup
[fork]:https://github.com/tinysou/tinysou-php/fork
[examples]:https://github.com/tinysou/tinysou-php/tree/master/examples
