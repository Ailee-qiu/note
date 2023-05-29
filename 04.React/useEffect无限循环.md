### useEffect无限循环

>[聊一聊useEffect中的无限循环陷阱 - 掘金 (juejin.cn)](https://juejin.cn/post/7022917127187202055)
>
>[5 种有趣的 useEffect 无限循环类型 - 掘金 (juejin.cn)](https://juejin.cn/post/7009946969099468831)

>省略依赖项是不安全的
>
>非状态的、非基本类型的不要放在依赖项里

#### 常见原因：

1.未正确指定依赖项：`useEffect`的第二个参数是一个依赖项数组，用于指定该效果所依赖的值或属性。如果依赖项数组未被正确指定，或者指定了一个空的依赖项数组（`[]`）

2.每次渲染都修改依赖项：如果在`useEffect`的回调函数中修改了依赖项的值，并且这个修改会导致组件重新渲染，那么会形成一个无限循环。

3.函数作为依赖项

确保每次渲染时函数的引用没有发生变化：使用`useCallback`来缓存函数的引用，确保在依赖项没有变化的情况下，函数的引用保持稳定。

4.数组作为依赖项

使用浅比较而非引用比较：在比较数组类型的依赖项时，避免直接比较数组的引用，而是进行浅比较。可以使用`isEqual`等工具函数来进行浅比较，或者手动比较数组中的每个元素。

```js
useEffect(() => {
  // 判断数组是否相等的逻辑...
}, [isEqual(dependencyArray, prevDependencyArray)]);
```

对于可变数组，可以使用`useMemo`来缓存数组的引用，以确保在依赖项没有变化时，使用相同的数组引用。

````js
const memoizedArray = useMemo(() => {
  return [dependency1, dependency2];
}, [dependency1, dependency2]);

useEffect(() => {
  // 使用缓存的数组
  // 依赖项为memoizedArray
}, [memoizedArray]);
````



5.对象作为依赖项

6.使用闭包引用了`useEffect`的回调函数：如果在`useEffect`的回调函数中使用了闭包，并且这个闭包引用了`useEffect`的回调函数本身，那么每次渲染时都会创建一个新的回调函数，导致无限循环。