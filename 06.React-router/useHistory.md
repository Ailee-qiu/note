### useHistory


在 React 中，`history` 对象是由 `react-router-dom` 库提供的，该库封装了 `history` 库的一些功能，提供了一些用于实现路由导航、页面跳转等功能的组件和 hook。下面是 `history` 对象中常用的属性和方法：

1. `length`：当前路由栈中的路由数量。
2. `location`：当前的位置信息对象，包含了 URL 中的 pathname、search、hash 等信息。
3. `action`：表示路由变化的类型，可能的值为 `PUSH`、`REPLACE` 或 `POP`。
4. `push(path: string, state?: any)`：跳转到指定的路由，可以传递额外的状态信息。
5. `replace(path: string, state?: any)`：替换当前的路由，可以传递额外的状态信息。
6. `go(n: number)`：前进或后退指定的步数，类似于浏览器的前进和后退功能。
7. `goBack()`：返回上一页，相当于 `go(-1)`。
8. `goForward()`：前进到下一页，相当于 `go(1)`。
9. `block(prompt?: boolean | string | TransitionPromptHook)`：阻止路由变化，可以用于在路由跳转前进行一些提示或操作。
10. `listen(listener: LocationListener) -> () => void`：监听路由变化，当路由发生变化时调用指定的回调函数，并传递当前的 `location` 对象作为参数。返回一个函数，调用该函数可以取消路由变化的监听。

需要注意的是，以上方法中的 `push`、`replace`、`goBack`、`goForward` 方法会触发路由变化，从而导致组件重新渲染。因此，我们需要在编写组件时小心使用这些方法，避免过度渲染导致性能问题。