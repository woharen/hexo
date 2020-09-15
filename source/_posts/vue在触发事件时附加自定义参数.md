## vue在触发事件时附加自定义参数以`ElementUI`的分页组件为例，
我们找个事件
比如
```
<el-pagination
  small
  layout="prev, pager, next"
  :current-change='changePage'
  :total="50">
</el-pagination>
```

```
/**
 * currentPage 改变时会触发
 * @param index 当前页
 */
changePage(index){
    console.log(index);
}
```

如上述代码，我们触发分页的事件后，changePage方法只能接受到一个index参数，如果我们修改成以下写法就会报错。
```
 :current-change='changePage(index, id)'
```
### 解决方法
使用ES写法解决
```
 :current-change='(index) => {changePage(index, id)}'
```
```
changePage(index, id){
   console.log(index);
   console.log(id);
}
```



