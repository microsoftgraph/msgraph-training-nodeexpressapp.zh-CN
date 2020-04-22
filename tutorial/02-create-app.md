<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="42ab8-101">在本练习中，将使用[Express](http://expressjs.com/)生成 web 应用程序。</span><span class="sxs-lookup"><span data-stu-id="42ab8-101">In this exercise you will use [Express](http://expressjs.com/) to build a web app.</span></span>

1. <span data-ttu-id="42ab8-102">打开您的 CLI，导航到您有权创建文件的目录，然后运行以下命令，以创建一个使用[Handlebars](http://handlebarsjs.com/)作为呈现引擎的新 Express 应用程序。</span><span class="sxs-lookup"><span data-stu-id="42ab8-102">Open your CLI, navigate to a directory where you have rights to create files, and run the following command to create a new Express app that uses [Handlebars](http://handlebarsjs.com/) as the rendering engine.</span></span>

    ```Shell
    npx express-generator@4.16.1 --hbs graph-tutorial
    ```

    <span data-ttu-id="42ab8-103">Express 生成器将创建一个名`graph-tutorial`为 "搭建基架" 的新目录，并将其作为 Express 应用程序。</span><span class="sxs-lookup"><span data-stu-id="42ab8-103">The Express generator creates a new directory called `graph-tutorial` and scaffolds an Express app.</span></span>

1. <span data-ttu-id="42ab8-104">导航到该`graph-tutorial`目录，然后输入以下命令以安装依赖项。</span><span class="sxs-lookup"><span data-stu-id="42ab8-104">Navigate to the `graph-tutorial` directory and enter the following command to install dependencies.</span></span>

    ```Shell
    npm install
    ```

1. <span data-ttu-id="42ab8-105">运行以下命令以使用报告的漏洞更新节点程序包。</span><span class="sxs-lookup"><span data-stu-id="42ab8-105">Run the following command to update Node packages with reported vulnerabilities.</span></span>

    ```Shell
    npm audit fix
    ```

1. <span data-ttu-id="42ab8-106">使用以下命令启动本地 web 服务器。</span><span class="sxs-lookup"><span data-stu-id="42ab8-106">Use the following command to start a local web server.</span></span>

    ```Shell
    npm start
    ```

1. <span data-ttu-id="42ab8-107">打开浏览器，并导航到 `http://localhost:3000`。</span><span class="sxs-lookup"><span data-stu-id="42ab8-107">Open your browser and navigate to `http://localhost:3000`.</span></span> <span data-ttu-id="42ab8-108">如果一切正常，你将看到 "欢迎使用快递" 消息。</span><span class="sxs-lookup"><span data-stu-id="42ab8-108">If everything is working, you will see a "Welcome to Express" message.</span></span> <span data-ttu-id="42ab8-109">如果看不到该消息，请查看[Express 入门指南](http://expressjs.com/starter/generator.html)。</span><span class="sxs-lookup"><span data-stu-id="42ab8-109">If you don't see that message, check the [Express getting started guide](http://expressjs.com/starter/generator.html).</span></span>

## <a name="install-node-packages"></a><span data-ttu-id="42ab8-110">安装节点包</span><span class="sxs-lookup"><span data-stu-id="42ab8-110">Install Node packages</span></span>

<span data-ttu-id="42ab8-111">在继续操作之前，请先安装将使用的其他一些程序包：</span><span class="sxs-lookup"><span data-stu-id="42ab8-111">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="42ab8-112">用于从 env 文件加载值的[dotenv](https://github.com/motdotla/dotenv) 。</span><span class="sxs-lookup"><span data-stu-id="42ab8-112">[dotenv](https://github.com/motdotla/dotenv) for loading values from a .env file.</span></span>
- <span data-ttu-id="42ab8-113">设置日期/时间值格式的[时刻](https://github.com/moment/moment/)。</span><span class="sxs-lookup"><span data-stu-id="42ab8-113">[moment](https://github.com/moment/moment/) for formatting date/time values.</span></span>
- <span data-ttu-id="42ab8-114">在应用程序中[连接-闪烁](https://github.com/jaredhanson/connect-flash)到闪存错误消息。</span><span class="sxs-lookup"><span data-stu-id="42ab8-114">[connect-flash](https://github.com/jaredhanson/connect-flash) to flash error messages in the app.</span></span>
- <span data-ttu-id="42ab8-115">将值存储在内存中的服务器端会话中的[快速会话](https://github.com/expressjs/session)。</span><span class="sxs-lookup"><span data-stu-id="42ab8-115">[express-session](https://github.com/expressjs/session) to store values in an in-memory server-side session.</span></span>
- <span data-ttu-id="42ab8-116">[passport-azure-ad](https://github.com/AzureAD/passport-azure-ad)身份验证和获取访问令牌。</span><span class="sxs-lookup"><span data-stu-id="42ab8-116">[passport-azure-ad](https://github.com/AzureAD/passport-azure-ad) for authenticating and getting access tokens.</span></span>
- <span data-ttu-id="42ab8-117">[简单-oauth2](https://github.com/lelylan/simple-oauth2)用于令牌管理。</span><span class="sxs-lookup"><span data-stu-id="42ab8-117">[simple-oauth2](https://github.com/lelylan/simple-oauth2) for token management.</span></span>
- <span data-ttu-id="42ab8-118">microsoft graph-用于调用 Microsoft Graph 的[客户端](https://github.com/microsoftgraph/msgraph-sdk-javascript)。</span><span class="sxs-lookup"><span data-stu-id="42ab8-118">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>
- <span data-ttu-id="42ab8-119">[isomorphic-](https://github.com/matthew-andrews/isomorphic-fetch) polyfill 获取对节点的提取。</span><span class="sxs-lookup"><span data-stu-id="42ab8-119">[isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) to polyfill the fetch for Node.</span></span> <span data-ttu-id="42ab8-120">`microsoft-graph-client`库需要获取 polyfill。</span><span class="sxs-lookup"><span data-stu-id="42ab8-120">A fetch polyfill is required for the `microsoft-graph-client` library.</span></span> <span data-ttu-id="42ab8-121">有关详细信息，请参阅[Microsoft Graph JavaScript 客户端库 wiki](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) 。</span><span class="sxs-lookup"><span data-stu-id="42ab8-121">See the [Microsoft Graph JavaScript client library wiki](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) for more information.</span></span>

1. <span data-ttu-id="42ab8-122">在 CLI 中运行以下命令。</span><span class="sxs-lookup"><span data-stu-id="42ab8-122">Run the following command in your CLI.</span></span>

    ```Shell
    npm install dotenv@8.2.0 moment@2.24.0 connect-flash@0.1.1 express-session@1.17.0 isomorphic-fetch@2.2.1
    npm install passport-azure-ad@4.2.1 simple-oauth2@3.3.0 @microsoft/microsoft-graph-client@2.0.0
    ```

    > [!TIP]
    > <span data-ttu-id="42ab8-123">Windows 用户在尝试在 Windows 上安装这些程序包时可能会收到以下错误消息。</span><span class="sxs-lookup"><span data-stu-id="42ab8-123">Windows users may get the following error message when trying to install these packages on Windows.</span></span>
    >
    > ```Shell
    > gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
    > ```
    >
    > <span data-ttu-id="42ab8-124">若要解决此错误，请运行以下命令，以使用安装 VS 生成工具和 Python 的提升（管理员）终端窗口来安装 Windows 生成工具。</span><span class="sxs-lookup"><span data-stu-id="42ab8-124">To resolve the error, run the following command to install the Windows Build Tools using an elevated (Administrator) terminal window which installs the VS Build Tools and Python.</span></span>
    >
    > ```Shell
    > npm install --global --production windows-build-tools
    > ```

1. <span data-ttu-id="42ab8-125">更新应用程序以使用`connect-flash`和`express-session`中间件。</span><span class="sxs-lookup"><span data-stu-id="42ab8-125">Update the application to use the `connect-flash` and `express-session` middleware.</span></span> <span data-ttu-id="42ab8-126">打开`./app.js`文件，并将以下`require`语句添加到文件顶部。</span><span class="sxs-lookup"><span data-stu-id="42ab8-126">Open the `./app.js` file and add the following `require` statement to the top of the file.</span></span>

    ```javascript
    var session = require('express-session');
    var flash = require('connect-flash');
    ```

1. <span data-ttu-id="42ab8-127">紧接着行的`var app = express();`后面添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="42ab8-127">Add the following code immediately after the `var app = express();` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SessionSnippet":::

## <a name="design-the-app"></a><span data-ttu-id="42ab8-128">设计应用程序</span><span class="sxs-lookup"><span data-stu-id="42ab8-128">Design the app</span></span>

<span data-ttu-id="42ab8-129">在本节中，您将实现应用程序的 UI。</span><span class="sxs-lookup"><span data-stu-id="42ab8-129">In this section you will implement the UI for the app.</span></span>

1. <span data-ttu-id="42ab8-130">打开`./views/layout.hbs`文件，并将整个内容替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="42ab8-130">Open the `./views/layout.hbs` file and replace the entire contents with the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/layout.hbs" id="LayoutSnippet":::

    <span data-ttu-id="42ab8-131">此代码添加简单样式的[引导](http://getbootstrap.com/)，并添加一些简单图标的[字体](https://fontawesome.com/)。</span><span class="sxs-lookup"><span data-stu-id="42ab8-131">This code adds [Bootstrap](http://getbootstrap.com/) for simple styling, and [Font Awesome](https://fontawesome.com/) for some simple icons.</span></span> <span data-ttu-id="42ab8-132">它还定义具有导航栏的全局布局。</span><span class="sxs-lookup"><span data-stu-id="42ab8-132">It also defines a global layout with a nav bar.</span></span>

1. <span data-ttu-id="42ab8-133">打开`./public/stylesheets/style.css`并将其全部内容替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="42ab8-133">Open `./public/stylesheets/style.css` and replace its entire contents with the following.</span></span>

    :::code language="css" source="../demo/graph-tutorial/public/stylesheets/style.css":::

1. <span data-ttu-id="42ab8-134">打开`./views/index.hbs`文件，并将其内容替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="42ab8-134">Open the `./views/index.hbs` file and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/index.hbs" id="IndexSnippet":::

1. <span data-ttu-id="42ab8-135">打开`./routes/index.js`文件，并将现有代码替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="42ab8-135">Open the `./routes/index.js` file and replace the existing code with the following.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/index.js" id="IndexRouterSnippet" highlight="6-10":::

1. <span data-ttu-id="42ab8-136">保存所有更改，然后重新启动服务器。</span><span class="sxs-lookup"><span data-stu-id="42ab8-136">Save all of your changes and restart the server.</span></span> <span data-ttu-id="42ab8-137">现在，应用程序看起来应非常不同。</span><span class="sxs-lookup"><span data-stu-id="42ab8-137">Now, the app should look very different.</span></span>

    ![重新设计的主页的屏幕截图](./images/create-app-01.png)
