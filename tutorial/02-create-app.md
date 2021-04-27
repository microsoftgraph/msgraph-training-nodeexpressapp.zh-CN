<!-- markdownlint-disable MD002 MD041 -->

在此练习中，你将使用 [Express](http://expressjs.com/) 生成 Web 应用。

1. 打开 CLI，导航到你拥有创建文件权限的目录，然后运行以下命令创建一个新的 Express 应用，该应用使用 [Handlebars](http://handlebarsjs.com/) 作为呈现引擎。

    ```Shell
    npx express-generator@4.16.1 --hbs graph-tutorial
    ```

    Express 生成器创建名为 的新目录，并 `graph-tutorial` 搭建 Express 应用基架。

1. 导航到 `graph-tutorial` 目录并输入以下命令以安装依赖项。

    ```Shell
    npm install
    ```

1. 运行以下命令以使用报告的漏洞更新节点包。

    ```Shell
    npm audit fix
    ```

1. 运行以下命令以更新 Express 的版本和其他依赖项。

    ```Shell
    npm install express@4.17.1 http-errors@1.8.0 morgan@1.10.0 debug@4.3.1 hbs@4.1.2
    ```

1. 使用以下命令启动本地 Web 服务器。

    ```Shell
    npm start
    ```

1. 打开浏览器，并导航到 `http://localhost:3000`。 如果一切正常，你将看到"欢迎使用 Express"消息。 如果看不到该邮件，请查看 [快速入门指南](http://expressjs.com/starter/generator.html)。

## <a name="install-node-packages"></a>安装节点包

在继续之前，请安装一些你稍后将使用的其他程序包：

- 用于从 .env 文件加载值的[dotenv。](https://github.com/motdotla/dotenv)
- 用于设置日期/时间值格式的[date-fns。](https://github.com/date-fns/date-fns)
- 用于将时区名称Windows IANA 时区 ID 的[windows-iana。](https://github.com/rubenillodo/windows-iana)
- [将闪存](https://github.com/jaredhanson/connect-flash) 连接到应用中的闪存错误消息。
- [express-session，](https://github.com/expressjs/session) 用于存储内存中服务器端会话中的值。
- [express-promise-router，](https://github.com/express-promise-router/express-promise-router) 以允许路由处理程序返回 Promise。
- [用于分析和验证表单数据的 express-validator。](https://github.com/express-validator/express-validator)
- [用于验证和](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) 获取访问令牌的 msal 节点。
- [用于调用](https://github.com/microsoftgraph/msgraph-sdk-javascript)Microsoft Graph 的 microsoft-graph-client。
- [isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) 以填充 Node 的提取。 库需要提取填充 `microsoft-graph-client` 。 有关详细信息，[请参阅 Microsoft Graph JavaScript](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required)客户端库 Wiki。

1. 在 CLI 中运行以下命令。

    ```Shell
    npm install dotenv@8.2.0 date-fns@2.21.1 date-fns-tz@1.1.4 connect-flash@0.1.1 express-validator@6.10.0
    npm install express-session@1.17.1 express-promise-router@4.1.0 isomorphic-fetch@3.0.0
    npm install @azure/msal-node@1.0.2 @microsoft/microsoft-graph-client@2.2.1 windows-iana@5.0.1
    ```

    > [!TIP]
    > Windows尝试在客户端安装这些程序包时，用户可能会Windows。
    >
    > ```Shell
    > gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
    > ```
    >
    > 若要解决此错误，请运行以下命令，使用提升的 (Administrator) 终端窗口安装 Windows Build Tools，该窗口将安装 VS Build Tools 和 Python。
    >
    > ```Shell
    > npm install --global --production windows-build-tools
    > ```

1. 更新应用程序以使用 `connect-flash` 和 `express-session` 中间件。 打开 **./app.js，** 将以下语句 `require` 添加到文件顶部。

    ```javascript
    const session = require('express-session');
    const flash = require('connect-flash');
    const msal = require('@azure/msal-node');
    ```

1. 紧接在 行后添加以下 `var app = express();` 代码。

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SessionSnippet":::

## <a name="design-the-app"></a>设计应用

在此部分中，你将实现应用的 UI。

1. 打开 **./views/layout.bms，** 将全部内容替换为以下代码。

    :::code language="html" source="../demo/graph-tutorial/views/layout.hbs" id="LayoutSnippet":::

    此代码添加了 [Bootstrap](http://getbootstrap.com/) 用于简单样式，[Font Awesome](https://fontawesome.com/) 用于简单图标。 它还定义具有导航栏的全局布局。

1. 打开 **./public/stylesheets/style.css，** 并将其全部内容替换为以下内容。

    :::code language="css" source="../demo/graph-tutorial/public/stylesheets/style.css":::

1. 打开 **./views/index.bms，** 并将其内容替换为以下内容。

    :::code language="html" source="../demo/graph-tutorial/views/index.hbs" id="IndexSnippet":::

1. 打开 **"./routes/index.js"，** 将现有代码替换为以下内容。

    :::code language="javascript" source="../demo/graph-tutorial/routes/index.js" id="IndexRouterSnippet" highlight="6-10":::

1. 保存全部更改，重新启动服务器。 现在，应用看起来应该非常不同。

    ![重新设计的主页的屏幕截图](./images/create-app-01.png)
