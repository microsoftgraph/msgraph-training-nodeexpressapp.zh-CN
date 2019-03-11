<!-- markdownlint-disable MD002 MD041 -->

在本练习中, 将把 Microsoft Graph 合并到应用程序中。 对于此应用程序, 您将使用[microsoft graph 客户端](https://github.com/microsoftgraph/msgraph-sdk-javascript)库调用 microsoft graph。

## <a name="get-calendar-events-from-outlook"></a>从 Outlook 获取日历事件

首先将新方法添加到`./graph.js`文件中, 以从日历中获取事件。 在中`module.exports` `./graph.js`的内添加以下函数。

```js
getEvents: async function(accessToken) {
  const client = getAuthenticatedClient(accessToken);

  const events = await client
    .api('/me/events')
    .select('subject,organizer,start,end')
    .orderby('createdDateTime DESC')
    .get();

  return events;
}
```

考虑此代码执行的操作。

- 将调用的 URL 为`/me/events`。
- 此`select`方法将为每个事件返回的字段限制为只是视图实际使用的字段。
- `orderby`方法按其创建日期和时间对结果进行排序, 最新项目最先开始。

在名为`./routes` `calendar.js`的目录中创建一个新文件, 并添加以下代码。

```js
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
    }
  }
);

module.exports = router;
```

更新`./app.js`以使用此新路由。 在`var app = express();`行**前面**添加以下行。

```js
var calendarRouter = require('./routes/calendar');
```

然后在`app.use('/auth', authRouter);`行**后面**添加以下行。

```js
app.use('/calendar', calendarRouter);
```

现在, 您可以对此进行测试。 登录并单击导航栏中的 "**日历**" 链接。 如果一切正常, 应在用户的日历上看到一个 JSON 转储的事件。

## <a name="display-the-results"></a>显示结果

现在, 您可以添加一个视图, 以对用户更友好的方式显示结果。 首先, 在`./app.js` **** `app.set('view engine', 'hbs');`行后面添加以下代码。

```js
var hbs = require('hbs');
var moment = require('moment');
// Helper to format date/time sent by Graph
hbs.registerHelper('eventDateTime', function(dateTime){
  return moment(dateTime).format('M/D/YY h:mm A');
});
```

这将实现[Handlebars 帮助](http://handlebarsjs.com/#helpers)程序, 将 Microsoft Graph 返回的 ISO 8601 日期格式化为人友好的内容。

在名为`./views` `calendar.hbs`的目录中创建一个新文件, 并添加以下代码。

```html
<h1>Calendar</h1>
<table class="table">
  <thead>
    <tr>
      <th scope="col">Organizer</th>
      <th scope="col">Subject</th>
      <th scope="col">Start</th>
      <th scope="col">End</th>
    </tr>
  </thead>
  <tbody>
    {{#each events}}
      <tr>
        <td>{{this.organizer.emailAddress.name}}</td>
        <td>{{this.subject}}</td>
        <td>{{eventDateTime this.start.dateTime}}</td>
        <td>{{eventDateTime this.end.dateTime}}</td>
      </tr>
    {{/each}}
  </tbody>
</table>
```

这将遍历一组事件并为每个事件添加一个表行。 现在, 更新中`./routes/calendar.js`的路由, 以使用此视图。 将现有路由替换为以下代码。

```js
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
        req.flash('error_msg', {
          message: 'Could not get access token. Try signing out and signing in again.',
          debug: JSON.stringify(err)
        });
      }

      if (accessToken && accessToken.length > 0) {
        try {
          // Get the events
          var events = await graph.getEvents(accessToken);
          params.events = events.value;
        } catch (err) {
          req.flash('error_msg', {
            message: 'Could not fetch events',
            debug: JSON.stringify(err)
          });
        }
      }

      res.render('calendar', params);
    }
  }
);
```

保存所做的更改, 重新启动服务器, 然后登录到应用。 单击 "**日历**" 链接, 应用现在应呈现一个事件表。

![事件表的屏幕截图](./images/add-msgraph-01.png)