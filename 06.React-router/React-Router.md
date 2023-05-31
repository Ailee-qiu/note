### React-Router

1.安装 React Router。可以使用 npm 或 yarn 进行安装：

````bash
npm install react-router-dom
yarn add react-router-dom
````



2.在应用程序的根组件中引入 React Router 相关的组件和函数：

````jsx
import { BrowserRouter as Router, Route, Switch, Link } from 'react-router-dom';
````



3.使用 `<Router>` 组件将应用程序包裹起来，作为整个应用程序的路由容器。通常，将其放置在应用程序的最外层。

````jsx
<Router>
  {/* 应用程序的其他内容 */}
</Router>
````



4.使用 `<Route>` 组件定义路由规则，将组件与特定的路径相关联。例如：

````jsx
<Route path="/" exact component={Home} />
<Route path="/about" component={About} />
<Route path="/contact" component={Contact} />
````



5.使用 `<Link>` 组件创建导航链接，使用户能够在不同的页面之间进行导航。例如：

````jsx
<ul>
  <li>
    <Link to="/">Home</Link>
  </li>
  <li>
    <Link to="/about">About</Link>
  </li>
  <li>
    <Link to="/contact">Contact</Link>
  </li>
</ul>
````

