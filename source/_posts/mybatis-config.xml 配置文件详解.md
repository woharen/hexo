## mybatis-config.xml 配置文件详解
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--通过这个配置文件，完成mybatis与数据库的连接  -->
<configuration>
    <!--  环境  -->
    <environments default="development">
    <!--    环境变量    -->
        <environment id="development">
            <!--  事务管理器，指定事务管理类型，type="JDBC"指直接简单使用了JDBC的提交和回滚设置 -->
            <transactionManager type="JDBC"/>
            <!--  dataSource指数据源配置，POOLED是JDBC连接对象的数据源连接池的实现。 -->
            <dataSource type="POOLED">
                <!--  数据属性 -->
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://127.0.0.1:3306/mybatis_study"/>
                <property name="username" value="root"/>
                <property name="password" value="ps123456"/>
            </dataSource>
        </environment>
    </environments>
    <!-- pojo的映射文件 -->
    <mappers>
        <!-- 用来告诉MyBatis从哪里去找映射文件，进而找到这些SQL语句。 -->
        <mapper resource="org/mybatis/example/stuMapper.xml"/>
    </mappers>
</configuration>
```
