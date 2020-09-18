---
title:  lombok plugin插件
date: 2020.09.18 14:00
tags:
  - java
---

>Lombok项目是一个Java库，它会自动插入编辑器和构建工具中，Lombok提供了一组有用的注释，用来消除Java类中的大量样板代码。仅五个字符(@Data)就可以替换数百行代码从而产生干净，简洁且易于维护的Java类。  ----百度百科
## 使用步骤
1.  添加lombok plugin插件
![lombok.jpeg](https://myblog-1252842020.cos.ap-guangzhou.myqcloud.com/lombok_1574561506851.jpeg)

2.添加lombok依赖,依赖的版本要跟加入插件的版本一致
```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.8</version>
    <scope>provided</scope>
</dependency>
```
3. 启用配置
![lombok2.jpeg](https://myblog-1252842020.cos.ap-guangzhou.myqcloud.com/lombok2_1574561576214.jpeg)
![lombok3.jpeg](https://myblog-1252842020.cos.ap-guangzhou.myqcloud.com/lombok3_1574561584715.jpeg)

4.代码中使用
```java
//使用该注解就可以省略 get set toString啦
@Data
public class{
    private String name;
    private ing age;
}
```

## 日志
**1 使用**
包名：
```java
import lombok.extern.log4j.Log4j2;
```
注解：
```java
@Log4j2
```
使用
```java
log.info("log日志打印")
```
**2. 配置**
```xml
application.yml的配置：
#debug: true
logging:
  level:
    #指定包，指定类，或者直接root
    #root: debug
    com.cobra.logdemo.controller: debug
  #日志输出配置二选一，只有两个都配置，只有一个生效，
  #区别是path默认生成的是spring.log文件，而path生成的是直接命名的文件，
  可以是相对路径也可以是绝对路径
  file: var/my.log
  #path: var/my.log
#  pattern:
    #控制台日志输出格式配置，仅对控制台有效
    #console: "%d -%msg%n"
```
采用配置文件形式
```xml
log配置之二，新建logback-spring.xml：
<?xml version="1.0" encoding="UTF-8" ?>
<configuration>
    <!--输出日志格式-->
    <appender name="consoleLog" class="ch.qos.logback.core.ConsoleAppender">
        <layout class="ch.qos.logback.classic.PatternLayout">
            <pattern>
                %d - %msg%n
            </pattern>
        </layout>
    </appender>
    <!--只保存info日志-->
    <appender name="fileInfoLog" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>INFO</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch>
        </filter>
        <encoder>
            <pattern>
                %d - %msg%n
            </pattern>
        </encoder>
        <!--滚动输出策略-->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!--路径-->
            <fileNamePattern>d:/logtest/info/info.%d.log</fileNamePattern>
        </rollingPolicy>
    </appender>
    
    <!--只保存warn日志-->
    <appender name="fileWarnLog" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>WARN</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch>
        </filter>
        <encoder>
            <pattern>
                %d - %msg%n
            </pattern>
        </encoder>
        <!--滚动输出策略-->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!--路径-->
            <fileNamePattern>d:/logtest/warn/warn.%d.log</fileNamePattern>
        </rollingPolicy>
    </appender>
    
    <!--只保存error日志-->
    <appender name="fileErrorLog" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>ERROR</level>
        </filter>
        <encoder>
            <pattern>
                %d - %msg%n
            </pattern>
        </encoder>
        <!--滚动输出策略-->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!--路径-->
            <fileNamePattern>d:/logtest/error/error.%d.log</fileNamePattern>
        </rollingPolicy>
    </appender>
 
    <root level="info">
        <appender-ref ref="consoleLog"/>
        <appender-ref ref="fileInfoLog"/>
        <appender-ref ref="fileWarnLog"/>
        <appender-ref ref="fileErrorLog"/>
    </root>
 
</configuration>
```

## 链接
[lombok官网](https://projectlombok.org/)