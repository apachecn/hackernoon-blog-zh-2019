# 写一些真正像样的日志

> 原文：<https://medium.com/hackernoon/writing-some-actually-decent-logs-4f547aa4f2f6>

## 日志的最佳实践和建议

![](img/690704f0ba8a8defbf9bb4b457cc193a.png)

日志记录通常不被视为一项至关重要的前期任务。一位工程师曾经对我说，*“嗯，日志有点像注释，对吗？”*。不，无名工程师。它们不像评论。它们不仅仅是评论。它们是查看系统性能最灵活的方式之一。他们经常被忽视、不被喜爱或被滥用。

我看过一些可怕的日志。我在凌晨 3 点登录了一台服务器，结果却在一个文件中一遍又一遍地发现类似`processing...`的日志。没用。因此，鉴于我的不眠之夜和疯狂的故障排除，我想我应该把一些可以把你的日志从日志变成**日志**的实践放在一起。

![](img/579727ac1579cdf705f6b1b22c0a86fb.png)

No not these types of logs, oh just read the damn article.

# 把你所有的鸡蛋放在一个篮子里。

也称为*日志聚合。这里的想法是为所有的应用程序日志建立一个单一的门户。您不希望在服务器之间跳来跳去，以找出应用程序的哪个实例出现了故障。人们最常用的工具是 [ELK stack](https://www.elastic.co/elk-stack) 。Guru99 制作了[一个伟大的深入](https://www.guru99.com/elk-stack-tutorial.html)到它是什么以及如何设置它。我强烈建议将某种日志聚合作为您的第一个访问端口。*

## 偶尔吃一些鸡蛋

经常被遗忘的事情是修剪日志。弹性搜索实例因大量不相关的日志而超载。像[馆长](https://postmarkapp.com/blog/tools-we-use-curator-for-elasticsearch)这样的工具在吹走旧原木时派上了用场。考虑到法规和调查要求，您需要就日志的保留期进行对话。

# 用可解析的格式写日志

这与上述建议有关。例如，当你用 JSON 写日志时，你就有了一些有趣的机会。JSON 限制了可读性，但是它创建了可查询、可搜索的日志。这是日志聚合向前迈出全新一步的地方。例如，在 Java 中使用 Slf4j，可以编写这样的日志:

```
log.info("A user has been created", value("userId", user.getId());
```

现在，当您搜索日志时，您可以在 Kibana(ELK 堆栈中的 K)中执行如下查询:

```
userId: 1234 AND message: "A user has been created"
```

不再需要筛选文件和编写随机的`grep`命令。只需查询您的 elasticsearch 存储。这同样适用于像关联 id 这样的东西，因此您可以跟踪消息在整个系统中的移动。

# 包括人类可读的消息

好吧，hotshot，你的日志在 JSON 里，存储在 Elasticsearch 里。您很好地利用了 Kibana 来查询它们，并且玩得很开心。但是不要忘记基础！包括一个简单的、人类可读的消息，描述正在发生的事情。把它放在自己的地方，作为日志的一部分。这不需要很长的句子，甚至根本不需要一个句子。甚至类似于:

```
log.info("UserCreated", ...);
```

需要有某个字段简洁地说明正在发生什么。这听起来是显而易见的，但它经常被忽略，并且会弹出类似`processing user`的日志消息。*处理*实际是什么意思？你要删除它们吗？谁知道呢！就像您的变量、应用程序名称和 REST 资源一样，您的日志应该清晰明了。

# 持续记录

通常，日志消息在您的应用程序中会有一点不同。为了避免这种情况，我建议将一些关键日志抽象成一个独立的服务。这防止了重复，但也创建了一个单一的地方，您可以检查，看看什么是正确的日志消息。

```
public class MyController {
    public void handleRequest(Request req) {
        MyLogger.logIncomingRequest(req); }
}public class MyLogger {
    public static void logIncomingRequest(Request req) {
        log.info("...");
    }
}
```

这种模式有一些缺点。您将日志隐藏在服务后面，并且您失去了日志可以提供的注释值。这样做的好处是，当您的核心日志都在同一个类中时，保持它们的一致性要容易得多。

# 在多个级别记录

一旦在日志中捕获了核心业务事件，您可能会想停下来。不要！继续，你还有很多事要做！在所有这些`UserCreated`消息之间，有数据库写、数据库读和对象创建。这就是为什么上帝创造了多个日志级别。有效使用这些日志的一个很好的例子是:

```
log.debug("UserCreationRequestReceived", ...);
log.debug("CheckingToSeeIfUserExists", ...);
log.trace("DB Query Completed", ...);
log.trace("MappingToUserObject", ...);
log.debug("UserDoesNotExist", ...);
log.debug("Creating User", ...);
...log.info("UserCreated", ...);
```

这看起来有些过分，但是当你处于危机情况下，你希望有这种可见性。那么如何控制噪音呢？

# 无需重启即可更改日志级别

当您在多个级别进行日志记录时，您需要决定哪些日志将被静默。例如，如果您将日志级别设置为`info`，那么您将禁止`debug`和`trace`级别的日志。如果你把它设置为`trace`，你将得到一切。

这里最重要的是，你可以在危机中快速拨打你的日志细节。如果你已经在使用 Spring Boot，你可以用[致动器](https://docs.spring.io/spring-boot/docs/current/actuator-api/html/#loggers-setting-level)免费这么做。这是非常特定于语言的，但是对于众所周知的语言和框架，已经存在许多工具。这是值得实行的。它是灾难恢复自动化的关键部分。

# 在日志中包含可查询的、有意义的值

如果您有一个 API，并且希望将与给定请求相关的所有活动链接在一起，那么将一个可查询的值附加到日志行是将常见活动分组在一起的一种非常强大的方法。这可以是在请求时生成的简单的 UUID。想象一些如下所示的日志记录代码:

```
log.info("UserArrived", 
    value("sessionId", "1234"),
    value("userId", "5432")
);
// Some stuff
log.error("UserError", 
    value("cause", "User account disabled"), 
    value("sessionId", "1234"),
    value("userId", "5432")
);
// Some more stuff
log.info("UserAccountEnabled", 
    value("sessionId", "1234"), 
    value("userId", "5432")
);
```

现在，当您在 Kibana 中查找给定的会话时，您需要做的就是查询该会话 ID。嘣，你就拥有了系统中发生的一切。如果您想了解那个特定的会话，您可以搜索`sessionId: 1234`，如果您想了解那个用户的全部信息，可以搜索`userId: 5432`。

这也将适用于微服务，创建一个将所有用户行为联系在一起的单一线程。诀窍是选择正确的价值观。会话 id 是可以的，但是它们可能是一个难题。首先，您需要找到会话，以便找到 ID。像用户 id 这样的东西更好，因为当您开始调查时，这些信息可能对您有用。这就产生了一个被称为“可追溯性”的属性。

如果你想关注的话，我会在[推特](https://twitter.com/chris_cooney)上详细记录我的想法！