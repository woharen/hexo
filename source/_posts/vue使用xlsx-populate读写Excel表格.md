## vue使用xlsx-populate读写Excel表格## 依赖
https://www.npmjs.com/package/xlsx-populate

## 读取文件
```java
// 读取表格数据
readExcelFile(file) {
    let _this = this;
    this.tableData = [];
    this.isCheck = true;
    XlsxPopulate.fromDataAsync(file).then(workbook => {
        let values = workbook.sheet(0).usedRange().value();
        window.console.log(values);
    }).catch(err => {
        window.console.error(err);
        _this.$message.error("读取文件错误");
        // 设置表格加载状态
        _this.tableLoading = false;
    });
}
```

## 导出
```java
/**
 * 导出
 * @param data 导出数据
 * @param fileName 文件名
 * @param type 后缀名
 */
exportAccesses(data, fileName, type){
    // 1.设计表头
    let row1 = ["xxx","xxx","xxxx"]
    data.unshift(row1);
    let workbook = XlsxPopulate.fromBlankAsync().then(workbook => {
        //获取第一个工作表
        var sheet = workbook.sheet(0);
        //设置值
        sheet.cell("A1").value(data).style({
            border: true
        });
        //设置单元格合并
        sheet.range("A1:E1").merged(true)
        //设置表头填充样式
        for (let i = 1; i <= 2; i++) {
            let cells = sheet.row(i)._cells;
            for (let j = 1; j < cells.length; j++) {
                cells[j].style("fill", "ddebf7");
            }
        }
        //导出
        return workbook.outputAsync().then(blob => {
            type = undefined === type ? "xlsx" : type;
            this.$fileUtils.saveFile(blob, fileName + "." + type);
        })
    }).catch(function (err) {
        window.console.log(err.message || err);
        throw err;
    });
}
```