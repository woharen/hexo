## idea 下 springboot 热部署## 依赖
```xml
<!--热部署 begin-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
</dependency>
<!--热部署 end-->
```
## 配置idea
1. 找到idea的Preferences -> Build, Execution, Deployment -> Compiler，勾选Build project automatically

2. 回到idea正常界面，Mac使用快捷键shift+option+command+/，window上的快捷键是Shift+Ctrl+Alt+/，打开Registry，勾选
compiler.automake.allow.when.app.running