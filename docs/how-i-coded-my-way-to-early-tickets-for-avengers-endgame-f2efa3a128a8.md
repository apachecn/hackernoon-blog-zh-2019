# 我是如何提前买到复仇者联盟决赛门票的

> 原文：<https://medium.com/hackernoon/how-i-coded-my-way-to-early-tickets-for-avengers-endgame-f2efa3a128a8>

![](img/0fb85282c20f4910debbfa8de306d461.png)

[https://www.marvel.com/movies/avengers-endgame](https://www.marvel.com/movies/avengers-endgame)

2019 年 4 月 26 日，我们这一代人最受炒作和最受期待的电影《复仇者联盟》上映了，我在班加罗尔的 IMAX 首映日抢了大厅里最好的座位。

当这部电影的上映日期公布时，我发现我已经预订了三个月前同一天晚上回家的机票。我住在班加罗尔，离机场差不多有 3 个小时的路程，我知道如果我不能买到早间演出的票，我等了将近一年的电影很可能会被毁掉&我将无法在 IMAX 上观看我渴望已久的电影。

我的处境很艰难，在这个城市，你需要至少提前 3-4 天预订周末的门票，而且随着《复仇者联盟》的大肆宣传，必须采取一些措施。我和我的朋友会定期更新 [bookmyshow](https://in.bookmyshow.com) ，这是一个流行的活动门票预订门户网站，这样我们就不会错过门票，但我知道这种策略永远不会奏效。

唯一的解决方案是以某种方式自动化，编写一个脚本，定期刷新网站，当门票可用时，以某种方式提醒我们不要错过它。

## 找出什么时候票卖完了

因此，每部即将在 bookmyshow 上上映的**电影都有自己的一页，每个城市都不一样。对于《复仇者联盟》在班加罗尔的结局，是这样的[https://in . bookmyshow . com/Bengal uru/movies/Avengers-Endgame/et 00090482](https://in.bookmyshow.com/bengaluru/movies/avengers-endgame/ET00090482)**

![](img/5e2de14a056fc2c462b49024d641b587.png)

Screenshot from bookmyshow.com portal

唯一的区别是它还在【T10 即将推出】阶段的时候没有**订票**按钮。因此，我需要做的就是定期加载页面，并检查它何时最终获得按钮元素。它的 shell 脚本非常简单。

```
# curl the bookmyshow websiteoutput=`curl -s [https://in.bookmyshow.com/bengaluru/movies/avengers-endgame/ET00090482](https://in.bookmyshow.com/bengaluru/movies/avengers-endgame/ET00090482) | grep -c 'Book Tickets'`if [ $output -gt 0 ]
then
  # Alert somehow, will come to that later
fi
```

为了定期运行它，我必须将脚本作为 [cron 作业](https://en.wikipedia.org/wiki/Cron)运行。但是把它加到我自己的机器上意味着我必须一直开着它，这是不可行的。我需要在某个远程服务器上添加这个 cron 作业，无论发生什么，它都将继续运行。因此，我将它添加到我为测试准备的远程 AWS 服务器中。如果您需要为自己提供 ec2 AWS 实例，您可以参考我的另一篇[中型文章](/@noel.varghese.22/setting-up-a-production-environment-using-our-local-development-server-and-aws-f5eea3b5be60)的第一部分。我添加了下面的 crontab 条目

```
# Run the check_ticket script every minute as a scheduled cron
*/1 * * * * sh /home/ec2-user/check_ticket.sh
```

## 有票时提醒

现在我需要某种方式，让脚本能够在票到手后提醒我们。我唯一随身携带的东西就是我的手机，所以发送短信或触发自动呼叫似乎是我的最佳选择。我发现我可以使用[Twilio](https://www.twilio.com/)API 来免费实现同样的功能，因为它们在开始时提供了足够的信用来生成相当多的调用&消息。

第一部分是获得一个 Twilio 号码，并注册几个手机号码，这些号码将使用这里的[提供的](https://www.twilio.com/docs/usage/tutorials/how-to-use-your-free-trial-account)文档获取电话和消息。[短信](https://www.twilio.com/docs/sms/quickstart/python)和[语音](https://www.twilio.com/docs/voice/quickstart/python)的快速入门指南简单易懂。最后，我写了一个脚本，它会通过给不同的号码发送消息来提醒两个人，以确保我的组中至少有人会被提醒。

```
#!/usr/bin/pythonfrom twilio.rest import Client# Your Account SID from twilio.com/console
account_sid = "xxxx"
# Your Auth Token from twilio.com/console
auth_token  = "xxxx"client = Client(account_sid, auth_token)message = client.messages.create(
    to="+xxxxxxxxxxxx",
    from_="+zzzzzzzzzzzz",
    body="Book Tickets for avengers endgame")call = client.calls.create(
    to="+yyyyyyyyyyyy",
    from_="+zzzzzzzzzzzz",
    url="https://handler.twilio.com/twiml/xxxx")# Send message
print(message.sid)# Trigger a call
print(call.sid)
```

我的 **check_ticket.sh** 脚本在计算出车票可用时调用上面的脚本。

```
# Curl the bookmyshow websiteoutput=`curl -s [https://in.bookmyshow.com/bengaluru/movies/avengers-endgame/ET00090482](https://in.bookmyshow.com/bengaluru/movies/avengers-endgame/ET00090482) | grep -c 'Book Tickets'`if [ $output -gt 0 ]
then
  /usr/bin/python /home/ec2-user/alert_by_twilio.sh
fi
```

## 结论

这就是全部——远程服务器上的两个小脚本就可以完成所有设置。当我为《复仇者联盟》建立网站时，我和我的朋友分别接到电话和信息，我们立即前往网站。门户网站花了一些时间来上传不同地点的各个大厅的门票。在多次付款失败后，我们终于得到了大厅里最好的座位，并有了一个惊人的免费剧透，第一次，IMAX 的电影体验。