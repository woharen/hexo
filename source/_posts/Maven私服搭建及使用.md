## Maven私服搭建及使用# Maven私服搭建及使用

## 一、搭建环境

### 1. 安装包

[安装包下载链接](https://www.sonatype.com/oss-thank-you-tar.gz)

[当前服务器使用nexus  3.19.1-01](https://sonatype-download.global.ssl.fastly.net/nexus/3/latest-unix.tar.gz)

### 2. 安装步骤

具体步骤参考资料 `Linux下使用Nexus创建maven私服`

## 二、注意事项

### 密码错误

帐号: admin

当前服务器使用的版本的初始密码不是之前的   admin123 

具体密码在 `/nexus-data/admin.password `中  （明文未加密）

### 部署jar出现401权限错误

该pom文件的pom.xml文件需配置

```xml
<distributionManagement>
    <!-- 正式版 -->
    <repository>
        <!-- id和maven的相关id一致 -->
        <id>nexus-releases</id>
        <name>nexus-releases</name>
        <url>http://jed:8081/repository/hadoop-hosted-test-repository/</url>
    </repository>
    <!-- 快照版 -->
    <snapshotRepository>
        <!-- id和maven的相关id一致 -->
        <id>nexus-snapshot</id>
        <name>nexus-snapshot</name>
        <url>http://jed:8081/repository/hadoop-hosted-test-repository-snapshots/</url>
    </snapshotRepository>
</distributionManagement>
```

maven的 settings.xml 需配置认证信息

```
<servers>
    <server>
      <id>nexus-releases</id>
      <username>hadoop</username>
      <password>hadoop</password>
    </server>
    <server>
      <id>nexus-snapshot</id>
      <username>hadoop</username>
      <password>hadoop</password>
    </server>
</servers>

...

<mirror>
    <id>dev-repository-group</id>
    <mirrorOf>*</mirrorOf>
    <name>dev-repository-group</name>
    <url>http://192.168.8.123:8081/repository/dev-repository-group</url>
</mirror>
<mirror>
    <id>dev-repository-hosted-snapshot</id>
    <mirrorOf>*</mirrorOf>
    <name>dev-repository-hosted-snapshot</name>
    <url>http://192.168.8.123:8081/repository/dev-repository-snapshot</url>
</mirror>
```

## 参考资料

[Linux下使用Nexus创建maven私服]( https://cloud.tencent.com/developer/article/1336556 )

额外参考 [本地私服仓库nexus3.3.1使用手册]( https://cloud.tencent.com/developer/article/1098081 )

[Docker 搭建Maven私服nexus 3.17初始密码登录不上问题/admin登陆不上问题]( https://www.cnblogs.com/wbl001/p/11154828.html )