# 在编程中命名事物

> 原文：<https://medium.com/hackernoon/naming-the-things-in-programming-230590016f00>

*“计算机科学只有两个硬东西:缓存失效和事物命名。”*

*——菲尔·卡尔顿*

[命名](https://hackernoon.com/tagged/naming)编程[中的事物](https://hackernoon.com/tagged/programming)实际上是一项相当艰巨的任务，当你有多种选择的时候。

在我编程的第一年，我认为命名是最简单的事情，直到我意识到我必须为人类编写代码。然后我开始花更多的时间给事物命名，然后我开始给那些命名的事物重新命名，在重构的时候我又开始重新命名事物。

我已经从:

```
var a,
var x, 
var finalDataarrayManipulation();
initialize();
start();tovar ProductPrice 
var UserEmailVerificationToken
var ProcessedDataarrayToJson();
fetchUserData();
startRenderingHtml()
```

但是我仍然觉得我在他们身上遗漏了一些意义。名字也更长了。我担心我让名字变得有意义的追求会把它们变成段落。

![](img/40990e6fc7dd4c418fcc0c07ed70ab48.png)

我开始意识到:

*   **变量或函数应根据其工作来命名** -:变量/函数的名称应始终试图表达其含义，而不要深入代码库。尝试在名称中包装有意义的信息。

![](img/952b2557542bbb144a0427cbfc5c62a8.png)

```
function arrayManipulation(array) {// converts array to json
// returns json}
```

在上面的例子中，命名并不具体，因为我们的方法只将 array 转换成 JSON。读者可能想知道**它是否是一种过滤数组的方法？，映射数组，将数组转换成 JSON，将数组转换成 XML 等等。**

```
function convertArrayToJson(array) {// converts array to json
// returns json
}
```

*`convertArrayToJson`* 听起来更具体，并说出它真正的作用。

*   **命名应该足够简单，以便每个人都能理解** -:使用复杂的词语来描述简单的事物只会在阅读代码时造成麻烦。

```
function constituteJournal() {//creates journal in DB
}function createJournal() {//creates journal in DB
}
```

在上面的例子中，使用` *createJournal* 比使用` *constituteJournal* 更有意义。

*   比起抽象的名字，更喜欢具体的名字 -:
    使用抽象的名字会让读者感到困惑。

![](img/55ae2183af4bc9dedfb427c310451fa8.png)

```
var start = (new Date()).getTime(); // top of the page
...
var elapsed = (new Date()).getTime() - start; // bottom of the page
document.writeln("Load time was: " + elapsed + " seconds");
```

这段代码没有什么明显的错误，但是它不能工作，因为' getTime()'返回毫秒，而不是秒。通过将 _ms 附加到我们的变量上，我们可以使一切变得更清楚:

```
var start_ms = (new Date()).getTime(); // top of the page
var elapsed_ms = (new Date()).getTime() - start_ms; // bottom of the page
document.writeln("Load time was: " + elapsed_ms / 1000 + " seconds")
```

*   不要犹豫使用更长的名字:我们使用短的单词，即使它不能提供关于代码的完整见解，这是一件不好的事情。

```
function killProcess(pid) {
// kills process by process id}
```

看起来‘kill process’并没有包含太多关于如何终止进程的信息。如果我们用了会更好。

```
function killProccessByid(pid) {//kills process by id
}
```

*   做好心理准备，你现在不会再写更多的评论了，你的命名会让你深入了解这个过程。

**结论** -:尽量避免通用名即可。使用具体/特定的名称，如果需要的话可以使用更长的名称，附上重要的细节(包含更多的信息),主要关注代码的可读性。事情需要时间来改变。编码快乐！

[灵感](https://www.amazon.com/Art-Readable-Code-Practical-Techniques/dp/0596802293)