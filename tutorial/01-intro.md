<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="b10f9-101">本教程指导你如何构建使用 Microsoft Node.js API 检索用户的日历信息的 Graph Express Web 应用。</span><span class="sxs-lookup"><span data-stu-id="b10f9-101">This tutorial teaches you how to build a Node.js Express web app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="b10f9-102">如果你想要只下载已完成的教程，可以通过两种方式下载它。</span><span class="sxs-lookup"><span data-stu-id="b10f9-102">If you prefer to just download the completed tutorial, you can download it in two ways.</span></span>
>
> - <span data-ttu-id="b10f9-103">下载 [Node.js快速入门](https://developer.microsoft.com/graph/quick-start?platform=option-node) ，以在数分钟内获取工作代码。</span><span class="sxs-lookup"><span data-stu-id="b10f9-103">Download the [Node.js quick start](https://developer.microsoft.com/graph/quick-start?platform=option-node) to get working code in minutes.</span></span>
> - <span data-ttu-id="b10f9-104">下载或克隆GitHub[存储库](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp)。</span><span class="sxs-lookup"><span data-stu-id="b10f9-104">Download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b10f9-105">必备条件</span><span class="sxs-lookup"><span data-stu-id="b10f9-105">Prerequisites</span></span>

<span data-ttu-id="b10f9-106">在开始此演示之前，应在开发 [Node.js](https://nodejs.org) 安装此演示。</span><span class="sxs-lookup"><span data-stu-id="b10f9-106">Before you start this demo, you should have [Node.js](https://nodejs.org) installed on your development machine.</span></span> <span data-ttu-id="b10f9-107">如果尚未安装Node.js，请访问上一链接，查看下载选项。</span><span class="sxs-lookup"><span data-stu-id="b10f9-107">If you do not have Node.js, visit the previous link for download options.</span></span>

> [!NOTE]
> <span data-ttu-id="b10f9-108">Windows用户可能需要安装 Python 和 Visual Studio 生成工具 以支持需要从 C/C++ 编译的 NPM 模块。</span><span class="sxs-lookup"><span data-stu-id="b10f9-108">Windows users may need to install Python and Visual Studio Build Tools to support NPM modules that need to be compiled from C/C++.</span></span> <span data-ttu-id="b10f9-109">Node.js安装程序Windows自动安装这些工具的选项。</span><span class="sxs-lookup"><span data-stu-id="b10f9-109">The Node.js installer on Windows gives an option to automatically install these tools.</span></span> <span data-ttu-id="b10f9-110">或者，也可以按照 中的说明进行操作 [https://github.com/nodejs/node-gyp#on-windows](https://github.com/nodejs/node-gyp#on-windows) 。</span><span class="sxs-lookup"><span data-stu-id="b10f9-110">Alternatively, you can follow instructions at [https://github.com/nodejs/node-gyp#on-windows](https://github.com/nodejs/node-gyp#on-windows).</span></span>

<span data-ttu-id="b10f9-111">您还应该有一个在 Outlook.com 上拥有邮箱的个人 Microsoft 帐户，或者一个 Microsoft 工作或学校帐户。</span><span class="sxs-lookup"><span data-stu-id="b10f9-111">You should also have either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span> <span data-ttu-id="b10f9-112">如果你没有 Microsoft 帐户，则有几个选项可以获取免费帐户：</span><span class="sxs-lookup"><span data-stu-id="b10f9-112">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="b10f9-113">你可以 [注册新的个人 Microsoft 帐户](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。</span><span class="sxs-lookup"><span data-stu-id="b10f9-113">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="b10f9-114">你可以[注册开发人员计划Office 365](https://developer.microsoft.com/office/dev-program)免费订阅Office 365订阅。</span><span class="sxs-lookup"><span data-stu-id="b10f9-114">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="b10f9-115">本教程是使用 Node 版本 14.15.0 编写的。</span><span class="sxs-lookup"><span data-stu-id="b10f9-115">This tutorial was written with Node version 14.15.0.</span></span> <span data-ttu-id="b10f9-116">本指南中的步骤可能与其他版本一起运行，但尚未经过测试。</span><span class="sxs-lookup"><span data-stu-id="b10f9-116">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="b10f9-117">反馈</span><span class="sxs-lookup"><span data-stu-id="b10f9-117">Feedback</span></span>

<span data-ttu-id="b10f9-118">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).</span><span class="sxs-lookup"><span data-stu-id="b10f9-118">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).</span></span>
