## 通过 pom 设置 Maven 通过 JDK 1.8 进行编译 ——maven-compiler-plugin插件maven项目会用maven-compiler-plugin默认的jdk版本来进行j编译，如果不指明版本就容易出现版本不匹配的问题，可能导致编译不通过的问题。解决办法：在pom文件中配置maven-compiler-plugin插件（以jdk1.8）。

1、方式一
```xml
<properties>
    <maven.compiler.target>1.8</maven.compiler.target>
    <maven.compiler.source>1.8</maven.compiler.source>
</properties>
```
2、方式二
```xml
 <build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.6.1</version>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
            </configuration>
        </plugin>
    </plugins>
</build>
```