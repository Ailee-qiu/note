### React-keepalive-router

>[GitHub - GoodLuckAlien/react-keepalive-router: The react cache component developed based on react 16.8 +, react router 4 + can be used to cache page components, similar to Vue keepalive package Vue router effect function.(基于react 16.8+ ,react-router 4+ 开发的react缓存组件，可以用于缓存页面组件，类似vue的keepalive包裹vue-router的效果功能)。](https://github.com/GoodLuckAlien/react-keepalive-router)

>`react-keepalive-router` 是一个用于 React 应用的路由库，它提供了组件缓存和保持活动状态的功能，以优化应用的性能和用户体验。
>
>以下是 `react-keepalive-router` 的主要作用：
>
>1. 组件缓存：`react-keepalive-router` 允许你在路由切换时对组件进行缓存，避免了组件的卸载和重新加载。被缓存的组件会保持其状态和数据，当再次切换到该路由时，它会快速恢复并显示，提供更流畅的用户体验。
>2. 保持活动状态：被缓存的组件在切换路由时会保持其活动状态，包括表单输入、滚动位置、播放状态等。这意味着用户可以无缝地切换路由并返回到之前的操作状态，而无需重新开始。
>3. 路由切换控制：`react-keepalive-router` 提供了自定义的路由切换控制组件，例如 `KeepAliveRouterSwitch`，它允许你更细粒度地控制哪些路由需要进行组件缓存和保持活动状态。
>4. 性能优化：通过使用组件缓存和保持活动状态，`react-keepalive-router` 可以减少不必要的组件加载和渲染，从而提高应用的性能和响应速度。它特别适用于复杂的页面、表单、列表等需要保持状态和数据的场景。
>
>使用 `react-keepalive-router` 可以改善 React 应用的用户体验，并减少不必要的资源消耗。它提供了一种简单且灵活的方式来管理组件的缓存和状态，以提高应用的效率和可维护性。

>KeepAliveRouterSwitch替换Switch
>
>KeepAliveRoute替换Route