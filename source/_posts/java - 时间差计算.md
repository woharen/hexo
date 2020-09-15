---
title:  时间差计算代码演示
date: 2019.07.01 10:06
tags:
  - java
description: 时间差计算代码演示
copyright: true
---

# java - 时间差计算代码演示
```java
/**
 * 获取两个日期的时间差
 * @param endDate  结束时间
 * @param currentDate   当前时间
 * @return
 */
public  static  long  getTimeDifference(String endDate ,String currentDate){
    
    DateFormat df = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
    try {
        Date end = df.parse(endDate);
        Date current = df.parse(currentDate);
        long diff = end.getTime() - current.getTime();
        long days = diff / (1000 * 60 * 60 * 24);
        return  days;
    } catch (Exception e) {
        System.out.println("算时间差错误"+e);
    }
    return 0;
}
```