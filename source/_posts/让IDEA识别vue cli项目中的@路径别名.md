---
title: 让IDEA识别vue cli项目中的@路径别名,支持跳转
date: 2020-02-10 09:21
categories: 教程
tags: 
- vue
- idea
- front
---

新的vue cli项目是@进行别名的导入是无法进行跳转的，这个如何处理呢？

<!--more-->
# 如何配置
在`setting -> languages&frameworks -> webpack`里选择配置文件路径为 `node_modules/@vue/cli-service/webpack.config.js`即可。

## scss文件

需要注意的是如果在scss中使用@别名则需要加~号。

比如在src目录下有一个var.scss文件，其他文件引用时则需写成：

```js
   @import "~@/var.scss";
```




### 参考文章：
https://stackoverflow.com/questions/51588009/vue-js-configuring-webstorm-to-set-in-path-files-to-the-src-folder