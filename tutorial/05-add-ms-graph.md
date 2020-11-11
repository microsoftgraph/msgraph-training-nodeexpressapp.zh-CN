<!-- markdownlint-disable MD002 MD041 -->

在本练习中，将在应用程序中加入 Microsoft Graph。 对于此应用程序，您将使用 [microsoft graph 客户端](https://github.com/microsoftgraph/msgraph-sdk-javascript) 库调用 microsoft graph。

## <a name="get-calendar-events-from-outlook"></a>从 Outlook 获取日历事件

1. 打开 **。/graph.js** 并将以下函数添加到内部 `module.exports` 。

    :::code language="javascript" source="../demo/graph-tutorial/graph.js" id="GetCalendarViewSnippet":::

    考虑此代码执行的操作。

    - 将调用的 URL 为 `/me/events` 。
    - 此 `select` 方法将为每个事件返回的字段限制为只是视图实际使用的字段。
    - `orderby`方法按其创建日期和时间对结果进行排序，最新项目最先开始。

1. 在名为 **calendar.js** 的 **/routes** 目录中创建一个新文件，并添加以下代码。

    ```javascript
    const router = require('express-promise-router')();
    const graph = require('../graph.js');
    const moment = require('moment-timezone');
    const iana = require('windows-iana');
    const { body, validationResult } = require('express-validator');
    const validator = require('validator');

    /* GET /calendar */
    router.get('/',
      async function(req, res) {
        if (!req.session.userId) {
          // Redirect unauthenticated requests to home page
          res.redirect('/')
        } else {
          const params = {
            active: { calendar: true }
          };

          // Get the user
          const user = req.app.locals.users[req.session.userId];
          // Convert user's Windows time zone ("Pacific Standard Time")
          // to IANA format ("America/Los_Angeles")
          // Moment needs IANA format
          const timeZoneId = iana.findOneIana(user.timeZone);
          console.log(`Time zone: ${timeZoneId.valueOf()}`);

          // Calculate the start and end of the current week
          // Get midnight on the start of the current week in the user's timezone,
          // but in UTC. For example, for Pacific Standard Time, the time value would be
          // 07:00:00Z
          var startOfWeek = moment.tz(timeZoneId.valueOf()).startOf('week').utc();
          var endOfWeek = moment(startOfWeek).add(7, 'day');
          console.log(`Start: ${startOfWeek.format()}`);

          // Get the access token
          var accessToken;
          try {
            accessToken = await getAccessToken(req.session.userId, req.app.locals.msalClient);
          } catch (err) {
            res.send(JSON.stringify(err, Object.getOwnPropertyNames(err)));
            return;
          }

          if (accessToken && accessToken.length > 0) {
            try {
              // Get the events
              const events = await graph.getCalendarView(
                accessToken,
                startOfWeek.format(),
                endOfWeek.format(),
                user.timeZone);

              res.json(events.value);
            } catch (err) {
              res.send(JSON.stringify(err, Object.getOwnPropertyNames(err)));
            }
          }
          else {
            req.flash('error_msg', 'Could not get an access token');
          }
        }
      }
    );

    async function getAccessToken(userId, msalClient) {
      // Look up the user's account in the cache
      try {
        const accounts = await msalClient
          .getTokenCache()
          .getAllAccounts();

        const userAccount = accounts.find(a => a.homeAccountId === userId);

        // Get the token silently
        const response = await msalClient.acquireTokenSilent({
          scopes: process.env.OAUTH_SCOPES.split(','),
          redirectUri: process.env.OAUTH_REDIRECT_URI,
          account: userAccount
        });

        return response.accessToken;
      } catch (err) {
        console.log(JSON.stringify(err, Object.getOwnPropertyNames(err)));
      }
    }

    module.exports = router;
    ```

1. 更新 **。/app.js** 以使用此新路由。 在行 **前面** 添加以下行 `var app = express();` 。

    ```javascript
    var calendarRouter = require('./routes/calendar');
    ```

1. 在行 **后面** 添加以下行 `app.use('/auth', authRouter);` 。

    ```javascript
    app.use('/calendar', calendarRouter);
    ```

1. 重新启动服务器。 登录并单击导航栏中的 " **日历** " 链接。 如果一切正常，应在用户的日历上看到一个 JSON 转储的事件。

## <a name="display-the-results"></a>显示结果

现在，您可以添加一个视图，以对用户更友好的方式显示结果。

1. 将以下代码添加到中的 " **/app.js" 行后。** `app.set('view engine', 'hbs');`

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="FormatDateSnippet":::

    这将实现 [Handlebars 帮助](http://handlebarsjs.com/#helpers) 程序，将 Microsoft Graph 返回的 ISO 8601 日期格式化为人友好的内容。

1. 在名为 " **hbs** " 的 **/views** 目录中创建一个新文件，并添加以下代码。

    :::code language="html" source="../demo/graph-tutorial/views/calendar.hbs" id="LayoutSnippet":::

    这将遍历一组事件并为每个事件添加一个表行。

1. 现在，更新 **/routes/calendar.js** 中的路由以使用此视图。 将现有路由替换为以下代码。

    :::code language="javascript" source="../demo/graph-tutorial/routes/calendar.js" id="GetRouteSnippet" highlight="33-36,49,51-54,61":::

1. 保存所做的更改，重新启动服务器，然后登录到应用。 单击 " **日历** " 链接，应用现在应呈现一个事件表。

    ![事件表的屏幕截图](./images/add-msgraph-01.png)
