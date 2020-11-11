<!-- markdownlint-disable MD002 MD041 -->

本教程向您介绍如何构建一个使用 Microsoft Graph API 的 Node.js Express web 应用程序来检索用户的日历信息。

> [!TIP]
> 如果您只想下载已完成的教程，可以通过两种方式下载它。
>
> - 下载 [Node.js 快速入门](https://developer.microsoft.com/graph/quick-start?platform=option-node) ，以分钟为单位获取工作代码。
> - 下载或克隆 [GitHub 存储库](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp)。

## <a name="prerequisites"></a>先决条件

在开始此演示之前，您的开发计算机上应安装有 [Node.js](https://nodejs.org) 。 如果您没有 Node.js，请访问 "下载选项" 的上一个链接。

> [!NOTE]
> Windows 用户可能需要安装 Python 和 Visual Studio 生成工具，以支持需要从 C/c + + 编译的 NPM 模块。 Windows 上的 Node.js 安装程序提供了一个选项，可用于自动安装这些工具。 或者，您可以按照中的说明进行操作 [https://github.com/nodejs/node-gyp#on-windows](https://github.com/nodejs/node-gyp#on-windows) 。

您还应具有一个个人的 Microsoft 帐户，其中包含 Outlook.com 上的邮箱，或 Microsoft 工作或学校帐户。 如果你没有 Microsoft 帐户，可以使用以下几种方法获取免费帐户：

- 你可以 [注册新的个人 Microsoft 帐户](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。
- 你可以 [注册 office 365 开发人员计划](https://developer.microsoft.com/office/dev-program) 以获取免费的 office 365 订阅。

> [!NOTE]
> 本教程是使用节点版本12.18.4 编写的。 本指南中的步骤可能适用于其他版本，但尚未经过测试。

## <a name="feedback"></a>反馈

请在 [GitHub 存储库](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp)中提供有关本教程的任何反馈。
