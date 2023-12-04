### forwardRef和useImperativeHandle

>`forwardRef` 允许组件使用 [ref](https://react.docschina.org/learn/manipulating-the-dom-with-refs) 将 DOM 节点暴露给父组件。
>
>- 参考
>  - [`forwardRef(render)`](https://react.docschina.org/reference/react/forwardRef#forwardref)
>  - [`render` 函数](https://react.docschina.org/reference/react/forwardRef#render-function)
>- 用法
>  - [将 DOM 节点暴露给父组件](https://react.docschina.org/reference/react/forwardRef#exposing-a-dom-node-to-the-parent-component)
>  - [在多个组件中转发 ref](https://react.docschina.org/reference/react/forwardRef#forwarding-a-ref-through-multiple-components)
>  - 暴露命令式句柄而非 DOM 节点

>https://segmentfault.com/a/1190000042532257

>`useImperativeHandle`是React的一个自定义钩子函数，它用于向父组件暴露子组件实例上的特定方法。它允许你在函数组件中定义并暴露一个或多个方法，使得父组件可以通过ref访问到这些方法。
>
>使用`useImperativeHandle`时，你需要结合`useRef`来创建一个ref，并将其传递给子组件。子组件内部通过调用`useImperativeHandle`来定义父组件可以访问的方法。这些方法将成为子组件实例的公开接口。
>
>

````jsx
import React, { useRef, useImperativeHandle, forwardRef } from 'react';

// 自定义文本输入组件
const CustomInput = forwardRef((props, ref) => {
  const inputRef = useRef();

  // 在useImperativeHandle中定义父组件可以访问的方法
  useImperativeHandle(ref, () => ({
    // 获取输入框的值
    getValue() {
      return inputRef.current.value;
    },
    // 清空输入框的值
    clearValue() {
      inputRef.current.value = '';
    },
  }));

  return (
    <input
      type="text"
      ref={inputRef}
      placeholder="Enter text"
    />
  );
});

// 父组件
const ParentComponent = () => {
  const inputRef = useRef();

  const handleClick = () => {
    // 调用子组件暴露的方法获取输入框的值
    const value = inputRef.current.getValue();
    console.log('Input value:', value);
  };

  const handleClear = () => {
    // 调用子组件暴露的方法清空输入框的值
    inputRef.current.clearValue();
  };

  return (
    <div>
      <CustomInput ref={inputRef} />
      <button onClick={handleClick}>Get Value</button>
      <button onClick={handleClear}>Clear Value</button>
    </div>
  );
};

````

在这个示例中，父组件`ParentComponent`包含一个自定义的文本输入组件`CustomInput`。通过使用`forwardRef`，我们可以将`inputRef`传递给`CustomInput`组件，使得父组件可以通过该ref访问到子组件的实例。

在`CustomInput`组件中，我们使用`useImperativeHandle`来定义了两个方法：`getValue`和`clearValue`。这些方法可以让父组件分别获取输入框的值和清空输入框的值。父组件可以通过`inputRef.current`来调用这些方法，并实现对文本输入组件的控制。