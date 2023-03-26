# 建造一个电报机器人🤖自动化 Web 流程

> 原文：<https://medium.com/hackernoon/building-a-telegram-bot-to-automate-web-processes-38a6ab9e664f>

## 使用 Python、Selenium 和 Telegram

![](img/f24a8355360bc710298b5817215c7870.png)

Created by [Katerina Limpitsouni](https://twitter.com/ninalimpi)

你认为在网上填写表格既无聊又乏味吗？我非常同意你。我写这篇文章是为了记录创建一个电报机器人来自动化 web 过程的过程，并展示我们如何通过使用编程来自动化简单的任务来节省时间和精力。我们开始吧！

**动机和想法💡**

![](img/d6057cb28fee5e94726780be2cd71304.png)

Created by [Katerina Limpitsouni](https://twitter.com/ninalimpi)

当我偶然发现一些有趣的东西时，我经常给我的朋友和家人发文章。然后，我们有很多要分享和讨论的，我们经常想，如果我们能强调和评论这篇文章，那将会很棒。在谷歌搜索解决方案后，我找到了 Outline.com 的，这是一个通过去除杂乱来增强在线文章可读性的工具，也允许读者进行注释。过程很简单:

1.  复制你想要的文章链接(如[https://towardsdatascience . com/an-introduction-to-JSON-c 9 ACB 464 f 43 e](https://towardsdatascience.com/an-introduction-to-json-c9acb464f43e))。
2.  转到[Outline.com](http://outline.com)，把它粘贴到框中，点击“创建大纲”(一个“大纲”基本上就是 Outline.com 为你的文章创建的——一个简单、易读、好看的版本)
3.  几秒钟之内，outline 会将你重定向到你的“Outline ”,并带有一个你可以与任何人分享的定制链接。任何有链接的人都可以看到您的突出显示和注释。

然而，我很快意识到，每次复制链接，继续[Outline.com](http://outline.com)，然后获取另一个(大纲的)链接来分享，这将是非常乏味的。因此，我决定自动化这个过程。由于我主要使用 Telegram 来共享这些链接，所以我决定编写一个 Telegram bot，它可以自动为我获取大纲链接。

**技术堆栈📚**

![](img/cc962396cfa2acc89e794cc62e8e3cee.png)

Created by [Katerina Limpitsouni](https://twitter.com/ninalimpi)

为了构建一个电报机器人，让我们使用 Python(因为它不太冗长，易于使用)3.7.x 版本，并且[有一个优秀的 API](https://github.com/python-telegram-bot/python-telegram-bot) 包装器。为了自动化这个过程，让我们使用 [Selenium](https://www.seleniumhq.org/) ，它是用于此目的的最广泛使用的开源工具。我们还将使用日志(更好地记录我们的程序)和时间(让它休眠)。因此，我们将使用以下 python 库:

*   [电报 Python 包装器](https://github.com/python-telegram-bot/python-telegram-bot)
*   [日志](https://realpython.com/python-logging/)(默认应该是 Python)
*   [Selenium Chrome WebDriver](https://www.seleniumhq.org/download/) (在这篇文章中，我们使用 macOS 上的 Chrome，但你可以选择任何 OS 和浏览器，因为 Selenium 支持它们中的大多数)。
*   [时间](https://docs.python.org/3/library/time.html)(默认应为 Python)

**流程👨🏻‍💻**

![](img/4097d5d14f479a2b8c88a40b545466dd.png)

Created by [Katerina Limpitsouni](https://twitter.com/ninalimpi)

有了清晰的概念和技术，让我们开始写一些代码。以下部分分为几个小节。如果您熟悉它，请随意跳过。

*设置日志*

让我们使用 Python 中的日志模块来设置日志。这是重要的一步，因为它允许我们更好地调试程序并监控程序的流程。你可以在这里阅读更多相关信息[。](https://realpython.com/python-logging/)

导入日志模块后，我们可以设置它:

```
logging.basicConfig(format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',level=logging.INFO)
```

这是最基本的。

*从机器人父亲处获取授权密钥。*

每个电报机器人都需要来自电报的认证密钥。它是一个复杂的字母数字字符串，需要进行身份验证并向 Bot API 发送请求。它帮助 Telegram 识别机器人的所有者。

在电报中，前往[机器人父亲](https://telegram.me/botfather)——一个通过电报处理所有机器人相关事务的机器人。接下来，我们可以发送`/newbot`命令来创建一个新的机器人。BotFather 需要相同的用户名和名称。

我们可以把它复制下来，存放在一个安全的地方。你可以在这里阅读更多关于机器人父亲及其各种功能的信息。

*输入授权标识，启动更新程序*

首先，让我们导入所有必需的库:

```
from telegram.ext import Updater, CommandHandler, MessageHandler, Filters
```

updater 对象从 Telegram 接收更新，并将它们发送给 dispatcher。你可以在这里阅读更多关于 T21 的信息。它基本上代表了电报。Bot 对象并充当它的前端。我们可以这样设置更新程序，并将其连接到调度程序:

```
updater = Updater(token='TOKEN', use_context=True)
dispatcher = updater.dispatcher
```

*注意:use_context=true 是一个特殊的参数，仅在库的版本 12 中需要。*

*设置更新器、调度器和启动/停止*

建议所有电报机器人都有全局命令:`/start`、`/help`和`/settings`。`/start`尤其重要，因为它开始与机器人互动。

让我们定义一个函数来处理/start 命令(当用户启动 bot 时)

```
def start(update, context):
    context.bot.send_message(chat_id=update.message.chat_id,        text="I'm a bot, please talk to me!")
```

现在，我们需要一个`CommandHandler`(还记得上面的导入吗？)来处理来自用户的`/start`命令。我们还需要在 dispatcher 中注册它:

```
dispatcher.add_handler(CommandHandler('start', start))
```

接下来，让我们添加`/read`命令——我们的机器人的主要特性。首先，让我们导入必要的:

```
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
```

让我们定义一个读取函数来处理`/read`命令

```
def read(update, context):
```

并设置一个处理程序，将其添加到 dispatcher:

```
dispatcher.add_handler(CommandHandler('read', read))
```

接下来，我们需要使用前面下载的 Selenium 驱动程序。让我们添加导入必要的项目:

```
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
```

现在，让我们设置一个浏览器对象。浏览器对象基本上是一个自动化的浏览器，可以由我们编写的 Python 命令来控制。你需要指定你之前下载的 chrome 驱动的目录。

```
browser = webdriver.Chrome('**directory of chromedriver**')
```

让我们去参观 Outline.com 吧:

```
browser.get('https://www.outline.com')
```

现在有趣的部分来了。*我们如何让它在大纲网站上提交用户到表单的链接？*我们可以使用 Chrome dev 工具来检查表单，并复制其 ID 以使 selenium 浏览器能够找到它。我们还可以提取链接(`/read`命令的唯一参数)并传递它:

```
linkbar = browser.find_element_by_id('source')
linkbar.send_keys(context.args) 
linkbar.send_keys(Keys.ENTER)
```

我们将不得不停止程序 10 秒钟，因为 URL 从仅仅[outline.com](http://outline.com)变成 outline.com/something 需要一些时间——这是我们想要的链接。最后，我们使用浏览器的 current_url 属性将获得的链接发送回用户:

```
time.sleep(10)
context.bot.send_message(chat_id=update.message.chat_id, text=browser.current_url)
```

最后，我们清理工作簿并将关键信息放入一个`main`函数中:

```
def main(): # Create updater and pass in Bot's API token. updater = Updater(token='your_key_here', use_context=True) # Get dispatcher to register handlers dispatcher = updater.dispatcher # answer commands dispatcher.add_handler(CommandHandler('start', start)) dispatcher.add_handler(CommandHandler('read', read)) # start the bot updater.start_polling() # stop updater.idle()if __name__ == '__main__':
   main()
```

这就是我们如何建造这个电报机器人的。下面是我们最终的代码:

请随意使用这个作为模板！要开始，您可以在这里分叉我的存储库，或者从上面的要点复制代码！

[](https://github.com/raivatshah/OutlineBot/blob/master/main.py) [## raivatshah/OutlineBot

### 电报机器人🤖自动获取 OutlineBot 链接的过程

github.com](https://github.com/raivatshah/OutlineBot/blob/master/main.py) 

祝编码愉快！以下是一些您可能会觉得有用的资源:

[](https://github.com/python-telegram-bot/python-telegram-bot) [## python 电报机器人/python 电报机器人

### 我们为你做了一个你无法拒绝的包装纸。通过以下方式为 python-telegram-bot/python-telegram-bot 开发做出贡献…

github.com](https://github.com/python-telegram-bot/python-telegram-bot) [](https://python-telegram-bot.readthedocs.io/en/stable/) [## 欢迎来到 Python Telegram Bot 的文档！- Python Telegram Bot 11.1.0 文档

### 欢迎来到 Python Telegram Bot 的文档！

- Python Telegram Bot 11.1.0 文档欢迎使用 Python Telegram Bot 的文档！python-telegram-bot . readthedocs . io](https://python-telegram-bot.readthedocs.io/en/stable/)