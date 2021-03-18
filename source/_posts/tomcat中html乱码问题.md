---
title: tomcat中html乱码
date: 2021-03-18 15:08:05
tags:
  - Tomcat
---

## tomcat中html乱码问题下面的配置都在conf文件夹里面配置
1. 在web.xml中配置
```xml
<mime-mapping>
    <extension>htm</extension>
    <mime-type>text/html;charset=UTF-8</mime-type>
</mime-mapping>
<mime-mapping>
    <extension>html</extension>
    <mime-type>text/html;charset=UTF-8</mime-type>
</mime-mapping>
```
2. Tomcat server.xml中设置编码Tomcat
添加一个 URIEncoding="UTF-8"
```xml
<Connector port="8080" protocol="HTTP/1.1"  connectionTimeout="20000"
         redirectPort="8443" URIEncoding="UTF-8"/>
```
3. 在idea中配置
```
-Dfile.encoding=UTF8
```
![tomcat1.jpeg](https://myblog-1252842020.cos.ap-guangzhou.myqcloud.com/tomcat1_1574570030361.jpeg)
![tomcat2.jpeg](https://myblog-1252842020.cos.ap-guangzhou.myqcloud.com/tomcat2_1574570035567.jpeg)