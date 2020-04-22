<!-- markdownlint-disable MD002 MD041 -->

在本练习中，将在应用程序中加入 Microsoft Graph。 对于此应用程序，您将使用[microsoft graph 客户端](https://github.com/microsoftgraph/msgraph-sdk-javascript)库调用 microsoft graph。

## <a name="get-calendar-events-from-outlook"></a>从 Outlook 获取日历事件

1. 在`./graph.js`中`module.exports`打开并添加以下函数。

    :::code language="javascript" source="../demo/graph-tutorial/graph.js" id="GetEventsSnippet":::

    考虑此代码执行的操作。

    - 将调用的 URL 为`/me/events`。
    - 此`select`方法将为每个事件返回的字段限制为只是视图实际使用的字段。
    - `orderby`方法按其创建日期和时间对结果进行排序，最新项目最先开始。

1. 在名为`./routes` `calendar.js`的目录中创建一个新文件，并添加以下代码。

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

1. 更新`./app.js`以使用此新路由。 在`var app = express();`行**前面**添加以下行。

    ```javascript
    var calendarRouter = require('./routes/calendar');
    ```

1. 在`app.use('/auth', authRouter);`行**后面**添加以下行。

    ```javascript
    app.use('/calendar', calendarRouter);
    ```

1. 重新启动服务器。 登录并单击导航栏中的 "**日历**" 链接。 如果一切正常，应在用户的日历上看到一个 JSON 转储的事件。

## <a name="display-the-results"></a>显示结果

现在，您可以添加一个视图，以对用户更友好的方式显示结果。

1. `./app.js` **after**在`app.set('view engine', 'hbs');`行后面添加以下代码。

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="FormatDateSnippet":::

    这将实现[Handlebars 帮助](http://handlebarsjs.com/#helpers)程序，将 Microsoft Graph 返回的 ISO 8601 日期格式化为人友好的内容。

1. 在名为`./views` `calendar.hbs`的目录中创建一个新文件，并添加以下代码。

    :::code language="html" source="../demo/graph-tutorial/views/calendar.hbs" id="LayoutSnippet":::

    这将遍历一组事件并为每个事件添加一个表行。

1. 现在，更新中`./routes/calendar.js`的路由，以使用此视图。 将现有路由替换为以下代码。

    :::code language="javascript" source="../demo/graph-tutorial/routes/calendar.js" id="GetRouteSnippet" highlight="16-19,26,28-31,37":::

1. 保存所做的更改，重新启动服务器，然后登录到应用。 单击 "**日历**" 链接，应用现在应呈现一个事件表。

    ![事件表的屏幕截图](./images/add-msgraph-01.png)
