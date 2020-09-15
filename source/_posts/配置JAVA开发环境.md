---
title: 配置JAVA开发环境
date: 2019.07.01 10:06
tags:
  - java
---



## 配置JAVA开发环境# JAVA 环境

在环境变量增加以下配置

JAVA_HOME
```
C:\Program Files\Java\jdk1.8.0_181
```

PATH
```
%JAVA_HOME%\bin\
%JAVA_HOE%\jre\bin\
```

CLASSPATH
```
.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar
```

## 测试命令
```
java -version
javac -version
```

