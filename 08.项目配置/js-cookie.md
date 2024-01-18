## js-cookie

[每天一个npm包 之 js-cookie - 简书 (jianshu.com)](https://www.jianshu.com/p/4bc884b92339)



1. **读取 Cookie 值：** 使用 `js-cookie`，开发人员可以轻松地读取浏览器中的 cookie 值。这对于获取保存在 cookie 中的用户信息、首选项或其他状态信息很有用。
2. **写入 Cookie 值：** 开发人员可以使用 `js-cookie` 设置新的 cookie 值，这在需要在客户端存储信息时非常实用，例如用户首选项、会话信息等。
3. **删除 Cookie：** `js-cookie` 提供了简单的方法来删除特定的 cookie。这在用户注销或者不再需要某些存储在 cookie 中的信息时很方便。
4. **处理 Cookie 的属性：** 开发人员可以使用 `js-cookie` 控制 cookie 的属性，如过期时间、路径、域等。这使得能够更精确地管理 cookie 的行为。
5. **跨站点脚本 (XSS) 防御：** `js-cookie` 实现了一些安全性的机制，以防范跨站点脚本 (XSS) 攻击。它提供了一些功能，确保对 cookie 的操作是安全的。
6. **方便的 API：** `js-cookie` 提供了一个简单而强大的 API，使开发人员能够更轻松地处理 cookie，而无需深入了解浏览器 cookie API 的细节。

````jsx
  // 设置 Cookie
  Cookies.set('user', 'John Doe');

  // 读取 Cookie
  var username = Cookies.get('user');
  console.log('Username:', username);

  // 删除 Cookie
  Cookies.remove('user');

  // 设置带有过期时间的 Cookie（例如，过期时间为一天）
  Cookies.set('token', 'abcd1234', { expires: 1 });

  // 读取带有过期时间的 Cookie
  var token = Cookies.get('token');
  console.log('Token:', token);

  // 删除所有 Cookie
  Cookies.remove();

  // 设置带有路径的 Cookie
  Cookies.set('theme', 'dark', { path: '/' });

  // 读取带有路径的 Cookie
  var theme = Cookies.get('theme');
  console.log('Theme:', theme);
````

