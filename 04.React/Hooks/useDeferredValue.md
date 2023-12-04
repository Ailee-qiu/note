# useDeferredValue

`useDeferredValue` 是一个 React Hook，可以让你延迟更新 UI 的某些部分。

```
const deferredValue = useDeferredValue(value)
```

- 参考
 - [`useDeferredValue(value)`](https://react.docschina.org/reference/react/useDeferredValue#usedeferredvalue)
- 用法
 - [在新内容加载期间显示旧内容。](https://react.docschina.org/reference/react/useDeferredValue#showing-stale-content-while-fresh-content-is-loading)
 - [表明内容已过时](https://react.docschina.org/reference/react/useDeferredValue#indicating-that-the-content-is-stale)
 - [延迟渲染 UI 的某些部分](https://react.docschina.org/reference/react/useDeferredValue#deferring-re-rendering-for-a-part-of-the-ui)

#### 参数 

- `value`：你想延迟的值，可以是任何类型。

#### 返回值 

在组件的初始渲染期间，返回的延迟值将与你提供的值相同。但是在组件更新时，React 将会先尝试使用旧值进行重新渲染（因此它将返回旧值），然后再在后台使用新值进行另一个重新渲染（这时它将返回更新后的值）。

#### 注意事项 

- 你应该向 `useDeferredValue` 传递原始值（如字符串和数字）或在渲染之外创建的对象。如果你在渲染期间创建了一个新对象，并立即将其传递给 `useDeferredValue`，那么每次渲染时这个对象都会不同，这将导致后台不必要的重新渲染。
- 当 `useDeferredValue` 接收到与之前不同的值（使用 [`Object.is`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/is) 进行比较）时，除了当前渲染（此时它仍然使用旧值），它还会安排一个后台重新渲染。这个后台重新渲染是可以被中断的，如果 `value` 有新的更新，React 会从头开始重新启动后台渲染。举个例子，如果用户在输入框中的输入速度比接收延迟值的图表重新渲染的速度快，那么图表只会在用户停止输入后重新渲染。
- `useDeferredValue` 与 [``](https://react.docschina.org/reference/react/Suspense) 集成。如果由于新值引起的后台更新导致 UI 暂停，用户将不会看到后备方案。他们将看到旧的延迟值，直到数据加载完成。
- `useDeferredValue` 本身并不能阻止额外的网络请求。
- `useDeferredValue` 本身不会引起任何固定的延迟。一旦 React 完成原始的重新渲染，它会立即开始使用新的延迟值处理后台重新渲染。由事件（例如输入）引起的任何更新都会中断后台重新渲染，并被优先处理。
- 由 `useDeferredValue` 引起的后台重新渲染在提交到屏幕之前不会触发 Effect。如果后台重新渲染被暂停，Effect 将在数据加载后和 UI 更新后运行。

## 用法 

### 在新内容加载期间显示旧内容。 

在组件的顶层调用 `useDeferredValue` 来延迟更新 UI 的某些部分。

````tsx
import { useState, useDeferredValue } from 'react';

function SearchPage() {
  const [query, setQuery] = useState('');
  const deferredQuery = useDeferredValue(query);
  // ...
}
````

在初始渲染期间，返回的 延迟值 与你提供的 值 相同。

在更新期间，延迟值 会“滞后于”最新的 值。具体地说，React 首先会在不更新延迟值的情况下进行重新渲染，然后在后台尝试使用新接收到的值进行重新渲染。

### 表明内容已过时 

在上面的示例中，当最新的查询结果仍在加载时，没有任何提示。如果新的结果需要一段时间才能加载完成，这可能会让用户感到困惑。为了更明显地告知用户结果列表与最新查询不匹配，你可以在显示旧的查询结果时添加一个视觉提示：



### 延迟渲染 UI 的某些部分 

你还可以将 `useDeferredValue` 作为性能优化的手段。当你的 UI 某个部分重新渲染很慢、没有简单的优化方法，同时你又希望避免它阻塞其他 UI 的渲染时，使用 `useDeferredValue` 很有帮助。

这个优化需要将 `SlowList` 包裹在 [`memo`](https://react.docschina.org/reference/react/memo) 中。这是因为每当 `text` 改变时，React 需要能够快速重新渲染父组件。在重新渲染期间，`deferredText` 仍然保持着之前的值，因此 `SlowList` 可以跳过重新渲染（它的 props 没有改变）。如果没有 [`memo`](https://react.docschina.org/reference/react/memo)，`SlowList` 仍会重新渲染，这将使优化失去意义。



>### 延迟一个值与防抖和节流之间有什么不同
>
>在上述的情景中，你可能会使用这两种常见的优化技术：
>
>- **防抖** 是指在用户停止输入一段时间（例如一秒钟）之后再更新列表。
>- **节流** 是指每隔一段时间（例如最多每秒一次）更新列表。
>
>虽然这些技术在某些情况下是有用的，但 `useDeferredValue` 更适合优化渲染，因为它与 React 自身深度集成，并且能够适应用户的设备。
>
>与防抖或节流不同，`useDeferredValue` 不需要选择任何固定延迟时间。如果用户的设备很快（比如性能强劲的笔记本电脑），延迟的重渲染几乎会立即发生并且不会被察觉。如果用户的设备较慢，那么列表会相应地“滞后”于输入，滞后的程度与设备的速度有关。
>
>此外，与防抖或节流不同，`useDeferredValue` 执行的延迟重新渲染默认是可中断的。这意味着，如果 React 正在重新渲染一个大型列表，但用户进行了另一次键盘输入，React 会放弃该重新渲染，先处理键盘输入，然后再次开始在后台渲染。相比之下，防抖和节流仍会产生不顺畅的体验，因为它们是阻*的：它们仅仅是将渲染阻塞键盘输入的时刻推迟了。
>
>如果你要优化的工作不是在渲染期间发生的，那么防抖和节流仍然非常有用。例如，它们可以让你减少网络请求的次数。你也可以同时使用这些技术。

