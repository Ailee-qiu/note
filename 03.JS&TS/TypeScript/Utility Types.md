### Utility Types

>[TypeScript 的 Utility Types，你真的懂吗？ - 掘金 (juejin.cn)](https://juejin.cn/post/6970083128345903135)
>
>[TypeScript: Documentation - Utility Types (typescriptlang.org)](https://www.typescriptlang.org/docs/handbook/utility-types.html)

>在TypeScript中，Utility Types（实用类型）是一组**预定义的类型工具**，用于在类型系统中进行常见的类型操作和转换。它们提供了一种方便的方式来处理和操作现有的类型，从而减少了重复的代码和提高了代码的可读性。
>
>Utility Types 在标准 TypeScript 库（`lib.d.ts`）中已经定义好了，可以直接在 TypeScript 项目中使用，无需额外安装或导入。

常用的Utility Types：

1.`Partial<T>`：创建一个类型，该类型将给定类型 `T` 中的所有属性转换为可选属性。

```tsx
interface User {
  name: string;
  age: number;
}

type PartialUser = Partial<User>;
// 等同于
// type PartialUser = {
//   name?: string;
//   age?: number;
// };
```

2.`Required<T>`：创建一个类型，该类型将给定类型 `T` 中的所有属性转换为必需属性。

````tsx
interface PartialUser {
  name?: string;
  age?: number;
}

type RequiredUser = Required<PartialUser>;
// 等同于
// type RequiredUser = {
//   name: string;
//   age: number;
// };
````



3.`Readonly<T>`：创建一个类型，该类型将给定类型 `T` 中的所有属性转换为只读属性。

```tsx
interface User {
  name: string;
  age: number;
}

type ReadonlyUser = Readonly<User>;
// 等同于
// type ReadonlyUser = {
//   readonly name: string;
//   readonly age: number;
// };
```



4.`Pick<T, K>`：从给定类型 `T` 中选取部分属性，并创建一个新类型。

````tsx
interface User {
  name: string;
  age: number;
  email: string;
}

type UserInfo = Pick<User, 'name' | 'email'>;
// 等同于
// type UserInfo = {
//   name: string;
//   email: string;
// };
````



5.`Omit<T, K>`：从给定类型 `T` 中排除指定的属性，并创建一个新类型。

````tsx
interface User {
  name: string;
  age: number;
  email: string;
}

type UserWithoutAge = Omit<User, 'age'>;
// 等同于
// type UserWithoutAge = {
//   name: string;
//   email: string;
// };
````

