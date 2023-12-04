# <StrictMode>

`<StrictMode>` 帮助你在开发过程中尽早地发现组件中的常见错误。

## 参考 

### `<StrictMode>` 

使用 `StrictMode` 来启用组件树内部的额外开发行为和警告：

````tsx
import { StrictMode } from 'react';
import { createRoot } from 'react-dom/client';

const root = createRoot(document.getElementById('root'));
root.render(
  <StrictMode>
    <App />
  </StrictMode>
);
````

严格模式启用了以下仅在开发环境下有效的行为：

- 组件将 [重新渲染一次](https://react.docschina.org/reference/react/StrictMode#fixing-bugs-found-by-double-rendering-in-development)，以查找由于非纯渲染而引起的错误。

- 组件将 [重新运行 Effect 一次](https://react.docschina.org/reference/react/StrictMode#fixing-bugs-found-by-re-running-effects-in-development)，以查找由于缺少 Effect 清理而引起的错误。

- 组件将被 [检查是否使用了已弃用的 API](https://react.docschina.org/reference/react/StrictMode#fixing-deprecation-warnings-enabled-by-strict-mode)。

  **所有这些检查仅在开发环境中进行，不会影响生产构建。**

#### 注意事项 

- 在由 `<StrictMode>` 包裹的树中，无法选择退出严格模式。这可以确保在 `<StrictMode>` 内的所有组件都经过检查。如果两个团队在一个产品上工作，并且对于这些检查是否有价值存在分歧，他们需要达成共识或将 `<StrictMode>` 下移到树的较低层级。

------

## 用法 

### 为整个应用启用严格模式 

严格模式为 `<StrictMode>` 组件内的整个组件树启用额外的开发环境检查，这些检查有助于在开发过程中尽早地发现组件中的常见错误。

如果要为整个应用启用严格模式，请在渲染根组件时使用 `<StrictMode>` 包裹它：

````tsx
import { StrictMode } from 'react';
import { createRoot } from 'react-dom/client';

const root = createRoot(document.getElementById('root'));
root.render(
  <StrictMode>
    <App />
  </StrictMode>
);
````

我们建议将整个应用程序包裹在严格模式中，特别是对于新创建的应用程序。如果你使用的是一个调用 [`createRoot`](https://react.docschina.org/reference/react-dom/client/createRoot) 的框架，请查阅其文档以了解如何启用严格模式。

尽管严格模式的检查 **仅在开发环境** 下运行，但它们有助于找出已经存在于代码中但在生产环境中可能难以复现的错误。严格模式让你在用户反馈之前就可以修复这些错误。

### 为应用程序的一部分启用严格模式 

你也可以为应用程序的任意一部分启用严格模式：

````tsx
import { StrictMode } from 'react';

function App() {
  return (
    <>
      <Header />
      <StrictMode>
        <main>
          <Sidebar />
          <Content />
        </main>
      </StrictMode>
      <Footer />
    </>
  );
}
````

在这个例子中，严格模式的检查不会对 `Header` 和 `Footer` 组件运行。然而，它们会在 `Sidebar` 和 `Content` 以及它们内部的所有组件上运行，无论多深。

### 修复在开发过程中通过双重渲染发现的错误 

[React 假设编写的每个组件都是纯函数](https://react.docschina.org/learn/keeping-components-pure)。这意味着编写的 React 组件在给定相同的输入（props、state 和 context）时必须始终返回相同的 JSX。

违反此规则的组件会表现得不可预测，并引发错误。为了帮助你找到意外的非纯函数代码，严格模式 **在开发环境中会调用一些函数两次**（仅限应为纯函数的函数）。这些函数包括：

- 组件函数体（仅限顶层逻辑，不包括事件处理程序内的代码）
- 传递给 [`useState`](https://react.docschina.org/reference/react/useState)、[`set` 函数](https://react.docschina.org/reference/react/useState#setstate)、[`useMemo`](https://react.docschina.org/reference/react/useMemo) 或 [`useReducer`](https://react.docschina.org/reference/react/useReducer) 的函数。
- 部分类组件的方法，例如 [`constructor`](https://react.docschina.org/reference/react/Component#constructor)、[`render`](https://react.docschina.org/reference/react/Component#render)、[`shouldComponentUpdate`](https://react.docschina.org/reference/react/Component#shouldcomponentupdate) 等（[请参阅完整列表](https://reactjs.org/docs/strict-mode.html#detecting-unexpected-side-effects)）。

如果一个函数是纯函数，运行两次不会改变其行为，因为纯函数每次都会产生相同的结果。然而，如果一个函数是非纯函数（例如，它会修改接收到的数据），运行两次通常会产生明显的差异（这就是它是非纯函数的原因！）。这有助于及早发现并修复错误。

### 修复在开发中通过重新运行 Effect 发现的错误 

严格模式也可以帮助发现 [Effect](https://react.docschina.org/learn/synchronizing-with-effects) 中的错误。

每个 Effect 都有一些 setup 和可能的 cleanup 函数。通常情况下，当组件挂载时，React 会调用 setup 代码；当组件卸载时，React 会调用 cleanup 代码。如果依赖关系在上一次渲染之后发生了变化，React 将再次调用 setup 代码和 cleanup 代码。

当开启严格模式时，React 还会在开发模式下为每个 Effect **额外运行一次 setup 和 cleanup 函数**。这可能会让人感到惊讶，但它有助于发现手动难以捕捉到的细微错误

### 修复严格模式发出的弃用警告 

React 会在任何一个位于 `<StrictMode>` 树中的组件使用以下弃用 API 时发出警告：

- [`findDOMNode`](https://react.docschina.org/reference/react-dom/findDOMNode)，[请参考替代方案](https://reactjs.org/docs/strict-mode.html#warning-about-deprecated-finddomnode-usage)。
- `UNSAFE_` 类生命周期方法，例如 [`UNSAFE_componentWillMount`](https://react.docschina.org/reference/react/Component#unsafe_componentwillmount)，[请参考替代方案](https://reactjs.org/blog/2018/03/27/update-on-async-rendering.html#migrating-from-legacy-lifecycles)。
- 旧版上下文（[`childContextTypes`](https://react.docschina.org/reference/react/Component#static-childcontexttypes)、[`contextTypes`](https://react.docschina.org/reference/react/Component#static-contexttypes) 和 [`getChildContext`](https://react.docschina.org/reference/react/Component#getchildcontext)），[请参考替代方案](https://react.docschina.org/reference/react/createContext)。
- 旧版字符串引用（[`this.refs`](https://react.docschina.org/reference/react/Component#refs)）,[请参考替代方案](https://reactjs.org/docs/strict-mode.html#warning-about-legacy-string-ref-api-usage)。

这些 API 主要用于旧版的 [类式组件](https://react.docschina.org/reference/react/Component)，因此在新版程序中很少出现。