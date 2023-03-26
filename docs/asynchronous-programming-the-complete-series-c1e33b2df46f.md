# 异步编程:第 1 部分(异步 JavaScript 基础)

> 原文：<https://medium.com/hackernoon/asynchronous-programming-the-complete-series-c1e33b2df46f>

![](img/1dcc597add46cad9e3ff84d6ecc96844.png)

在这个博客文章系列中，我们将讨论 javascript 最难的话题之一，著名的“异步 javascript”。如果您对 javascript 语法有基本的了解，那么您就可以阅读本系列。为了全面理解异步编程，我们将涉及所有相关的内容。

开始之前的最后一件事，本系列中有许多代码片段，如果您启动机器并自己玩代码，这将对您非常有帮助，这将加快您的理解，并且您将有机会实践代码。

## 异步编程

setTimeout(arg1，arg2): setTimeout()是一个内置函数，用于在给定时间后执行一些给定的代码。arg1 是您希望执行的函数形式的代码，arg2 是您希望代码(arg1)执行的毫秒时间。

期待下面的代码输出，然后在您的机器上运行。

```
*console*.log(“I am first”);*console*.log(“I am second”);*console*.log(“I am third”);//output1
I am first
I am second
I am third
```

现在来看这个

```
*console*.log(“I am first”);setTimeout(function(){*console*.log(“I am second”);}, 2000);*console*.log(“I am third”);//output2
I am first
I am third
I am second
```

output1 和 output 2 不一样，为什么？这是因为我们通过使用 setTimeout()函数在代码的第二部分引入了异步，也就是说，第二个代码片段有两个部分，一个是“现在”，即将执行的代码

```
*console*.log(“I am first”);setTimeout(function(){...}, 2000);console.log("I am third");
```

另一个是“稍后”，这个代码将稍后执行，即两秒钟后。

```
*console*.log(“I am second”);
```

任何现在不能执行的代码都将异步执行，即 jsEngine 将“稍后”将我们的代码部分交给浏览器提供的一个 WebApi。两秒钟后，这个 WebApi 将通知我们的环境，以便 js 运行时可以执行 arg1 函数。当 WebApi 执行代码的“稍后”部分时，jsRuntime 将继续执行代码的“现在”部分，我们会在“我是第二个”之前看到“我是第三个”，因为 jsRuntime 在 WebApi 返回 setTimeout()函数的响应之前执行第三行。这种并行执行的代码被称为异步。但是请注意，js 是单线程的，这意味着 jsRuntime 一次只执行一件事情。我们将更深入地了解这一切是如何发生的，但在此之前，让我们学习如何通过请求在应用程序和服务器之间建立连接。

## 向服务器发出请求

你可能已经很熟悉了，在我们的大多数网络应用程序中，我们使用的数据并不在我们的机器上，而是来自一个不知道在哪里的数据库，无论是 facebook，medium，insta 每当你打开你的 feed，你看到的数据都来自一个非本地的来源，这是怎么回事呢？很明显，我们正在向服务器发送一些信息，告诉它我们需要什么数据，然后服务器会检查，如果我们有权访问所请求的数据，那么它会返回我们的数据，否则它会忽略我们的请求。

我们通过 XMLHttpsRequest 类型的对象与服务器通信，尽管它的名字可以用来访问所有类型的数据。

```
const request = XMLHttpRequest();// XMLHttpsRequest() is a constructor which initializes our variable request with an objectrequest.open("GET", "http://puzzle.mead.io/puzzle?wordCount=3");//We are calling a method open(), first argument specifies the type of request, second argument is an API of source of our data,this source returns a random puzzle request.send();//send() method sends our requests to the server
```

我们的请求发生了什么，我们如何知道它有我们请求的数据或者它失败了？接下来让我们来看看。readystatechange 是一个事件，每当我们的请求状态改变时就会触发。readyState 和 status 是两个属性，可以用来检查我们的请求是否成功，请参考 mdn 文档了解更多信息

```
request.addEventListener(“readystatechange”, (e) => {if(e.target.readyState === 4 && e.target.status === 200){const data = JSON.parse(e.target.responseText);//responseText property of our request has response in form of json text and we use JSON.parse() method to parse json.console.log(e.target); }else if(e.target.readyState === 4){

     console.log("Error");
})
```

## 为什么异步

现在您知道了如何发出请求并获取数据。如果您已经运行了前面部分代码，那么您一定已经注意到请求需要一些时间来返回响应，这个时间取决于许多因素。

因此，如果我同时请求两个谜题，下一个请求只有在我们得到前一个请求的响应后才会继续，因此，如果一个请求需要一秒钟，那么我们的两个请求都需要 2 秒钟才能完成，在这段时间内，你的用户界面会被阻塞，用户会感到沮丧。

![](img/b7d671dc30817590d7fad9058ade9afd.png)

异步地，由于请求分布在不同的 WebApi 中，我们不必等待第一个请求完成，就可以开始第二个请求。我们将启动第一个请求，将其传递给 webApi，然后启动第二个请求，将其传递给另一个 webApi，然后继续执行代码，而不等待请求的响应。我们没有通过使用异步编程来改变服务器处理请求的时间，在两种情况下是一样的。但是两个请求的等待时间是重叠的，如果我们有大量的请求，这将大大减少总时间。这就是为什么异步是进行 http 请求的默认方式。

![](img/f976eb629a48efe18dbce2e1b07831f0.png)

## 幕后:事件循环、回调队列和堆栈

因为 js 是单线程的，也就是说，js 运行时会在任何时候执行一段代码，那么它是如何决定何时打印从服务器返回的响应的呢？你的答案可能是，在它得到响应的瞬间，但是如果 jsRuntime 在同一瞬间执行我们文件中那些请求之后的一些代码，它如何决定做什么呢？

让我们用一个例子来解决这个问题，下面代码的输出应该是什么

```
*console*.log(“starting”);setTimeout(() => {*console*.log(“waiting 1”);}, 2000);setTimeout(() => {*console*.log(“waiting 2”);}, 0);*console*.log(“stopping”);//output
starting
stopping
waiting 2
waiting 1
```

您一定想知道，尽管 second setTimeout()函数的等待时间是 0 秒，但它仍然在“停止”后打印。这是因为堆栈、回调队列和事件循环。

栈是我们函数调用的地方，栈顶的函数首先被执行，新的函数调用总是放在栈顶。

当我们从请求中获得响应时，或者在这种情况下，当 setTimeout()中指定的时间结束时，响应函数不直接进入堆栈，而是放在一个称为回调队列的队列中。该队列存储响应的方式使得最先到达的响应在堆栈中最先执行。

只有当堆栈为空时，我们的回调队列才会开始向堆栈发送响应。事件循环负责通知回调队列堆栈为空。

最初，第一个 console.log()调用放在堆栈中，它将执行并从堆栈中删除，然后第一个 setTimeout()调用放在堆栈中，因为它是一个异步调用，我们知道 WebApi 处理异步调用，所以它从堆栈中删除，浏览器负责在 2 秒后将此异步调用的响应放入回调队列中，现在堆栈再次为空，因此第二个 setTimeout()调用放在堆栈中，这也将发送到浏览器，最后一个 console.log()调用放在堆栈中。它被执行，我们在控制台上看到“停止”。

现在可以清楚地看到，来自第二个 setTimeout()调用的响应仍然在回调队列中，与此同时，最后一个 console.log()被执行，我们看到在“waiting 2”之前“stopping”。

来自第一个 setTimeout()调用的响应甚至在第二个 setTimeout()调用的响应之后才进入回调队列，所以它在最后进入堆栈，我们在最后看到“waiting 1”。

我们已经为这个博客介绍了足够多的内容，但是我们才刚刚开始，还有更多的内容要介绍。在下一篇博客中，我们将讨论回调和关闭。