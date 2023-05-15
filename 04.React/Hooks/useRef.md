>useRef
>
>- 返回一个可变的 ref 对象，该对象只有个 current 属性，初始值为传入的参数( initialValue )。
>- 返回的 ref 对象在组件的整个生命周期内保持不变
>- 当更新 current 值时并不会 re-render ，这是与 useState 不同的地方
>- 更新 useRef 是 side effect (副作用)，所以一般写在 useEffect 或 event handler 里
>- useRef 类似于类组件的 this

`useRef` 的一般使用场景包括：

1.引用 DOM 元素：可以使用 `useRef` 创建一个引用，然后将其赋值给组件中的某个 DOM 元素，以便在需要时直接访问和操作该元素。

````jsx
import React, { useRef, useEffect } from 'react';

function MyComponent() {
  const myElementRef = useRef(null);

  useEffect(() => {
    // 访问和操作 myElement
    //此时是访问到div标签上的属性
    console.log(myElementRef.current);
  }, []);

  return <div ref={myElementRef}>Hello, World!</div>;
}

````

2.保存和访问之前的值：`useRef` 的引用对象可以在组件的多次渲染之间保持不变，因此可以用来保存和访问之前的值。

```jsx
import React, { useState, useRef } from 'react';

function MyComponent() {
  const [count, setCount] = useState(0);
  const previousCountRef = useRef(0);

  useEffect(() => {
    previousCountRef.current = count;
  }, [count]);

  return (
    <div>
      <p>Current count: {count}</p>
      <p>Previous count: {previousCountRef.current}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

3.存储和共享任意可变值：`useRef` 可以用于存储和共享组件中的任意可变值，不会触发组件的重新渲染。

````jsx
import React, { useRef } from 'react';

function MyComponent() {
  const dataRef = useRef({ name: 'John', age: 25 });

  const updateName = () => {
    dataRef.current.name = 'Alice';
  };

  return (
    <div>
      <p>Name: {dataRef.current.name}</p>
      <p>Age: {dataRef.current.age}</p>
      <button onClick={updateName}>Update Name</button>
    </div>
  );
}
````

总而言之，`useRef` 的一般使用场景包括访问和操作 DOM 元素、保存和访问之前的值，以及存储和共享任意可变值。通过创建一个 `useRef` 引用并将其与相应的元素或值关联，你可以在函数组件中轻松地进行这些操作。