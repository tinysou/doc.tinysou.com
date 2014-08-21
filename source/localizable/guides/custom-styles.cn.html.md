---
layout: guides.cn
section: custom-styles
toc: article
title: 自定义搜索样式
---

你可以通过添加 CSS 代码来覆盖默认的样式。

注意：为了定制微搜索样式，你需要对HTML和CSS相关知识有一些了解。

## 搜索框样式

安装微搜索搜索框时，可以为 `input` 元素添加 `ts-search-input` CSS class来覆盖默认的样式。

```
<form>
  <input type="text" id="ts-search-input" class="ts-searching-input" />
</form>
```

定制搜索表单的样式，需要覆盖 `form input.ts-search-input`样式。如果去掉 `ts-search-input` class，表单将使用你网站的默认表单样式。

你可以随意定制搜索框的样式。例如：

```
body form input.ts-search-input {
  color: green;
  font-weight: bold;
}
```

## 自动补全下拉框样式

定制自动补全下拉框的样式，需覆盖 `.tinysou-widget .autocomplete` 的默认样式。

```
/* 改变搜索结果字体颜色 */
.tinysou-widget .autocomplete ul li p.title {
  color: red;
}
/* 改变选中的搜索结果的背景 */
.tinysou-widget .autocomplete ul li.active {
  background: none;
  background-color: red;
  border-top: 1px solid darkred;
  border-bottom: 1px solid darkred;
  -webkit-box-shadow: 0 1px 0 darkred inset;
}
/* 改变section的字体颜色 */
.tinysou-widget .autocomplete ul li.active .sections em {
  color: white;
}
```
## 搜索结果样式

### 搜索结果链接颜色

默认情况下，搜索结果链接的颜色是你网站的默认链接样式。如果希望改变默认的样式，需要为 `.st-result a` 添加样式。

```
.ts-result a {
  color: #C0242D;
}
.ts-result a:visited {
  color: #0B2644;
}
.ts-result a:hover {
  color: red;
}
```

### 搜索结果高亮


如果希望改变搜索结果中匹配内容的高亮样式，需覆盖 `em` 标签的默认样式。

```
.ts-snippet em {
  font-weight: bold;
  font-style: normal;
  font-size: 150%;
  text-transform: uppercase;
}
```

### 搜索结果字体

你也可以改变搜索结果的字体样式，通过覆盖 `ts-result` class的默认样式实现。

```
.ts-result {
  font-family: 'Comic Sans MS';
}
.ts-result h3 a {
  font-family: 'Comic Sans MS';
  font-size: 20px;
}
.ts-result .ts-snippet {
  color: Crimson;
  font-size: 18px;
}
```

## 更多定制

如果你熟悉HTML/CSS/Javascript，可以通过查看[微搜索插件](https://github.com/tinysou/tinysou-search.js)来定制搜索结果的显示。
