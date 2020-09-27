---
title: decodeURIComponent出现Uncaught URIError URI malformed错误
date: 2020.09.27 10:10
tags:
  - javascript
---

最近使用showdoc编辑表格后突然数据丢失，研究源码发现是在解析时出现错误导致渲染失败

>url加密传参有时候会出现Uncaught URIError: URI malformed的错误，这是因为你的url中包含了“%”字符，浏览器在对“%”执行decodeURIComponent时报错，正确的解决是将%全部替换为%25再进行传输：
>urlStr.replace(/%/g, '%25');