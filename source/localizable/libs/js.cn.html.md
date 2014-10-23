---
layout: libs.cn
section: javascript
toc: article
title: Javascript 库
---

## 总述

Javascript 库为网站的搜索框供搜索功能。直接使用该库能够帮助开发者方便快捷地使用微搜索的各项功能。所有源码托管在 github， 开发者们可前往查看 [最新版本][github]。源码同时附带 [例子][examples]，方便用户学习使用。

## 准备

### 环境依赖

本库提供分别基于 jquery 和 zepto 的 TinySou 插件，移动端将使用基于 Zepto 的轻量级 Javascript 库，PC 端使用基于 Jquery 库的插件。

### 安装

通过 npm 以模块化的方式安装

```
npm install tinysouJS
```

### engine_key

使用 Javascript 库之前，你需要有一个 engine_key。

在 TinySou 控制台 Engines-> 概述 里，你可以找到你的 engine_key；如果你没有账号，请先 [注册][setup]。

## 使用

### 初始化

 复制粘贴下面的代码到 HTML 得 script 标签中，并填写你的 engine_key：

```html
<form><input type='text' id='ts-search-input'></form>
```

```js
var option = {
  engineKey: 'YOUT_ENGINE_KEY'
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
```

### Options

你可以通过传入特定的 options 值，来方便定制搜索行为。下面是默认的 option 列表：

```js
$.fn.tinysouSearch.defaults = {
  attachTo: undefined,
  collection: 'page',
  // filters: undefined,
  engineKey: undefined,
  searchFields: undefined,
  functionalBoosts: undefined,
  sort: undefined,
  fetchFields: ['title', 'url', 'sections'],
  renderStyle: undefined,
  resultPageURL: undefined,
  resultContainingElement: undefined,
  preRenderFunction: undefined,
  postRenderFunction: defaultPostRenderFunction,
  loadingFunction: defaultLoadingFunction,
  renderResultsFunction: defaultRenderResultsFunction,
  renderFunction: defaultRenderFunction,
  perPage: 10,
  spelling: 'strict',
  //autocomplete
  activeItemClass: 'active',
  noResultsClass: 'noResults',
  noResultsMessage: undefined,
  onComplete: defaultOnComplete,
  renderActResultsFunction: defaultRenderActResultsFunction,
  renderActFunction: defaultRenderActFunction,
  dropdownStylesFunction: defaultDropdownStylesFunction,
  resultLimit: 5,
  autocompleteListType: 'ul',
  autocompleteListClass: 'autocomplete',
  resultListSelector: 'li',
  setWidth: true,
  typingDelay: 80,
  disableAutocomplete: false,
  autocompleteContainingElement: 'body'
};
```

## 参与

1. Fork 我们的项目 ( [https://github.com/tinysou/tinysou-search.js/fork][fork] )
2. 创建你的新分支 (`git checkout -b my-new-feature`)
3. 提交你的修改 (`git commit -am 'Add some feature'`)
4. 推送到新分支 (`git push origin my-new-feature`)
5. 创建一个合并请求


[github]:https://github.com/tinysou/tinysou-search.js
[setup]:http://dashboard.tinysou.com/signup
[fork]:https://github.com/tinysou/tinysou-search.js/fork
[examples]:https://github.com/tinysou/tinysou-search.js/tree/master/examples
