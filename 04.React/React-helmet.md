### React-helmet

>[React Helmet的简介及使用教程 - Made with React](https://madewith.cn/699)
>
>React Helmet 是一个用于管理文档头部（head）的 React 库。在 React 应用程序中，文档头部包含了一些重要的信息，例如页面标题、元数据（如描述、关键字）、样式表和脚本等。React Helmet 提供了一种便捷的方式来动态地修改和管理这些文档头部的内容。

#### 特点：

- 支持所有有效的`head`标签: `title`、 `base`、 `meta`、 `link`、 `script`、 `noscript`、 和`style`。
- 支持`body`、 `html` 和 `title` 的属性
- 支持服务端渲染
- 嵌套组件覆盖重复的head标签更改。
- 在同一组件中定义时，将保留重复的head标签更改。(支持如"apple-touch-icon"的标签).
- 支持跟踪DOM更改的回调

使用 React Helmet 的步骤如下：

1.安装 React Helmet 库。可以使用 npm 或 yarn 进行安装：

```bash
npm install react-helmet
yarn add react-helmet


```

2.在需要使用 React Helmet 的组件中引入 Helmet 组件：

````jsx
import { Helmet } from 'react-helmet';
````

3.在组件的 render 方法中使用 Helmet 组件来修改文档头部的内容。例如，可以通过设置 title 属性来修改页面标题：

````jsx
render() {
  return (
    <div>
      <Helmet>
        <title>My Page Title</title>
      </Helmet>
      {/* 其他组件内容 */}
    </div>
  );
}
````

`helmet`实体包括以下属性：

- `base`
- `bodyAttributes`
- `htmlAttributes`
- `link`
- `meta`
- `noscript`
- `script`
- `style`
- `title`

每个属性都有`toComponent()` 和`toString()`方法：

作为String输出：

````jsx
const html = `
    <!doctype html>
    <html ${helmet.htmlAttributes.toString()}>
        <head>
            ${helmet.title.toString()}
            ${helmet.meta.toString()}
            ${helmet.link.toString()}
        </head>
        <body ${helmet.bodyAttributes.toString()}>
            <div id="content">
                // React stuff here
            </div>
        </body>
    </html>
`;
````

作为React组件使用：

````jsx
function HTML () {
    const htmlAttrs = helmet.htmlAttributes.toComponent();
    const bodyAttrs = helmet.bodyAttributes.toComponent();

    return (
        <html {...htmlAttrs}>
            <head>
                {helmet.title.toComponent()}
                {helmet.meta.toComponent()}
                {helmet.link.toComponent()}
            </head>
            <body {...bodyAttrs}>
                <div id="content">
                    // React stuff here
                </div>
            </body>
        </html>
    );
}
````

