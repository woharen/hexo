## java定时任务## 依赖

```xml
 <!-- jar包版 -->
 <dependency>
    <groupId>org.quartz-scheduler</groupId>
    <artifactId>quartz</artifactId>
     <version>2.3.1</version>
 </dependency>


<!--  启动器版    -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-quartz</artifactId>
    <version>2.1.6.RELEASE</version>
</dependency>
```

## 使用方式

### 1. 启动类注解

@EnableScheduling

```java
@EnableScheduling
@SpringBootApplication
@MapperScan("com.panshi.qssys.dao")
public class MarketingApp {
    public static void main(String[] args) {
        SpringApplication.run(MarketingApp.class);
    }
}
```

### 2. 定时任务类

@Component

所有的任务类命名规范   xxxTask , xxxTask
放到task包下
当任务比较多的时候，可以将task形成一个模块，项目

### 3. 定时方法

使用`cron`表达式

```java
@Scheduled(cron = "0/5 * * * * *")
public void method(){
    System.out.println(111);
}
```

[cron表达式](http://cron.qqe2.com/)

