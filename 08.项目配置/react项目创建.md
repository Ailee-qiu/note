>相关链接：
>
>[超全面详细一条龙教程！从零搭建React项目全家桶（上篇） - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/104771562)
>
>[(12条消息) Vite创建React项目_猫老板的豆的博客-CSDN博客_vite创建react项目](https://blog.csdn.net/x550392236/article/details/120328130#:~:text=启动项目 1 进入项目： cd vite-react 2 安装依赖： npm,编译项目： npm run build 或 npx vite build)

1.官方脚手架

````shell
npx create-react-app react-app-name
cd react-app-name
yarn start/npm start
````

2.Vite创建React项目

````
# npm 6.x
npm init vite@latest my-react-vite-app --template react

# yarn
yarn create vite my-react-vite-app --template react
````

- 进入项目：`cd vite-react`
- 安装依赖：`npm install`
- 运行项目：`npm run dev` 或 `npx vite`
- 编译项目：`npm run build` 或 `npx vite build`