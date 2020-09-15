## vue读取json文件和下载文件## 读取json文件
1. 获取file文件
    具体步骤不说了，input上传文件或者elementUI上传文件都可以，只要拿到file对象就可以了
2. 读取内容
```
let file = "获取file文件对象";
let reader = new FileReader();
reader.readAsText(file, "UTF-8");
reader.onload = function (e) {
    let data = JSON.parse(e.target.result);
    // 打印json数据
    window.console.log(data);
}
```

## 下载文件

```
let services = {"key": "value"};
let data = JSON.stringify(services);
let blob = new Blob([data], {type: 'text/plain;charset=utf-8'});
// json或者txt都可以
let fileName = '文件名.json';
FileSaver.saveAs(blob, fileName);
```
