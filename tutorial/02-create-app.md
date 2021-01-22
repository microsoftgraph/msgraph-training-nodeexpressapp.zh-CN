<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="d7691-101">在此练习中，你将使用 [Express](http://expressjs.com/) 生成 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="d7691-101">In this exercise you will use [Express](http://expressjs.com/) to build a web app.</span></span>

1. <span data-ttu-id="d7691-102">打开 CLI，导航到你拥有创建文件权限的目录，然后运行以下命令以创建使用 [Handlebars](http://handlebarsjs.com/) 作为呈现引擎的新 Express 应用。</span><span class="sxs-lookup"><span data-stu-id="d7691-102">Open your CLI, navigate to a directory where you have rights to create files, and run the following command to create a new Express app that uses [Handlebars](http://handlebarsjs.com/) as the rendering engine.</span></span>

    ```Shell
    npx express-generator@4.16.1 --hbs graph-tutorial
    ```

    <span data-ttu-id="d7691-103">Express 生成器创建一个称为"Express"应用的新目录并搭建 `graph-tutorial` 其基架。</span><span class="sxs-lookup"><span data-stu-id="d7691-103">The Express generator creates a new directory called `graph-tutorial` and scaffolds an Express app.</span></span>

1. <span data-ttu-id="d7691-104">导航到 `graph-tutorial` 目录并输入以下命令以安装依赖项。</span><span class="sxs-lookup"><span data-stu-id="d7691-104">Navigate to the `graph-tutorial` directory and enter the following command to install dependencies.</span></span>

    ```Shell
    npm install
    ```

1. <span data-ttu-id="d7691-105">运行以下命令以使用报告的漏洞更新节点包。</span><span class="sxs-lookup"><span data-stu-id="d7691-105">Run the following command to update Node packages with reported vulnerabilities.</span></span>

    ```Shell
    npm audit fix
    ```

1. <span data-ttu-id="d7691-106">运行以下命令以更新 Express 的版本和其他依赖项。</span><span class="sxs-lookup"><span data-stu-id="d7691-106">Run the following command to update the version of Express and other dependencies.</span></span>

    ```Shell
    npm install express@4.17.1 http-errors@1.8.0 morgan@1.10.0 debug@4.1.1
    ```

1. <span data-ttu-id="d7691-107">使用以下命令启动本地 Web 服务器。</span><span class="sxs-lookup"><span data-stu-id="d7691-107">Use the following command to start a local web server.</span></span>

    ```Shell
    npm start
    ```

1. <span data-ttu-id="d7691-108">打开浏览器，并导航到 `http://localhost:3000`。</span><span class="sxs-lookup"><span data-stu-id="d7691-108">Open your browser and navigate to `http://localhost:3000`.</span></span> <span data-ttu-id="d7691-109">如果一切正常，你将看到"欢迎使用 Express"消息。</span><span class="sxs-lookup"><span data-stu-id="d7691-109">If everything is working, you will see a "Welcome to Express" message.</span></span> <span data-ttu-id="d7691-110">如果看不到该邮件，请查看 [Express 入门指南](http://expressjs.com/starter/generator.html)。</span><span class="sxs-lookup"><span data-stu-id="d7691-110">If you don't see that message, check the [Express getting started guide](http://expressjs.com/starter/generator.html).</span></span>

## <a name="install-node-packages"></a><span data-ttu-id="d7691-111">安装节点包</span><span class="sxs-lookup"><span data-stu-id="d7691-111">Install Node packages</span></span>

<span data-ttu-id="d7691-112">在继续之前，请安装一些稍后将使用的附加程序包：</span><span class="sxs-lookup"><span data-stu-id="d7691-112">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="d7691-113">[用于从](https://github.com/motdotla/dotenv) .env 文件加载值的 dotenv。</span><span class="sxs-lookup"><span data-stu-id="d7691-113">[dotenv](https://github.com/motdotla/dotenv) for loading values from a .env file.</span></span>
- <span data-ttu-id="d7691-114">[设置](https://github.com/moment/moment/) 日期/时间值的格式的时间。</span><span class="sxs-lookup"><span data-stu-id="d7691-114">[moment](https://github.com/moment/moment/) for formatting date/time values.</span></span>
- <span data-ttu-id="d7691-115">用于将 Windows 时区名称转换为 IANA 时区 ID 的[windows-iana。](https://github.com/rubenillodo/windows-iana)</span><span class="sxs-lookup"><span data-stu-id="d7691-115">[windows-iana](https://github.com/rubenillodo/windows-iana) for translating Windows time zone names to IANA time zone IDs.</span></span>
- <span data-ttu-id="d7691-116">[连接到应用中](https://github.com/jaredhanson/connect-flash) 的闪存错误消息。</span><span class="sxs-lookup"><span data-stu-id="d7691-116">[connect-flash](https://github.com/jaredhanson/connect-flash) to flash error messages in the app.</span></span>
- <span data-ttu-id="d7691-117">[express-session](https://github.com/expressjs/session) 以在内存中服务器端会话中存储值。</span><span class="sxs-lookup"><span data-stu-id="d7691-117">[express-session](https://github.com/expressjs/session) to store values in an in-memory server-side session.</span></span>
- <span data-ttu-id="d7691-118">[express-promise-router](https://github.com/express-promise-router/express-promise-router) 以允许路由处理程序返回 Promise。</span><span class="sxs-lookup"><span data-stu-id="d7691-118">[express-promise-router](https://github.com/express-promise-router/express-promise-router) to allow route handlers to return a Promise.</span></span>
- <span data-ttu-id="d7691-119">[用于分析和验证表单数据的 express-validator。](https://github.com/express-validator/express-validator)</span><span class="sxs-lookup"><span data-stu-id="d7691-119">[express-validator](https://github.com/express-validator/express-validator) for parsing and validating form data.</span></span>
- <span data-ttu-id="d7691-120">[用于验证和](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) 获取访问令牌的 msal 节点。</span><span class="sxs-lookup"><span data-stu-id="d7691-120">[msal-node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) for authenticating and getting access tokens.</span></span>
- <span data-ttu-id="d7691-121">由 msal-node 用来生成 GUID 的[uuid。](https://github.com/uuidjs/uuid)</span><span class="sxs-lookup"><span data-stu-id="d7691-121">[uuid](https://github.com/uuidjs/uuid) used by msal-node to generate GUIDs.</span></span>
- <span data-ttu-id="d7691-122">[用于调用 Microsoft](https://github.com/microsoftgraph/msgraph-sdk-javascript) Graph 的 microsoft-graph-client。</span><span class="sxs-lookup"><span data-stu-id="d7691-122">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>
- <span data-ttu-id="d7691-123">[isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) 以填充 Node 的提取。</span><span class="sxs-lookup"><span data-stu-id="d7691-123">[isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) to polyfill the fetch for Node.</span></span> <span data-ttu-id="d7691-124">库需要提取填充 `microsoft-graph-client` 。</span><span class="sxs-lookup"><span data-stu-id="d7691-124">A fetch polyfill is required for the `microsoft-graph-client` library.</span></span> <span data-ttu-id="d7691-125">有关详细信息，请参阅 [Microsoft Graph JavaScript 客户端](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) 库 Wiki。</span><span class="sxs-lookup"><span data-stu-id="d7691-125">See the [Microsoft Graph JavaScript client library wiki](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) for more information.</span></span>

1. <span data-ttu-id="d7691-126">在 CLI 中运行以下命令。</span><span class="sxs-lookup"><span data-stu-id="d7691-126">Run the following command in your CLI.</span></span>

    ```Shell
    npm install dotenv@8.2.0 moment@2.29.1 moment-timezone@0.5.31 connect-flash@0.1.1 express-session@1.17.1 express-promise-router@4.0.1 isomorphic-fetch@3.0.0
    npm install @azure/msal-node@1.0.0-beta.0 @microsoft/microsoft-graph-client@2.1.1 windows-iana@4.2.1 express-validator@6.6.1 uuid@8.3.1
    ```

    > [!TIP]
    > <span data-ttu-id="d7691-127">在 Windows 上尝试安装这些程序包时，Windows 用户可能会收到以下错误消息。</span><span class="sxs-lookup"><span data-stu-id="d7691-127">Windows users may get the following error message when trying to install these packages on Windows.</span></span>
    >
    > ```Shell
    > gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
    > ```
    >
    > <span data-ttu-id="d7691-128">若要解决此错误，请运行以下命令，以使用安装 VS 生成工具和 Python 的提升 (管理员) 安装 Windows 生成工具。</span><span class="sxs-lookup"><span data-stu-id="d7691-128">To resolve the error, run the following command to install the Windows Build Tools using an elevated (Administrator) terminal window which installs the VS Build Tools and Python.</span></span>
    >
    > ```Shell
    > npm install --global --production windows-build-tools
    > ```

1. <span data-ttu-id="d7691-129">更新应用程序以使用 `connect-flash` 中间 `express-session` 件。</span><span class="sxs-lookup"><span data-stu-id="d7691-129">Update the application to use the `connect-flash` and `express-session` middleware.</span></span> <span data-ttu-id="d7691-130">打开 **./app.js，** 将以下语句 `require` 添加到文件顶部。</span><span class="sxs-lookup"><span data-stu-id="d7691-130">Open **./app.js** and add the following `require` statement to the top of the file.</span></span>

    ```javascript
    const session = require('express-session');
    const flash = require('connect-flash');
    const msal = require('@azure/msal-node');
    ```

1. <span data-ttu-id="d7691-131">紧接在行后添加以下 `var app = express();` 代码。</span><span class="sxs-lookup"><span data-stu-id="d7691-131">Add the following code immediately after the `var app = express();` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SessionSnippet":::

## <a name="design-the-app"></a><span data-ttu-id="d7691-132">设计应用</span><span class="sxs-lookup"><span data-stu-id="d7691-132">Design the app</span></span>

<span data-ttu-id="d7691-133">在此部分中，你将实现应用的 UI。</span><span class="sxs-lookup"><span data-stu-id="d7691-133">In this section you will implement the UI for the app.</span></span>

1. <span data-ttu-id="d7691-134">打开 **./views/layout.bms，** 将全部内容替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="d7691-134">Open **./views/layout.hbs** and replace the entire contents with the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/layout.hbs" id="LayoutSnippet":::

    <span data-ttu-id="d7691-135">此代码为[简单样式设置添加 Bootstrap，](http://getbootstrap.com/)为一些简单图标添加[Font Awesome。](https://fontawesome.com/)</span><span class="sxs-lookup"><span data-stu-id="d7691-135">This code adds [Bootstrap](http://getbootstrap.com/) for simple styling, and [Font Awesome](https://fontawesome.com/) for some simple icons.</span></span> <span data-ttu-id="d7691-136">它还定义具有导航栏的全局布局。</span><span class="sxs-lookup"><span data-stu-id="d7691-136">It also defines a global layout with a nav bar.</span></span>

1. <span data-ttu-id="d7691-137">打开 **./public/stylesheets/style.css，** 并将其全部内容替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="d7691-137">Open **./public/stylesheets/style.css** and replace its entire contents with the following.</span></span>

    :::code language="css" source="../demo/graph-tutorial/public/stylesheets/style.css":::

1. <span data-ttu-id="d7691-138">打开 **./views/index.bms，** 并将其内容替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="d7691-138">Open **./views/index.hbs** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/index.hbs" id="IndexSnippet":::

1. <span data-ttu-id="d7691-139">打开 **./routes/index.js，** 将现有代码替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="d7691-139">Open **./routes/index.js** and replace the existing code with the following.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/index.js" id="IndexRouterSnippet" highlight="6-10":::

1. <span data-ttu-id="d7691-140">保存所有更改并重新启动服务器。</span><span class="sxs-lookup"><span data-stu-id="d7691-140">Save all of your changes and restart the server.</span></span> <span data-ttu-id="d7691-141">现在，应用看起来应该非常不同。</span><span class="sxs-lookup"><span data-stu-id="d7691-141">Now, the app should look very different.</span></span>

    ![重新设计主页的屏幕截图](./images/create-app-01.png)
