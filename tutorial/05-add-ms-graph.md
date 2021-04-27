<!-- markdownlint-disable MD002 MD041 -->

在此练习中，你将 microsoft Graph应用程序。 对于此应用程序，你将使用[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript)库调用 Microsoft Graph。

## <a name="get-calendar-events-from-outlook"></a>从 Outlook 获取日历事件

1. 打开 **./graph.js** ，在 中添加以下函数 `module.exports` 。

    :::code language="javascript" source="../demo/graph-tutorial/graph.js" id="GetCalendarViewSnippet":::

    考虑此代码将执行什么工作。

    - 将调用的 URL 为 `/me/calendarview`。
    - `header`方法将 `Prefer: outlook.timezone` 标头添加到请求中，导致在用户的时区返回开始时间和结束时间。
    - `query`方法设置 `startDateTime` 日历 `endDateTime` 视图的 和 参数。
    - `select`方法将每个事件返回的字段限定为视图将实际使用的字段。
    - `orderby`方法按开始时间对结果进行排序。
    - `top`方法将结果限制为 50 个事件。

1. 在名为calendar.js的 **./routes** **目录中** 创建新文件，并添加以下代码。

    ```javascript
    const router = require('express-promise-router')();
    const graph = require('../graph.js');
    const addDays = require('date-fns/addDays');
    const formatISO = require('date-fns/formatISO');
    const startOfWeek = require('date-fns/startOfWeek');
    const zonedTimeToUtc = require('date-fns-tz/zonedTimeToUtc');
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
          const timeZoneId = iana.findIana(user.timeZone)[0];
          console.log(`Time zone: ${timeZoneId.valueOf()}`);

          // Calculate the start and end of the current week
          // Get midnight on the start of the current week in the user's timezone,
          // but in UTC. For example, for Pacific Standard Time, the time value would be
          // 07:00:00Z
          var weekStart = zonedTimeToUtc(startOfWeek(new Date()), timeZoneId.valueOf());
          var weekEnd = addDays(weekStart, 7);
          console.log(`Start: ${formatISO(weekStart)}`);

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
                formatISO(weekStart),
                formatISO(weekEnd),
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

1. 更新 **./app.js** 以使用此新路由。 在行之前 **添加以下** `var app = express();` 行。

    ```javascript
    var calendarRouter = require('./routes/calendar');
    ```

1. 在行后 **添加以下** `app.use('/auth', authRouter);` 行。

    ```javascript
    app.use('/calendar', calendarRouter);
    ```

1. 重新启动服务器。 登录并单击导航 **栏中** 的"日历"链接。 如果一切正常，应在用户日历上看到事件被 JSON 卸载。

## <a name="display-the-results"></a>显示结果

现在可以添加一个视图，以对用户更加友好的方式显示结果。

1. 在 **./app.js 行后添加** `app.set('view engine', 'hbs');` 以下代码。

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="FormatDateSnippet":::

    这将实现[Handlebars](http://handlebarsjs.com/#helpers)帮助程序，将 Microsoft Graph返回的 ISO 8601 日期格式化为更友好的内容。

1. 在 **./views** 目录中新建一个名为 **calendar.bms** 的文件，并添加以下代码。

    :::code language="html" source="../demo/graph-tutorial/views/calendar.hbs" id="LayoutSnippet":::

    该操作将循环遍历事件集合，并针对每个事件添加一个表格行。

1. 现在更新 **./routes/calendar.js** 中的路由以使用此视图。 将现有路由替换为以下代码。

    :::code language="javascript" source="../demo/graph-tutorial/routes/calendar.js" id="GetRouteSnippet" highlight="33-36,49,51-54,61":::

1. 保存更改，重新启动服务器，然后登录应用。 单击" **日历"** 链接，应用现在应呈现一个事件表。

    ![事件表的屏幕截图](./images/add-msgraph-01.png)
