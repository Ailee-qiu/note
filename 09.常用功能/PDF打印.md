## **PDF打印**



1.window.print()

````js
var printWindow = window.open('','');
//strHtml是打印的内容，是html代码，可以通过拼接字符串的形式设置样式
printWindow.document.write(strHtml);
printWindow.document.close();
printWindow.print();
printWindow.close();
````

在**IE浏览器**打印时，IE浏览器自身会根据数据类型做字体大小的区分，String类型会加粗，Number类型不会。

修正方式

````html
th,td {
font-weight:normal !important
}
````

