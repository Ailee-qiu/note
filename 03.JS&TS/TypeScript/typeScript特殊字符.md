typeScript特殊字符

>[前端 - typescript的?? 和?: 和?.和!.什么意思 - 个人文章 - SegmentFault 思否](https://segmentfault.com/a/1190000038782759)

`?:`是指可选参数，可以理解为参数自动加上undefined

````tsx
function echo(x: number, y?: number) {
    return x + (y || 0);
}
getval(1); // 1
getval(1, null); // error, 'null' is not assignable to 'number | undefined'

interface IProListForm {
  enterpriseId: string | number;
  pageNum: number;
  pageSize: number;
  keyword?: string; // 可选属性
}
````



?? 和 || 的意思有点相似，但是又有点区别,??相较||比较严谨, 当值等于0的时候||就把他给排除了，但是?? 不会

````tsx
console.log(null || 5)   //5
console.log(null ?? 5)     //5

console.log(undefined || 5)      //5
console.log(undefined ?? 5)      //5

console.log(0 || 5)       //5
console.log(0 ?? 5)      //0
````



?.的意思基本和 && 是一样的

a?.b 相当于 a && a.b ? a.b : undefined

````tsx
const a = {
      b: { c: 7 }
};
console.log(a?.b?.c);     //7
console.log(a && a.b && a.b.c);    //7
````



!.的意思是断言，告诉ts你这个对象里一定有某个值

````tsx
const inputRef = useRef<HTMLEInputlement>(null);
// 定义了输入框，初始化是null，但是你在调用他的时候相取输入框的value，这时候dom实例一定是有值的，所以用断言
const value: string = inputRef.current!.value;
// 这样就不会报错了
````

