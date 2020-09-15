## spring-boot 经典布局 及 启动配置## 依赖
pom.xml
```xml

<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
</properties>
    <!--
         spring-boot-starter-parent是一个特殊启动器，提供一些Maven的默认值。
         它还提供依赖管理 dependency-management 标签，以便您可以省略子模块依赖关系的版本标签。
    -->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.1.6.RELEASE</version>
</parent>
<dependencies>


    <!-- 依赖Spring boot web 启动器 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!--配置文件提示-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-configuration-processor</artifactId>
        <optional>true</optional>
    </dependency>
</dependencies>

    <!-- 可执行jar插件； 运行jar就等于运行了一个web项目-->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```

## 布局
com
- example
    - myproject
        - Application.java
        - domain
            - Customer.java
            - CustomerRepository.java
        - service
            - CustomerService.java
        - web
            - CustomerController.java

## 启动函数
```java
package com.example.myproject;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

//@Configuration
//@EnableAutoConfiguration
//@ComponentScan
//以上注解可直接换成@SpringBootApplication使用
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```
## 静态资源
- 默认情况下，SpringBoot将从类路径或ServletContext的根目录中的名为/static（或/public或/resources或/META-INF/resources）的目录提供静态内容
- 它使用Spring MVC中的ResourceHttpRequestHandler，因此您可以通过添加自己的WebMvcConfigurerAdapter并覆盖addResourceHandlers方法来修改该行为。
- 默认情况下，资源映射到/ ，但可以通过spring.mvc.static-path-pattern调整。 例如，将所有资源重定位到 /resources/可以配置如下：
    ```xml
    spring.mvc.static‐path‐pattern=/resources/**
    
    
    # 修改前
    127.0.0.1:8080/xxx.html
    # 修改后
    127.0.0.1:8080/resources/xxx.html
    ```

## 错误页
resources.static.error.404.html

## 自定义图标
resources.static.favicon.ico

## 热部署
pom.xml依赖
<!-- 热部署 -->

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
</dependency>
```


IDEA配置

当我们修改了Java类后，IDEA默认是不自动编译的，而spring-boot-devtools又是监测classpath下的文件发生变化才会重启应用，所以需要设置IDEA的自动编译：

（1）File-Settings-Compiler-Build Project automatically

（2）ctrl + shift + alt + /,选择Registry,勾上 Compiler autoMake allow when app running

你可以实时自动重启了