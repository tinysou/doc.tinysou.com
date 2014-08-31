## 总述

本 SDK 提供微搜索API的 Node.js 封装。直接使用本SDK能够帮助开发者方便快捷地使用微搜索的各项功能。本 SDK 的源码托管在 github， 可前往查看[最新版本][github]。源码同时附带 demo，方便用户学习使用。

## 准备

### 环境依赖

本 SDK 适用于 Node.js 0.8.0 及以上版本

### 安装

通过npm以模块化的方式安装

```
npm install tinysou

```

### auth_token

使用本SDK之前，你需要有一个 auth_token 进行授权。

在你的 账号设置-> Auth 设置 里，你可以找到你的 auth_token；如果你没有账号，请先[注册][setup]。

## 使用

### 初始化环境

首先请先在你的应用里 require tinysou，并填写你的 auth_token：

```javascript
var Tinysou = require('tinysou')
var tinysou = new Tinysou('your_token')
```

### Engine

#### 罗列`engine`
罗列出你的所有`engine`：
```javascript
tinysou.engines.list(function(err, res) {
  console.log(res);
});
```

#### 创建`engine`

创建一个名称为 'blog'，显示名为 'Blog' 的`engine`：

```javascript
tinysou.engines.create({
  name: 'blog',
  display_name: 'Blog'
}, function(err, res) {
  console.log(res);
});
```

#### 获取一个`engine`

通过名称 'blog' 获取该`engine`：

```javascript
tinysou.engines.get('blog', function(err, res) {
  console.log(res);
});
```

#### 更新一个`engine`

对 'blog' 进行更新：

```javascript
tinysou.engines.update('blog', {
  display_name: 'My Blog'
}, function(err, res) {
  console.log(res);
});
```
只有`engine`的显示名可以被更新。如示例代码中，将原来的 'Blog' 更改为 'My Blog'。

#### 删除一个`engine`

删除名为`blog`的`engine`:

```javascript
tinysou.engines.delete('blog', function(err, res) {
  console.log(res);
});
```

### Collection

#### 罗列`collection`

罗列出 'blog' 内包含的所有`collection`:

```javascript
tinysou.collections.list('blog', function(err, res) {
  console.log(res);
});
```

#### 创建一个`collection`

在 'blog' 下创建一个`collection`，名为 'posts'，在`posts`里有 'title'，'tags'，'author'，'date' 和 'body' 这些 `field`，并分别指定这些`field·的类型，如 'title' 类型为 'string' :

```javascript
tinysou.collections.create('blog', {
  name: 'posts',
  field_types: {
    title: 'string',
    tags: 'string',
    author: 'enum',
    date: 'date',
    body: 'text'
  }
}, function(err, res) {
  console.log(res);
});
```

#### 获取一个`collection`

返回'blog'下的 'posts'：

```javascript
tinysou.collections.get('blog', 'posts', function(err, rest) {
  console.log(res);
});
```

#### 删除一个collection

删除'blog'下的 'posts'：

```javascript
tinysou.collections.delete('blog', 'posts', function(err, res) {
  console.log(res);
});
```

### Document

#### 罗列 `document`

罗列出 'blog' 下、'posts'里的所有`document`

```javascript
tinysou.documents.list('blog', 'posts', function(err, res) {
  console.log(res);
});
```

#### 创建一个 `document`

在 'blog' 中 'posts' 里创建一个 `document`:

```javascript
tinysou.documents.create('blog', 'posts', {
  title: 'My First Post',
  tags: ['news'],
  author: 'Author',
  date: '2014-08-16T00:00:00Z',
  body: 'Tinysou start online today!'
}, function(err, res) {
  console.log(res);
});
```

#### 获取一个 `document`

与`engine`和`collection`不同，`document`只能通过唯一的'documentId'来进行访问。在 'blog' 中的 'posts' 里，依据 documentId 获取一个`document`:

```javascript
tinysou.documents.get('blog', 'post', documentId, function(err, res) {
  console.log(res);
});
```


#### 更新一个 `document`

在 'blog' 中的 'posts' 里更新一个 `document`:

```javascript
tinysou.documents.update('blog', 'post', documentId, {
  title: 'My First Post',
  tags: ['news'],
  author: 'Author',
  date: '2014-08-16T00:00:00Z',
  body: 'Tinysou start online today!'
}, fucntion(err, res) {
  console.log(res);
});
```

#### 删除一个 `document`

在 'blog' 中的 'posts' 里删除一个 `document`:

```javascript
tinysou.documents.delete('blog', 'post', documentId, function(err, res) {
  console.log(res);
});
```









[github]:https://github.com/tinysou/tinysou-node
[setup]:http://dashboard.tinysou.com/signup
