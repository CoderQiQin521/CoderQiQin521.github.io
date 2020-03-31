---
title: restful API
date: 2019-11-20 15:01:01
tags: [resetful,api,接口规范]
categories: server
---
> demo地址: [https://www.showdoc.cc/567957914338376?page_id=3352725715640070](https://www.showdoc.cc/567957914338376?page_id=3352725715640070)

URI格式规范
URI(Uniform Resource Identifiers) 统一资源标示符
URL(Uniform Resource Locator) 统一资源定位符
URI的格式定义如下：
URI = scheme "://" authority "/" path [ "?" query ] [ "#" fragment ]

URL是URI的一个子集(一种具体实现)，对于REST API来说一个资源一般对应一个唯一的URI(URL)。在URI的设计中，我们会遵循一些规则，使接口看起透明易读，方便使用者调用。

关于分隔符“/”的使用
"/"分隔符一般用来对资源层级的划分，例如
http://api.canvas.restapi.org/shapes/polygons/quadrilaterals/squares

对于REST API来说，"/"只是一个分隔符，并无其他含义。为了避免混淆，"/"不应该出现在URL的末尾。例如以下两个地址实际表示的都是同一个资源：
http://api.canvas.restapi.org/shapes/
http://api.canvas.restapi.org/shapes

REST API对URI资源的定义具有唯一性，一个资源对应一个唯一的地址。为了使接口保持清晰干净，如果访问到末尾包含 "/" 的地址，服务端应该301到没有 "/"的地址上。当然这个规则也仅限于REST API接口的访问，对于传统的WEB页面服务来说，并不一定适用这个规则。

URI中尽量使用连字符"-"代替下划线"_"的使用
连字符"-"一般用来分割URI中出现的字符串(单词)，来提高URI的可读性，例如：
http://api.example.restapi.org/blogs/mark-masse/entries/this-is-my-first-post

使用下划线""来分割字符串(单词)可能会和链接的样式冲突重叠，而影响阅读性。但实际上，"-"和""对URL中字符串的分割语意上还是有些差异的："-"分割的字符串(单词)一般各自都具有独立的含义，可参见上面的例子。而"_"一般用于对一个整体含义的字符串做了层级的分割，方便阅读，例如你想在URL中体现一个ip地址的信息：210_110_25_88 .

## 参考资料
  1. [理解RESTful架构https://www.ruanyifeng.com/blog/2011/09/restful.html](https://www.ruanyifeng.com/blog/2011/09/restful.html)
  2. https://blog.csdn.net/qq_41606973/article/details/86352787