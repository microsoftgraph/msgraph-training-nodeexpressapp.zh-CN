<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="f7859-101">在本练习中，将使用 [Express](http://expressjs.com/) 生成 web 应用程序。</span><span class="sxs-lookup"><span data-stu-id="f7859-101">In this exercise you will use [Express](http://expressjs.com/) to build a web app.</span></span>

1. <span data-ttu-id="f7859-102">打开您的 CLI，导航到您有权创建文件的目录，然后运行以下命令，以创建一个使用 [Handlebars](http://handlebarsjs.com/) 作为呈现引擎的新 Express 应用程序。</span><span class="sxs-lookup"><span data-stu-id="f7859-102">Open your CLI, navigate to a directory where you have rights to create files, and run the following command to create a new Express app that uses [Handlebars](http://handlebarsjs.com/) as the rendering engine.</span></span>

    ```Shell
    npx express-generator@4.16.1 --hbs graph-tutorial
    ```

    <span data-ttu-id="f7859-103">Express 生成器将创建一个名为 " `graph-tutorial` 搭建基架" 的新目录，并将其作为 Express 应用程序。</span><span class="sxs-lookup"><span data-stu-id="f7859-103">The Express generator creates a new directory called `graph-tutorial` and scaffolds an Express app.</span></span>

1. <span data-ttu-id="f7859-104">导航到该 `graph-tutorial` 目录，然后输入以下命令以安装依赖项。</span><span class="sxs-lookup"><span data-stu-id="f7859-104">Navigate to the `graph-tutorial` directory and enter the following command to install dependencies.</span></span>

    ```Shell
    npm install
    ```

1. <span data-ttu-id="f7859-105">运行以下命令以使用报告的漏洞更新节点程序包。</span><span class="sxs-lookup"><span data-stu-id="f7859-105">Run the following command to update Node packages with reported vulnerabilities.</span></span>

    ```Shell
    npm audit fix
    ```

1. <span data-ttu-id="f7859-106">运行以下命令以更新 Express 和其他依赖项的版本。</span><span class="sxs-lookup"><span data-stu-id="f7859-106">Run the following command to update the version of Express and other dependencies.</span></span>

    ```Shell
    npm install express@4.17.1 http-errors@1.8.0 morgan@1.10.0 debug@4.1.1
    ```

1. <span data-ttu-id="f7859-107">使用以下命令启动本地 web 服务器。</span><span class="sxs-lookup"><span data-stu-id="f7859-107">Use the following command to start a local web server.</span></span>

    ```Shell
    npm start
    ```

1. <span data-ttu-id="f7859-108">打开浏览器，并导航到 `http://localhost:3000`。</span><span class="sxs-lookup"><span data-stu-id="f7859-108">Open your browser and navigate to `http://localhost:3000`.</span></span> <span data-ttu-id="f7859-109">如果一切正常，你将看到 "欢迎使用快递" 消息。</span><span class="sxs-lookup"><span data-stu-id="f7859-109">If everything is working, you will see a "Welcome to Express" message.</span></span> <span data-ttu-id="f7859-110">如果看不到该消息，请查看 [Express 入门指南](http://expressjs.com/starter/generator.html)。</span><span class="sxs-lookup"><span data-stu-id="f7859-110">If you don't see that message, check the [Express getting started guide](http://expressjs.com/starter/generator.html).</span></span>

## <a name="install-node-packages"></a><span data-ttu-id="f7859-111">安装节点包</span><span class="sxs-lookup"><span data-stu-id="f7859-111">Install Node packages</span></span>

<span data-ttu-id="f7859-112">在继续操作之前，请先安装将使用的其他一些程序包：</span><span class="sxs-lookup"><span data-stu-id="f7859-112">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="f7859-113">用于从 env 文件加载值的[dotenv](https://github.com/motdotla/dotenv) 。</span><span class="sxs-lookup"><span data-stu-id="f7859-113">[dotenv](https://github.com/motdotla/dotenv) for loading values from a .env file.</span></span>
- <span data-ttu-id="f7859-114">设置日期/时间值格式的[时刻](https://github.com/moment/moment/)。</span><span class="sxs-lookup"><span data-stu-id="f7859-114">[moment](https://github.com/moment/moment/) for formatting date/time values.</span></span>
- <span data-ttu-id="f7859-115">用于将 Windows 时区名称转换为 IANA 时区 Id 的[windows-iana](https://github.com/rubenillodo/windows-iana) 。</span><span class="sxs-lookup"><span data-stu-id="f7859-115">[windows-iana](https://github.com/rubenillodo/windows-iana) for translating Windows time zone names to IANA time zone IDs.</span></span>
- <span data-ttu-id="f7859-116">在应用程序中[连接-闪烁](https://github.com/jaredhanson/connect-flash)到闪存错误消息。</span><span class="sxs-lookup"><span data-stu-id="f7859-116">[connect-flash](https://github.com/jaredhanson/connect-flash) to flash error messages in the app.</span></span>
- <span data-ttu-id="f7859-117">将值存储在内存中的服务器端会话中的[快速会话](https://github.com/expressjs/session)。</span><span class="sxs-lookup"><span data-stu-id="f7859-117">[express-session](https://github.com/expressjs/session) to store values in an in-memory server-side session.</span></span>
- <span data-ttu-id="f7859-118">[表示](https://github.com/express-promise-router/express-promise-router) 允许路由处理程序返回承诺的路由器。</span><span class="sxs-lookup"><span data-stu-id="f7859-118">[express-promise-router](https://github.com/express-promise-router/express-promise-router) to allow route handlers to return a Promise.</span></span>
- <span data-ttu-id="f7859-119">用于分析和验证表单数据的[快速验证](https://github.com/express-validator/express-validator)程序。</span><span class="sxs-lookup"><span data-stu-id="f7859-119">[express-validator](https://github.com/express-validator/express-validator) for parsing and validating form data.</span></span>
- <span data-ttu-id="f7859-120">[msal-](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) 用于验证和获取访问令牌的节点。</span><span class="sxs-lookup"><span data-stu-id="f7859-120">[msal-node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) for authenticating and getting access tokens.</span></span>
- <span data-ttu-id="f7859-121">由 msal-节点用于生成 Guid 的[uuid](https://github.com/uuidjs/uuid) 。</span><span class="sxs-lookup"><span data-stu-id="f7859-121">[uuid](https://github.com/uuidjs/uuid) used by msal-node to generate GUIDs.</span></span>
- <span data-ttu-id="f7859-122">microsoft graph-用于调用 Microsoft Graph 的[客户端](https://github.com/microsoftgraph/msgraph-sdk-javascript)。</span><span class="sxs-lookup"><span data-stu-id="f7859-122">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>
- <span data-ttu-id="f7859-123">[isomorphic-](https://github.com/matthew-andrews/isomorphic-fetch) polyfill 获取对节点的提取。</span><span class="sxs-lookup"><span data-stu-id="f7859-123">[isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) to polyfill the fetch for Node.</span></span> <span data-ttu-id="f7859-124">库需要获取 polyfill `microsoft-graph-client` 。</span><span class="sxs-lookup"><span data-stu-id="f7859-124">A fetch polyfill is required for the `microsoft-graph-client` library.</span></span> <span data-ttu-id="f7859-125">有关详细信息，请参阅 [Microsoft Graph JavaScript 客户端库 wiki](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) 。</span><span class="sxs-lookup"><span data-stu-id="f7859-125">See the [Microsoft Graph JavaScript client library wiki](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) for more information.</span></span>

1. <span data-ttu-id="f7859-126">在 CLI 中运行以下命令。</span><span class="sxs-lookup"><span data-stu-id="f7859-126">Run the following command in your CLI.</span></span>

    ```Shell
    npm install dotenv@8.2.0 moment@2.29.1 moment-timezone@0.5.31 connect-flash@0.1.1 express-session@1.17.1 isomorphic-fetch@3.0.0
    npm install @azure/msal-node@1.0.0-beta.0 @microsoft/microsoft-graph-client@2.1.1 windows-iana@4.2.1 express-validator@6.6.1 uuid@8.3.1
    ```

    > [!TIP]
    > <span data-ttu-id="f7859-127">Windows 用户在尝试在 Windows 上安装这些程序包时可能会收到以下错误消息。</span><span class="sxs-lookup"><span data-stu-id="f7859-127">Windows users may get the following error message when trying to install these packages on Windows.</span></span>
    >
    > ```Shell
    > gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
    > ```
    >
    > <span data-ttu-id="f7859-128">若要解决此错误，请运行以下命令，以使用提升的 (管理员) 终端窗口（安装 VS 生成工具和 Python）安装 Windows 生成工具。</span><span class="sxs-lookup"><span data-stu-id="f7859-128">To resolve the error, run the following command to install the Windows Build Tools using an elevated (Administrator) terminal window which installs the VS Build Tools and Python.</span></span>
    >
    > ```Shell
    > npm install --global --production windows-build-tools
    > ```

1. <span data-ttu-id="f7859-129">更新应用程序以使用 `connect-flash` 和 `express-session` 中间件。</span><span class="sxs-lookup"><span data-stu-id="f7859-129">Update the application to use the `connect-flash` and `express-session` middleware.</span></span> <span data-ttu-id="f7859-130">打开 **。/app.js** 并将以下 `require` 语句添加到文件顶部。</span><span class="sxs-lookup"><span data-stu-id="f7859-130">Open **./app.js** and add the following `require` statement to the top of the file.</span></span>

    ```javascript
    const session = require('express-session');
    const flash = require('connect-flash');
    const msal = require('@azure/msal-node');
    ```

1. <span data-ttu-id="f7859-131">紧接着行的后面添加以下代码 `var app = express();` 。</span><span class="sxs-lookup"><span data-stu-id="f7859-131">Add the following code immediately after the `var app = express();` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SessionSnippet":::

## <a name="design-the-app"></a><span data-ttu-id="f7859-132">设计应用程序</span><span class="sxs-lookup"><span data-stu-id="f7859-132">Design the app</span></span>

<span data-ttu-id="f7859-133">在本节中，您将实现应用程序的 UI。</span><span class="sxs-lookup"><span data-stu-id="f7859-133">In this section you will implement the UI for the app.</span></span>

1. <span data-ttu-id="f7859-134">打开 **/views/layout.hbs** ，并使用以下代码替换整个内容。</span><span class="sxs-lookup"><span data-stu-id="f7859-134">Open **./views/layout.hbs** and replace the entire contents with the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/layout.hbs" id="LayoutSnippet":::

    <span data-ttu-id="f7859-135">此代码添加简单样式的 [引导](http://getbootstrap.com/) ，并添加一些简单图标的 [字体](https://fontawesome.com/) 。</span><span class="sxs-lookup"><span data-stu-id="f7859-135">This code adds [Bootstrap](http://getbootstrap.com/) for simple styling, and [Font Awesome](https://fontawesome.com/) for some simple icons.</span></span> <span data-ttu-id="f7859-136">它还定义具有导航栏的全局布局。</span><span class="sxs-lookup"><span data-stu-id="f7859-136">It also defines a global layout with a nav bar.</span></span>

1. <span data-ttu-id="f7859-137">打开 **/public/stylesheets/style.css** ，并将其全部内容替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="f7859-137">Open **./public/stylesheets/style.css** and replace its entire contents with the following.</span></span>

    :::code language="css" source="../demo/graph-tutorial/public/stylesheets/style.css":::

1. <span data-ttu-id="f7859-138">打开 **/views/index.hbs** ，并将其内容替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="f7859-138">Open **./views/index.hbs** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/index.hbs" id="IndexSnippet":::

1. <span data-ttu-id="f7859-139">打开 **/routes/index.js** 并将现有代码替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="f7859-139">Open **./routes/index.js** and replace the existing code with the following.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/index.js" id="IndexRouterSnippet" highlight="6-10":::

1. <span data-ttu-id="f7859-140">保存所有更改，然后重新启动服务器。</span><span class="sxs-lookup"><span data-stu-id="f7859-140">Save all of your changes and restart the server.</span></span> <span data-ttu-id="f7859-141">现在，应用程序看起来应非常不同。</span><span class="sxs-lookup"><span data-stu-id="f7859-141">Now, the app should look very different.</span></span>

    ![重新设计的主页的屏幕截图](./images/create-app-01.png)
