## vue配合ElementUI导入导出excel表格本文主要使用elementUI上传文件
# 导入
## 1. 导入依赖
```
npm install -S file-saver xlsx
npm install -D script-loader
```
>import XLSX from 'xlsx'

## 2.ElementUI上传文件代码
```
<el-upload
        class="upload-demo"
        :action="'service/plugins/policies/importExcel/' + plugin"
        :on-change="upLoad"
        :limit="1"
        accept="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet,application/vnd.ms-excel"
        :auto-upload="false"
        :file-list="config.data">
    <el-button size="small" type="primary">选择文件</el-button>
    <div slot="tip" class="el-upload__tip">只能上传xls/xlsx文件</div>
</el-upload>
```
## 3.js代码
```
upLoad(file, fileList) {
    let fileTemp = file.raw;
    if (fileTemp) {
        if ((fileTemp.type === 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet')
            || (fileTemp.type === 'application/vnd.ms-excel')) {
            this.readExcelFile(fileTemp);
        } else {
            this.$message({
                type: 'warning',
                message: '附件格式错误，请删除后重新上传！'
            })
        }
    } else {
        this.$message({
            type: 'warning',
            message: '请上传附件！'
        })
    }
},
// 主要方法
readExcelFile(file) {
    // 通过DOM取文件数据
    var rABS = false; //是否将文件读取为二进制字符串
    var reader = new FileReader();
    FileReader.prototype.readAsBinaryString = function (file) {
        var binary = "";
        var rABS = false; //是否将文件读取为二进制字符串
        var workbook; //读取完成的数据
        var outdata;
        var reader = new FileReader();
        reader.onload = function (e) {
            var bytes = new Uint8Array(reader.result);
            var length = bytes.byteLength;
            for (var i = 0; i < length; i++) {
                binary += String.fromCharCode(bytes[i]);
            }
            var XLSX = require('xlsx');
            if (rABS) {
                workbook = XLSX.read(btoa(fixdata(binary)), { //手动转化
                    type: 'base64'
                });
            } else {
                workbook = XLSX.read(binary, {
                    type: 'binary'
                });
            }
            outdata = XLSX.utils.sheet_to_json(workbook.Sheets[workbook.SheetNames[0]]);

            window.console.log("表格参数:" + workbook);
            window.console.log("数据:" + outdata);
        };
        reader.readAsArrayBuffer(file);
    };

    if (rABS) {
        reader.readAsArrayBuffer(file);
    } else {
        reader.readAsBinaryString(file);
    }

},
```

# 导出
[vue使用xlsx修改样式导出excel](https://blog.csdn.net/zfz5720/article/details/90676359)


# 参考
[Vue+Element前端导入导出Excel](https://segmentfault.com/a/1190000018993619?utm_source=tag-newest)