<!-- markdownlint-disable MD002 MD041 -->

在本练习中，将使用 [Express](http://expressjs.com/) 生成 web 应用程序。

1. 打开您的 CLI，导航到您有权创建文件的目录，然后运行以下命令，以创建一个使用 [Handlebars](http://handlebarsjs.com/) 作为呈现引擎的新 Express 应用程序。

    ```Shell
    npx express-generator@4.16.1 --hbs graph-tutorial
    ```

    Express 生成器将创建一个名为 " `graph-tutorial` 搭建基架" 的新目录，并将其作为 Express 应用程序。

1. 导航到该 `graph-tutorial` 目录，然后输入以下命令以安装依赖项。

    ```Shell
    npm install
    ```

1. 运行以下命令以使用报告的漏洞更新节点程序包。

    ```Shell
    npm audit fix
    ```

1. 运行以下命令以更新 Express 和其他依赖项的版本。

    ```Shell
    npm install express@4.17.1 http-errors@1.8.0 morgan@1.10.0 debug@4.1.1
    ```

1. 使用以下命令启动本地 web 服务器。

    ```Shell
    npm start
    ```

1. 打开浏览器，并导航到 `http://localhost:3000`。 如果一切正常，你将看到 "欢迎使用快递" 消息。 如果看不到该消息，请查看 [Express 入门指南](http://expressjs.com/starter/generator.html)。

## <a name="install-node-packages"></a>安装节点包

在继续操作之前，请先安装将使用的其他一些程序包：

- 用于从 env 文件加载值的[dotenv](https://github.com/motdotla/dotenv) 。
- 设置日期/时间值格式的[时刻](https://github.com/moment/moment/)。
- 用于将 Windows 时区名称转换为 IANA 时区 Id 的[windows-iana](https://github.com/rubenillodo/windows-iana) 。
- 在应用程序中[连接-闪烁](https://github.com/jaredhanson/connect-flash)到闪存错误消息。
- 将值存储在内存中的服务器端会话中的[快速会话](https://github.com/expressjs/session)。
- [表示](https://github.com/express-promise-router/express-promise-router) 允许路由处理程序返回承诺的路由器。
- 用于分析和验证表单数据的[快速验证](https://github.com/express-validator/express-validator)程序。
- [msal-](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) 用于验证和获取访问令牌的节点。
- 由 msal-节点用于生成 Guid 的[uuid](https://github.com/uuidjs/uuid) 。
- microsoft graph-用于调用 Microsoft Graph 的[客户端](https://github.com/microsoftgraph/msgraph-sdk-javascript)。
- [isomorphic-](https://github.com/matthew-andrews/isomorphic-fetch) polyfill 获取对节点的提取。 库需要获取 polyfill `microsoft-graph-client` 。 有关详细信息，请参阅 [Microsoft Graph JavaScript 客户端库 wiki](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) 。

1. 在 CLI 中运行以下命令。

    ```Shell
    npm install dotenv@8.2.0 moment@2.29.1 moment-timezone@0.5.31 connect-flash@0.1.1 express-session@1.17.1 isomorphic-fetch@3.0.0
    npm install @azure/msal-node@1.0.0-beta.0 @microsoft/microsoft-graph-client@2.1.1 windows-iana@4.2.1 express-validator@6.6.1 uuid@8.3.1
    ```

    > [!TIP]
    > Windows 用户在尝试在 Windows 上安装这些程序包时可能会收到以下错误消息。
    >
    > ```Shell
    > gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
    > ```
    >
    > 若要解决此错误，请运行以下命令，以使用提升的 (管理员) 终端窗口（安装 VS 生成工具和 Python）安装 Windows 生成工具。
    >
    > ```Shell
    > npm install --global --production windows-build-tools
    > ```

1. 更新应用程序以使用 `connect-flash` 和 `express-session` 中间件。 打开 **。/app.js** 并将以下 `require` 语句添加到文件顶部。

    ```javascript
    const session = require('express-session');
    const flash = require('connect-flash');
    const msal = require('@azure/msal-node');
    ```

1. 紧接着行的后面添加以下代码 `var app = express();` 。

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SessionSnippet":::

## <a name="design-the-app"></a>设计应用程序

在本节中，您将实现应用程序的 UI。

1. 打开 **/views/layout.hbs** ，并使用以下代码替换整个内容。

    :::code language="html" source="../demo/graph-tutorial/views/layout.hbs" id="LayoutSnippet":::

    此代码添加简单样式的 [引导](http://getbootstrap.com/) ，并添加一些简单图标的 [字体](https://fontawesome.com/) 。 它还定义具有导航栏的全局布局。

1. 打开 **/public/stylesheets/style.css** ，并将其全部内容替换为以下内容。

    :::code language="css" source="../demo/graph-tutorial/public/stylesheets/style.css":::

1. 打开 **/views/index.hbs** ，并将其内容替换为以下内容。

    :::code language="html" source="../demo/graph-tutorial/views/index.hbs" id="IndexSnippet":::

1. 打开 **/routes/index.js** 并将现有代码替换为以下代码。

    :::code language="javascript" source="../demo/graph-tutorial/routes/index.js" id="IndexRouterSnippet" highlight="6-10":::

1. 保存所有更改，然后重新启动服务器。 现在，应用程序看起来应非常不同。

    ![重新设计的主页的屏幕截图](./images/create-app-01.png)
