---
title:  分页插件pagehelper使用
date: 2019.07.01 10:06
tags:
  - java
---

## 分页插件pagehelperjar包
```xml
<!--分页插件 begin-->
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper</artifactId>
    <version>5.1.10</version>
</dependency>
```

代码
```java
//设置分页属性
PageHelper.startPage(recordQuery.getPageNum(), recordQuery.getPageSize());
List<RecordVo> list = recordService.findRecord(recordQuery);
PageInfo<RecordVo> pageInfo = new PageInfo<>(list);
```
