<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="62395-101">本教程向您介绍如何生成使用 Microsoft Graph API 检索用户的日历信息的 node.js Express web 应用程序。</span><span class="sxs-lookup"><span data-stu-id="62395-101">This tutorial teaches you how to build a Node.js Express web app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="62395-102">如果您只想下载已完成的教程, 可以通过两种方式下载它。</span><span class="sxs-lookup"><span data-stu-id="62395-102">If you prefer to just download the completed tutorial, you can download it in two ways.</span></span>
>
> - <span data-ttu-id="62395-103">下载[node.js 快速入门](https://developer.microsoft.com/graph/quick-start?platform=option-node), 以分钟为单位获取工作代码。</span><span class="sxs-lookup"><span data-stu-id="62395-103">Download the [Node.js quick start](https://developer.microsoft.com/graph/quick-start?platform=option-node) to get working code in minutes.</span></span>
> - <span data-ttu-id="62395-104">下载或克隆[GitHub 存储库](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp)。</span><span class="sxs-lookup"><span data-stu-id="62395-104">Download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="62395-105">先决条件</span><span class="sxs-lookup"><span data-stu-id="62395-105">Prerequisites</span></span>

<span data-ttu-id="62395-106">在开始此演示之前, 您应已在开发计算机上安装了[node.js](https://nodejs.org) 。</span><span class="sxs-lookup"><span data-stu-id="62395-106">Before you start this demo, you should have [Node.js](https://nodejs.org) installed on your development machine.</span></span> <span data-ttu-id="62395-107">如果您没有 node.js, 请访问上一个链接 "下载选项"。</span><span class="sxs-lookup"><span data-stu-id="62395-107">If you do not have Node.js, visit the previous link for download options.</span></span>

> [!NOTE]
> <span data-ttu-id="62395-108">本教程是使用节点版本10.7.0 编写的。</span><span class="sxs-lookup"><span data-stu-id="62395-108">This tutorial was written with Node version 10.7.0.</span></span> <span data-ttu-id="62395-109">本指南中的步骤可能适用于其他版本, 但尚未经过测试。</span><span class="sxs-lookup"><span data-stu-id="62395-109">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="62395-110">反馈</span><span class="sxs-lookup"><span data-stu-id="62395-110">Feedback</span></span>

<span data-ttu-id="62395-111">请在[GitHub 存储库](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp)中提供有关本教程的任何反馈。</span><span class="sxs-lookup"><span data-stu-id="62395-111">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).</span></span>