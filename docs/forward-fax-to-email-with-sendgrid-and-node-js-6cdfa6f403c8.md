# 用 SendGrid 和 Node.js 将传真转发到电子邮件

> 原文：<https://medium.com/hackernoon/forward-fax-to-email-with-sendgrid-and-node-js-6cdfa6f403c8>

![](img/d78a89fd0001c4c81a771241c22c1b6e.png)

现在是 2019 年，你需要接收一份传真。你是做什么的？你可以买一台传真机，把它接到电话线上，然后把你的号码发出去。但现在是 2019 年，而不是 1979 年，我们生活在未来，所以让我们抓住 Node.js，选择几个 API，将传真转换为电子邮件。

![](img/4f932626e401f93580b6a0fc814a0957.png)

你将需要一个 [Twilio 账户](https://www.twilio.com/try-twilio)、一个 [SendGrid 账户](https://signup.sendgrid.com/)和[这个 noise](https://www.youtube.com/watch?v=XC3uDuRXJyA) 来提醒你在构建你自己的传真到电子邮件转换器时错过了什么。

# 接收传真

我们将使用 Twilio 号码接收传入的传真，而不是笨重的机器。你将需要一个支持传真的 Twilio 号码来构建这个应用程序，因此请登录你的 [Twilio 帐户](http://twilio.com/console)。您可以[购买一个新号码](https://www.twilio.com/console/phone-numbers/search)或者您[可能已经有一个](https://www.twilio.com/console/phone-numbers/incoming)，只需寻找此图标，表明它可以接收传真:

当你有一个数字准备好了，我们将需要设置接收一些网络挂钩。你可能认为传真的工作原理和信息传递一样，但它更像是语音通话。我们需要回应两个进来的网络钩子。对于第一种情况，我们有两种选择:接收或拒绝传真。我们可以用`[<Receive>](https://www.twilio.com/docs/fax/twiml/receive)`或`[<Reject>](https://www.twilio.com/docs/fax/twiml/reject)`传真 TwiML 动词来做到这一点。

拒绝接收传真会挂断连接，我们就完成了。选择接收传入的传真意味着 Twilio 将代表您接听来电并接收传真。要做到这一点，我们需要设置第二个 webhook URL 作为`<Receive>`元素的 action 属性，当传真被完全接收时将会请求这个属性。

第二个 webhook 是我们将要做的所有工作，将传真下载为 PDF 格式，并作为电子邮件发送。我们将使用 Node.js 将它构建为一个 [Twilio 函数](https://www.twilio.com/docs/api/runtime/functions)(尽管您可以用任何语言来实现这一点，并自己托管应用程序)。

# 下载传真

## 配置

我们将使用`request` npm 模块下载传真 PDF 文件，就像[我的队友 Sam 对 MMS 消息中的媒体所做的那样](https://www.twilio.com/blog/2018/05/how-to-receive-and-download-picture-messages-in-node-js-with-twilio-mms.html)，并且还将它发送到 [SendGrid API](https://sendgrid.com/docs/API_Reference/api_v3.html) 。打开 Twilio 控制台的[运行时依赖](https://www.twilio.com/console/runtime/functions/configure)部分，添加`request`版本 2.88.0。

当您在 config 部分时，[在您的 SendGrid 帐户](https://app.sendgrid.com/settings/api_keys)中创建一个 API 键(确保它有发送电子邮件的权限)，并将其保存为一个名为`SENDGRID_API_KEY`的环境变量。

在构建函数之前，我们还需要两位配置。你需要一个接收传真的电子邮件地址，以及一个发送传真的电子邮件地址。将`TO_EMAIL_ADDRESS`和`FROM_EMAIL_ADDRESS`添加到环境变量中。

在进入下一部分之前，请确保保存配置。

# 编写函数

创建一个[新功能](https://www.twilio.com/console/runtime/functions/manage)并选择空白模板。我们将通过要求`request`并创建处理函数来开始代码。

```
const request = require('request');

exports.handler = function(context, event, callback) {

}
```

webhook 请求向我们发送一个 URL，它描述了包含传真的 PDF 文件的位置。URL 在参数`MediaUrl`中。

我们要用`request`下载那个 PDF。为了将它发送到 SendGrid API，我们将需要它作为一个`Buffer`，我们可以通过将编码设置为`null`来使用`request`来做到这一点。

将以下内容添加到您的函数中:

```
exports.handler = function(context, event, callback) {
  const faxUrl = event.MediaUrl;

  request.get({ uri: faxUrl, encoding: null }, (error, response, body) => {
    // body is the PDF file as a Buffer object
  });
}
```

现在我们需要构建[我们想要发送给 SendGrid API](https://sendgrid.com/docs/API_Reference/Web_API_v3/Mail/index.html) 的请求。我以前探索过这个问题，当时我构建了一个函数来将短信作为电子邮件转发。在回调中添加以下代码:

```
request.get({ uri: faxUrl, encoding: null }, (error, response, body) => {
    const email = {
      personalizations: [{ to: [{ email: context.TO_EMAIL_ADDRESS }] }],
      from: { email: context.FROM_EMAIL_ADDRESS },
      subject: `New fax from ${event.From}`,
      content: [
        {
          type: 'text/plain',
          value: 'Your fax is attached.'
        }
      ],
      attachments: []
    };
    // more to come
  }
```

我们使用之前保存的环境变量添加`to`和`from`电子邮件地址。主题说有一个新传真，来自发送它的号码，内容是一个简单的消息，说有一个传真附件。最后，我们添加一组附件。

如果传真下载成功，我们会将其作为附件添加到电子邮件中。为此，我们将其作为具有三个键的对象提供:

*   `content`:一个 base64 编码的字符串，来自我们下载的 PDF 的`Buffer`
*   `filename`:根据传真的 Sid 标识符创建
*   `type`:文件的 MIME 类型，我们可以直接从下载传真的响应的头中得到

```
request.get({ uri: faxUrl, encoding: null }, (error, response, body) => {
    const email = { ... };
    if (!error && response.statusCode === 200) {
      email.attachments.push({
        content: body.toString('base64'),
        filename: `${event.FaxSid}.pdf`,
        type: response.headers['content-type']
      });
    }
    // more to come
  }
```

如果下载传真时出现错误，我们会跳过添加附件，但我们会继续发送电子邮件作为通知。

现在我们已经建立了电子邮件，我们需要将它发送到 SendGrid API。我们将以 JSON 的形式发送，将我们在这里创建的`email`对象打包为主体，并添加我们之前创建的 API 令牌作为授权。

如果[响应成功，状态码为 202](https://sendgrid.com/docs/API_Reference/Web_API_v3/Mail/index.html#apiv3example-post)，那么我们可以发送一个空的 TwiML `<Response>`给 Twilio，让它知道一切正常。如果有一个错误，那么我们把错误或者主体作为第一个参数传递给回调函数，这样我们的函数就把它记录为一个错误。

```
request.get({ uri: faxUrl, encoding: null }, (error, response, body) => {
    const email = { ... };
    if (!error && response.statusCode == 200) {
      // add attachment
    }
    request.post(
      {
        uri: 'https://api.sendgrid.com/v3/mail/send',
        body: email,
        auth: {
          bearer: context.SENDGRID_API_KEY
        },
        json: true
      },
      (error, response, body) => {
        if (error) {
          return callback(error);
        } else {
          if (response.statusCode === 202) {
            return callback(null, new Twilio.twiml.VoiceResponse());
          } else {
            return callback(body);
          }
        }
      }
    );
  }
```

这就是我们转发传真所需要的。完整代码如下:

```
const request = require('request');

exports.handler = function(context, event, callback) {
  const faxUrl = event.MediaUrl;

  request.get({ uri: faxUrl, encoding: null }, (error, response, body) => {
    const email = {
      personalizations: [{ to: [{ email: context.TO_EMAIL_ADDRESS }] }],
      from: { email: context.FROM_EMAIL_ADDRESS },
      subject: `New fax from ${event.From}`,
      content: [
        {
          type: 'text/plain',
          value: 'Your fax is attached.'
        }
      ],
      attachments: []
    };
    if (!error && response.statusCode === 200) {
      email.attachments.push({
        content: body.toString('base64'),
        filename: `${event.FaxSid}.pdf`,
        type: response.headers['content-type']
      });
    }
    request.post(
      {
        uri: 'https://api.sendgrid.com/v3/mail/send',
        body: email,
        auth: {
          bearer: context.SENDGRID_API_KEY
        },
        json: true
      },
      (error, response, body) => {
        if (error) {
          return callback(error);
        } else {
          if (response.statusCode === 202) {
            return callback(null, new Twilio.twiml.VoiceResponse());
          } else {
            return callback(body);
          }
        }
      }
    );
  });
}
```

给函数一个路径并保存它。

# 把所有的放在一起

返回编辑您的传真号码。在“语音和传真”部分，确保您已设置为接受传入的传真。

![](img/6564bcdf97a94a11b4d18420a96e9af0.png)

对于“有传真进来”,选择 *TwiML* ,然后点击红色按钮创建一个新的 TwiML 接收箱来接收传入的传真呼叫。输入以下 TwiML，用函数 URL 替换操作 URL:

```
<?xml version="1.0" encoding="UTF-8"?>
<Response>
  <Receive action="FUNCTION_URL" />
</Response>
```

保存号码配置，您就可以通过电子邮件接收传真了。

# 测试它

正如我们已经确定的，现在是 2019 年，我们没有传真机来测试这一点。现在，你可以去你当地的图书馆或打印店借他们的，或者打开 [Twilio API explorer 并通过 API](https://www.twilio.com/console/runtime/api-explorer/fax/fax/create) 给自己发一份传真(你可以在这里使用你现有的传真号码作为`To`和`From`号码)。发送传真时，你需要把 PDF 文件放在 Twilio 能拿到的地方。如果你没有，可以在这里使用我们的测试 PDF 文件。

当你将传真机发送到世界各地时，不要忘记播放传真机的真实声音。

等待几分钟(传真需要时间！)然后查看你的邮箱。

![](img/da148b08f8fb23366fbfb1c6040f1ff5.png)

你应该已经收到你的新传真了。

庆祝的时间到了！

![](img/0aad1baaad294766ae7e20b09a1d7878.png)

# 在写这篇文章的时候，没有传真机受到损害

凭借 Twilio number、JavaScript、Twilio 函数、TwiML Bin 和 SendGrid API 的强大功能，我们现在可以将传真直接发送到我们的电子邮件收件箱。

我们已经看到了如何使用`[request](https://www.npmjs.com/package/request)`下载文件，然后将它们直接发送到 [SendGrid API](https://sendgrid.com/docs/API_Reference/Web_API_v3/Mail/index.html#apiv3example-post) 。您也可以使用同样的技术将[收到的彩信](https://www.twilio.com/docs/sms/tutorials/how-to-send-sms-messages)转发到您的电子邮件。

你曾经想象过 JavaScript 和电子邮件会解决传真吗？对传统科技的未来有什么想法吗？请在下面的评论中给我留言，或者在 Twitter[上](https://twitter.com/philnash)告诉我你的其他复古未来发明。2019 传真万岁！

*标题中的传真图标由 Emojione 第 2 版提供。*

*原载于*[*www.twilio.com*](https://www.twilio.com/blog/fax-email-sendgrid-nodejs)*。*