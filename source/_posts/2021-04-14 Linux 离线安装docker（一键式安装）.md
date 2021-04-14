---
title: Linux 离线安装docker（一键式安装）
date: 2021.04.14 13:43:44
tags:
  - Linux
  - Centos
  - Dockerl
---

## 前言

有时候会遇到服务器不能联网的情况，这样就没法用yum安装软件，docker也是如此，针对这种情况，总结了一下离线安装docker的步骤，分享给大家

### 1. 准备docker离线包

> docker官方离线包下载地址 https://download.docker.com/linux/static/stable/x86_64/

下载需要安装的docker版本，我此次下载的是
docker-17.03.2-ce.tgz版本

### 2. 准备docker.service 系统配置文件

```service
docker.service
 
[unit]
description=docker application container engine
documentation=https://docs.docker.com
after=network-online.target firewalld.service
wants=network-online.target
 
[service]
type=notify
# the default is not to use systemd for cgroups because the delegate issues still
# exists and systemd currently does not support the cgroup feature set required
# for containers run by docker
execstart=/usr/bin/dockerd
execreload=/bin/kill -s hup $mainpid
# having non-zero limit*s causes performance problems due to accounting overhead
# in the kernel. we recommend using cgroups to do container-local accounting.
limitnofile=infinity
limitnproc=infinity
limitcore=infinity
# uncomment tasksmax if your systemd version supports it.
# only systemd 226 and above support this version.
#tasksmax=infinity
timeoutstartsec=0
# set delegate yes so that systemd does not reset the cgroups of docker containers
delegate=yes
# kill only the docker process, not all processes in the cgroup
killmode=process
# restart the docker process if it exits prematurely
restart=on-failure
startlimitburst=3
startlimitinterval=60s
 
[install]
wantedby=multi-user.target
```

### 3. 准备安装脚本和卸载脚本

安装脚本 `install.sh`

```sh
#!/bin/sh
echo '解压tar包...'
tar -xvf $1
echo '将docker目录移到/usr/bin目录下...'
cp docker/* /usr/bin/
echo '将docker.service 移到/etc/systemd/system/ 目录...'
cp docker.service /etc/systemd/system/
echo '添加文件权限...'
chmod +x /etc/systemd/system/docker.service
echo '重新加载配置文件...'
systemctl daemon-reload
echo '启动docker...'
systemctl start docker
echo '设置开机自启...'
systemctl enable docker.service
echo 'docker安装成功...'
docker -v
```

卸载脚本 `uninstall.sh`

```sh
#!/bin/sh
echo '删除docker.service...'
rm -f /etc/systemd/system/docker.service
echo '删除docker文件...'
rm -rf /usr/bin/docker*
echo '重新加载配置文件'
systemctl daemon-reload
echo '卸载成功...'
```

### 4. 安装

1. 此时目录为：(只需要关注docker-17.03.2-ce.tgz、docker.service、install.sh、uninstall.sh即可)

![0b35fb439dd5fbdb2034a050bec19167.png](https://img-blog.csdnimg.cn/img_convert/0b35fb439dd5fbdb2034a050bec19167.png)

2.  执行脚本 `sh install.sh docker-17.03.2-ce.tgz`

执行过程如下：

![f65799ebac2243eb161c2fe79b6a373e.png](https://img-blog.csdnimg.cn/img_convert/f65799ebac2243eb161c2fe79b6a373e.png)

待脚本执行完毕后，执行`docker -v`
发现此时docker已安装成功，可以用`docker --help` 查看docker命令，从现在开始你就可以自己安装image和container了

3. 如果你想卸载docker，此时执行脚本 `sh uninstall.sh`即可，执行过程如下：

![22e01bc886df265c4080e78cc7822d02.png](https://img-blog.csdnimg.cn/img_convert/22e01bc886df265c4080e78cc7822d02.png)

此时输入`docker -v`，发现docker已经卸载

相关资源：[*docker**离线**安装*资源包](http://download.csdn.net/download/weixin_36293623/10728656?spm=1001.2101.3001.5697)