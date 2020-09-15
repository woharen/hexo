## Jetbrains系列产品2019.3.3最新激活方法其实这篇直接是拿了别人的教程过来的，也算是做个备份吧，原文出处在最下面。
ok,让我们开始

1. 下载破解用的jar包

   在这里说明下，这个是本文编辑时下载的，后续更新可能失效，如果失效请在下方留言，或者直接去出处找。

    [百度云下载](https://pan.baidu.com/s/1FrpavOrzkzLDYShndnqRZQ)
    提取码：2c4t。

## 自动挡
2. 在创建项目界面，将破解jar包直接拖到IDE窗口，点 "Restart" 按钮重启IDE。
3. 在弹出的JetbrainsAgent Helper对话框中，选择激活方式，点击安装按钮。
   如果你是无外网环境，在对话框中勾选：我无法访问外网 选项。
4. 重启IDE，搞定。

>x. 支持两种注册方式：License server 和 Activation code:
    1). 选择License server方式，地址填入：http://fls.jetbrains-agent.com （网络不佳的用第2种方式）
    2). 选择Activation code方式离线激活，请使用：ACTIVATION_CODE.txt 内的注册码激活
        如果激活窗口一直弹出（error 1653219），请去hosts文件里“移除”jetbrains相关的项目
        License key is in legacy format == Key invalid，表示agent配置未生效。
        如果你需要自定义License name，请访问：https://zhile.io/custom-license.html
    3). 现在你可以使用jetbrains-agent + license server激活jetbrains平台的付费插件了！
        除了："IEDIS 2" 和 "MINBATIS"，这两个请使用 IEDIS_MINBATIS_CODE.txt 来激活。
        现在有这些付费插件：https://plugins.jetbrains.com/search?isPaid=true

## 手动挡
2. 将解压出来的`jetbrains-agent.jar`文件放在idea的安装目录下
  右键点击idea图标，查看属性,点击 `打开文件所在的位置`，到达安装

![image.png](https://myblog-1252842020.cos.ap-guangzhou.myqcloud.com/image_1579490182427.png)
![image.png](https://myblog-1252842020.cos.ap-guangzhou.myqcloud.com/image_1579492017245.png)
3. 编辑同级目录下的`idea64.exe.vmoptions`文件
   注意，只能用idea编辑，不能用其他编辑器！！！

在这里小伙伴们就有疑问了，我还没激活怎么编辑呢？试用30天呗。
![image.png](https://myblog-1252842020.cos.ap-guangzhou.myqcloud.com/image_1579492218228.png)
![image.png](https://myblog-1252842020.cos.ap-guangzhou.myqcloud.com/image_1579492317245.png)

>一定要自己确认好路径(不要使用中文路径)，填错会导致IDE打不开！！！最好使用绝对路径。

一个vmoptions内只能有一个-javaagent参数。
    示例:
      mac:      -javaagent:/Users/neo/jetbrains-agent.jar
      linux:    -javaagent:/home/neo/jetbrains-agent.jar
      windows:  -javaagent:C:\Users\neo\jetbrains-agent.jar
    如果还是填错了，参考这篇文章编辑vmoptions补救：
    https://intellij-support.jetbrains.com/hc/en-us/articles/206544519

注:到这一步的朋友很有可能因为路径问题导致idea打不开，根据上文的链接，我们找到
```
C:\Users\你的用户名\.你的idea版本\config
```
路径下的 `idea64.exe.vmoptions` 文件，删掉即可，

4. 重启后激活
   > 点击IDE菜单 "Help" -> "Register..." 或 "Configure" -> "Manage License..."
    支持两种注册方式：License server 和 Activation code:
    1). 选择License server方式，地址填入：http://jetbrains-license-server （应该会自动填上）
        或者点击按钮："Discover Server"来自动填充地址。网络不佳的见第2种方式。
    2). 选择Activation code方式离线激活，请使用：ACTIVATION_CODE.txt 内的注册码激活

如果激活窗口一直弹出（error 1653219），请去hosts文件里移除jetbrains相关的项目
License key is in legacy format == Key invalid，表示agent配置未生效。
如果你需要自定义License name，请访问：https://zhile.io/custom-license.html
   ![image.png](https://myblog-1252842020.cos.ap-guangzhou.myqcloud.com/image_1579492726091.png)
![image.png](https://myblog-1252842020.cos.ap-guangzhou.myqcloud.com/image_1579492755357.png)
![image.png](https://myblog-1252842020.cos.ap-guangzhou.myqcloud.com/image_1579492773929.png)

## 本文环境
可能之前小伙伴们用其他方式激活过，导致按照本文方法激活没有效果，下面是本文在激活时的环境。
 - hosts文件未做任何修改
 - idea版本 IntelliJ IDEA 2019.3.1 

在以下IDE版本测试可成功激活（v3.0.3）：
- IntelliJ IDEA 2019.3.3及以下
- AppCode 2019.3.5及以下
- CLion 2019.3.4及以下
- DataGrip 2019.3.3及以下
- GoLand 2019.3.3及以下
- PhpStorm 2019.3.3及以下
- PyCharm 2019.3.3及以下
- Rider 2019.3.4及以下
- RubyMine 2019.3.3及以下
- WebStorm 2019.3.3及以下

## 参考
[Jetbrains系列产品2019.3.3最新激活方法[持续更新]](https://zhile.io/2018/08/25/jetbrains-license-server-crack.html)