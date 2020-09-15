## git和svn提交时怎么忽略文件## git
1. 在项目根目录下创建`.gitignore`文件
2. 在`.gitignore`文件中添加要屏蔽的文件
    ```.gitignore
    # 指定文件
    xxx.text
    # 指定文件夹
    dir
    # 匹配后缀名为 txt的文件
    *.txt
    ```
## svn
1. 打开SVN的Version Control中的Local Changes下的Configure Ignored Files
![svn1.png](https://myblog-1252842020.cos.ap-guangzhou.myqcloud.com/svn1_1574652846178.png)
2. 增加特定文件或文件夹
![SVN](https://myblog-1252842020.cos.ap-guangzhou.myqcloud.com/SVN_1574652823070.png)
![svn3.png](https://myblog-1252842020.cos.ap-guangzhou.myqcloud.com/svn3_1574652904330.png)

新版本如下
1. 打开 Settings→Editor→File Types
2. 在窗口最下方“Ignore files and folders”一栏中添加如下忽略： 
3. 注意要用英文`;`符号隔开
    ![svn4.png](https://myblog-1252842020.cos.ap-guangzhou.myqcloud.com/svn4_1574653034383.png)
