>uesState



>**useEffect**
>
>useEffect 在 React 的渲染过程中是被异步调用的
>
>当我们需要依赖于某个变量时，我们不仅要给 useEffect 传递第二个参数，还要在 effect 中使用变量

````js
import React, { useState, useEffect } from 'react'

function App() {
  const [count, setCount] = useState(0);
  
  useEffect(()=>{
    console.log('Hello React 17 + Vite App!', count)
  }, [count])

  return (
    <>
      Count: {count}
      <button onClick={() => setCount(0)}>Reset</button>
      <button onClick={() => setCount(prevCount => prevCount - 1)}>-</button>
      <button onClick={() => setCount(prevCount => prevCount + 1)}>+</button>
    </>
  );
}

export default App
````



>componentDidMount
>
>componentDidUpdate
>
>componentWillUnmount
>
>无需清除的effect：发送网络请求，手动变更 DOM，记录日志
>
>需要清除的effect：订阅外部的数据源（避免内存泄漏）
>
>useContext
>
>useReducer
>
>useRef



>useMemo (因变量)
>
>第一个参数是函数，第二个参数为数组
>
>useMemo缓存的结果就是第一个函数return的值，主要用于缓存计算结果的值
>
>如果第二个参数不传，则每次都会执行第一个函数，无意义
>
>第二个参数为空数组，则只执行一次，在渲染期间完成（与useEffect类似，在渲染后进行）
>
>





>useCallback（因变量，返回值为一个function）
>
>主要作用是用来缓存函数，避免函数重复创建导致子组件的重新渲染。
>
>调用时机取决于其依赖项(dependencies)的变化。当useCallback函数所依赖的任何值发生变化时，React将会调用该函数并返回一个新的函数。**useCallback的返回值是一个被缓存的函数**。如果依赖项没有发生变化，则useCallback函数会返回之前缓存的函数对象，否则会返回一个新的函数对象。
>
>被缓存的函数会在某些情况下被调用，具体取决于该函数被作为依赖项传递给其他hooks函数的方式。
>
>当被缓存的函数作为依赖项传递给了useEffect、useLayoutEffect、useImperativeHandle等hooks函数时，该函数会在组件渲染后被调用。如果被缓存的函数直接被调用或者作为普通的函数参数传递给其他函数时，也会被调用。
