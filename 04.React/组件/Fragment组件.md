# Fragment组件

`<Fragment>` 通常使用 `<>...</>` 代替，它们都允许你在不添加额外节点的情况下将子元素组合。

```tsx
<>
  <OneChild />
  <AnotherChild />
</>
```

## 参考 

### `<Fragment>` 

当你需要单个元素时，你可以使用 `<Fragment>` 将其他元素组合起来，使用 `<Fragment>` 组合后的元素不会对 DOM 产生影响，就像元素没有被组合一样。在大多数情况下，`<Fragment></Fragment>` 可以简写为空的 JSX 元素 `<></>`。

#### 参数 

- **可选** `key`：列表中 `<Fragment>` 的可以拥有 [keys](https://react.docschina.org/learn/rendering-lists#keeping-list-items-in-order-with-key)。

#### 注意事项 

- 如果你要传递 `key` 给一个 `<Fragment>`，你不能使用 `<>...</>`，你必须从 `'react'` 中导入 `Fragment` 且表示为`<Fragment key={yourKey}>...</Fragment>`。
- 当你要从 `<><Child /></>` 转换为  `[<Child />]` 或 `<><Child /></>` 转换为 `<Child />`，React 并不会[重置 state](https://react.docschina.org/learn/preserving-and-resetting-state)。这个规则只在一层深度的情况下生效，如果从 `<><><Child /></></>` 转换为 `<Child />` 则会重置 state。在[这里](https://gist.github.com/clemmy/b3ef00f9507909429d8aa0d3ee4f986b)查看更详细的介绍。

## 用法 

### 返回多个元素 

使用 `Fragment` 或简写语法 `<>...</>` 将多个元素组合在一起，你可以使用它将多个元素等效于单个元素。例如，一个组件只能返回一个元素，但是可以使用 `Fragment` 将多个元素组合在一起，并作为一个元素返回：

```tsx
function Post() {
  return (
    <>
      <PostTitle />
      <PostBody />
    </>
  );
}
```

### 分配多个元素给一个变量 

和其他元素一样，你可以将 `Fragment` 元素分配给变量，作为 props 传递等：

````tsx
function CloseDialog() {
  const buttons = (
    <>
      <OKButton />
      <CancelButton />
    </>
  );
  return (
    <AlertDialog buttons={buttons}>
      Are you sure you want to leave this page?
    </AlertDialog>
  );
}
````

### 组合文本与组件 

你可以使用 `Fragment` 将文本与组件组合在一起：

```tsx
function DateRangePicker({ start, end }) {
  return (
    <>
      From
      <DatePicker date={start} />
      to
      <DatePicker date={end} />
    </>
  );
}
```

### 渲染 `Fragment` 列表 

在这种情况下，你需要显式地表示为 `Fragment`，而不是使用简写语法 `<></>`。当你在[循环中渲染多个元素](https://react.docschina.org/learn/rendering-lists)时，你需要为每一个元素分配一个 `key`。如果这个元素为 `Fragment` 时，则需要使用普通的 JSX 语法来提供 `key` 属性。

```tsx
function Blog() {
  return posts.map(post =>
    <Fragment key={post.id}>
      <PostTitle title={post.title} />
      <PostBody body={post.body} />
    </Fragment>
  );
}
```

