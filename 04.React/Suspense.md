### Suspense

>在 React 中，Suspense API 用于处理组件的延迟加载和错误边界。它提供了一种优雅的方式来处理异步加载数据或组件时的等待状态，并在加载完成后显示内容。
>
>Suspense API 的主要作用如下：
>
>1. 延迟加载组件：Suspense 组件可以包裹一个或多个需要延迟加载的组件。当这些组件被渲染时，Suspense 会显示一个备用的加载状态，直到组件的代码和依赖被加载完成。
>2. 显示加载状态：通过使用 `<Fallback>` 组件作为 Suspense 的子组件，你可以自定义加载状态的 UI，例如显示一个加载动画、加载提示或占位符组件。当延迟加载的组件还没有准备好时，将会显示 `<Fallback>` 组件。
>3. 处理加载错误：Suspense API 也提供了处理加载错误的能力。你可以使用 `<ErrorBoundary>` 组件作为 Suspense 的子组件，用于捕获延迟加载组件的错误并显示备用的错误 UI。
>
>使用 Suspense API 可以改善应用的用户体验，特别是在处理慢加载组件或异步数据获取时。它使得在等待加载完成时可以显示有意义的反馈信息，而不是让用户看到空白或不友好的界面。

````jsx
import { Suspense } from 'react';

function LazyComponent() {
  // 模拟延迟加载
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(<div>延迟加载的组件</div>);
    }, 2000);
  });
}

function LoadingFallback() {
  return <div>加载中...</div>;
}

function ErrorFallback() {
  return <div>加载出错了！</div>;
}

function App() {
  return (
    <div>
      <h1>React Suspense 示例</h1>
      <Suspense fallback={<LoadingFallback />}>
        <LazyComponent />
      </Suspense>
    </div>
  );
}
````

在上面的示例中，`LazyComponent` 是一个延迟加载的组件，它的加载状态通过 `<LoadingFallback>` 组件显示。当组件加载完成后，它将被渲染并显示在页面上。

你还可以使用 `<ErrorBoundary>` 组件来处理加载错误，以提供更好的错误处理和用户体验。

请注意，Suspense API 目前主要用于支持 React 的异步渲染模式（如 React.lazy 和 React.Suspense），而不是在常规的同步渲染中使用。