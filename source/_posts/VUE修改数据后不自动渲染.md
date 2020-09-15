## VUE修改数据后不自动渲染最近写vue写的脑瓜子有点疼，特别今天出现的问题更加头疼。
先说说这个问题，
## 场景
vue + element

修改表格数据后，不重新加载整个表格的情况下修改局部数据发现数据不能自动渲染。
![image.png](https://myblog-1252842020.cos.ap-guangzhou.myqcloud.com/image_1578628368283.png)
wtf？？？
那就是没得谈咯,好吧，那就使用手动渲染吧。
## 解决方案
1. 给表格绑定key值
    ```
    <el-table :data="xxx" :key="tableKey">...
    ```

2. 在方法里添加以下代码
   ```
    this.tableKey++;
   ```
## 参考文档
[VUE官网 - 维护状态](https://cn.vuejs.org/v2/guide/list.html#%E7%BB%B4%E6%8A%A4%E7%8A%B6%E6%80%81)
