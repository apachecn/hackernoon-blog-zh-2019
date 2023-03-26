# 异步编程 101

> 原文：<https://medium.com/hackernoon/asynchronous-programming-101-5ac0fa4e84e8>

C#中异步编程的自顶向下介绍

![](img/4dac4e56e99201c39cbb92aa60b38c75.png)

Ships at sea as seen from a ferry from Redwood City to San Francisco, July 2018

在这篇博文中，我将自上而下地介绍 C#中的异步编程。这篇文章可能对 C#新手和学习过线程和并发的学生有用。我相信自顶向下的方法将是理想的，因为它将首先向您展示什么是异步编程，这样您就可以对它的*如何*和*为什么*感兴趣。

这篇博客中使用的代码可以在我的 GitHub [这里](https://github.com/DeeptanshuM/AsyncProgramming101/blob/master/AsyncProgrammingDemo/Program.cs)获得。

# 基本面快速回顾

## 延迟和吞吐量

延迟是对工作所用时间单位的度量。吞吐量是衡量单位时间内完成的工作的单位。

## 并发

并发意味着多个计算同时发生[1]。它不同于[本](https://stackoverflow.com/questions/1050222/what-is-the-difference-between-concurrency-and-parallelism) stackoverflow 答案中解释的并行性:

> 并发性:至少有两个线程正在进行的情况。一种更通用的并行形式，可以将时间片作为虚拟并行的一种形式[2]。
> 
> 并行性:当至少两个线程同时执行时出现的情况[2]。

## 任务

任务是代表操作的抽象[3]。此操作可能会也可能不会得到多个线程的支持，并且可能会也可能不会同时执行。我们可以使用任务，而不是直接使用线程，这样我们就不需要手动、显式地实现线程的锁定、连接和同步操作，也不需要手动直接创建或销毁线程。默认情况下，C#任务使用线程池中的线程。线程池是一种维护多个线程的设计模式，这样它们就可以被重用/回收。我们可以使用这种设计模式来减少重复创建、启动和删除线程的延迟。

## 同步与异步操作

同步操作会在返回到调用者之前完成它的工作，而异步操作会在返回到调用者之后完成它的部分(包括全部)工作[3]。

![](img/d0fe50f11756a22e507d4203fc6d7214.png)

Purdue University, West Lafayette, IN, I think sometime during winter 2018–2019

## 用类比来概括这些基本概念

让我们考虑一个自助洗衣店。如果你几乎同时将衣物放入两台洗衣机，例如一只手拿着一台洗衣机，另一只手同时拿着另一台洗衣机*，那么你是在平行*放置衣物*。但是，如果您同时将衣物放入两台洗衣机，在给定的时间内，您一次只能将衣物放入一台洗衣机，那么您将同时放入衣物*和*。*

*洗衣机洗一负荷衣服所用的时间将是一台洗衣机的*潜伏期*。为了减少等待洗涤 n 件衣物所花费的时间，可以同时运行 n 台机器，而不是运行一台机器 n 次:在两种情况下，一台机器所做的工作花费相同的时间，但不同之处在于，在后一种情况下，在相同时间内所做的工作量(同时洗涤 n 件衣物)更多，即*吞吐量*更多。*

*要做所有这些事情，你可能会问前台的人要 25 美分的硬币:当时你什么也做不了，你排在队伍的前面，给了那个人两张 1 美元的钞票，然后等着他们退回 25 美分的硬币。这是一个*同步*操作。当洗衣机运行时，你仍然保持控制——你可以在第一台洗衣机仍在运行时继续加载并开始运行另一台洗衣机——这使得洗衣机的运行成为*异步*操作。*

# *异步编程演示*

*让我们看一个演示来理解如何定义和调用异步函数，以及异步编程如何通过提高吞吐量来影响程序延迟。*

*C#中的异步函数是通过在函数定义中添加关键字`async`来定义的。它们可以像其他函数一样被调用。C#中的异步函数必须返回一个任务或某种类型的任务。在下面的演示程序中，我们将看到如何启动这些任务，如何同步和异步运行它们，以及如何使用`await`关键字停止代码流以等待任务的结果。*

## *虚拟同步和异步功能*

*让我们实现一个虚拟异步函数:*

```
*private async Task<int> longRunningMultiplyBy10(int number)
{
    Console.WriteLine("# Initiated long running op on for the input number=" + number + " from Thread " + System.Threading.Thread.CurrentThread.ManagedThreadId + " #"); //100 delays of 100 ms = 10 seconds
    for (int i = 0; i < 100 ; i++)
    {
        await Task.Delay(100);
    } Console.WriteLine("# Completed long running op on for the input number=" + number + " from Thread " + System.Threading.Thread.CurrentThread.ManagedThreadId + " #");
    return number * 10;
}*
```

*在这个函数中，我们使用本机异步`Delay`函数，通过使用`await`关键字异步调用它，总共等待 10 秒，然后将输入数字乘以 10 并返回它。我们现在将看到同步和异步调用时的性能差异。为此，我们将实现一些函数来输入数字，对每个数字执行虚拟的长时间运行操作，并返回它们的总和。*

*同步调用`longRunningMultiplyBy10`的函数是:*

```
*private int syncMultiplyBy10AndAdd(int num1, int num2)
{
    Console.WriteLine("Calling long running Op from Thread " + System.Threading.Thread.CurrentThread.ManagedThreadId);
    int a = longRunningMultiplyBy10(num1).Result; Console.WriteLine("Calling long running Op from Thread " + System.Threading.Thread.CurrentThread.ManagedThreadId);
    int b = longRunningMultiplyBy10(num2).Result; Console.WriteLine("Executing return from Thread " + System.Threading.Thread.CurrentThread.ManagedThreadId);
    return a + b;
}*
```

*让我们编写一个使用异步编程的函数:*

```
*private async Task<int> asyncMultiplyBy10AndAddAwaitImmediately(int num1, int num2)
{
    Console.WriteLine("Calling long running Op from Thread " + System.Threading.Thread.CurrentThread.ManagedThreadId);
    int a = await longRunningMultiplyBy10(num1); Console.WriteLine("Calling long running Op from Thread " + System.Threading.Thread.CurrentThread.ManagedThreadId);
    int b = await longRunningMultiplyBy10(num2); Console.WriteLine("Executing return from Thread " + System.Threading.Thread.CurrentThread.ManagedThreadId);
    return a + b;
}*
```

*我们可以在这里观察到，我们不需要`await`长时间运行的操作，直到我们需要访问任务的结果——让我们写一个函数来反映这一点，并观察它的性能。*

```
*private async Task<int> asyncMultiplyBy10AndAddAwaitAtEnd(int num1, int num2)
{
    Console.WriteLine("Calling long running Op from Thread " + System.Threading.Thread.CurrentThread.ManagedThreadId);
    Task<int> a = longRunningMultiplyBy10(num1); Console.WriteLine("Calling long running Op from Thread " + System.Threading.Thread.CurrentThread.ManagedThreadId);
    Task<int> b = longRunningMultiplyBy10(num2); Console.WriteLine("Executing return from Thread " + System.Threading.Thread.CurrentThread.ManagedThreadId);
    return await a + await b;
}*
```

*![](img/e0749bafc34dfceb16f97fe9ef6733ff.png)*

*A tree at Purdue University, West Lafayette, IN, 1st or 2nd week of October, 2018*

## *帮助器函数调用虚拟函数并记录它们的性能*

**本节没有必要，可能会分散注意力，请跳到下一节**

*我们将调用`demo`来调用我们的演示函数，并测量它们的性能，如下所示:*

```
*private void demo()
{
    Console.WriteLine("##### Testing synchronous code #####");
    demoSyncFunction(syncMultiplyBy10AndAdd);
    Console.WriteLine("####################################" + Environment.NewLine); Console.WriteLine("##### Testing await immediately #####");
    demoAsyncFunction(asyncMultiplyBy10AndAddAwaitImmediately);
    Console.WriteLine("####################################" + Environment.NewLine);
    Console.WriteLine(); Console.WriteLine("##### Testing await at end #####");
    demoAsyncFunction(asyncMultiplyBy10AndAddAwaitAtEnd);
    Console.WriteLine("####################################" + Environment.NewLine);
    Console.WriteLine();
}private void demoSyncFunction(Func<int, int, int> asyncDemoFunction)
{
    int num1 = 15;
    int num2 = 33;
    int result;
    Stopwatch stopWatch = new Stopwatch(); stopWatch.Start();
    result = asyncDemoFunction(num1, num2);
    stopWatch.Stop();
    displayRuntime(stopWatch.Elapsed);}private void demoAsyncFunction(Func<int, int, Task<int>> asyncDemoFunction)
{
    int num1 = 15;
    int num2 = 33;
    Stopwatch stopWatch = new Stopwatch(); stopWatch.Start(); int result = asyncDemoFunction(num1, num2).Result; stopWatch.Stop(); displayRuntime(stopWatch.Elapsed);
}private void displayRuntime(TimeSpan ts)
{
    string elapsedTime = String.Format("{0:00}min:{1:00}s.{2:00}ms",
        ts.Minutes, ts.Seconds,
        ts.Milliseconds / 10); Console.WriteLine("Runtime: " + elapsedTime);
}*
```

*![](img/4894d2158e56afe23e50ee48d7c5e7cb.png)*

*A bridge I saw from the window of Amtrak en route to Portland from Seattle or maybe the other around*

## *结果呢*

*输出如下所示:*

*对于功能`syncMultiplyBy10AndAdd`*

```
*private int syncMultiplyBy10AndAdd(int num1, int num2)
{
    Console.WriteLine("Calling long running Op from Thread " + System.Threading.Thread.CurrentThread.ManagedThreadId);
    int a = longRunningMultiplyBy10(num1).Result; Console.WriteLine("Calling long running Op from Thread " + System.Threading.Thread.CurrentThread.ManagedThreadId);
    int b = longRunningMultiplyBy10(num2).Result; Console.WriteLine("Executing return from Thread " + System.Threading.Thread.CurrentThread.ManagedThreadId);
    return a + b;
}*
```

*输出是:*

```
*##### Testing synchronous code #####
Calling long running Op from Thread 1
# Initiated long running op on for the input number=15 from Thread 1 #
# Completed long running op on for the input number=15 from Thread 8 #
Calling long running Op from Thread 1
# Initiated long running op on for the input number=33 from Thread 1 #
# Completed long running op on for the input number=33 from Thread 8 #
Executing return from Thread 1
Runtime: 00min:20s.08ms
####################################*
```

*我们观察到线程 1 开始执行`syncMultiplyBy10AndAdd`。当它调用`longRunningMultiplyBy10`时，`syncMultiplyBy10AndAdd`的执行被阻止。然后，线程 1 开始执行异步运行的长时间运行的操作。当函数异步运行时，线程 1 从执行`longRunningMultiplyBy10`的任务中释放出来。然而，因为`syncMultiplyBy10AndAdd`是同步的，当`longRunningMultiplyBy10`完成时，线程 1 始终负责执行`syncMultiplyBy10AndAdd`。我们注意到该函数执行用了**20.08 毫秒**。*

*现在，当执行功能`asyncMultiplyBy10AndAddAwaitImmediately`时:*

```
*private async Task<int> asyncMultiplyBy10AndAddAwaitImmediately(int num1, int num2)
{
    Console.WriteLine("Calling long running Op from Thread " + System.Threading.Thread.CurrentThread.ManagedThreadId);
    int a = await longRunningMultiplyBy10(num1); Console.WriteLine("Calling long running Op from Thread " + System.Threading.Thread.CurrentThread.ManagedThreadId);
    int b = await longRunningMultiplyBy10(num2); Console.WriteLine("Executing return from Thread " + System.Threading.Thread.CurrentThread.ManagedThreadId);
    return a + b;
}*
```

*输出是*

```
*##### Testing await immediately #####
Calling long running Op from Thread 1
# Initiated long running op on for the input number=15 from Thread 1 #
# Completed long running op on for the input number=15 from Thread 7 #
Calling long running Op from Thread 7
# Initiated long running op on for the input number=33 from Thread 7 #
# Completed long running op on for the input number=33 from Thread 8 #
Executing return from Thread 8
Runtime: 00min:20s.04ms
####################################*
```

*我们观察到线程 1 开始执行`asyncMultiplyBy10AndAddAwaitImmediately`。这一次的不同之处在于，因为对`longRunningMultiplyBy10`的调用是异步的，所以线程 1 从执行`asyncMultiplyBy10AndAddAwaitImmediately`的任务中释放出来，线程池中的另一个线程，即线程 7，继续执行。我们注意到这个函数用了**20.04 毫秒**。这肯定比同步调用实现更快，我们可以直观地看到在现实世界中延迟的改善是多么显著。*

*我们还可以观察到低效率:我们在需要使用长时间运行操作的结果时提前完成了该操作。因此，尽管我们使用线程池通过使用异步编程将执行`asyncMultiplyBy10AndAddAwaitImmediately`的任务更有效地分配给一个线程，但我们没有利用异步编程的全部能力来允许长时间运行的操作(在本例中为`longRunningMultiplyBy10` )与调用程序函数的执行(在本例中为`asyncMultiplyBy10AndAddAwaitImmediately`)同时运行。*

*当我们解决这种低效率的时候，直到我们需要使用任务的结果，然后测试我们的功能*

```
*private async Task<int> asyncMultiplyBy10AndAddAwaitAtEnd(int num1, int num2)
{
    Console.WriteLine("Calling long running Op from Thread " + System.Threading.Thread.CurrentThread.ManagedThreadId);
    Task<int> a = longRunningMultiplyBy10(num1); Console.WriteLine("Calling long running Op from Thread " + System.Threading.Thread.CurrentThread.ManagedThreadId);
    Task<int> b = longRunningMultiplyBy10(num2); Console.WriteLine("Executing return from Thread " + System.Threading.Thread.CurrentThread.ManagedThreadId);
    return await a + await b;
}*
```

*我们得到以下结果:*

```
*##### Testing await at end #####
Calling long running Op from Thread 1
# Initiated long running op on for the input number=15 from Thread 1 #
Calling long running Op from Thread 1
# Initiated long running op on for the input number=33 from Thread 1 #
Executing return from Thread 1
# Completed long running op on for the input number=33 from Thread 8 #
# Completed long running op on for the input number=15 from Thread 6 #
Runtime: 00min:10s.04ms
####################################*
```

*长时间运行的操作一被调用就开始执行。调用函数和长时间运行的操作同时继续执行。有效异步编码的函数的运行时间只有**10 秒 04 毫秒**:这比调用 2 个长时间运行的操作的同步实现有 50%的延迟改进**。由于异步编码的函数操作调用两个长时间运行的操作，从而允许它们并发运行，因此 50%的性能提升是意料之中的。***

***![](img/cf219d5ed2598e91a756acb7188c4028.png)***

***Santa Monica on a hottest August day in 2018 (my most favorite picture of the past 12 months)***

# ***什么是异步编程，它是如何工作的***

***正如我们现在所观察到的，异步编程允许我们有效地利用线程并发运行任务。当一个异步函数被调用时，控制立即返回给调用函数(一旦异步函数的同步部分完成)，调用函数和被调用函数在不同的线程上同时执行，直到调用函数必须完成被调用函数。这有助于通过有效地利用线程池将任务实例分配给并发运行的不同线程来增加吞吐量(单位时间内完成的工作),从而改善整体程序延迟(程序运行所需的总时间)。***

# ***参考***

***[1][https://web.mit.edu/6.005/www/fa14/classes/17-concurrency/](https://web.mit.edu/6.005/www/fa14/classes/17-concurrency/)
【2】定义多线程术语 [Oracle 多线程编程指南](https://docs.oracle.com/cd/E19455-01/806-5257/6je9h032b/index.html)
【3】第十四章并发&C # 5.0 的异步[简而言之:权威参考第五版作者:Joseph Albahari，Ben Albahari(作者)](https://www.amazon.com/C-5-0-Nutshell-Definitive-Reference-dp-1449320104/dp/1449320104/ref=mt_paperback?_encoding=UTF8&me=&qid=)***

***Hilton Lange 的一个大喊:他对异步编程和演示异步程序的解释启发了我写这篇博客。***

****原载于 2019 年 7 月 3 日*[*deeptanshumalik.com*](https://deeptanshumalik.com/2019/07/03/async-programming-demo/)*。****