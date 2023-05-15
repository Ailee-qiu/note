````ruby
npm install xlsx@0.18.5 --save
yarn add xlsx@0.18.5
````

>相关链接：
>
>[xlsx库实现纯前端导入导出Excel - 掘金 (juejin.cn)](https://juejin.cn/post/7097426696365670430)

### 常用的数据表格式(Common Spreadsheet Format)

`js-xlsx`符合常用的数据表格式(CSF)。

#### 一般结构

单元格地址对象的存储格式为`{c:C, r:R}`，其中`C`和`R`分别代表的是 0 索引列和行号。例如单元格地址`B5`用对象`{c:1, r:4}`表示。

单元格范围对象存储格式为`{s:S, e:E}`，其中`S`是第一个单元格，`E`是最后一个单元格。范围是包含关系。例如范围 `A3:B7`用对象`{s:{c:0, r:2}, e:{c:1, r:6}}`表示。

#### 单元格对象

单元格对象是纯粹的 JS 对象，它的 keys 和 values 遵循下列的约定：

| Key  | Description                                                  |
| ---- | ------------------------------------------------------------ |
| `v`  | 原始值(查看数据类型部分获取更多的信息)                       |
| `w`  | 格式化文本(如果可以使用)                                     |
| `t`  | 内行: `b` Boolean, `e` Error, `n` Number, `d` Date, `s` Text, `z` Stub |
| `f`  | 单元格公式编码为 A1 样式的字符串(如果可以使用)               |
| `F`  | 如果公式是数组公式，则包围数组的范围(如果可以使用)           |
| `r`  | 富文本编码 (如果可以使用)                                    |
| `h`  | 富文本渲染成 HTML (如果可以使用)                             |
| `c`  | 与单元格关联的注释                                           |
| `z`  | 与单元格关联的数字格式字符串(如果有必要)                     |
| `l`  | 单元格的超链接对象 (`.Target` 长联接, `.Tooltip` 是提示消息) |
| `s`  | 单元格的样式/主题 (如果可以使用)                             |

如果`w`文本可以使用，内置的导出工具(比如 CSV 导出方法)就会使用它。要想改变单元格的值，在打算导出之前确保删除`cell.w`(或者设置 `cell.w`为`undefined`)。工具函数会根据数字格式(`cell.z`)和原始值(如果可用)重新生成`w`文本。

真实的数组公式存储在数组范围中第一个单元个的`f`字段内。此范围内的其他单元格会省略`f`字段。

#### 导出一般分为两种：

数据导出 Excel

页面表格导出 Excel



#### 数据导出 Excel

````tsx
var arr = [];//json数据格式
var ws = xlsx.utils.json_to_sheet(arr, { sheetStubs: true });
var wb = xlsx.utils.book_new();
xlsx.utils.book_append_sheet(wb, ws, 'Sheet1');
var wbout = xlsx.write(wb, {
    bookType: 'xlsx',
    bookSST: false,
    type:'binary'
})
````



#### 页面表格导出 Excel

````js
var table = document.getElementById('table');//获取到dom元素
var ws = xlsx.utils.table_to_sheet(table, { sheetStubs: true });
var wb = xlsx.utils.book_new();
xlsx.utils.book_append_sheet(wb, ws, 'Sheet1');
var wbout = xlsx.write(wb, {
    bookType: 'xlsx',
    bookSST: false,
    type:'binary'
})
````



#### IE浏览器

````js
var thisTst=null;
var thisExcelIframe=null;
var thisExcelDiv=null;
var excelDiv=document.createElement('div');
thisExcelDiv = excelDiv;
excelDiv.style.display="none";
var excelIframe=document.createElement('iframe');
thisExcelIframe = excelIframe;
excelIframe.src="";
excelIframe.id="excelIframe";
excelDiv.appendChild(excelIframe);
document.body.appendChild(excelDiv);
var tst=window.frames['excelIframe'];
tst.document.open();
thisTst = tst;
tst.document.write(wb);
thisTst.document.execCommand('saveAs', true, new Date().getTime()+'.xls');
thisTst.close();
thisTst.focus();
thisExcelDiv.removeChild(thisExcelIframe);
document.body.removeChild(thisExcelDiv);
thisTst = null;
window.CollectGarbage();
window.CollectGarbage();
````



#### 非IE浏览器

````js
var excelA = document.craeteElement('a');
var data = "\ufeff"+xml;
var blob = new Blob([data],{type:'text/csv,charset=UTF-8'});
var csvUrl = webkitURL.createObjectURL(blob);
excelA.href = csvUrl;
excelA.download = ("filename"+'.xls');
document.body.appendChild(excelA);
excelA.click();
data = null;
blob = null;
document.body.removeChild(excelA);
````

