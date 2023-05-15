### 使用 RTK 构建 Redux store 的基本步骤

1. 安装 Redux Toolkit：

   ````bash
   npm install @reduxjs/toolkit
   ````

   

2. 创建 Redux Slice：

   ````ts
   // counterSlice.js
   import { createSlice } from '@reduxjs/toolkit';
   
   const initData:IData = {
       
   }
   
   export const counterSlice = createSlice({
     name: 'counter',
     initialState: initData,
     reducers: {
       increment: (state:,action:)=>{
          state.name = action.payload;
       },
       decrement: (state:,action:)=>{
           state.name = action.payload;
       },
     },
     extraReducers:{
         []:(state,{payload})=>{
             
         }
     }
   });
   
   export const { increment, decrement } = counterSlice.actions;
   export default counterSlice.reducer;
   
   ````

   

3. 创建 Redux Store：

   ```ts
   // store.js
   import { Action,ThunkAction,configureStore } from '@reduxjs/toolkit';
   import counterReducer from './counterSlice';
   import stuReducer from "./stuSlice";
   import schoolReducer from "./schoolSlice";
   const store = configureStore({
     reducer:{
          stuReducer,
          schoolReducer,
          counterReducer
     }
   });
   export type AppDispatch = type store.dispatch
   export type RootState = ReturnType<type store.getState>
   export type AppThunk<ReturnType = void>=ThunkAction<ReturnType,RootState,unknown,Action<string>>
   export default store;
   
   ```

   在上述代码中，我们使用 `typeof store.dispatch` 来获取 `dispatch` 函数的类型，然后将它赋值给 `AppDispatch` 类型。这样我们就可以在应用中使用 `AppDispatch` 来派发 action。

   `ReturnType<typeof store.getState>` 用于获取 `store.getState` 函数的返回类型，并将其赋值给 `RootState` 类型。这样我们就可以使用 `RootState` 来获取应用的状态。

   最后，`ThunkAction<ReturnType, RootState, unknown, Action<string>>` 表示一个泛型类型 `AppThunk`，用于定义 thunk action 的类型。它接受一个 `ReturnType` 参数，表示 thunk action 的返回类型，默认为 `void`。其中的 `RootState` 表示应用的状态类型，`unknown` 表示 thunk extra argument 的类型，而 `Action<string>` 表示默认的 action 类型为字符串。

   通过添加上述类型定义，可以使 Redux store 中的 dispatch、state 和 thunk action 在整个应用中拥有正确的类型推导和类型检查。

4. 在应用中使用 Redux Store：

   ````ts
   import React from 'react';
   import ReactDOM from 'react-dom';
   import { Provider } from 'react-redux';
   import store from './store';
   import App from './App';
   
   ReactDOM.render(
     <Provider store={store}>
       <App />
     </Provider>,
     document.getElementById('root')
   );
   ````

   

当使用 Redux Toolkit (RTK) 和 React 进行状态管理时，可以使用 `useDispatch` 和 `useSelector` 这两个 React Redux 提供的钩子来简化对 Redux store 的操作。

1. 使用 `useDispatch`：

   `useDispatch` 钩子用于获取 Redux store 的 `dispatch` 函数，以便在组件中派发 action。在组件中使用 `useDispatch` 钩子的示例如下：

   ````js
   import { useDispatch } from 'react-redux';
   import { increment, decrement } from './counterSlice';
   
   function MyComponent() {
     const dispatch = useDispatch();
   
     const handleIncrement = () => {
       dispatch(increment());
     };
   
     const handleDecrement = () => {
       dispatch(decrement());
     };
   
     return (
       <div>
         <button onClick={handleIncrement}>Increment</button>
         <button onClick={handleDecrement}>Decrement</button>
       </div>
     );
   }
   ````

   在上面的示例中，通过调用 `useDispatch` 钩子，我们可以获取到 Redux store 的 `dispatch` 函数，并将其赋值给 `dispatch` 变量。然后我们可以在组件中使用 `dispatch` 函数来派发相应的 action。

2. 使用 `useSelector`：

   `useSelector` 钩子用于从 Redux store 中获取状态数据。它接收一个函数作为参数，该函数返回我们所需的状态数据。每当 Redux store 中的状态发生变化时，`useSelector` 钩子将自动重新计算所选取的状态并返回。

   ````js
   import { useSelector } from 'react-redux';
   
   function MyComponent() {
     const counter = useSelector(state => state.counter);
   
     return <div>Counter: {counter}</div>;
   }
   ````

   在上面的示例中，我们使用 `useSelector` 钩子来获取 Redux store 中的 `counter` 状态。`useSelector` 的参数是一个函数，该函数接收整个 Redux store 的状态，并返回我们需要的部分状态数据。在此例中，我们将 `state.counter` 返回作为所选取的状态数据。

   通过使用 `useDispatch` 和 `useSelector` 钩子，我们可以更简洁地在 React 组件中操作 Redux store 的状态和派发 action，无需手动编写繁琐的连接代码。这样可以提高开发效率，并使代码更具可读性和可维护性。