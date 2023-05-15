### dangerouslySetInnerHTML

`dangerouslySetInnerHTML` 是 React 提供的一个属性，用于在组件中直接设置 HTML 内容。它被称为 "dangerously"，因为它会绕过 React 的默认的 HTML 转义和防止 XSS 攻击的机制，所以需要谨慎使用。

使用 `dangerouslySetInnerHTML` 属性，你可以将一个包含 HTML 标记的字符串分配给组件的内容，然后 React 将会原样渲染该字符串作为 HTML，而不是将其视为纯文本。



```jsx
import React from 'react';

function MyComponent() {
  const htmlContent = '<strong>Hello, <em>World!</em></strong>';

  return <div dangerouslySetInnerHTML={{ __html: htmlContent }} />;
}

```

在这个示例中，我们定义了一个名为 `htmlContent` 的字符串，其中包含一些 HTML 标记。然后，我们将这个字符串分配给 `div` 组件的 `dangerouslySetInnerHTML` 属性。注意，`dangerouslySetInnerHTML` 接收一个对象作为值，对象中的 `__html` 属性包含了要渲染的 HTML 内容。

需要特别注意的是，由于 `dangerouslySetInnerHTML` 绕过了 React 的默认机制，直接注入 HTML 内容，因此在使用它时要确保内容是可信的，并且避免从用户输入或不受信任的来源中直接获取并渲染 HTML。这样可以防止潜在的安全风险，比如跨站脚本攻击（XSS）。只有在确保内容的安全性并且没有其他替代方案时，才应该使用 `dangerouslySetInnerHTML`。