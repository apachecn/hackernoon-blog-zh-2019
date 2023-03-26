# 如何使用 Node.js、Cosmic JS 和 Stripe 构建单页销售漏斗 App

> 原文：<https://medium.com/hackernoon/how-to-build-a-single-page-sales-funnel-app-using-node-js-cosmic-js-and-stripe-eeb52003e247>

![](img/8cb04bf19779fb38b17762464a96d50f.png)

什么是销售漏斗？为什么它很重要？销售漏斗是你网站的访问者在购买产品之前选择的一条路径。如果你不了解你的销售漏斗，你就无法优化它。建立一个伟大的销售漏斗可以影响访客如何通过漏斗，以及他们是否最终转化。在这个小演示中，我将向您展示如何使用 Node JS、Cosmic JS 和 Stripe 构建一个简单的单页销售漏斗来处理信用卡支付。

# TL；速度三角形定位法(dead reckoning)

*   [演示](https://cosmicjs.com/apps/single-page-sales-funnel)
*   [源代码](https://github.com/cosmicjs/nodejs-sales-page)
*   [条纹](https://stripe.com/docs/stripe-js/elements/quickstart/)
*   [宇宙 JS](https://cosmicjs.com/)

# 我为什么选宇宙 JS

如果你需要快速自由地建立一个销售页面，你可以简单地注册一个免费的宇宙 JS 帐户，复制我的应用程序，定制它，你就大功告成了。你会在几分钟内准备好你的销售漏斗页面。如果您需要添加一个部分或特性，那么您可以简单地下载源代码，并更改该应用程序背后的任何标记或 JavaScript。

# 如何克隆您自己版本的应用程序:

为了克隆此应用程序并拥有自己的自定义副本，您需要执行以下步骤:

*   注册一个免费的宇宙 JS 账户
*   登录宇宙 JS 账号
*   转到[桶](https://cosmicjs.com/buckets)页面
*   点击页面右上角的`Add New Bucket`按钮
*   选择`Choose an app below option`并滚动至应用程序列表
*   尝试找到`Sales Funnel`应用
*   打开应用程序后，点击`Install Free`按钮
*   你可以保持相同的标题，点击`Install App`按钮确认
*   然后，您将被带到应用程序存储桶
*   然后你需要克隆 github repo。打开终端窗口并键入:

```
git clone [https://github.com/cosmicjs/nodejs-sales-page](https://github.com/cosmicjs/nodejs-sales-page)
cd crowd-pitch
npm install
```

*   编辑和更改位于`/crowd-pitch/.env.local`文件中的 Cosmic JS 和 Stripe API 密钥

```
COSMIC_BUCKET=your-bucket-slug
COSMIC_READ_KEY=your-cosmic-read-key
COSMIC_WRITE_KEY=your-cosmic-write-key
STRIPE_KEY=your-stripe-api-key
```

*   在本地机器上运行应用服务器

```
# start the app
npm run server
```

*   在 [http://localhost:3000](http://localhost:3000/) 的浏览器中打开应用程序

# 如何定制应用程序内容

要更改文本、图像和应用程序内容，您需要遵循以下步骤:

*   登录[宇宙 JS 仪表板](https://cosmicjs.com/buckets)
*   转到桶→众筹
*   进入页面→谷歌提款机
*   通过编辑网页每个部分的文本来更改页面部分，如页面标题、页眉等。
*   通过添加您自己的图像来更改页面图像。确保保持相同的图像标题和 slug。
*   点击保存并发布

这部分工作就像任何 CMS 系统，在那里你作出改变的后端和网站可以立即改变。

# 如何向此应用程序添加新功能

这部分和下面将解释如何开发应用程序前端，以及如何深入挖掘定制更多的选项，如布局，css，颜色，以及从用户收集哪些字段。这个应用程序主要使用 Node JS 和 Stripe API 构建。所以让我们看一下 server.js 文件

从上面的代码中可以看出，我们使用了以下 Javascript 组件:

*   [Express:](https://www.npmjs.com/package/express) 节点的轻量级 web 服务器
*   [Express-Handlebars:](https://www.npmjs.com/package/express-handlebars) 此 html 模板库为 [handlebars.js](http://handlebarsjs.com/) 语法
*   [CosmicJs:](https://www.npmjs.com/package/cosmicjs) 从服务器处理与 Cosmic JS API 的交互
*   [条纹:](https://www.npmjs.com/package/stripe)处理与[条纹支付 API](https://stripe.com/docs/payments) 的交互

在 server.js 中，基本上有两个函数来处理服务器路由:

*   app.get('/')来处理用户访问应用程序时的 get 请求。在这个函数中，我们简单地调用 Cosmic JS 从我们的桶中获取所有数据，并将其作为服务器响应局部变量注入。第二部分只是渲染主页视图，这只是一个 HTML/Handlebars 模板页面。
*   app.post('/pay ')来处理付款表单过账。在这个函数中，只需调用一次 Stripe charges API，就可以将一笔费用添加到指定的信用卡中。

# 如何自定义应用程序的布局和 CSS？

如前所述，在这个应用程序中，我们使用 handlebars.js 作为页面模板。从服务器的 get 函数中，我们呈现了一个简单的 html 页面，带有一些 handlebars 标签，用来自 Cosmic JS CMS 的值替换服务器变量。让我们来看看:

如您所见，我们在双花括号中引用了服务器变量。例如`{{ cosmic.metadata.top_logo.url }}`的意思是从 Cosmic JS 中获取变量的值，即图标图像的 url，并将其放在主页中。也有类似的语法使用 handlebars 语法来处理 if 和 loop。有关完整的语法帮助，请参考[车把用户文档](http://handlebarsjs.com/)。

您还可以在主页中更改页面的一些样式，因为我们只是使用了[引导框架](https://getbootstrap.com/docs/4.3/getting-started/introduction/)。对于其他一些样式属性，您可以直接从`/public/css/styles.css`中进行更改

对于应用程序布局有文件`/views/layouts/main.handlebars`

这基本上是我们应用程序中每个页面的主要 html 模板。值得一提的是，我们从客户端引用了几个库，例如:

*   JQuery
*   种类
*   引导程序
*   字体-棒极了
*   Axios.js 来处理 Ajax 调用

# 使用 Stripe 和 Axios 处理信用卡支付

为了在我们的应用程序中接受信用卡支付，我们使用了三个步骤。

*   首先，我们使用 [Stripe card 元素](https://stripe.com/docs/stripe-js/elements/quickstart)在 html 表单中注入信用卡输入元素。

*   当用户点击提交支付按钮时，通过调用`stripe.createToken`来验证信用卡信息。如果成功，这个函数将简单地发送信息到 Strip API 并获得一个有效的令牌。否则它将返回一个无效的令牌。
*   将付款表单数据发布到服务器发布方法。这个方法将获取从客户端表单提交的数据，并通过调用`stripe.charges.create`将它们作为条带费用再次提交。看一看:

如您所见，一旦费用发送到 stripe，如果 Stripe 没有错误，我们将向客户端返回成功。否则，我们将向客户端发回条带错误。

*   最后一步是在客户端，如果成功，我们将在客户端表单上显示支付结果，如果支付失败，则显示错误消息。

# 如何添加多页漏斗到我的应用程序？

有时应用程序需要不止一个页面来捕捉用户的最终动作。如果您想这样做，您可以简单地向服务器视图添加更多的页面、更多的路由，并通过 javascript 函数或服务器 post 方法处理从一个页面到另一个页面的 post。

# main.js 文件呢？

这个文件包含一个函数来处理倒计数。然而，如果您与您的用户有任何其他 javascript 交互，您可以使用它。对于倒计数计数器，我们在 Cosmic JS 服务器上存储了一个名为

```
const dealCountDown = {{ cosmic.metadata.deal_countdown }};
```

然后我们调用`initializeClock`函数，它将向下运行计数，直到这个变量达到零。

查看 main.js 文件，了解完整的实现细节。

![](img/06997e93f6f42b24c7ae61f8ed710fb9.png)

# 通过 A/B 测试检查用户参与度

正如大多数营销人员已经意识到的那样，获得任何高质量流量的成本都是巨大的。A/B 测试让你充分利用现有的流量，帮助你提高转化率，而不必花费在获取新的流量上。A/B 测试可以给你很高的投资回报率，因为有时，即使是最微小的变化也可以导致转化率的显著增加。
对于这个应用程序，我将添加两个`style.css`文件，一旦用户访问网站，我将随机选择一个样式表。样式表的选择将影响颜色主题的外观。所以基本上用户可以看到网站的版本 A 或者版本 b。

然后，我们将样式 A 或 B 加载到`main.handlebars`文件中，如下所示:

我们还有一个 javascript 变量，将在支付过程中使用，以捕捉用户来自哪个页面。该信息将与条带费用信息一起存储。

```
const pageSource = {{#if pageB}} 'pageB' {{else}} 'pageA' {{/if}};
```

然后我们保存页面源沿着服务器 post 方法上的条带收费。

# 结论

在这个应用程序中，我演示了如何利用 Cosmic JS CMS 和其他一些处理支付功能的库来构建显示产品信息和处理信用卡支付的页面。您可以添加一个功能，在提交付款后向用户发送电子邮件。或者添加一个功能，将用户发送到另一个安全页面，让他下载数字产品。Cosmic JS 社区有很多关于如何处理与电子邮件功能、下载功能和第三方平台集成的例子。

一如既往，我真的希望你喜欢这个小应用程序，请不要犹豫，给我发送您的想法或意见，我可以做得更好。

如果你对用 Cosmic JS 构建应用有任何意见或问题，[在 Twitter 上联系我们](https://twitter.com/cosmic_js)和[加入 Slack 上的对话](https://cosmicjs.com/community)。