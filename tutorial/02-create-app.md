<!-- markdownlint-disable MD002 MD041 -->

在此练习中，你将使用 [Express](http://expressjs.com/) 生成 Web 应用。

1. 打开 CLI，导航到你拥有创建文件权限的目录，然后运行以下命令以创建使用 [Handlebars](http://handlebarsjs.com/) 作为呈现引擎的新 Express 应用。

    ```Shell
    npx express-generator@4.16.1 --hbs graph-tutorial
    ```

    Express 生成器创建一个称为"Express"应用的新目录并搭建 `graph-tutorial` 其基架。

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
    npm install express@4.17.1 http-errors@1.8.0 morgan@1.10.0 debug@4.1.1
    ```

1. 使用以下命令启动本地 Web 服务器。

    ```Shell
    npm start
    ```

1. 打开浏览器，并导航到 `http://localhost:3000`。 如果一切正常，你将看到"欢迎使用 Express"消息。 如果看不到该邮件，请查看 [Express 入门指南](http://expressjs.com/starter/generator.html)。

## <a name="install-node-packages"></a>安装节点包

在继续之前，请安装一些稍后将使用的附加程序包：

- [用于从](https://github.com/motdotla/dotenv) .env 文件加载值的 dotenv。
- [设置](https://github.com/moment/moment/) 日期/时间值的格式的时间。
- 用于将 Windows 时区名称转换为 IANA 时区 ID 的[windows-iana。](https://github.com/rubenillodo/windows-iana)
- [连接到应用中](https://github.com/jaredhanson/connect-flash) 的闪存错误消息。
- [express-session](https://github.com/expressjs/session) 以在内存中服务器端会话中存储值。
- [express-promise-router](https://github.com/express-promise-router/express-promise-router) 以允许路由处理程序返回 Promise。
- [用于分析和验证表单数据的 express-validator。](https://github.com/express-validator/express-validator)
- [用于验证和](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) 获取访问令牌的 msal 节点。
- 由 msal-node 用来生成 GUID 的[uuid。](https://github.com/uuidjs/uuid)
- [用于调用 Microsoft](https://github.com/microsoftgraph/msgraph-sdk-javascript) Graph 的 microsoft-graph-client。
- [isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) 以填充 Node 的提取。 库需要提取填充 `microsoft-graph-client` 。 有关详细信息，请参阅 [Microsoft Graph JavaScript 客户端](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) 库 Wiki。

1. 在 CLI 中运行以下命令。

    ```Shell
    npm install dotenv@8.2.0 moment@2.29.1 moment-timezone@0.5.31 connect-flash@0.1.1 express-session@1.17.1 express-promise-router@4.0.1 isomorphic-fetch@3.0.0
    npm install @azure/msal-node@1.0.0-beta.0 @microsoft/microsoft-graph-client@2.1.1 windows-iana@4.2.1 express-validator@6.6.1 uuid@8.3.1
    ```

    > [!TIP]
    > 在 Windows 上尝试安装这些程序包时，Windows 用户可能会收到以下错误消息。
    >
    > ```Shell
    > gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
    > ```
    >
    > 若要解决此错误，请运行以下命令，以使用安装 VS 生成工具和 Python 的提升 (管理员) 安装 Windows 生成工具。
    >
    > ```Shell
    > npm install --global --production windows-build-tools
    > ```

1. 更新应用程序以使用 `connect-flash` 中间 `express-session` 件。 打开 **./app.js，** 将以下语句 `require` 添加到文件顶部。

    ```javascript
    const session = require('express-session');
    const flash = require('connect-flash');
    const msal = require('@azure/msal-node');
    ```

1. 紧接在行后添加以下 `var app = express();` 代码。

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SessionSnippet":::

## <a name="design-the-app"></a>设计应用

在此部分中，你将实现应用的 UI。

1. 打开 **./views/layout.bms，** 将全部内容替换为以下代码。

    :::code language="html" source="../demo/graph-tutorial/views/layout.hbs" id="LayoutSnippet":::

    此代码为[简单样式设置添加 Bootstrap，](http://getbootstrap.com/)为一些简单图标添加[Font Awesome。](https://fontawesome.com/) 它还定义具有导航栏的全局布局。

1. 打开 **./public/stylesheets/style.css，** 并将其全部内容替换为以下内容。

    :::code language="css" source="../demo/graph-tutorial/public/stylesheets/style.css":::

1. 打开 **./views/index.bms，** 并将其内容替换为以下内容。

    :::code language="html" source="../demo/graph-tutorial/views/index.hbs" id="IndexSnippet":::

1. 打开 **./routes/index.js，** 将现有代码替换为以下内容。

    :::code language="javascript" source="../demo/graph-tutorial/routes/index.js" id="IndexRouterSnippet" highlight="6-10":::

1. 保存所有更改并重新启动服务器。 现在，应用看起来应该非常不同。

    ![重新设计主页的屏幕截图](./images/create-app-01.png)
