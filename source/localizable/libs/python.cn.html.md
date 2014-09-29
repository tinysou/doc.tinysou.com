---
layout: libs.cn
section: python
toc: article
title: Python 库
---

## 总述

Python 库提供微搜索API的 Python 封装。直接使用该库能够帮助开发者方便快捷地使用微搜索的各项功能。所有源码托管在 github， 开发者们可前往查看[最新版本][github]。源码同时附带 [例子][examples]，方便用户学习使用。

## 安装

```
$ pip install tinysou
```

## 使用

```python
import tinysou
client = tinysou.Client('YOUR_TOKEN')
```

### Engine

#### 罗列`engine`

罗列出你的所有`engine`：

```python
client.engines.list()
```

#### 创建`engine`

创建一个名称为 'blog'，显示名为 'Blog' 的`engine`：

```python
client.engines.create({'name': 'blog', 'display_name': 'Blog'})
```

> 完整参数请参见 [http://doc.tinysou.com/v1/indexing.html#2-2-创建一个-Engine](/v1/indexing.html#2-2-创建一个-Engine)

#### 读取一个`engine`

通过名称 'blog' 读取该`engine`：

```python
client.engines.get('blog')
```

#### 更新一个`engine`

对 'blog' 进行更新：

```python
client.engines.update('blog', {'display_name': 'My Blog'})
```

#### 删除一个`engine`

删除名为`blog`的`engine`:

```python
client.engines.delete('blog')
```

### Collection

#### 罗列`collection`

罗列出 'blog' 内包含的所有`collection`:

```python
client.collections.list('blog')
```

#### 创建一个`collection`

在 'blog' 下创建一个`collection`，名为 'posts'。在`posts`里有 'title'，'tags'，'author'，'date' 和 'body' 这些`field`，并分别指定这些`field·的类型，如 'title' 类型为 'string' :

```python
client.collections.create('blog',
                          {'name': 'posts',
                           'field_types': {
                                'title': 'string',
                                'tags': 'string',
                                'author': 'enum',
                                'date': 'date',
                                'body': 'text'
                           }})
```

> 完整参数请参见 [http://doc.tinysou/v1/indexing.html#3-7-创建一个-Collection](http://doc.tinysou/v1/indexing.html#3-7-创建一个-Collection)

#### 读取一个`collection`

返回'blog'下的 'posts'：

```python
client.collections.get('blog', 'posts')
```

#### 删除一个collection

删除'blog'下的 'posts'：

```python
client.collections.delete('blog', 'posts')
```

### Document

#### 罗列`document`

罗列出 'blog' 下、'posts'里的所有`document`，并让返回的结果从第0页开始显示，每页显示20项：

```python
client.documents.list('blog', 'posts', {'page': 0, 'per_page': 20})
```

#### 创建一个`document`

在 'blog' 下的 'posts' 里创建一个`document`：

```python
client.documents.create('blog', 'posts', {
    'title': 'My First Post',
    'tags': ['news'],
    'author': 'Author',
    'date': '2014-08-16T00:00:00Z',
    'body': 'Tinysou start online today!'
})
```

> 完整参数请参见 [http://doc.tinysou/v1/indexing.html#4-11-创建一个-Document(自动生成-id-方式)](http://doc.tinysou/v1/indexing.html#4-11-创建一个-Document(自动生成-id-方式))

#### 读取一个`document`

与`engine`和`collection`不同，`document`只能通过唯一的`documentId`来进行访问。在下面的例子中，`documentId`就是'293ddf9205df9b36ba5761d61ca59a29'：

```python
client.documents.get('blog', 'posts', '293ddf9205df9b36ba5761d61ca59a29')
```


#### 更新一个`document`

在 'blog' 中的 'posts' 里更新一个`document`：

```python
client.documents.update('blog', 'posts', '293ddf9205df9b36ba5761d61ca59a29', {
    'title': 'First Post',
    'tags': ['news'],
    'author': 'Author',
    'date': '2014-08-16T00:00:00Z',
    'body': 'Tinysou start online today!'
})
```

> 完整参数请参见 [http://doc.tinysou/v1/indexing.html#4-14-创建或更新一个-Document](http://doc.tinysou/v1/indexing.html#4-14-创建或更新一个-Document)

#### 删除一个`document`

在 'blog' 中的 'posts' 里删除一个`document`：

```python
client.documents.delete('blog', 'posts', '293ddf9205df9b36ba5761d61ca59a29')
```

### 搜索

在 'blog' 下的 'posts' 里搜索关键词'tinysou'：

```python
client.search('blog', {'q': 'tinysou', 'c': 'posts'})
```

> 完整参数请参见 [http://doc.tinysou/v1/searching.html#2-2-2.搜索多个collection](http://doc.tinysou/v1/searching.html#2-2-2.搜索多个collection)

### 自动补全

在 'posts' 里对关键词 'tinys' 进行补全：

```python
client.autocomplete('blog', {'q': 't', 'c': 'posts'})
```

自动补全 API 与搜索 API 使用相同的权限验证，参数和返回格式。
完整参数请参考搜索的参数。

## 参与

1. Fork 我们的项目 ( [https://github.com/tinysou/tinysou-python/fork][fork] )
2. 创建你的新分支 (`git checkout -b my-new-feature`)
3. 提交你的修改 (`git commit -am 'Add some feature'`)
4. 推送到新分支 (`git push origin my-new-feature`)
5. 创建一个合并请求


[github]:https://github.com/tinysou/tinysou-python
[setup]:http://dashboard.tinysou.com/signup
[fork]:https://github.com/tinysou/tinysou-python/fork
[examples]:https://github.com/tinysou/tinysou-python/tree/master/examples
