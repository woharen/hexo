## mongodb入门## 简介
MongoDB 是由C++语言编写的，是一个基于分布式文件存储的开源数据库系统。
在高负载的情况下，添加更多的节点，可以保证服务器性能。
MongoDB 将数据存储为一个文档，数据结构由键值(key=>value)对组成。MongoDB 文档类似于 JSON 对象。字段值可以包含其他文档，数组及文档数组。
## 使用
### 下载&安装
https://www.mongodb.com/download-center#community

- 创建数据目录

   MongoDB将数据目录存储在db目录下。但是这个数据目录不会主动创建，我们在安装完成后需要创建它。

### 启动MongoDB服务

```dos
创建数据库目录
例如：E:\mongodb\db

执行如下命令 (启动服务器) (进入到mongodb的bin目录)
> mongod --dbpath E:\mongodb\db

使用客户端连接 (到mongodb的安装目录下)
> mongo.exe
```

### 使用配置文件

1. 在安装目录下创建配置文件 config.cfg

2. 编辑内容

```cfg
#数据库数据存放目录
dbpath=E:\mongodb\db
#端口号 默认为27017
port=27017 
#mongodb所绑定的ip地址(运行本机与100两台电脑访问)
bind_ip=127.0.0.1,192.168.1.100
```

3. 启动命令

```dos
> mongod -f config.cfg
```

##  概念解析
| SQL术语/概念 | MongoDB术语/概念 | 解释/说明                           |
| ------------ | ---------------- | ----------------------------------- |
| database     | database         | 数据库                              |
| table        | collection       | 数据库表/集合                       |
| row          | document         | 数据记录行/文档                     |
| column       | field            | 数据字段/域                         |
| index        | index            | 索引                                |
| table joins  | 不支持           | 表连接,MongoDB不支持                |
| primary key  | primary key      | 主键,MongoDB自动将_id字段设置为主键 |