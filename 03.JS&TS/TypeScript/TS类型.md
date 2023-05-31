### TS类型

1.number

2.string

3.array

在TS中，array一般指所有元素类型相同的值的集合

4.Boolean

5.函数

6.any

7.void

绝大部分情况下，只会用在这一个地方：表示函数不返回任何值或者返回undefined（因为函数不返回任何值的时候===返回undefined）

8.object

9.tuple

10.enum

11.null和undefined

12.unknown

13.nerve

什么时候需要声明类型？

声明函数、组件、hook等需要声明参数和返回值的类型

.d.ts

JS文件+.d.ts文件 === ts文件

.d.ts文件可以让JS文件继续维持自己JS文件的身份，而拥有TS的类型保护。

用泛型来规范类型

鸭子类型（duck typing）:面向接口编程，而不是面向对象编程

npx imooc-jira-tool

npm i jira-dev-tool