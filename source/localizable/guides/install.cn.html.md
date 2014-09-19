---
layout: guides.cn
section: install
toc: article
title: 安装插件
---

微搜索提供三种方式显示搜索结果，也可以通过自定义（高级）的方式。

## 弹出

搜索结果显示在一个弹出窗口，最简单的安装方式。

1.复制代码，粘贴到网页的HTML代码中。

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

1.添加div，id为 `ts-results-container` ，放在要显示搜索结果的地方。

```html
<div id='ts-results-container'></div>
```

2.复制代码，粘贴到要显示搜索框的地方。

```html
<form><input type='text' id='ts-search-input'></form>
```

3.复制代码，粘贴到网页的HTML代码中。

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

1.复制代码，粘贴到要显示搜索框的地方。

```html
<form><input type='text' id='ts-search-input'></form>
```

2.在显示搜索结果的页面上，添加div，放在要显示搜索结果的地方。

```html
<div id='ts-results-container'></div>
```

3.复制代码，粘贴到网页的HTML代码中。将代码中的 `resultPageURL` 值替换为你用来显示搜索结果的页面的地址。例如 `/search.html`。

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

## 高级

用TinySou库实现一个自定义的搜索框，仅供高级用户使用。

1.需要在你的网页中包含 `jquery.tinysou.min.js` , `tinysou.min.css` 和其依赖库文件，如下：

```html
<script src='//tinysou-cdn.b0.upaiyun.com/jquery.tinysou.min.js'></script>
<script src='jquery.js'></script>
<script src='jquery.modal.js'></script>
<script src='jquery.hashchange.js'></script>
<script src='hogan.js'></script>
<link type='text/css' rel='stylesheet' href='//tinysou-cdn.b0.upaiyun.com/tinysou.min.css' media='all' />
```

2.添加搜索框。

```html
<form><input type='text' id='ts-search-input'></form>
```

3.添加显示搜索结果的div。

```html
<div id='ts-results-container'></div>
```

4.配置插件。

```html
<script>
  $(document).ready(function(){
    $('#ts-search-input').tinysouSearch({
      engineKey: '7d5f1328cf2823e0dac6',
      perPage: 4,
    });
  })
</script>
```

5.[更多配置][custom-js]。

[resource_structure]:resource_structure.png
[add-domain]:add-domain.png
[field_types]:/v1/overview.html#3-Field-Types
[search_api]:/v1/searching.html
[custom-js]:/guides/custom-search.html
