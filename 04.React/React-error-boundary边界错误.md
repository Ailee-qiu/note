### React-error-boundary边界错误

>react-error-boundary:
>
>[GitHub - bvaughn/react-error-boundary: Simple reusable React error boundary component](https://github.com/bvaughn/react-error-boundary)

>React Error Boundary（错误边界）是 React 中的一种错误处理机制。在 React 应用程序中，当组件发生 JavaScript 错误时，错误会沿着组件树向上冒泡，最终导致整个应用程序崩溃。为了避免这种情况，React 提供了错误边界的概念。

>错误边界是一种特殊类型的 React 组件，可以捕获并处理其子组件（包括后代组件）中发生的 JavaScript 错误，从而防止整个应用程序崩溃。当错误发生时，错误边界会渲染备用的 UI，展示给用户错误信息或其他反馈。简单来讲：，就是用于：子组件发生错误时，渲染备用UI的组件。
>
>**只有class组件才可以成为错误边界组件。**

使用步骤如下：

1.创建一个错误边界组件，通过继承 `React.Component` 并实现 `componentDidCatch` 方法来捕获错误。例如：

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  componentDidCatch(error, errorInfo) {
    this.setState({ hasError: true });
    // 在这里可以记录错误信息或发送错误报告
  }

  render() {
    if (this.state.hasError) {
      // 渲染备用 UI
      return <h1>发生了错误。</h1>;
    }
    return this.props.children;
  }
}
```

2.在需要使用错误边界的组件包裹它们。只有被错误边界包裹的组件及其子组件中发生的错误才会被捕获和处理。例如：

```jsx
<ErrorBoundary>
  <MyComponent />
</ErrorBoundary>
```

这样，当 `MyComponent` 或其子组件中发生错误时，错误边界将捕获错误，并根据定义的备用 UI 渲染错误信息。

值得注意的是，错误边界仅能捕获其子组件中的错误，无法捕获以下情况的错误：

- 事件处理器中的错误

- 异步代码（例如 `setTimeout` 或 `requestAnimationFrame` 回调函数）

- 服务端渲染

- 它自身抛出来的错误（并非它的子组件）

  

异步代码、事件中的错误可以通过`try {} catch(e) {}`来处理

