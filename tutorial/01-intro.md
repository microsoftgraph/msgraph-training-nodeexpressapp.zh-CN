<!-- markdownlint-disable MD002 MD041 -->

本教程指导你如何构建使用 Microsoft Node.js API 检索用户的日历信息的 Graph Express Web 应用。

> [!TIP]
> 如果你想要只下载已完成的教程，可以通过两种方式下载它。
>
> - 下载 [Node.js快速入门](https://developer.microsoft.com/graph/quick-start?platform=option-node) ，以在数分钟内获取工作代码。
> - 下载或克隆GitHub[存储库](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp)。

## <a name="prerequisites"></a>必备条件

在开始此演示之前，应在开发 [Node.js](https://nodejs.org) 安装此演示。 如果尚未安装Node.js，请访问上一链接，查看下载选项。

> [!NOTE]
> Windows用户可能需要安装 Python 和 Visual Studio 生成工具 以支持需要从 C/C++ 编译的 NPM 模块。 Node.js安装程序Windows自动安装这些工具的选项。 或者，也可以按照 中的说明进行操作 [https://github.com/nodejs/node-gyp#on-windows](https://github.com/nodejs/node-gyp#on-windows) 。

您还应该有一个在 Outlook.com 上拥有邮箱的个人 Microsoft 帐户，或者一个 Microsoft 工作或学校帐户。 如果你没有 Microsoft 帐户，则有几个选项可以获取免费帐户：

- 你可以 [注册新的个人 Microsoft 帐户](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。
- 你可以[注册开发人员计划Office 365](https://developer.microsoft.com/office/dev-program)免费订阅Office 365订阅。

> [!NOTE]
> 本教程是使用 Node 版本 14.15.0 编写的。 本指南中的步骤可能与其他版本一起运行，但尚未经过测试。

## <a name="feedback"></a>反馈

Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).
