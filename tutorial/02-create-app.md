<!-- markdownlint-disable MD002 MD041 -->

在本练习中，将使用[Express](http://expressjs.com/)生成 web 应用程序。 如果尚未安装 Express 生成器，则可以使用以下命令从命令行界面（CLI）安装它。

```Shell
npm install express-generator -g
```

打开您的 CLI，导航到您有权创建文件的目录，然后运行以下命令，以创建一个使用[Handlebars](http://handlebarsjs.com/)作为呈现引擎的新 Express 应用程序。

```Shell
express --hbs graph-tutorial
```

Express 生成器将创建一个名`graph-tutorial`为 "搭建基架" 的新目录，并将其作为 Express 应用程序。 导航到此新目录，并输入以下命令来安装依赖项。

```Shell
npm install
```

命令完成后，使用以下命令启动本地 web 服务器。

```Shell
npm start
```

打开浏览器，并导航到 `http://localhost:3000`。 如果一切正常，你将看到 "欢迎使用快递" 消息。 如果看不到该消息，请查看[Express 入门指南](http://expressjs.com/starter/generator.html)。

在继续操作之前，请先安装您将使用的一些其他宝石：

- 用于从 env 文件加载值的[dotenv](https://github.com/motdotla/dotenv) 。
- 设置日期/时间值格式的[时刻](https://github.com/moment/moment/)。
- 在应用程序中[连接-闪烁](https://github.com/jaredhanson/connect-flash)到闪存错误消息。
- 将值存储在内存中的服务器端会话中的[快速会话](https://github.com/expressjs/session)。
- [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad)身份验证和获取访问令牌。
- [简单-oauth2](https://github.com/lelylan/simple-oauth2)用于令牌管理。
- microsoft graph-用于调用 Microsoft Graph 的[客户端](https://github.com/microsoftgraph/msgraph-sdk-javascript)。

在 CLI 中运行以下命令。

```Shell
npm install dotenv@8.1.0 moment@2.24.0 connect-flash@0.1.1 express-session@1.16.2
npm install passport-azure-ad@4.1.0 simple-oauth2@2.4.0 @microsoft/microsoft-graph-client@1.7.0
```

> [!TIP]
> Windows 用户在尝试在 Windows 上安装这些程序包时可能会收到以下错误消息。
>
> ```Shell
> gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
> ```
>
> 若要解决此错误，请运行以下命令，以使用安装 VS 生成工具和 Python 的提升（管理员）终端窗口来安装 Windows 生成工具。
>
> ```Shell
> npm install --global --production windows-build-tools
> ```

现在`connect-flash` ，更新应用程序以使用和`express-session`中间件。 打开`./app.js`文件，并将以下`require`语句添加到文件顶部。

```js
var session = require('express-session');
var flash = require('connect-flash');
```

紧接着行的`var app = express();`后面添加以下代码。

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

## <a name="design-the-app"></a>设计应用程序

首先，创建应用的全局布局。 打开`./views/layout.hbs`文件，并将整个内容替换为以下代码。

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

此代码添加简单样式的[引导](http://getbootstrap.com/)，并添加一些简单图标的[字体](https://fontawesome.com/)。 它还定义具有导航栏的全局布局。

现在打开`./public/stylesheets/style.css`并将其全部内容替换为以下内容。

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

现在更新默认页面。 打开`./views/index.hbs`文件，并将其内容替换为以下内容。

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

打开`./routes/index.js`文件，并将现有代码替换为以下代码。

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

保存所有更改，然后重新启动服务器。 现在，应用程序看起来应非常不同。

![重新设计的主页的屏幕截图](./images/create-app-01.png)
