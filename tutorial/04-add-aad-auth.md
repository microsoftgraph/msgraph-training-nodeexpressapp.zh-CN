<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="d5568-101">在本练习中，你将扩展上一练习中的应用程序，以支持 Azure AD 的身份验证。</span><span class="sxs-lookup"><span data-stu-id="d5568-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="d5568-102">若要获取所需的 OAuth 访问令牌以调用 Microsoft Graph，这是必需的。</span><span class="sxs-lookup"><span data-stu-id="d5568-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="d5568-103">在此步骤中，将 [msal 节点](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) 库集成到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="d5568-103">In this step you will integrate the [msal-node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) library into the application.</span></span>

1. <span data-ttu-id="d5568-104">在应用程序的根目录中，创建一个名为 **. env** 的新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="d5568-104">Create a new file named **.env** in the root of your application, and add the following code.</span></span>

    :::code language="ini" source="../demo/graph-tutorial/example.env":::

    <span data-ttu-id="d5568-105">将替换 `YOUR_CLIENT_SECRET_HERE` 为应用程序注册门户中的应用程序 ID，并将替换 `YOUR_APP_SECRET_HERE` 为您生成的客户端密码。</span><span class="sxs-lookup"><span data-stu-id="d5568-105">Replace `YOUR_CLIENT_SECRET_HERE` with the application ID from the Application Registration Portal, and replace `YOUR_APP_SECRET_HERE` with the client secret you generated.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="d5568-106">如果您使用的是源代码管理（如 git），现在是从源代码控制中排除 **env** 文件以避免无意中泄漏您的应用程序 ID 和密码的一段时间。</span><span class="sxs-lookup"><span data-stu-id="d5568-106">If you're using source control such as git, now would be a good time to exclude the **.env** file from source control to avoid inadvertently leaking your app ID and password.</span></span>

1. <span data-ttu-id="d5568-107">打开 **。/app.js** 并将以下行添加到文件顶部，以加载该 **env** 文件。</span><span class="sxs-lookup"><span data-stu-id="d5568-107">Open **./app.js** and add the following line to the top of the file to load the **.env** file.</span></span>

    ```javascript
    require('dotenv').config();
    ```

## <a name="implement-sign-in"></a><span data-ttu-id="d5568-108">实施登录</span><span class="sxs-lookup"><span data-stu-id="d5568-108">Implement sign-in</span></span>

1. <span data-ttu-id="d5568-109">找到中的 "" 行 `var indexRouter = require('./routes/index');` **。/app.js** 。</span><span class="sxs-lookup"><span data-stu-id="d5568-109">Locate the line `var indexRouter = require('./routes/index');` in **./app.js**.</span></span> <span data-ttu-id="d5568-110">**在该行前** 插入以下代码。</span><span class="sxs-lookup"><span data-stu-id="d5568-110">Insert the following code **before** that line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="MsalInitSnippet":::

    <span data-ttu-id="d5568-111">此代码使用应用程序的应用 ID 和密码初始化 msal 的节点库。</span><span class="sxs-lookup"><span data-stu-id="d5568-111">This code initializes the msal-node library with the app ID and password for the app.</span></span>

1. <span data-ttu-id="d5568-112">在名为 **auth.js** 的 **/routes** 目录中创建一个新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="d5568-112">Create a new file in the **./routes** directory named **auth.js** and add the following code.</span></span>

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

    <span data-ttu-id="d5568-113">这将定义具有三个路由的路由器： `signin` 、 `callback` 和 `signout` 。</span><span class="sxs-lookup"><span data-stu-id="d5568-113">This defines a router with three routes: `signin`, `callback`, and `signout`.</span></span>

    <span data-ttu-id="d5568-114">`signin`路由调用 `getAuthCodeUrl` 函数以生成登录 url，然后将浏览器重定向到该 URL。</span><span class="sxs-lookup"><span data-stu-id="d5568-114">The `signin` route calls the `getAuthCodeUrl` function to generate the login URL, then redirects the browser to that URL.</span></span>

    <span data-ttu-id="d5568-115">`callback`路由是在登录完成后 Azure 重定向的位置。</span><span class="sxs-lookup"><span data-stu-id="d5568-115">The `callback` route is where Azure redirects after the signin is complete.</span></span> <span data-ttu-id="d5568-116">代码调用 `acquireTokenByCode` 函数以交换访问令牌的授权代码。</span><span class="sxs-lookup"><span data-stu-id="d5568-116">The code calls the `acquireTokenByCode` function to exchange the authorization code for an access token.</span></span> <span data-ttu-id="d5568-117">获取令牌后，它会使用临时错误值中的访问令牌重定向回主页面。</span><span class="sxs-lookup"><span data-stu-id="d5568-117">Once the token is obtained, it redirects back to the home page with the access token in the temporary error value.</span></span> <span data-ttu-id="d5568-118">在继续操作之前，我们将使用此操作来验证登录是否正常工作。</span><span class="sxs-lookup"><span data-stu-id="d5568-118">We'll use this to verify that our sign-in is working before moving on.</span></span> <span data-ttu-id="d5568-119">在测试之前，我们需要将 Express 应用配置为使用 **/routes/auth.js** 中的新路由器。</span><span class="sxs-lookup"><span data-stu-id="d5568-119">Before we test, we need to configure the Express app to use the new router from **./routes/auth.js**.</span></span>

    <span data-ttu-id="d5568-120">该 `signout` 方法将注销用户并销毁会话。</span><span class="sxs-lookup"><span data-stu-id="d5568-120">The `signout` method logs the user out and destroys the session.</span></span>

1. <span data-ttu-id="d5568-121">打开 **。/app.js** 并在该行 **前** 插入以下代码 `var app = express();` 。</span><span class="sxs-lookup"><span data-stu-id="d5568-121">Open **./app.js** and insert the following code **before** the `var app = express();` line.</span></span>

    ```javascript
    var authRouter = require('./routes/auth');
    ```

1. <span data-ttu-id="d5568-122">在行 **后** 插入以下代码 `app.use('/', indexRouter);` 。</span><span class="sxs-lookup"><span data-stu-id="d5568-122">Insert the following code **after** the `app.use('/', indexRouter);` line.</span></span>

    ```javascript
    app.use('/auth', authRouter);
    ```

<span data-ttu-id="d5568-123">启动服务器并浏览到 `https://localhost:3000` 。</span><span class="sxs-lookup"><span data-stu-id="d5568-123">Start the server and browse to `https://localhost:3000`.</span></span> <span data-ttu-id="d5568-124">单击 "登录" 按钮，您应会被重定向到 `https://login.microsoftonline.com` 。</span><span class="sxs-lookup"><span data-stu-id="d5568-124">Click the sign-in button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="d5568-125">使用你的 Microsoft 帐户登录，并同意请求的权限。</span><span class="sxs-lookup"><span data-stu-id="d5568-125">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="d5568-126">浏览器重定向到应用程序，并显示令牌。</span><span class="sxs-lookup"><span data-stu-id="d5568-126">The browser redirects to the app, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="d5568-127">获取用户详细信息</span><span class="sxs-lookup"><span data-stu-id="d5568-127">Get user details</span></span>

1. <span data-ttu-id="d5568-128">在名为 **graph.js** 的项目的根目录中创建一个新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="d5568-128">Create a new file in the root of the project named **graph.js** and add the following code.</span></span>

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

    <span data-ttu-id="d5568-129">这将导出 `getUserDetails` 函数，该函数使用 Microsoft GRAPH SDK 调用 `/me` 终结点并返回结果。</span><span class="sxs-lookup"><span data-stu-id="d5568-129">This exports the `getUserDetails` function, which uses the Microsoft Graph SDK to call the `/me` endpoint and return the result.</span></span>

1. <span data-ttu-id="d5568-130">打开 **/routes/auth.js** 并将以下语句添加 `require` 到文件顶部。</span><span class="sxs-lookup"><span data-stu-id="d5568-130">Open **./routes/auth.js** and add the following `require` statements to the top of the file.</span></span>

    ```javascript
    var graph = require('../graph');
    ```

1. <span data-ttu-id="d5568-131">将现有的回调路由替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="d5568-131">Replace the existing callback route with the following code.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/auth.js" id="CallbackSnippet" highlight="13-23":::

    <span data-ttu-id="d5568-132">新代码会在会话中保存用户的帐户 ID，从 Microsoft Graph 获取用户的详细信息，并将其保存在应用的用户存储中。</span><span class="sxs-lookup"><span data-stu-id="d5568-132">The new code saves the user's account ID in the session, gets the user's details from Microsoft Graph, and saves it in the app's user storage.</span></span>

1. <span data-ttu-id="d5568-133">重新启动服务器并完成登录过程。</span><span class="sxs-lookup"><span data-stu-id="d5568-133">Restart the server and go through the sign-in process.</span></span> <span data-ttu-id="d5568-134">您应该最后返回到主页，但 UI 应更改以指示您已登录。</span><span class="sxs-lookup"><span data-stu-id="d5568-134">You should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

    ![登录后主页的屏幕截图](./images/add-aad-auth-01.png)

1. <span data-ttu-id="d5568-136">单击右上角的用户头像以访问 " **注销** " 链接。</span><span class="sxs-lookup"><span data-stu-id="d5568-136">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="d5568-137">单击 " **注销** " 重置会话并返回到主页。</span><span class="sxs-lookup"><span data-stu-id="d5568-137">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

    ![带有 "注销" 链接的下拉菜单的屏幕截图](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a><span data-ttu-id="d5568-139">存储和刷新令牌</span><span class="sxs-lookup"><span data-stu-id="d5568-139">Storing and refreshing tokens</span></span>

<span data-ttu-id="d5568-140">此时，您的应用程序具有访问令牌，该令牌是在 `Authorization` API 调用的标头中发送的。</span><span class="sxs-lookup"><span data-stu-id="d5568-140">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="d5568-141">这是允许应用代表用户访问 Microsoft Graph 的令牌。</span><span class="sxs-lookup"><span data-stu-id="d5568-141">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="d5568-142">但是，此令牌的生存期较短。</span><span class="sxs-lookup"><span data-stu-id="d5568-142">However, this token is short-lived.</span></span> <span data-ttu-id="d5568-143">令牌在发出后会过期一小时。</span><span class="sxs-lookup"><span data-stu-id="d5568-143">The token expires an hour after it is issued.</span></span> <span data-ttu-id="d5568-144">这就是刷新令牌变得有用的地方。</span><span class="sxs-lookup"><span data-stu-id="d5568-144">This is where the refresh token becomes useful.</span></span> <span data-ttu-id="d5568-145">OAuth 规范引入刷新令牌，该令牌允许应用在不要求用户重新登录的情况下请求新的访问令牌。</span><span class="sxs-lookup"><span data-stu-id="d5568-145">The OAuth specification introduces a refresh token, which allows the app to request a new access token without requiring the user to sign in again.</span></span>

<span data-ttu-id="d5568-146">由于应用程序使用的是 msal 的包，因此无需实现任何令牌存储或刷新逻辑。</span><span class="sxs-lookup"><span data-stu-id="d5568-146">Because the app is using the msal-node package, you do not need to implement any token storage or refresh logic.</span></span> <span data-ttu-id="d5568-147">应用使用默认的 msal-内存中的令牌缓存，这足以满足示例应用程序的要求。</span><span class="sxs-lookup"><span data-stu-id="d5568-147">The app uses the default msal-node in-memory token cache, which is sufficient for a sample application.</span></span> <span data-ttu-id="d5568-148">生产应用程序应提供自己的 [缓存插件](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-node/docs/configuration.md) ，以在安全、可靠的存储媒体中序列化令牌缓存。</span><span class="sxs-lookup"><span data-stu-id="d5568-148">Production applications should provide their own [caching plugin](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-node/docs/configuration.md) to serialize the token cache in a secure, reliable storage medium.</span></span>
