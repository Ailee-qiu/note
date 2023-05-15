### JavaScript获取字符的ASCII值，将ASCII值转换为字符

A 65 a 97

````js
//js
var num = "a".charCodeAt() //65

var str = String.fromCharCode(65)
````

### 创建二维数组

````js
//错误，有缺陷 修改 arr[0][0] =1,会使一列改变
var arr = new Array(10).fill(new Array(10).fill(0))

var arr = new Array(10); //表格有10行
for(var i = 0;i < arr.length; i++){
 arr[i] = new Array(10).fill(0); //每行有10列
}
````

### 数组去重（7种） NaN!==NaN(JS)