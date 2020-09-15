---
title: NTP服务器 - 时间同步
date: 2020-09-15 14:10:47
tags:
  - windowns
  - linux
---

## 最近在研究集群的时候发现基本都有一项设置，那就是配置ntp服务器
为什么要配置ntp服务器呢?

我们先看看什么是NTP
>NTP是用于同步网络中计算机时间的协议，全称为网络时间协议（Network Time Protocol）。

那有什么用呢，我们看看腾讯云对他的介绍
> 网络时间协议（Network Time Protocol，NTP），用于同步网络中各个计算机的时间的协议。其用途是将计算机的时钟同步到世界协调时 UTC。在 NTP 设计时考虑到了各种网络延迟，当您通过公共网络同步时，误差可以降低到10毫秒以内；当您通过本地网络同步时，误差可以降低到1毫秒。

总体就是说，为了保证时区和时间一致性以免影响到业务，这里可以看看阿里云怎么说。
> 时区和时间一致性对于云服务器ECS非常重要，有时会直接影响到任务执行的结果，例如，您在更新数据库或者分析日志时，时间顺序对结果有很大影响。为避免在实例上运行业务时出现逻辑混乱和网络请求错误等问题，您需要统一相关实例的时区设置。另外，您还可以通过NTP服务同步网络中所有服务器的本地时间。

下面介绍下国内的一些网络授时服务器

国家科学院国家授时中心
```
ntp.ntsc.ac.cn
```

阿里云
```
ntp.cloud.aliyuncs.com
```

腾讯云
```
time1.cloud.tencent.com
time2.cloud.tencent.com
time3.cloud.tencent.com	
time4.cloud.tencent.com
time5.cloud.tencent.com
```

华为云
```
华北区 ntp.myhuaweicloud.com
华东区 ntp.myhuaweicloud.com
华南区 ntp.myhuaweicloud.com
东北区 ntp.myhuaweicloud.com
亚太-香港 ntp.myhuaweicloud.com
亚太-曼谷 ntp.myhuaweicloud.com
```

### 安装

[Linux 实例设置 NTP 服务](https://cloud.tencent.com/document/product/213/30393)

[Windows 实例设置 NTP 服务](https://cloud.tencent.com/document/product/213/30394)

### 参考

[中科院 - 关于“网络授时域名”全面试运行测试的公告](http://www.ntsc.ac.cn/shye/tzgg/201809/t20180921_5086032.html)

[阿里云NTP服务器](https://help.aliyun.com/document_detail/92704.html?spm=a2c4g.11186623.3.4.6ab43f0cdAn3yv)

[腾讯云NTP 服务概述](https://cloud.tencent.com/document/product/213/30392)

[华为云有没有提供NTP服务器，怎样安装？](https://support.huaweicloud.com/ecs_faq/zh-cn_topic_0093971249.html)