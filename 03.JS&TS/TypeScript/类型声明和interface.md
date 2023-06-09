### 类型声明和interface

>[TS 中 interface 和 type 究竟有什么区别？ - 掘金 (juejin.cn)](https://juejin.cn/post/7063521133340917773)



>在TypeScript中，类型声明（Type Declarations）和接口（Interface）是两种不同的类型定义方式，它们有一些区别和适用场景。

>类型声明使用 `type` 关键字，用于定义自定义的类型。它可以表示各种类型，包括原始类型、联合类型、交叉类型、函数类型等。类型声明还支持使用泛型和条件类型等高级特性

````tsx
type Point = {
  x: number;
  y: number;
};
````

>接口是一种在 TypeScript 中用于描述对象形状的结构化类型。它定义了对象应该具有的属性和方法，并可以用于类型检查和类的实现。

````tsx
interface Point {
  x: number;
  y: number;
}
//接口可以继承其他接口，从而组合多个接口的成员。
interface Shape {
  color: string;
}

interface Rectangle extends Shape {
  width: number;
  height: number;
}
````

>1. 继承与实现：接口可以相互继承，一个接口可以扩展另一个接口的成员。而类型声明不支持继承。接口还可以被类实现，用于约束类的结构。
>2. 同名合并：当多个同名的接口定义出现时，它们会自动合并成一个接口，将相同名称的属性进行合并。而类型声明不会发生合并，会导致命名冲突。
>3. 扩展能力：类型声明支持使用联合类型、交叉类型、条件类型等高级特性，可以更灵活地定义复杂的类型。接口的扩展性相对较弱，不支持这些高级特性。
>4. 可读性与可维护性：接口通常用于描述对象的结构，因此在阅读代码时更加直观和可读。类型声明则更适合用于定义通用的类型别名，或者用于定义复杂类型的操作和转换。
>
>在大多数情况下，类型声明和接口可以互相替代使用。选择使用哪种方式取决于具体的需求和个人偏好。一般而言，如果你需要描述对象的结构和约束类的实现，使用接口会更合适；如果你需要定义复杂的类型别名或使用高级类型特性，使用类型声明会更灵活。