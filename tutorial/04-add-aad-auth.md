<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="fa29b-101">在本练习中，你将扩展上一练习中的应用程序，以支持 Azure AD 的身份验证。</span><span class="sxs-lookup"><span data-stu-id="fa29b-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="fa29b-102">若要获取所需的 OAuth 访问令牌以调用 Microsoft Graph，这是必需的。</span><span class="sxs-lookup"><span data-stu-id="fa29b-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="fa29b-103">在此步骤中，将[passport azure ad](https://github.com/AzureAD/passport-azure-ad)库集成到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="fa29b-103">In this step you will integrate the [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad) library into the application.</span></span>

1. <span data-ttu-id="fa29b-104">在应用程序的根目录`.env`中，创建一个名为 file 的新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="fa29b-104">Create a new file named `.env` file in the root of your application, and add the following code.</span></span>

    :::code language="ini" source="../demo/graph-tutorial/.env.example":::

    <span data-ttu-id="fa29b-105">将`YOUR APP ID HERE`替换为应用程序注册门户中的应用程序 ID， `YOUR APP SECRET HERE`并将替换为您生成的密码。</span><span class="sxs-lookup"><span data-stu-id="fa29b-105">Replace `YOUR APP ID HERE` with the application ID from the Application Registration Portal, and replace `YOUR APP SECRET HERE` with the password you generated.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="fa29b-106">如果您使用的是源代码管理（如 git），现在可以从源代码管理中排除`.env`该文件，以避免无意中泄漏您的应用程序 ID 和密码。</span><span class="sxs-lookup"><span data-stu-id="fa29b-106">If you're using source control such as git, now would be a good time to exclude the `.env` file from source control to avoid inadvertently leaking your app ID and password.</span></span>

1. <span data-ttu-id="fa29b-107">打开`./app.js`并将以下行添加到文件顶部，以加载`.env`文件。</span><span class="sxs-lookup"><span data-stu-id="fa29b-107">Open `./app.js` and add the following line to the top of the file to load the `.env` file.</span></span>

    ```javascript
    require('dotenv').config();
    ```

## <a name="implement-sign-in"></a><span data-ttu-id="fa29b-108">实施登录</span><span class="sxs-lookup"><span data-stu-id="fa29b-108">Implement sign-in</span></span>

1. <span data-ttu-id="fa29b-109">找到中`./app.js`的`var indexRouter = require('./routes/index');`行。</span><span class="sxs-lookup"><span data-stu-id="fa29b-109">Locate the line `var indexRouter = require('./routes/index');` in `./app.js`.</span></span> <span data-ttu-id="fa29b-110">**在该行前**插入以下代码。</span><span class="sxs-lookup"><span data-stu-id="fa29b-110">Insert the following code **before** that line.</span></span>

    ```javascript
    var passport = require('passport');
    var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

    // Configure passport

    // In-memory storage of logged-in users
    // For demo purposes only, production apps should store
    // this in a reliable storage
    var users = {};

    // Passport calls serializeUser and deserializeUser to
    // manage users
    passport.serializeUser(function(user, done) {
      // Use the OID property of the user as a key
      users[user.profile.oid] = user;
      done (null, user.profile.oid);
    });

    passport.deserializeUser(function(id, done) {
      done(null, users[id]);
    });

    // Callback function called once the sign-in is complete
    // and an access token has been obtained
    async function signInComplete(iss, sub, profile, accessToken, refreshToken, params, done) {
      if (!profile.oid) {
        return done(new Error("No OID found in user profile."));
      }

      // Save the profile and tokens in user storage
      users[profile.oid] = { profile, accessToken };
      return done(null, users[profile.oid]);
    }

    // Configure OIDC strategy
    passport.use(new OIDCStrategy(
      {
        identityMetadata: `${process.env.OAUTH_AUTHORITY}${process.env.OAUTH_ID_METADATA}`,
        clientID: process.env.OAUTH_APP_ID,
        responseType: 'code id_token',
        responseMode: 'form_post',
        redirectUrl: process.env.OAUTH_REDIRECT_URI,
        allowHttpForRedirectUrl: true,
        clientSecret: process.env.OAUTH_APP_PASSWORD,
        validateIssuer: false,
        passReqToCallback: false,
        scope: process.env.OAUTH_SCOPES.split(' ')
      },
      signInComplete
    ));
    ```

    <span data-ttu-id="fa29b-111">此代码会将[Passport .js](http://www.passportjs.org/)库初始化为使用`passport-azure-ad`库，并使用应用程序的应用 ID 和密码对其进行配置。</span><span class="sxs-lookup"><span data-stu-id="fa29b-111">This code initializes the [Passport.js](http://www.passportjs.org/) library to use the `passport-azure-ad` library, and configures it with the app ID and password for the app.</span></span>

1. <span data-ttu-id="fa29b-112">找到中`./app.js`的`app.use('/', indexRouter);`行。</span><span class="sxs-lookup"><span data-stu-id="fa29b-112">Locate the line `app.use('/', indexRouter);` in `./app.js`.</span></span> <span data-ttu-id="fa29b-113">**在该行前**插入以下代码。</span><span class="sxs-lookup"><span data-stu-id="fa29b-113">Insert the following code **before** that line.</span></span>

    ```javascript
    // Initialize passport
    app.use(passport.initialize());
    app.use(passport.session());
    ```

1. <span data-ttu-id="fa29b-114">在名为`./routes` `auth.js`的目录中创建一个新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="fa29b-114">Create a new file in the `./routes` directory named `auth.js` and add the following code.</span></span>

    ```javascript
    var express = require('express');
    var passport = require('passport');
    var router = express.Router();

    /* GET auth callback. */
    router.get('/signin',
      function  (req, res, next) {
        passport.authenticate('azuread-openidconnect',
          {
            response: res,
            prompt: 'login',
            failureRedirect: '/',
            failureFlash: true,
            successRedirect: '/'
          }
        )(req,res,next);
      }
    );

    router.post('/callback',
      function(req, res, next) {
        passport.authenticate('azuread-openidconnect',
          {
            response: res,
            failureRedirect: '/',
            failureFlash: true
          }
        )(req,res,next);
      },
      function(req, res) {
        // TEMPORARY!
        // Flash the access token for testing purposes
        req.flash('error_msg', {message: 'Access token', debug: req.user.accessToken});
        res.redirect('/');
      }
    );

    router.get('/signout',
      function(req, res) {
        req.session.destroy(function(err) {
          req.logout();
          res.redirect('/');
        });
      }
    );

    module.exports = router;
    ```

    <span data-ttu-id="fa29b-115">这将定义具有三个路由的`signin`路由器`callback`：、 `signout`和。</span><span class="sxs-lookup"><span data-stu-id="fa29b-115">This defines a router with three routes: `signin`, `callback`, and `signout`.</span></span>

    <span data-ttu-id="fa29b-116">`signin`路由将调用`passport.authenticate`方法，从而导致应用重定向到 Azure 登录页面。</span><span class="sxs-lookup"><span data-stu-id="fa29b-116">The `signin` route calls the `passport.authenticate` method, causing the app to redirect to the Azure login page.</span></span>

    <span data-ttu-id="fa29b-117">`callback`路由是在登录完成后 Azure 重定向的位置。</span><span class="sxs-lookup"><span data-stu-id="fa29b-117">The `callback` route is where Azure redirects after the signin is complete.</span></span> <span data-ttu-id="fa29b-118">代码再次调用该`passport.authenticate`方法，从而导致`passport-azure-ad`策略请求访问令牌。</span><span class="sxs-lookup"><span data-stu-id="fa29b-118">The code calls the `passport.authenticate` method again, causing the `passport-azure-ad` strategy to request an access token.</span></span> <span data-ttu-id="fa29b-119">获取令牌后，将调用下一个处理程序，该处理程序将重定向回包含临时错误值中的访问令牌的主页。</span><span class="sxs-lookup"><span data-stu-id="fa29b-119">Once the token is obtained, the next handler is called, which redirects back to the home page with the access token in the temporary error value.</span></span> <span data-ttu-id="fa29b-120">在继续操作之前，我们将使用此操作来验证登录是否正常工作。</span><span class="sxs-lookup"><span data-stu-id="fa29b-120">We'll use this to verify that our sign-in is working before moving on.</span></span> <span data-ttu-id="fa29b-121">在测试之前，我们需要将 Express 应用配置为使用新的路由器`./routes/auth.js`。</span><span class="sxs-lookup"><span data-stu-id="fa29b-121">Before we test, we need to configure the Express app to use the new router from `./routes/auth.js`.</span></span>

    <span data-ttu-id="fa29b-122">该`signout`方法将注销用户并销毁会话。</span><span class="sxs-lookup"><span data-stu-id="fa29b-122">The `signout` method logs the user out and destroys the session.</span></span>

1. <span data-ttu-id="fa29b-123">打开`./app.js`并插入以下代码， **before**并将`var app = express();`其插入到行前面。</span><span class="sxs-lookup"><span data-stu-id="fa29b-123">Open `./app.js` and insert the following code **before** the `var app = express();` line.</span></span>

    ```javascript
    var authRouter = require('./routes/auth');
    ```

1. <span data-ttu-id="fa29b-124">在`app.use('/', indexRouter);`行**后**插入以下代码。</span><span class="sxs-lookup"><span data-stu-id="fa29b-124">Insert the following code **after** the `app.use('/', indexRouter);` line.</span></span>

    ```javascript
    app.use('/auth', authRouter);
    ```

<span data-ttu-id="fa29b-125">启动服务器并浏览到`https://localhost:3000`。</span><span class="sxs-lookup"><span data-stu-id="fa29b-125">Start the server and browse to `https://localhost:3000`.</span></span> <span data-ttu-id="fa29b-126">单击 "登录" 按钮，您应会被重定向`https://login.microsoftonline.com`到。</span><span class="sxs-lookup"><span data-stu-id="fa29b-126">Click the sign-in button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="fa29b-127">使用你的 Microsoft 帐户登录，并同意请求的权限。</span><span class="sxs-lookup"><span data-stu-id="fa29b-127">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="fa29b-128">浏览器重定向到应用程序，并显示令牌。</span><span class="sxs-lookup"><span data-stu-id="fa29b-128">The browser redirects to the app, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="fa29b-129">获取用户详细信息</span><span class="sxs-lookup"><span data-stu-id="fa29b-129">Get user details</span></span>

1. <span data-ttu-id="fa29b-130">在名为`graph.js`的项目的根目录中创建一个新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="fa29b-130">Create a new file in the root of the project named `graph.js` and add the following code.</span></span>

    ```javascript
    var graph = require('@microsoft/microsoft-graph-client');
    require('isomorphic-fetch');

    module.exports = {
      getUserDetails: async function(accessToken) {
        const client = getAuthenticatedClient(accessToken);

        const user = await client.api('/me').get();
        return user;
      }
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

    <span data-ttu-id="fa29b-131">这将`getUserDetails`导出函数，该函数使用 MICROSOFT Graph SDK 调用`/me`终结点并返回结果。</span><span class="sxs-lookup"><span data-stu-id="fa29b-131">This exports the `getUserDetails` function, which uses the Microsoft Graph SDK to call the `/me` endpoint and return the result.</span></span>

1. <span data-ttu-id="fa29b-132">打开`/app.js`并将以下`require`语句添加到文件顶部。</span><span class="sxs-lookup"><span data-stu-id="fa29b-132">Open `/app.js` and add the following `require` statements to the top of the file.</span></span>

    ```javascript
    var graph = require('./graph');
    ```

1. <span data-ttu-id="fa29b-133">将现有的 `signInComplete` 函数替换成以下代码。</span><span class="sxs-lookup"><span data-stu-id="fa29b-133">Replace the existing `signInComplete` function with the following code.</span></span>

    ```javascript
    async function signInComplete(iss, sub, profile, accessToken, refreshToken, params, done) {
      if (!profile.oid) {
        return done(new Error("No OID found in user profile."));
      }

      try{
        const user = await graph.getUserDetails(accessToken);

        if (user) {
          // Add properties to profile
          profile['email'] = user.mail ? user.mail : user.userPrincipalName;
        }
      } catch (err) {
        return done(err);
      }

      // Save the profile and tokens in user storage
      users[profile.oid] = { profile, accessToken };
      return done(null, users[profile.oid]);
    }
    ```

    <span data-ttu-id="fa29b-134">新代码会使用 Microsoft `profile` Graph 返回的数据更新由`email` Passport 提供的属性，以添加属性。</span><span class="sxs-lookup"><span data-stu-id="fa29b-134">The new code updates the `profile` provided by Passport to add an `email` property, using the data returned by Microsoft Graph.</span></span>

1. <span data-ttu-id="fa29b-135">**在** `app.use(passport.session());`行后面添加以下项。</span><span class="sxs-lookup"><span data-stu-id="fa29b-135">Add the following **after** the `app.use(passport.session());` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="AddProfileSnippet":::

    <span data-ttu-id="fa29b-136">此代码将用户配置文件加载到`locals`响应的属性中。</span><span class="sxs-lookup"><span data-stu-id="fa29b-136">This code loads the user profile into the `locals` property of the response.</span></span> <span data-ttu-id="fa29b-137">这将使其可用于应用程序中的所有视图。</span><span class="sxs-lookup"><span data-stu-id="fa29b-137">This will make it available to all of the views in the app.</span></span>

## <a name="storing-the-tokens"></a><span data-ttu-id="fa29b-138">存储令牌</span><span class="sxs-lookup"><span data-stu-id="fa29b-138">Storing the tokens</span></span>

<span data-ttu-id="fa29b-139">现在，您可以获取令牌，现在是时候实现将它们存储在应用程序中的方法了。</span><span class="sxs-lookup"><span data-stu-id="fa29b-139">Now that you can get tokens, it's time to implement a way to store them in the app.</span></span> <span data-ttu-id="fa29b-140">目前，应用程序将原始访问令牌存储在内存中的用户存储中。</span><span class="sxs-lookup"><span data-stu-id="fa29b-140">Currently, the app is storing the raw access token in the in-memory user storage.</span></span> <span data-ttu-id="fa29b-141">由于这是一个示例应用程序，因此为简单起见，你将继续将其存储在此处。</span><span class="sxs-lookup"><span data-stu-id="fa29b-141">Since this is a sample app, for simplicity's sake, you'll continue to store them there.</span></span> <span data-ttu-id="fa29b-142">实际应用程序将使用更可靠的安全存储解决方案，就像数据库一样。</span><span class="sxs-lookup"><span data-stu-id="fa29b-142">A real-world app would use a more reliable secure storage solution, like a database.</span></span>

<span data-ttu-id="fa29b-143">但是，仅存储访问令牌不允许您检查过期或刷新令牌。</span><span class="sxs-lookup"><span data-stu-id="fa29b-143">However, storing just the access token doesn't allow you to check expiration or refresh the token.</span></span> <span data-ttu-id="fa29b-144">若要启用此目的，请更新示例以包装`AccessToken`对象中的对象和`simple-oauth2`库中的标记。</span><span class="sxs-lookup"><span data-stu-id="fa29b-144">In order to enable that, update the sample to wrap the tokens in an `AccessToken` object from the `simple-oauth2` library.</span></span>

1. <span data-ttu-id="fa29b-145">打开`./app.js`并在`signInComplete`函数**前面**添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="fa29b-145">Open `./app.js` and add the following code **before** the `signInComplete` function.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="ConfigureOAuth2Snippet":::

1. <span data-ttu-id="fa29b-146">将现有的 `signInComplete` 函数替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="fa29b-146">Replace the existing `signInComplete` function with the following.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SignInCompleteSnippet" highlight="17-18, 21":::

1. <span data-ttu-id="fa29b-147">将中的现有回调路由`./routes/auth.js`替换为以下项。</span><span class="sxs-lookup"><span data-stu-id="fa29b-147">Replace the existing callback route in  `./routes/auth.js` with the following.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/auth.js" id="CallbackRouteSnippet" highlight="17-18":::

1. <span data-ttu-id="fa29b-148">重新启动服务器并完成登录过程。</span><span class="sxs-lookup"><span data-stu-id="fa29b-148">Restart the server and go through the sign-in process.</span></span> <span data-ttu-id="fa29b-149">您应该最后返回到主页，但 UI 应更改以指示您已登录。</span><span class="sxs-lookup"><span data-stu-id="fa29b-149">You should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

    ![登录后主页的屏幕截图](./images/add-aad-auth-01.png)

1. <span data-ttu-id="fa29b-151">单击右上角的用户头像以访问 "**注销**" 链接。</span><span class="sxs-lookup"><span data-stu-id="fa29b-151">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="fa29b-152">单击 "**注销**" 重置会话并返回到主页。</span><span class="sxs-lookup"><span data-stu-id="fa29b-152">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

    ![带有 "注销" 链接的下拉菜单的屏幕截图](./images/add-aad-auth-02.png)

## <a name="refreshing-tokens"></a><span data-ttu-id="fa29b-154">刷新令牌</span><span class="sxs-lookup"><span data-stu-id="fa29b-154">Refreshing tokens</span></span>

<span data-ttu-id="fa29b-155">此时，您的应用程序具有访问令牌，该令牌是在 API `Authorization`调用的标头中发送的。</span><span class="sxs-lookup"><span data-stu-id="fa29b-155">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="fa29b-156">这是允许应用代表用户访问 Microsoft Graph 的令牌。</span><span class="sxs-lookup"><span data-stu-id="fa29b-156">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="fa29b-157">但是，此令牌的生存期较短。</span><span class="sxs-lookup"><span data-stu-id="fa29b-157">However, this token is short-lived.</span></span> <span data-ttu-id="fa29b-158">令牌在发出后会过期一小时。</span><span class="sxs-lookup"><span data-stu-id="fa29b-158">The token expires an hour after it is issued.</span></span> <span data-ttu-id="fa29b-159">这就是刷新令牌变得有用的地方。</span><span class="sxs-lookup"><span data-stu-id="fa29b-159">This is where the refresh token becomes useful.</span></span> <span data-ttu-id="fa29b-160">刷新令牌允许应用在不要求用户重新登录的情况下请求新的访问令牌。</span><span class="sxs-lookup"><span data-stu-id="fa29b-160">The refresh token allows the app to request a new access token without requiring the user to sign in again.</span></span>

1. <span data-ttu-id="fa29b-161">在名`tokens.js`为的项目的根目录中创建一个新文件，以保留令牌管理函数。</span><span class="sxs-lookup"><span data-stu-id="fa29b-161">Create a new file in the root of the project named `tokens.js` to hold token management functions.</span></span> <span data-ttu-id="fa29b-162">添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="fa29b-162">Add the following code.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/tokens.js" id="TokensSnippet":::

<span data-ttu-id="fa29b-163">此方法首先检查访问令牌是否已过期或接近即将过期。</span><span class="sxs-lookup"><span data-stu-id="fa29b-163">This method first checks if the access token is expired or close to expiring.</span></span> <span data-ttu-id="fa29b-164">如果是，则它使用刷新令牌获取新令牌，然后更新缓存并返回新的访问令牌。</span><span class="sxs-lookup"><span data-stu-id="fa29b-164">If it is, then it uses the refresh token to get new tokens, then updates the cache and returns the new access token.</span></span> <span data-ttu-id="fa29b-165">只要您需要从存储区中获取访问令牌，就可以使用此方法。</span><span class="sxs-lookup"><span data-stu-id="fa29b-165">You'll use this method whenever you need to get the access token out of storage.</span></span>
