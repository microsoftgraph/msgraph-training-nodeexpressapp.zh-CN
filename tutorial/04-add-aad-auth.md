<!-- markdownlint-disable MD002 MD041 -->

在此练习中，你将扩展上一练习中的应用程序，以支持使用 Azure AD 进行身份验证。 这是获取调用 Microsoft Graph 所需的 OAuth 访问令牌所必需的。 在此步骤中，您将 [msal 节点](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) 库集成到应用程序中。

1. 在应用程序的根目录下创建一个名为 **.env** 的新文件，并添加以下代码。

    :::code language="ini" source="../demo/graph-tutorial/example.env":::

    替换为 `YOUR_CLIENT_SECRET_HERE` 应用程序注册门户中的应用程序 ID，并 `YOUR_APP_SECRET_HERE` 替换为生成的客户端密码。

    > [!IMPORTANT]
    > 如果你使用的是源代码管理（如 git），那么现在应该从源代码管理中排除 **.env** 文件，以避免意外泄露应用 ID 和密码。

1. 打开 **./app.js，** 将以下行添加到文件顶部以加载 **.env** 文件。

    ```javascript
    require('dotenv').config();
    ```

## <a name="implement-sign-in"></a>实现登录

1. 找到 `var app = express();` **./app.js。** 在该行后 **插入以下** 代码。

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="MsalInitSnippet":::

    此代码使用应用 ID 和密码初始化 msal 节点库。

1. 在名为auth.js的 **./routes** 目录中创建新文件， **并** 添加以下代码。

    ```javascript
    var router = require('express-promise-router')();

    /* GET auth callback. */
    router.get('/signin',
      async function (req, res) {
        const urlParameters = {
          scopes: process.env.OAUTH_SCOPES.split(','),
          redirectUri: process.env.OAUTH_REDIRECT_URI
        };

        try {
          const authUrl = await req.app.locals
            .msalClient.getAuthCodeUrl(urlParameters);
          res.redirect(authUrl);
        }
        catch (error) {
          console.log(`Error: ${error}`);
          req.flash('error_msg', {
            message: 'Error getting auth URL',
            debug: JSON.stringify(error, Object.getOwnPropertyNames(error))
          });
          res.redirect('/');
        }
      }
    );

    router.get('/callback',
      async function(req, res) {
        const tokenRequest = {
          code: req.query.code,
          scopes: process.env.OAUTH_SCOPES.split(','),
          redirectUri: process.env.OAUTH_REDIRECT_URI
        };

        try {
          const response = await req.app.locals
            .msalClient.acquireTokenByCode(tokenRequest);

          // TEMPORARY!
          // Flash the access token for testing purposes
          req.flash('error_msg', {
            message: 'Access token',
            debug: response.accessToken
          });
        } catch (error) {
          req.flash('error_msg', {
            message: 'Error completing authentication',
            debug: JSON.stringify(error, Object.getOwnPropertyNames(error))
          });
        }

        res.redirect('/');
      }
    );

    router.get('/signout',
      async function(req, res) {
        // Sign out
        if (req.session.userId) {
          // Look up the user's account in the cache
          const accounts = await req.app.locals.msalClient
            .getTokenCache()
            .getAllAccounts();

          const userAccount = accounts.find(a => a.homeAccountId === req.session.userId);

          // Remove the account
          if (userAccount) {
            req.app.locals.msalClient
              .getTokenCache()
              .removeAccount(userAccount);
          }
        }

        // Destroy the user's session
        req.session.destroy(function (err) {
          res.redirect('/');
        });
      }
    );

    module.exports = router;
    ```

    这将定义具有三个路由的路由器：、 `signin` `callback` 和 `signout` 。

    该 `signin` 路由调用 `getAuthCodeUrl` 函数以生成登录 URL，然后将浏览器重定向到该 URL。

    此 `callback` 路由是 Azure 在登录完成后重定向的地方。 代码调用 `acquireTokenByCode` 函数以交换访问令牌的授权代码。 获取令牌后，它将使用临时错误值中的访问令牌重定向回主页。 在继续之前，我们将使用此验证登录是否正常工作。 测试之前，我们需要将 Express 应用配置为使用 **./routes/auth.js。**

    `signout`该方法将用户注销并销毁会话。

1. 打开 **./app.js，** 并将以下代码 **插入行** `var app = express();` 之前。

    ```javascript
    var authRouter = require('./routes/auth');
    ```

1. 在行后 **插入以下** `app.use('/', indexRouter);` 代码。

    ```javascript
    app.use('/auth', authRouter);
    ```

启动服务器并浏览到 `https://localhost:3000` 。 单击登录按钮，应重定向到 `https://login.microsoftonline.com` 。 使用 Microsoft 帐户登录，并同意请求的权限。 浏览器重定向到应用，显示令牌。

### <a name="get-user-details"></a>获取用户详细信息

1. 在名为graph.js **项目根** 目录下创建一个新文件，并添加以下代码。

    ```javascript
    var graph = require('@microsoft/microsoft-graph-client');
    require('isomorphic-fetch');

    module.exports = {
      getUserDetails: async function(accessToken) {
        const client = getAuthenticatedClient(accessToken);

        const user = await client
          .api('/me')
          .select('displayName,mail,mailboxSettings,userPrincipalName')
          .get();
        return user;
      },
    };

    function getAuthenticatedClient(accessToken) {
      // Initialize Graph client
      const client = graph.Client.init({
        // Use the provided access token to authenticate
        // requests
        authProvider: (done) => {
          done(null, accessToken);
        }
      });

      return client;
    }
    ```

    这将导出 `getUserDetails` 函数，该函数使用 Microsoft Graph SDK 调用 `/me` 终结点并返回结果。

1. 打开 **./routes/auth.js，** 将以下语句 `require` 添加到文件顶部。

    ```javascript
    var graph = require('../graph');
    ```

1. 将现有的回调路由替换为以下代码。

    :::code language="javascript" source="../demo/graph-tutorial/routes/auth.js" id="CallbackSnippet" highlight="13-23":::

    新代码将用户的帐户 ID 保存在会话中，从 Microsoft Graph 获取用户的详细信息，并保存到应用的用户存储中。

1. 重新启动服务器并完成登录过程。 你最终应返回主页，但 UI 应更改以指示你已登录。

    ![登录后主页的屏幕截图](./images/add-aad-auth-01.png)

1. 单击右上角的用户头像以访问 **"注销"** 链接。 单击 **"注销** "可重置会话，并返回到主页。

    ![包含"注销"链接的下拉菜单屏幕截图](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a>存储和刷新令牌

此时，应用程序具有访问令牌，该令牌在 API 调用 `Authorization` 标头中发送。 这是允许应用代表用户访问 Microsoft Graph 的令牌。

但是，此令牌是短期的。 令牌在颁发后一小时过期。 刷新令牌在这里变得有用。 OAuth 规范引入了刷新令牌，允许应用请求新的访问令牌，而无需用户重新登录。

由于应用使用的是 msal 节点包，因此无需实现任何令牌存储或刷新逻辑。 应用使用默认的 msal-node 内存中令牌缓存，这足以用于示例应用程序。 生产应用程序应提供自己的 [缓存插件](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-node/docs/configuration.md) ，以在安全、可靠的存储介质中序列化令牌缓存。
