## Centos7下docker使用## 安装

1. 安装需要的软件包

   ```shell
   yum install -y yum-utils device-mapper-persistent-data 
   ```

2.  设置yum源 

   ```shell
   yum-config-manager --add-repo
   ```

3. 查看docker版本

   ```shell
   yum list docker-ce --showduplicates | sort -r
   ```

4. 安装docker

   ```shell
   yum install docker-ce-版本号
   ```

## 容器使用

```shell
# 启动
systemctl start docker
# 停止
systemctl stop docker
# 查看所有容器的状态
docker ps -a
```

### 查看容器

- docker ps

   查看当前正在运行的容器 

- docker ps -a

  查看所有容器的状态

```
# 容器编号      图片     命令      已创建    状态    映射端口  名字
CONTAINER ID   IMAGE   COMMAND   CREATED   STATUS   PORTS   NAMES
```

### 操作容器

-  docker start|stop|restart   id/name

  根据id或者name启动、停止、重启容器

  - **-d** 后台运行容器
  - **-p 8800:80** 指定对外暴露的端口，容器内部用80，对外8800 
  - **--name** 指定容器的名字

- docker rm id/name

  删除某个容器

-  docker rename old_name new_name 

   给这个容器命名。再次启动或停止容器时，就可以直接使用这个名字。 

## 镜像使用

### 查看镜像

- docker images

  查看所有镜像

### 操作镜像

- docker run 镜像名

  将进行放到容器中， 变成运行时容器 

-  docker rmi id/name 

   删除某个镜像 