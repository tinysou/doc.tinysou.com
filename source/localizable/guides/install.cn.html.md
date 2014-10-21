---
layout: guides.cn
section: install
toc: article
title: 安装插件
---

微搜索提供三种方式显示搜索结果，也可以通过自定义（高级）的方式。

## 弹出

搜索结果显示在一个弹出窗口，最简单的安装方式。

1.复制代码，粘贴到网页的 HTML 代码中。

```html
<script>
var option = {
  engineKey: '7d5f1328cf2823e0dac6'
};
(function(w,d,t,u,n,s,e){
  s = d.createElement(t);
  s.src = u;
  s.async = 1;
  w[n] = function(r){
    w[n].opts = r;
  };
  e = d.getElementsByTagName(t)[0];
  e.parentNode.insertBefore(s, e);
})(window,document,'script','//tinysou-cdn.b0.upaiyun.com/ts.js','_ts');
_ts(option);
</script>
```

2.复制代码，粘贴到显示搜索框的地方。

```html
<form><input type='text' id='ts-search-input'></form>
```

3.[更多配置][custom-js]

## 嵌入页面

在当前页面的一个区域里，显示搜索结果。

1.添加 div，id 为 `ts-results-container` ，放在要显示搜索结果的地方。

```html
<div id='ts-results-container'></div>
```

2.复制代码，粘贴到要显示搜索框的地方。

```html
<form><input type='text' id='ts-search-input'></form>
```

3.复制代码，粘贴到网页的 HTML 代码中。

```html
<script>
var option = {
  engineKey: '7d5f1328cf2823e0dac6',
  resultContainingElement: '#ts-results-container',
  renderStyle: 'inline'
};
(function(w,d,t,u,n,s,e){
  s = d.createElement(t);
  s.src = u;
  s.async = 1;
  w[n] = function(r){
    w[n].opts = r;
  };
  e = d.getElementsByTagName(t)[0];
  e.parentNode.insertBefore(s, e);
})(window,document,'script','//tinysou-cdn.b0.upaiyun.com/ts.js','_ts');
_ts(option);
</script>
```

4.[更多配置][custom-js]。

## 新页面

搜索结果将显示在新的页面里，推荐高级用户使用。

1.复制代码，为当前页面和显示搜索的页面添加搜索框。

```html
<form><input type='text' id='ts-search-input'></form>
```

2.在显示搜索结果的页面上，添加 div，放在要显示搜索结果的地方。

```html
<div id='ts-results-container'></div>
```

3.复制代码，粘贴到网页的 HTML 代码中。将代码中的 `resultPageURL` 值替换为你用来显示搜索结果的页面地址的路径部分，即 URL 的 `pathname`。例如 `/search.html`。

```html
<script>
var option = {
  engineKey: '7d5f1328cf2823e0dac6',
  resultContainingElement: '#ts-results-container',
  renderStyle: 'new_page',
  resultPageURL: 'YOUR_SEARCH_PAGE_URL'
};
(function(w,d,t,u,n,s,e){
  s = d.createElement(t);
  s.src = u;
  s.async = 1;
  w[n] = function(r){
    w[n].opts = r;
  };
  e = d.getElementsByTagName(t)[0];
  e.parentNode.insertBefore(s, e);
})(window,document,'script','//tinysou-cdn.b0.upaiyun.com/ts.js','_ts');
_ts(option);
</script>
```

4.[更多配置][custom-js]。

[resource_structure]:resource_structure.png
[add-domain]:add-domain.png
[field_types]:/v1/overview.html#3-Field-Types
[search_api]:/v1/searching.html
[custom-js]:/guides/custom-search.html
