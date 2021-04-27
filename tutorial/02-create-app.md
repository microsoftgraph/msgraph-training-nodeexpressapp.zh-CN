<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="b2ab6-101">在此练习中，你将使用 [Express](http://expressjs.com/) 生成 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-101">In this exercise you will use [Express](http://expressjs.com/) to build a web app.</span></span>

1. <span data-ttu-id="b2ab6-102">打开 CLI，导航到你拥有创建文件权限的目录，然后运行以下命令创建一个新的 Express 应用，该应用使用 [Handlebars](http://handlebarsjs.com/) 作为呈现引擎。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-102">Open your CLI, navigate to a directory where you have rights to create files, and run the following command to create a new Express app that uses [Handlebars](http://handlebarsjs.com/) as the rendering engine.</span></span>

    ```Shell
    npx express-generator@4.16.1 --hbs graph-tutorial
    ```

    <span data-ttu-id="b2ab6-103">Express 生成器创建名为 的新目录，并 `graph-tutorial` 搭建 Express 应用基架。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-103">The Express generator creates a new directory called `graph-tutorial` and scaffolds an Express app.</span></span>

1. <span data-ttu-id="b2ab6-104">导航到 `graph-tutorial` 目录并输入以下命令以安装依赖项。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-104">Navigate to the `graph-tutorial` directory and enter the following command to install dependencies.</span></span>

    ```Shell
    npm install
    ```

1. <span data-ttu-id="b2ab6-105">运行以下命令以使用报告的漏洞更新节点包。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-105">Run the following command to update Node packages with reported vulnerabilities.</span></span>

    ```Shell
    npm audit fix
    ```

1. <span data-ttu-id="b2ab6-106">运行以下命令以更新 Express 的版本和其他依赖项。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-106">Run the following command to update the version of Express and other dependencies.</span></span>

    ```Shell
    npm install express@4.17.1 http-errors@1.8.0 morgan@1.10.0 debug@4.3.1 hbs@4.1.2
    ```

1. <span data-ttu-id="b2ab6-107">使用以下命令启动本地 Web 服务器。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-107">Use the following command to start a local web server.</span></span>

    ```Shell
    npm start
    ```

1. <span data-ttu-id="b2ab6-108">打开浏览器，并导航到 `http://localhost:3000`。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-108">Open your browser and navigate to `http://localhost:3000`.</span></span> <span data-ttu-id="b2ab6-109">如果一切正常，你将看到"欢迎使用 Express"消息。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-109">If everything is working, you will see a "Welcome to Express" message.</span></span> <span data-ttu-id="b2ab6-110">如果看不到该邮件，请查看 [快速入门指南](http://expressjs.com/starter/generator.html)。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-110">If you don't see that message, check the [Express getting started guide](http://expressjs.com/starter/generator.html).</span></span>

## <a name="install-node-packages"></a><span data-ttu-id="b2ab6-111">安装节点包</span><span class="sxs-lookup"><span data-stu-id="b2ab6-111">Install Node packages</span></span>

<span data-ttu-id="b2ab6-112">在继续之前，请安装一些你稍后将使用的其他程序包：</span><span class="sxs-lookup"><span data-stu-id="b2ab6-112">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="b2ab6-113">用于从 .env 文件加载值的[dotenv。](https://github.com/motdotla/dotenv)</span><span class="sxs-lookup"><span data-stu-id="b2ab6-113">[dotenv](https://github.com/motdotla/dotenv) for loading values from a .env file.</span></span>
- <span data-ttu-id="b2ab6-114">用于设置日期/时间值格式的[date-fns。](https://github.com/date-fns/date-fns)</span><span class="sxs-lookup"><span data-stu-id="b2ab6-114">[date-fns](https://github.com/date-fns/date-fns) for formatting date/time values.</span></span>
- <span data-ttu-id="b2ab6-115">用于将时区名称Windows IANA 时区 ID 的[windows-iana。](https://github.com/rubenillodo/windows-iana)</span><span class="sxs-lookup"><span data-stu-id="b2ab6-115">[windows-iana](https://github.com/rubenillodo/windows-iana) for translating Windows time zone names to IANA time zone IDs.</span></span>
- <span data-ttu-id="b2ab6-116">[将闪存](https://github.com/jaredhanson/connect-flash) 连接到应用中的闪存错误消息。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-116">[connect-flash](https://github.com/jaredhanson/connect-flash) to flash error messages in the app.</span></span>
- <span data-ttu-id="b2ab6-117">[express-session，](https://github.com/expressjs/session) 用于存储内存中服务器端会话中的值。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-117">[express-session](https://github.com/expressjs/session) to store values in an in-memory server-side session.</span></span>
- <span data-ttu-id="b2ab6-118">[express-promise-router，](https://github.com/express-promise-router/express-promise-router) 以允许路由处理程序返回 Promise。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-118">[express-promise-router](https://github.com/express-promise-router/express-promise-router) to allow route handlers to return a Promise.</span></span>
- <span data-ttu-id="b2ab6-119">[用于分析和验证表单数据的 express-validator。](https://github.com/express-validator/express-validator)</span><span class="sxs-lookup"><span data-stu-id="b2ab6-119">[express-validator](https://github.com/express-validator/express-validator) for parsing and validating form data.</span></span>
- <span data-ttu-id="b2ab6-120">[用于验证和](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) 获取访问令牌的 msal 节点。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-120">[msal-node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) for authenticating and getting access tokens.</span></span>
- <span data-ttu-id="b2ab6-121">[用于调用](https://github.com/microsoftgraph/msgraph-sdk-javascript)Microsoft Graph 的 microsoft-graph-client。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-121">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>
- <span data-ttu-id="b2ab6-122">[isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) 以填充 Node 的提取。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-122">[isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) to polyfill the fetch for Node.</span></span> <span data-ttu-id="b2ab6-123">库需要提取填充 `microsoft-graph-client` 。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-123">A fetch polyfill is required for the `microsoft-graph-client` library.</span></span> <span data-ttu-id="b2ab6-124">有关详细信息，[请参阅 Microsoft Graph JavaScript](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required)客户端库 Wiki。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-124">See the [Microsoft Graph JavaScript client library wiki](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) for more information.</span></span>

1. <span data-ttu-id="b2ab6-125">在 CLI 中运行以下命令。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-125">Run the following command in your CLI.</span></span>

    ```Shell
    npm install dotenv@8.2.0 date-fns@2.21.1 date-fns-tz@1.1.4 connect-flash@0.1.1 express-validator@6.10.0
    npm install express-session@1.17.1 express-promise-router@4.1.0 isomorphic-fetch@3.0.0
    npm install @azure/msal-node@1.0.2 @microsoft/microsoft-graph-client@2.2.1 windows-iana@5.0.1
    ```

    > [!TIP]
    > <span data-ttu-id="b2ab6-126">Windows尝试在客户端安装这些程序包时，用户可能会Windows。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-126">Windows users may get the following error message when trying to install these packages on Windows.</span></span>
    >
    > ```Shell
    > gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
    > ```
    >
    > <span data-ttu-id="b2ab6-127">若要解决此错误，请运行以下命令，使用提升的 (Administrator) 终端窗口安装 Windows Build Tools，该窗口将安装 VS Build Tools 和 Python。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-127">To resolve the error, run the following command to install the Windows Build Tools using an elevated (Administrator) terminal window which installs the VS Build Tools and Python.</span></span>
    >
    > ```Shell
    > npm install --global --production windows-build-tools
    > ```

1. <span data-ttu-id="b2ab6-128">更新应用程序以使用 `connect-flash` 和 `express-session` 中间件。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-128">Update the application to use the `connect-flash` and `express-session` middleware.</span></span> <span data-ttu-id="b2ab6-129">打开 **./app.js，** 将以下语句 `require` 添加到文件顶部。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-129">Open **./app.js** and add the following `require` statement to the top of the file.</span></span>

    ```javascript
    const session = require('express-session');
    const flash = require('connect-flash');
    const msal = require('@azure/msal-node');
    ```

1. <span data-ttu-id="b2ab6-130">紧接在 行后添加以下 `var app = express();` 代码。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-130">Add the following code immediately after the `var app = express();` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SessionSnippet":::

## <a name="design-the-app"></a><span data-ttu-id="b2ab6-131">设计应用</span><span class="sxs-lookup"><span data-stu-id="b2ab6-131">Design the app</span></span>

<span data-ttu-id="b2ab6-132">在此部分中，你将实现应用的 UI。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-132">In this section you will implement the UI for the app.</span></span>

1. <span data-ttu-id="b2ab6-133">打开 **./views/layout.bms，** 将全部内容替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-133">Open **./views/layout.hbs** and replace the entire contents with the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/layout.hbs" id="LayoutSnippet":::

    <span data-ttu-id="b2ab6-134">此代码添加了 [Bootstrap](http://getbootstrap.com/) 用于简单样式，[Font Awesome](https://fontawesome.com/) 用于简单图标。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-134">This code adds [Bootstrap](http://getbootstrap.com/) for simple styling, and [Font Awesome](https://fontawesome.com/) for some simple icons.</span></span> <span data-ttu-id="b2ab6-135">它还定义具有导航栏的全局布局。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-135">It also defines a global layout with a nav bar.</span></span>

1. <span data-ttu-id="b2ab6-136">打开 **./public/stylesheets/style.css，** 并将其全部内容替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-136">Open **./public/stylesheets/style.css** and replace its entire contents with the following.</span></span>

    :::code language="css" source="../demo/graph-tutorial/public/stylesheets/style.css":::

1. <span data-ttu-id="b2ab6-137">打开 **./views/index.bms，** 并将其内容替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-137">Open **./views/index.hbs** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/index.hbs" id="IndexSnippet":::

1. <span data-ttu-id="b2ab6-138">打开 **"./routes/index.js"，** 将现有代码替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-138">Open **./routes/index.js** and replace the existing code with the following.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/index.js" id="IndexRouterSnippet" highlight="6-10":::

1. <span data-ttu-id="b2ab6-139">保存全部更改，重新启动服务器。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-139">Save all of your changes and restart the server.</span></span> <span data-ttu-id="b2ab6-140">现在，应用看起来应该非常不同。</span><span class="sxs-lookup"><span data-stu-id="b2ab6-140">Now, the app should look very different.</span></span>

    ![重新设计的主页的屏幕截图](./images/create-app-01.png)
