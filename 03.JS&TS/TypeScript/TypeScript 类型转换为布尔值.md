# TypeScript 类型转换为布尔值

①使用 Boolean() 函数进行类型转换

在 TypeScript 中，可以使用 Boolean() 函数将任意类型转换为布尔值。该函数接受一个参数，并将其转换为对应的布尔值。

````tsx
let num: number = 0;
let str: string = "";
let obj: object = {};

console.log(Boolean(num));  // false
console.log(Boolean(str));  // false
console.log(Boolean(obj));  // true
````



②使用 !! 运算符进行类型转换

除了使用 Boolean() 函数，我们还可以使用 !! 运算符将不同类型转换为布尔值。该运算符通过两次逻辑非运算，将值转换为对应的布尔值。

````tsx
let num: number = 42;
let str: string = "Hello";
let arr: any[] = [1, 2, 3];

console.log(!!num);  // true
console.log(!!str);  // true
console.log(!!arr);  // true
````



③使用 if 语句进行条件判断

在 TypeScript 中，可以使用 if 语句进行条件判断，将不同类型转换为布尔值。通过在 if 语句中使用待转换的值作为条件，可以根据其真假值执行不同的逻辑。

````tsx
let num: number = 10;

if (num) {
  console.log("Number is truthy.");
} else {
  console.log("Number is falsy.");
}

let str: string = "";

if (str) {
  console.log("String is truthy.");
} else {
  console.log("String is falsy.");
}
````



④使用类型断言进行类型转换

在 TypeScript 中，可以使用类型断言来告诉编译器某个变量的实际类型，从而进行类型转换。通过在变量后加上尖括号或使用 as 关键字，可以将变量转换为指定的类型。

````tsx
let value: unknown = "Hello, world!";

let strLength1: number = (value as string).length;
let strLength2: number = (<string>value).length;

console.log(strLength1);  // 13
console.log(strLength2);  // 13

````

