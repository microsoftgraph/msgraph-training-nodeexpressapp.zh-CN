<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="6cf8b-101">在本练习中, 将使用[Express](http://expressjs.com/)生成 web 应用程序。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-101">In this exercise you will use [Express](http://expressjs.com/) to build a web app.</span></span> <span data-ttu-id="6cf8b-102">如果尚未安装 Express 生成器, 则可以使用以下命令从命令行界面 (CLI) 安装它。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-102">If you don't already have the Express generator installed, you can install it from your command-line interface (CLI) with the following command.</span></span>

```Shell
npm install express-generator -g
```

<span data-ttu-id="6cf8b-103">打开您的 CLI, 导航到您有权创建文件的目录, 然后运行以下命令, 以创建一个使用[Handlebars](http://handlebarsjs.com/)作为呈现引擎的新 Express 应用程序。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-103">Open your CLI, navigate to a directory where you have rights to create files, and run the following command to create a new Express app that uses [Handlebars](http://handlebarsjs.com/) as the rendering engine.</span></span>

```Shell
express --hbs graph-tutorial
```

<span data-ttu-id="6cf8b-104">Express 生成器将创建一个名`graph-tutorial`为 "搭建基架" 的新目录, 并将其作为 Express 应用程序。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-104">The Express generator creates a new directory called `graph-tutorial` and scaffolds an Express app.</span></span> <span data-ttu-id="6cf8b-105">导航到此新目录, 并输入以下命令来安装依赖项。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-105">Navigate to this new directory and enter the following command to install dependencies.</span></span>

```Shell
npm install
```

<span data-ttu-id="6cf8b-106">命令完成后, 使用以下命令启动本地 web 服务器。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-106">Once that command completes, use the following command to start a local web server.</span></span>

```Shell
npm start
```

<span data-ttu-id="6cf8b-107">打开浏览器，并导航到 `http://localhost:3000`。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-107">Open your browser and navigate to `http://localhost:3000`.</span></span> <span data-ttu-id="6cf8b-108">如果一切正常, 你将看到 "欢迎使用快递" 消息。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-108">If everything is working, you will see a "Welcome to Express" message.</span></span> <span data-ttu-id="6cf8b-109">如果看不到该消息, 请查看[Express 入门指南](http://expressjs.com/starter/generator.html)。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-109">If you don't see that message, check the [Express getting started guide](http://expressjs.com/starter/generator.html).</span></span>

<span data-ttu-id="6cf8b-110">在继续操作之前, 请先安装您将使用的一些其他宝石:</span><span class="sxs-lookup"><span data-stu-id="6cf8b-110">Before moving on, install some additional gems that you will use later:</span></span>

- <span data-ttu-id="6cf8b-111">用于从 env 文件加载值的[dotenv](https://github.com/motdotla/dotenv) 。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-111">[dotenv](https://github.com/motdotla/dotenv) for loading values from a .env file.</span></span>
- <span data-ttu-id="6cf8b-112">设置日期/时间值格式的[时刻](https://github.com/moment/moment/)。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-112">[moment](https://github.com/moment/moment/) for formatting date/time values.</span></span>
- <span data-ttu-id="6cf8b-113">在应用程序中[连接-闪烁](https://github.com/jaredhanson/connect-flash)到闪存错误消息。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-113">[connect-flash](https://github.com/jaredhanson/connect-flash) to flash error messages in the app.</span></span>
- <span data-ttu-id="6cf8b-114">将值存储在内存中的服务器端会话中的[快速会话](https://github.com/expressjs/session)。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-114">[express-session](https://github.com/expressjs/session) to store values in an in-memory server-side session.</span></span>
- <span data-ttu-id="6cf8b-115">[passport-azure-ad](https://github.com/AzureAD/passport-azure-ad)身份验证和获取访问令牌。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-115">[passport-azure-ad](https://github.com/AzureAD/passport-azure-ad) for authenticating and getting access tokens.</span></span>
- <span data-ttu-id="6cf8b-116">[简单-oauth2](https://github.com/lelylan/simple-oauth2)用于令牌管理。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-116">[simple-oauth2](https://github.com/lelylan/simple-oauth2) for token management.</span></span>
- <span data-ttu-id="6cf8b-117">microsoft graph-用于调用 Microsoft Graph 的[客户端](https://github.com/microsoftgraph/msgraph-sdk-javascript)。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-117">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>

<span data-ttu-id="6cf8b-118">在 CLI 中运行以下命令。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-118">Run the following command in your CLI.</span></span>

```Shell
npm install dotenv@6.2.0 moment@2.24.0 connect-flash@0.1.1 express-session@1.15.6
npm install passport-azure-ad@4.0.0 simple-oauth2@2.2.1 @microsoft/microsoft-graph-client@1.5.2
```

> [!TIP]
> <span data-ttu-id="6cf8b-119">Windows 用户在尝试在 Windows 上安装这些程序包时可能会收到以下错误消息。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-119">Windows users may get the following error message when trying to install these packages on Windows.</span></span>
>
> ```Shell
> gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
> ```
>
> <span data-ttu-id="6cf8b-120">若要解决此错误, 请运行以下命令, 以使用安装 VS 生成工具和 Python 的提升 (管理员) 终端窗口来安装 Windows 生成工具。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-120">To resolve the error, run the following command to install the Windows Build Tools using an elevated (Administrator) terminal window which installs the VS Build Tools and Python.</span></span>
>
> ```Shell
> npm install --global --production windows-build-tools
> ```

<span data-ttu-id="6cf8b-121">现在`connect-flash` , 更新应用程序以使用和`express-session`中间件。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-121">Now update the application to use the `connect-flash` and `express-session` middleware.</span></span> <span data-ttu-id="6cf8b-122">打开`./app.js`文件, 并将以下`require`语句添加到文件顶部。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-122">Open the `./app.js` file and add the following `require` statement to the top of the file.</span></span>

```js
var session = require('express-session');
var flash = require('connect-flash');
```

<span data-ttu-id="6cf8b-123">紧接着行的`var app = express();`后面添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-123">Add the following code immediately after the `var app = express();` line.</span></span>

```js
// Session middleware
// NOTE: Uses default in-memory session store, which is not
// suitable for production
app.use(session({
  secret: 'your_secret_value_here',
  resave: false,
  saveUninitialized: false,
  unset: 'destroy'
}));

// Flash middleware
app.use(flash());

// Set up local vars for template layout
app.use(function(req, res, next) {
  // Read any flashed errors and save
  // in the response locals
  res.locals.error = req.flash('error_msg');

  // Check for simple error string and
  // convert to layout's expected format
  var errs = req.flash('error');
  for (var i in errs){
    res.locals.error.push({message: 'An error occurred', debug: errs[i]});
  }

  next();
});
```

## <a name="design-the-app"></a><span data-ttu-id="6cf8b-124">设计应用程序</span><span class="sxs-lookup"><span data-stu-id="6cf8b-124">Design the app</span></span>

<span data-ttu-id="6cf8b-125">首先, 创建应用的全局布局。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-125">Start by creating the global layout for the app.</span></span> <span data-ttu-id="6cf8b-126">打开`./views/layout.hbs`文件, 并将整个内容替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-126">Open the `./views/layout.hbs` file and replace the entire contents with the following code.</span></span>

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Node.js Graph Tutorial</title>

    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.1/css/bootstrap.min.css"
      integrity="sha384-WskhaSGFgHYWDcbwN70/dfYBj47jz9qbsMId/iRN3ewGhXQFZCSftd1LZCfmhktB" crossorigin="anonymous">
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.1.0/css/all.css"
      integrity="sha384-lKuwvrZot6UHsBSfcMvOkWwlCMgc0TaWr+30HWe3a4ltaBwTZhyTEggF5tJv8tbt" crossorigin="anonymous">
    <link rel='stylesheet' href='/stylesheets/style.css' />
    <script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.3/umd/popper.min.js"
      integrity="sha384-ZMP7rVo3mIykV+2+9J3UJ46jBk0WLaUAdn689aCwoqbBJiSnjAK/l8WvCWPIPm49" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.1.1/js/bootstrap.min.js"
      integrity="sha384-smHYKdLADwkXOn1EmN1qk/HfnUcbVRZyYmZ4qpPea6sjB/pTJ0euyQp0Mk8ck+5T" crossorigin="anonymous"></script>
  </head>

  <body>
    <nav class="navbar navbar-expand-md navbar-dark fixed-top bg-dark">
      <div class="container">
        <a href="/" class="navbar-brand">Node.js Graph Tutorial</a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarCollapse"
          aria-controls="navbarCollapse" aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarCollapse">
          <ul class="navbar-nav mr-auto">
            <li class="nav-item">
              <a href="/" class="nav-link{{#if active.home}} active{{/if}}">Home</a>
            </li>
            {{#if user}}
              <li class="nav-item" data-turbolinks="false">
                <a href="/calendar" class="nav-link{{#if active.calendar}} active{{/if}}">Calendar</a>
              </li>
            {{/if}}
          </ul>
          <ul class="navbar-nav justify-content-end">
            <li class="nav-item">
              <a class="nav-link" href="https://developer.microsoft.com/graph/docs/concepts/overview" target="_blank">
                <i class="fas fa-external-link-alt mr-1"></i>Docs
              </a>
            </li>
            {{#if user}}
              <li class="nav-item dropdown">
                <a class="nav-link dropdown-toggle" data-toggle="dropdown" href="#" role="button" aria-haspopup="true"
                  aria-expanded="false">
                  {{#if user.avatar}}
                    <img src="{{ user.avatar }}" class="rounded-circle align-self-center mr-2" style="width: 32px;">
                  {{else}}
                    <i class="far fa-user-circle fa-lg rounded-circle align-self-center mr-2" style="width: 32px;"></i>
                  {{/if}}
                </a>
                <div class="dropdown-menu dropdown-menu-right">
                  <h5 class="dropdown-item-text mb-0">{{ user.displayName }}</h5>
                  <p class="dropdown-item-text text-muted mb-0">{{ user.email }}</p>
                  <div class="dropdown-divider"></div>
                  <a href="/auth/signout" class="dropdown-item">Sign Out</a>
                </div>
              </li>
            {{else}}
              <li class="nav-item">
                <a href="/auth/signin" class="nav-link">Sign In</a>
              </li>
            {{/if}}
          </ul>
        </div>
      </div>
    </nav>
    <main role="main" class="container">
      {{#each error}}
        <div class="alert alert-danger" role="alert">
          <p class="mb-3">{{ this.message }}</p>
          {{#if this.debug }}
            <pre class="alert-pre border bg-light p-2"><code>{{ this.debug }}</code></pre>
          {{/if}}
        </div>
      {{/each}}

      {{{body}}}
    </main>
  </body>
</html>
```

<span data-ttu-id="6cf8b-127">此代码添加简单样式的[引导](http://getbootstrap.com/), 并添加一些简单图标的[字体](https://fontawesome.com/)。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-127">This code adds [Bootstrap](http://getbootstrap.com/) for simple styling, and [Font Awesome](https://fontawesome.com/) for some simple icons.</span></span> <span data-ttu-id="6cf8b-128">它还定义具有导航栏的全局布局。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-128">It also defines a global layout with a nav bar.</span></span>

<span data-ttu-id="6cf8b-129">现在打开`./public/stylesheets/style.css`并将其全部内容替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-129">Now open `./public/stylesheets/style.css` and replace its entire contents with the following.</span></span>

```css
body {
  padding-top: 4.5rem;
}

.alert-pre {
  word-wrap: break-word;
  word-break: break-all;
  white-space: pre-wrap;
}
```

<span data-ttu-id="6cf8b-130">现在更新默认页面。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-130">Now update the default page.</span></span> <span data-ttu-id="6cf8b-131">打开`./views/index.hbs`文件, 并将其内容替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-131">Open the `./views/index.hbs` file and replace its contents with the following.</span></span>

```html
<div class="jumbotron">
  <h1>Node.js Graph Tutorial</h1>
  <p class="lead">This sample app shows how to use the Microsoft Graph API to access Outlook and OneDrive data from Node.js</p>
  {{#if user}}
    <h4>Welcome {{ user.displayName }}!</h4>
    <p>Use the navigation bar at the top of the page to get started.</p>
  {{else}}
    <a href="/auth/signin" class="btn btn-primary btn-large">Click here to sign in</a>
  {{/if}}
</div>
```

<span data-ttu-id="6cf8b-132">打开`./routes/index.js`文件, 并将现有代码替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-132">Open the `./routes/index.js` file and replace the existing code with the following.</span></span>

```js
var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('/', function(req, res, next) {
  let params = {
    active: { home: true }
  };

  res.render('index', params);
});

module.exports = router;
```

<span data-ttu-id="6cf8b-133">保存所有更改, 然后重新启动服务器。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-133">Save all of your changes and restart the server.</span></span> <span data-ttu-id="6cf8b-134">现在, 应用程序看起来应非常不同。</span><span class="sxs-lookup"><span data-stu-id="6cf8b-134">Now, the app should look very different.</span></span>

![重新设计的主页的屏幕截图](./images/create-app-01.png)