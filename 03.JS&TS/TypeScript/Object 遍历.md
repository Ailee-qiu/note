Object 遍历

````ts
Object.keys()
//返回一个数组，其元素是字符串，对应于直接在对象上找到的可枚举的字符串键属性名。
//Object.keys() 返回的数组顺序和与 for...in 循环提供的顺序相同。

// 简单数组
const arr = ["a", "b", "c"];
console.log(Object.keys(arr)); // ['0', '1', '2']

// 类数组对象
const obj = { 0: "a", 1: "b", 2: "c" };
console.log(Object.keys(obj)); // ['0', '1', '2']

// 键的顺序随机的类数组对象
const anObj = { 100: "a", 2: "b", 7: "c" };
console.log(Object.keys(anObj)); // ['2', '7', '100']
````

````ts
Object.values()
//Object.values() 静态方法返回一个给定对象的自有可枚举字符串键属性值组成的数组。

const object1 = {
  a: 'somestring',
  b: 42,
  c: false
};

console.log(Object.values(object1));
// Expected output: Array ["somestring", 42, false]
````

```ts
Object.entries()
//Object.entries() 静态方法返回一个数组，包含给定对象自有的可枚举字符串键属性的键值对。

const object1 = {
  a: 'somestring',
  b: 42
};

for (const [key, value] of Object.entries(object1)) {
  console.log(`${key}: ${value}`);
}

// Expected output:
// "a: somestring"
// "b: 42"

const obj = { foo: "bar", baz: 42 };
console.log(Object.entries(obj)); // [ ['foo', 'bar'], ['baz', 42] ]

// 类数组对象
const obj = { 0: "a", 1: "b", 2: "c" };
console.log(Object.entries(obj)); // [ ['0', 'a'], ['1', 'b'], ['2', 'c'] ]

// 具有随机键排序的类数组对象
const anObj = { 100: "a", 2: "b", 7: "c" };
console.log(Object.entries(anObj)); // [ ['2', 'b'], ['7', 'c'], ['100', 'a'] ]

// getFoo 是一个不可枚举的属性
const myObj = Object.create(
  {},
  {
    getFoo: {
      value() {
        return this.foo;
      },
    },
  },
);
myObj.foo = "bar";
console.log(Object.entries(myObj)); // [ ['foo', 'bar'] ]

```

