<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="14980-101">在本练习中，将在应用程序中加入 Microsoft Graph。</span><span class="sxs-lookup"><span data-stu-id="14980-101">In this exercise you will incorporate Microsoft Graph into the application.</span></span> <span data-ttu-id="14980-102">对于此应用程序，您将使用[microsoft graph 客户端](https://github.com/microsoftgraph/msgraph-sdk-javascript)库调用 microsoft graph。</span><span class="sxs-lookup"><span data-stu-id="14980-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="14980-103">从 Outlook 获取日历事件</span><span class="sxs-lookup"><span data-stu-id="14980-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="14980-104">在`./graph.js`中`module.exports`打开并添加以下函数。</span><span class="sxs-lookup"><span data-stu-id="14980-104">Open `./graph.js` and add the following function inside `module.exports`.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/graph.js" id="GetEventsSnippet":::

    <span data-ttu-id="14980-105">考虑此代码执行的操作。</span><span class="sxs-lookup"><span data-stu-id="14980-105">Consider what this code is doing.</span></span>

    - <span data-ttu-id="14980-106">将调用的 URL 为`/me/events`。</span><span class="sxs-lookup"><span data-stu-id="14980-106">The URL that will be called is `/me/events`.</span></span>
    - <span data-ttu-id="14980-107">此`select`方法将为每个事件返回的字段限制为只是视图实际使用的字段。</span><span class="sxs-lookup"><span data-stu-id="14980-107">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
    - <span data-ttu-id="14980-108">`orderby`方法按其创建日期和时间对结果进行排序，最新项目最先开始。</span><span class="sxs-lookup"><span data-stu-id="14980-108">The `orderby` method sorts the results by the date and time they were created, with the most recent item being first.</span></span>

1. <span data-ttu-id="14980-109">在名为`./routes` `calendar.js`的目录中创建一个新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="14980-109">Create a new file in the `./routes` directory named `calendar.js`, and add the following code.</span></span>

    ```javascript
    var express = require('express');
    var router = express.Router();
    var tokens = require('../tokens.js');
    var graph = require('../graph.js');

    /* GET /calendar */
    router.get('/',
      async function(req, res) {
        if (!req.isAuthenticated()) {
          // Redirect unauthenticated requests to home page
          res.redirect('/')
        } else {
          let params = {
            active: { calendar: true }
          };

          // Get the access token
          var accessToken;
          try {
            accessToken = await tokens.getAccessToken(req);
          } catch (err) {
            res.json(err);
          }

          if (accessToken && accessToken.length > 0) {
            try {
              // Get the events
              var events = await graph.getEvents(accessToken);

              res.json(events.value);
            } catch (err) {
              res.json(err);
            }
          }
          else {
            req.flash('error_msg', 'Could not get an access token');
          }
        }
      }
    );

    module.exports = router;
    ```

1. <span data-ttu-id="14980-110">更新`./app.js`以使用此新路由。</span><span class="sxs-lookup"><span data-stu-id="14980-110">Update `./app.js` to use this new route.</span></span> <span data-ttu-id="14980-111">在`var app = express();`行**前面**添加以下行。</span><span class="sxs-lookup"><span data-stu-id="14980-111">Add the following line **before** the `var app = express();` line.</span></span>

    ```javascript
    var calendarRouter = require('./routes/calendar');
    ```

1. <span data-ttu-id="14980-112">在`app.use('/auth', authRouter);`行**后面**添加以下行。</span><span class="sxs-lookup"><span data-stu-id="14980-112">Add the following line **after** the `app.use('/auth', authRouter);` line.</span></span>

    ```javascript
    app.use('/calendar', calendarRouter);
    ```

1. <span data-ttu-id="14980-113">重新启动服务器。</span><span class="sxs-lookup"><span data-stu-id="14980-113">Restart the server.</span></span> <span data-ttu-id="14980-114">登录并单击导航栏中的 "**日历**" 链接。</span><span class="sxs-lookup"><span data-stu-id="14980-114">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="14980-115">如果一切正常，应在用户的日历上看到一个 JSON 转储的事件。</span><span class="sxs-lookup"><span data-stu-id="14980-115">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="14980-116">显示结果</span><span class="sxs-lookup"><span data-stu-id="14980-116">Display the results</span></span>

<span data-ttu-id="14980-117">现在，您可以添加一个视图，以对用户更友好的方式显示结果。</span><span class="sxs-lookup"><span data-stu-id="14980-117">Now you can add a view to display the results in a more user-friendly manner.</span></span>

1. <span data-ttu-id="14980-118">`./app.js` **after**在`app.set('view engine', 'hbs');`行后面添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="14980-118">Add the following code in `./app.js` **after** the `app.set('view engine', 'hbs');` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="FormatDateSnippet":::

    <span data-ttu-id="14980-119">这将实现[Handlebars 帮助](http://handlebarsjs.com/#helpers)程序，将 Microsoft Graph 返回的 ISO 8601 日期格式化为人友好的内容。</span><span class="sxs-lookup"><span data-stu-id="14980-119">This implements a [Handlebars helper](http://handlebarsjs.com/#helpers) to format the ISO 8601 date returned by Microsoft Graph into something more human-friendly.</span></span>

1. <span data-ttu-id="14980-120">在名为`./views` `calendar.hbs`的目录中创建一个新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="14980-120">Create a new file in the `./views` directory named `calendar.hbs` and add the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/calendar.hbs" id="LayoutSnippet":::

    <span data-ttu-id="14980-121">这将遍历一组事件并为每个事件添加一个表行。</span><span class="sxs-lookup"><span data-stu-id="14980-121">That will loop through a collection of events and add a table row for each one.</span></span>

1. <span data-ttu-id="14980-122">现在，更新中`./routes/calendar.js`的路由，以使用此视图。</span><span class="sxs-lookup"><span data-stu-id="14980-122">Now update the route in `./routes/calendar.js` to use this view.</span></span> <span data-ttu-id="14980-123">将现有路由替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="14980-123">Replace the existing route with the following code.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/calendar.js" id="GetRouteSnippet" highlight="16-19,26,28-31,37":::

1. <span data-ttu-id="14980-124">保存所做的更改，重新启动服务器，然后登录到应用。</span><span class="sxs-lookup"><span data-stu-id="14980-124">Save your changes, restart the server, and sign in to the app.</span></span> <span data-ttu-id="14980-125">单击 "**日历**" 链接，应用现在应呈现一个事件表。</span><span class="sxs-lookup"><span data-stu-id="14980-125">Click on the **Calendar** link and the app should now render a table of events.</span></span>

    ![事件表的屏幕截图](./images/add-msgraph-01.png)
