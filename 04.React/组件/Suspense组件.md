# <Suspense>



````js
//<Suspense> 允许在子组件完成加载前展示后备方案。
<Suspense fallback={<Loading />}>
  <SomeComponent />
</Suspense>
````

>- 用法
>  - [当内容正在加载时显示后备方案](https://react.docschina.org/reference/react/Suspense#displaying-a-fallback-while-content-is-loading)
>  - [同时展示内容](https://react.docschina.org/reference/react/Suspense#revealing-content-together-at-once)
>  - [逐步加载内容](https://react.docschina.org/reference/react/Suspense#revealing-nested-content-as-it-loads)
>  - [在新内容加载时展示过时内容](https://react.docschina.org/reference/react/Suspense#showing-stale-content-while-fresh-content-is-loading)
>  - [阻止隐藏已经显示的内容](https://react.docschina.org/reference/react/Suspense#preventing-already-revealed-content-from-hiding)
>  - [表明 transition 正在发生](https://react.docschina.org/reference/react/Suspense#indicating-that-a-transition-is-happening)
>  - [在导航时重置 Suspense 边界](https://react.docschina.org/reference/react/Suspense#resetting-suspense-boundaries-on-navigation)
>  - [为服务器错误和客户端内容提供后备方案](https://react.docschina.org/reference/react/Suspense#providing-a-fallback-for-server-errors-and-client-only-content)