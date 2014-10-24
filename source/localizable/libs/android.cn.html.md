---
layout: libs.cn
section: android
toc: article
title: Android 库
---


## 总述

Android 库通过调用 public 接口，实现发送搜索请求、接收处理搜索响应等功能。直接使用该库能够帮助开发者在 Android 应用上快速添加微搜索，实现搜索和自动补全的功能。所有源码托管在 GitHub， 开发者们可前往查看[最新版本][github]。源码同时附带 [例子][examples]，方便用户学习使用。

## 导入

导入 tinysou-android-library-v1.00.jar 到 Android project 的 libs 目录中
在 Android Studio 中，鼠标右击 jar 文件，选择 Add As Library

## 使用

### 初始化
填写微搜索的 engine_key

```java
    String engine_key = "YOUR_ENGINE_KEY";
     // 初始化客户端
    TinySouClient client = new TinySouClient(engine_key);
```
    

### 搜索 API

发送搜索请求，获得搜索结果

```java
    // 设置搜索结果来自于第几页
    client.setPage(searchPage);
    // 获得 String 格式的搜索结果
    String result = client.Search(search_content);
```
    
### 自动补全 API

发送自动补全请求，获得自动补全结果

```java
    // 获得 String 格式的自动补全结果
    String result = client.AutoSearch(search_content);
```
    
其他参数的获取方法请参考微搜索的[API文档](http://doc.tinysou.com/v1/overview.html)。

[github]:https://github.com/tinysou/tinysou-android
[examples]:https://github.com/tinysou/tinysou-android-demo
