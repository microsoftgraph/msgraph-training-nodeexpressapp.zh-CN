# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="0a963-101">如何运行已完成的项目</span><span class="sxs-lookup"><span data-stu-id="0a963-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0a963-102">必备条件</span><span class="sxs-lookup"><span data-stu-id="0a963-102">Prerequisites</span></span>

<span data-ttu-id="0a963-103">若要在此文件夹中运行已完成的项目，您需要：</span><span class="sxs-lookup"><span data-stu-id="0a963-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="0a963-104">[Node.js](https://nodejs.org) 安装在开发计算机上。</span><span class="sxs-lookup"><span data-stu-id="0a963-104">[Node.js](https://nodejs.org) installed on your development machine.</span></span> <span data-ttu-id="0a963-105">如果尚未安装Node.js，请访问上一链接，查看下载选项。</span><span class="sxs-lookup"><span data-stu-id="0a963-105">If you do not have Node.js, visit the previous link for download options.</span></span> <span data-ttu-id="0a963-106"> (**注意：** 本教程是使用节点版本 14.15.0 编写的。</span><span class="sxs-lookup"><span data-stu-id="0a963-106">(**Note:** This tutorial was written with Node version 14.15.0.</span></span> <span data-ttu-id="0a963-107">本指南中的步骤可能与其他版本一起运行，但尚未经过测试) </span><span class="sxs-lookup"><span data-stu-id="0a963-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="0a963-108">在 Outlook.com 上拥有邮箱的个人 Microsoft 帐户，或 Microsoft 工作或学校帐户。</span><span class="sxs-lookup"><span data-stu-id="0a963-108">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="0a963-109">如果你没有 Microsoft 帐户，则有几个选项可以获取免费帐户：</span><span class="sxs-lookup"><span data-stu-id="0a963-109">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="0a963-110">你可以 [注册新的个人 Microsoft 帐户](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。</span><span class="sxs-lookup"><span data-stu-id="0a963-110">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="0a963-111">你可以[注册开发人员计划Office 365](https://developer.microsoft.com/office/dev-program)免费订阅Office 365订阅。</span><span class="sxs-lookup"><span data-stu-id="0a963-111">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="0a963-112">向管理中心Azure Active Directory Web 应用程序</span><span class="sxs-lookup"><span data-stu-id="0a963-112">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="0a963-113">打开浏览器，并转到 [Azure Active Directory 管理中心](https://aad.portal.azure.com)。</span><span class="sxs-lookup"><span data-stu-id="0a963-113">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="0a963-114">使用 **个人帐户**（亦称为“Microsoft 帐户”）或 **工作或学校帐户** 登录。</span><span class="sxs-lookup"><span data-stu-id="0a963-114">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="0a963-115">选择左侧导航栏中的“**Azure Active Directory**”，再选择“**管理**”下的“**应用注册**”。</span><span class="sxs-lookup"><span data-stu-id="0a963-115">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="0a963-116">应用注册屏幕截图</span><span class="sxs-lookup"><span data-stu-id="0a963-116">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="0a963-117">选择“新注册”。</span><span class="sxs-lookup"><span data-stu-id="0a963-117">Select **New registration**.</span></span> <span data-ttu-id="0a963-118">在“注册应用”页上，按如下方式设置值。</span><span class="sxs-lookup"><span data-stu-id="0a963-118">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="0a963-119">将“名称”设置为“`Node.js Graph Tutorial`”。</span><span class="sxs-lookup"><span data-stu-id="0a963-119">Set **Name** to `Node.js Graph Tutorial`.</span></span>
    - <span data-ttu-id="0a963-120">将“受支持的帐户类型”设置为“任何组织目录中的帐户和个人 Microsoft 帐户”。</span><span class="sxs-lookup"><span data-stu-id="0a963-120">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="0a963-121">在“重定向 URI”下，将第一个下拉列表设置为“`Web`”，并将值设置为“`http://localhost:3000/auth/callback`”。</span><span class="sxs-lookup"><span data-stu-id="0a963-121">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `http://localhost:3000/auth/callback`.</span></span>

    ![注册应用程序页面的屏幕截图](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="0a963-123">选择 **“注册”**。</span><span class="sxs-lookup"><span data-stu-id="0a963-123">Choose **Register**.</span></span> <span data-ttu-id="0a963-124">在Node.js Graph **教程**"页上，复制"应用程序 (客户端) **ID"的值并** 保存它，下一步中将需要该值。</span><span class="sxs-lookup"><span data-stu-id="0a963-124">On the **Node.js Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![新应用注册的应用程序 ID 的屏幕截图](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="0a963-126">选择“管理”下的“证书和密码”。</span><span class="sxs-lookup"><span data-stu-id="0a963-126">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="0a963-127">选择“新客户端密码”按钮。</span><span class="sxs-lookup"><span data-stu-id="0a963-127">Select the **New client secret** button.</span></span> <span data-ttu-id="0a963-128">在“说明”中输入值，并选择一个“过期”选项，再选择“添加”。</span><span class="sxs-lookup"><span data-stu-id="0a963-128">Enter a value in **Description** and select one of the options for **Expires** and choose **Add**.</span></span>

    ![添加客户端密码对话框的屏幕截图](/tutorial/images/aad-new-client-secret.png)

1. <span data-ttu-id="0a963-130">离开此页前，先复制客户端密码值。</span><span class="sxs-lookup"><span data-stu-id="0a963-130">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="0a963-131">将在下一步中用到它。</span><span class="sxs-lookup"><span data-stu-id="0a963-131">You will need it in the next step.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="0a963-132">此客户端密码不会再次显示，所以请务必现在就复制它。</span><span class="sxs-lookup"><span data-stu-id="0a963-132">This client secret is never shown again, so make sure you copy it now.</span></span>

    ![新添加的客户端密码的屏幕截图](/tutorial/images/aad-copy-client-secret.png)

## <a name="configure-the-sample"></a><span data-ttu-id="0a963-134">配置示例</span><span class="sxs-lookup"><span data-stu-id="0a963-134">Configure the sample</span></span>

1. <span data-ttu-id="0a963-135">将文件 `example.env` 重命名为 `.env` 。</span><span class="sxs-lookup"><span data-stu-id="0a963-135">Rename the `example.env` file to `.env`.</span></span>
1. <span data-ttu-id="0a963-136">编辑 `.env` 文件并做出以下更改。</span><span class="sxs-lookup"><span data-stu-id="0a963-136">Edit the `.env` file and make the following changes.</span></span>
    1. <span data-ttu-id="0a963-137">将 `YOUR_CLIENT_SECRET_HERE` 替换为 **从** 应用注册门户获得的应用程序 ID。</span><span class="sxs-lookup"><span data-stu-id="0a963-137">Replace `YOUR_CLIENT_SECRET_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
    1. <span data-ttu-id="0a963-138">将 `YOUR_APP_PASSWORD_HERE` 替换为从应用注册门户获得的密码。</span><span class="sxs-lookup"><span data-stu-id="0a963-138">Replace `YOUR_APP_PASSWORD_HERE` with the password you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="0a963-139">在命令行界面中 (CLI) ，导航到此目录并运行以下命令以安装要求。</span><span class="sxs-lookup"><span data-stu-id="0a963-139">In your command-line interface (CLI), navigate to this directory and run the following command to install requirements.</span></span>

    ```Shell
    npm install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="0a963-140">运行示例</span><span class="sxs-lookup"><span data-stu-id="0a963-140">Run the sample</span></span>

1. <span data-ttu-id="0a963-141">在 CLI 中运行以下命令以启动应用程序。</span><span class="sxs-lookup"><span data-stu-id="0a963-141">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    npm start
    ```

1. <span data-ttu-id="0a963-142">打开浏览器，然后转到 `http://localhost:3000`。</span><span class="sxs-lookup"><span data-stu-id="0a963-142">Open a browser and browse to `http://localhost:3000`.</span></span>
