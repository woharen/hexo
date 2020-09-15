---
title:  记一次svn删除文件
date: 2019.07.01 10:06
tags:
  - svn
  - 错误记录
---



## 因为原来的项目结构有问题，导致子项目需要进行改名，结果改名之后推送代码后拉取，发现原来的子项目还存在，这就出现了SVN和git的一个特性了，

> SVN：按需拉取，有什么拉什么，有什么推什么
>
> git：全部拉取或者推送

![image20191125135023988.png](https://myblog-1252842020.cos.ap-guangzhou.myqcloud.com/image-20191125135023988_1574661782974.png)

下面说说怎么处理：

1. 找到要删除的文件所在的目录

2. 右键点击空白处 TortoiseSVN -> Repo-browser(版本库浏览器) 

    ![image-20191125135232186](https://myblog-1252842020.cos.ap-guangzhou.myqcloud.com/image-20191125135232186_1574661721687.png)

3. 对着要删除的文件点击右键，选择删除

   ![image20191125135620642.png](https://myblog-1252842020.cos.ap-guangzhou.myqcloud.com/image-20191125135620642_1574661856562.png)

4. 输入删除说明后点击ok即可

   ![image20191125135850290.png](https://myblog-1252842020.cos.ap-guangzhou.myqcloud.com/image-20191125135850290_1574661869305.png)

5. 删除本地代码就搞定了