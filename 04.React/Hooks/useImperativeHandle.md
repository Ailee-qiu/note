# useImperativeHandle

>[useImperativeHandle – React 中文文档 (docschina.org)](https://react.docschina.org/reference/react/useImperativeHandle)
>
>参考
>
>- useImperativeHandle(ref, createHandle, dependencies?)
>
>使用方法：
>
>- 向父组件暴露一个自定义的ref句柄
>- 暴露你自己的命令时方法



>#### 参数 ****
>
>- `ref`：该 `ref` 是你从 [`forwardRef` 渲染函数](https://react.docschina.org/reference/react/forwardRef#render-function) 中获得的第二个参数。
>- `createHandle`：该函数无需参数，它返回你想要暴露的 ref 的句柄。该句柄可以包含任何类型。通常，你会返回一个包含你想暴露的方法的对象。
>- **可选的** `dependencies`：函数 `createHandle` 代码中所用到的所有反应式的值的列表。反应式的值包含 props、状态和其他所有直接在你组件体内声明的变量和函数。倘若你的代码检查器已 [为 React 配置好](https://react.docschina.org/learn/editor-setup#linting)，它会验证每一个反应式的值是否被正确指定为依赖项。该列表的长度必须是一个常数项，并且必须按照 `[dep1, dep2, dep3]` 的形式罗列各依赖项。React 会使用 [`Object.is`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/is) 来比较每一个依赖项与其对应的之前值。如果一次重新渲染导致某些依赖项发生了改变，或你没有提供这个参数列表，你的函数 `createHandle` 将会被重新执行，而新生成的句柄则会被分配给 ref。
>
>#### 返回值 
>
>`useImperativeHandle` 返回 `undefined`。



>### **warn**
>
>**不要滥用 ref。** 你应当仅在你没法通过 prop 来表达 *命令式* 行为的时候才使用 ref：例如，滚动到指定节点、聚焦某个节点、触发一次动画，以及选择文本等等。
>
>**如果可以通过 prop 实现，那就不应该使用 ref**。例如，你不应该从一个 `Model` 组件暴露出 `{open, close}` 这样的命令式句柄，最好是像 `<Modal isOpen={isOpen} />` 这样，将 `isOpen` 作为一个 prop。[副作用](https://react.docschina.org/learn/synchronizing-with-effects) 可以帮你通过 prop 来暴露一些命令式的行为。