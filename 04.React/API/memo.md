## memo

`memo` 允许你的组件在 props 没有改变的情况下跳过重新渲染。

## 参考 

### `memo(Component, arePropsEqual?)` 

使用 `memo` 将组件包装起来，以获得该组件的一个 **记忆化** 版本。通常情况下，只要该组件的 props 没有改变，这个记忆化版本就不会在其父组件重新渲染时重新渲染。但 React 仍可能会重新渲染它：记忆化是一种性能优化，而非保证。

````tsx
import { memo } from 'react';

const SomeComponent = memo(function SomeComponent(props) {
  // ...
});
````

#### 参数 

- `Component`：要进行记忆化的组件。`memo` 不会修改该组件，而是返回一个新的、记忆化的组件。它接受任何有效的 React 组件，包括函数组件和 [`forwardRef`](https://react.docschina.org/reference/react/forwardRef) 组件。
- **可选参数** `arePropsEqual`：一个函数，接受两个参数：组件的前一个 props 和新的 props。如果旧的和新的 props 相等，即组件使用新的 props 渲染的输出和表现与旧的 props 完全相同，则它应该返回 `true`。否则返回 `false`。通常情况下，你不需要指定此函数。默认情况下，React 将使用 [`Object.is`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/is) 比较每个 prop。

#### 返回值 

`memo` 返回一个新的 React 组件。它的行为与提供给 `memo` 的组件相同，只是当它的父组件重新渲染时 React 不会总是重新渲染它，除非它的 props 发生了变化。

## 用法 

### 当 props 没有改变时跳过重新渲染 

React 通常在其父组件重新渲染时重新渲染一个组件。你可以使用 `memo` 创建一个组件，当它的父组件重新渲染时，只要它的新 props 与旧 props 相同时，React 就不会重新渲染它。这样的组件被称为 **记忆化的**（memoized）组件。

要记忆化一个组件，请将它包装在 `memo` 中，使用它返回的值替换原来的组件：

````tsx
const Greeting = memo(function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
});

export default Greeting;
````

React 组件应该始终具有 [纯粹的渲染逻辑](https://react.docschina.org/learn/keeping-components-pure)。这意味着如果其 props、state 和 context 没有改变，则必须返回相同的输出。通过使用 `memo`，你告诉 React 你的组件符合此要求，因此只要其 props 没有改变，React 就不需要重新渲染。即使使用 `memo`，如果它自己的 state 或正在使用的 context 发生更改，组件也会重新渲染。

### 使用 state 更新记忆化（memoized）组件 

即使一个组件被记忆化了，当它自身的状态发生变化时，它仍然会重新渲染。memoization 只与从父组件传递给组件的 props 有关。

### 使用 context 更新记忆化（memoized）组件 

即使组件已被记忆化，当其使用的 context 发生变化时，它仍将重新渲染。记忆化只与从父组件传递给组件的 props 有关。

### 最小化 props 的变化 

当你使用 `memo` 时，只要任何一个 prop 与先前的值不是 **浅层相等** 的话，你的组件就会重新渲染。这意味着 React 会使用 [`Object.is`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/is) 比较组件中的每个 prop 与其先前的值。注意，`Object.is(3, 3)` 为 `true`，但 `Object.is({}, {})` 为 `false`。

为了最大化使用 `memo` 的效果，应该尽量减少 props 的变化次数。例如，如果 props 是一个对象，可以使用 [`useMemo`](https://react.docschina.org/reference/react/useMemo) 避免父组件每次都重新创建该对象：

````tsx
function Page() {
  const [name, setName] = useState('Taylor');
  const [age, setAge] = useState(42);

  const person = useMemo(
    () => ({ name, age }),
    [name, age]
  );

  return <Profile person={person} />;
}

const Profile = memo(function Profile({ person }) {
  // ...
});
````

最小化 props 的改变的更好的方法是确保组件在其 props 中接受必要的最小信息。例如，它可以接受单独的值而不是整个对象：

````tsx
function Page() {
  const [name, setName] = useState('Taylor');
  const [age, setAge] = useState(42);
  return <Profile name={name} age={age} />;
}

const Profile = memo(function Profile({ name, age }) {
  // ...
});
````

即使是单个值有时也可以投射为不经常变更的值。例如，这里的组件接受一个布尔值，表示是否存在某个值，而不是值本身：

````tsx
function GroupsLanding({ person }) {
  const hasGroups = person.groups !== null;
  return <CallToAction hasGroups={hasGroups} />;
}

const CallToAction = memo(function CallToAction({ hasGroups }) {
  // ...
});
````

当你需要将一个函数传递给记忆化（memoized）组件时，要么在组件外声明它，以确保它永远不会改变，要么使用 [`useCallback`](https://react.docschina.org/reference/react/useCallback#skipping-re-rendering-of-components) 在重新渲染之间缓存其定义。

### 指定自定义比较函数 

在极少数情况下，最小化 memoized 组件的 props 更改可能是不可行的。在这种情况下，你可以提供一个自定义比较函数，React 将使用它来比较旧的和新的 props，而不是使用浅比较。这个函数作为 `memo` 的第二个参数传递。它应该仅在新的 props 与旧的 props 具有相同的输出时返回 `true`；否则应该返回 `false`。

````tsx
const Chart = memo(function Chart({ dataPoints }) {
  // ...
}, arePropsEqual);

function arePropsEqual(oldProps, newProps) {
  return (
    oldProps.dataPoints.length === newProps.dataPoints.length &&
    oldProps.dataPoints.every((oldPoint, index) => {
      const newPoint = newProps.dataPoints[index];
      return oldPoint.x === newPoint.x && oldPoint.y === newPoint.y;
    })
  );
}
````

如果这样做，请使用浏览器开发者工具中的性能面板来确保你的比较函数实际上比重新渲染组件要快。你可能会因此感到惊讶。

在进行性能测量时，请确保 React 处于生产模式下运行。

## 疑难解答 

### 当组件的某个 prop 是对象、数组或函数时，我的组件会重新渲染。 

React 通过浅比较来比较旧的和新的 prop：也就是说，它会考虑每个新的 prop 是否与旧 prop 引用相等。如果每次父组件重新渲染时创建一个新的对象或数组，即使它们每个元素都相同，React 仍会认为它已更改。同样地，如果在渲染父组件时创建一个新的函数，即使该函数具有相同的定义，React 也会认为它已更改。为了避免这种情况，[可以简化 props 或在父组件中记忆化（memoize）props](https://react.docschina.org/reference/react/memo#minimizing-props-changes)。