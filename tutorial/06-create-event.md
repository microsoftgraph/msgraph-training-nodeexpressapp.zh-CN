<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="10d50-101">在本节中，您将添加在用户日历上创建事件的功能。</span><span class="sxs-lookup"><span data-stu-id="10d50-101">In this section you will add the ability to create events on the user's calendar.</span></span>

## <a name="create-a-new-event-form"></a><span data-ttu-id="10d50-102">创建新的事件表单</span><span class="sxs-lookup"><span data-stu-id="10d50-102">Create a new event form</span></span>

1. <span data-ttu-id="10d50-103">在名为 **newevent** 的 **/views** 目录中创建一个新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="10d50-103">Create a new file in the **./views** directory named **newevent.hbs** and add the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/newevent.hbs" id="NewEventFormSnippet":::

1. <span data-ttu-id="10d50-104">将下面的代码添加到 **/routes/calendar.js** 文件中的 `module.exports = router;` 第一行。</span><span class="sxs-lookup"><span data-stu-id="10d50-104">Add the following code to the **./routes/calendar.js** file before the `module.exports = router;` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/calendar.js" id="GetEventFormSnippet":::

<span data-ttu-id="10d50-105">这将为用户输入实现一个表单，并呈现该表单。</span><span class="sxs-lookup"><span data-stu-id="10d50-105">This implements a form for user input and renders it.</span></span>

## <a name="create-the-event"></a><span data-ttu-id="10d50-106">创建事件</span><span class="sxs-lookup"><span data-stu-id="10d50-106">Create the event</span></span>

1. <span data-ttu-id="10d50-107">打开 **。/graph.js** 并将以下函数添加到内部 `module.exports` 。</span><span class="sxs-lookup"><span data-stu-id="10d50-107">Open **./graph.js** and add the following function inside `module.exports`.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/graph.js" id="CreateEventSnippet":::

    <span data-ttu-id="10d50-108">此代码使用窗体字段创建 Graph 事件对象，然后将 POST 请求发送到 `/me/events` 终结点，以在用户的默认日历上创建事件。</span><span class="sxs-lookup"><span data-stu-id="10d50-108">This code uses the form fields to create a Graph event object, then sends a POST request to the `/me/events` endpoint to create the event on the user's default calendar.</span></span>

1. <span data-ttu-id="10d50-109">将下面的代码添加到 **/routes/calendar.js** 文件中的 `module.exports = router;` 第一行。</span><span class="sxs-lookup"><span data-stu-id="10d50-109">Add the following code to the **./routes/calendar.js** file before the `module.exports = router;` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/calendar.js" id="PostEventFormSnippet":::

    <span data-ttu-id="10d50-110">此代码验证并净化了窗体输入，然后调用 `graph.createEvent` 以创建事件。</span><span class="sxs-lookup"><span data-stu-id="10d50-110">This code validates and sanitized the form input, then calls `graph.createEvent` to create the event.</span></span> <span data-ttu-id="10d50-111">调用完成后，它将重定向回 "日历" 视图。</span><span class="sxs-lookup"><span data-stu-id="10d50-111">It redirects back to the calendar view after the call completes.</span></span>

1. <span data-ttu-id="10d50-112">保存更改并重新启动该应用。</span><span class="sxs-lookup"><span data-stu-id="10d50-112">Save your changes and restart the app.</span></span> <span data-ttu-id="10d50-113">单击 " **日历** " 导航项，然后单击 " **创建事件** " 按钮。</span><span class="sxs-lookup"><span data-stu-id="10d50-113">Click the **Calendar** nav item, then click the **Create event** button.</span></span> <span data-ttu-id="10d50-114">填写值，然后单击 " **创建** "。</span><span class="sxs-lookup"><span data-stu-id="10d50-114">Fill in the values and click **Create**.</span></span> <span data-ttu-id="10d50-115">创建新事件后，应用程序将返回到日历视图。</span><span class="sxs-lookup"><span data-stu-id="10d50-115">The app returns to the calendar view once the new event is created.</span></span>

    ![新事件表单的屏幕截图](images/create-event-01.png)
