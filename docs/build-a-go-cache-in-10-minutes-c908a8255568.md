# 在 10 分钟内建立 Go 缓存

> 原文：<https://medium.com/hackernoon/build-a-go-cache-in-10-minutes-c908a8255568>

![](img/3d688cee80c715100e2b2d33a58d6a9e.png)

高速缓存是计算机科学最伟大的创新之一🔥🔥🔥。它显著减少了 [CPU](https://hackernoon.com/tagged/cpu) 的工作，并在速度方面提供了巨大的性能增益。😃

它的工作原理是保存以后可能需要的计算结果。例如:假设你有一个服务，给定一个字符串，生成一个散列。缓存可以通过检查接收到的字符串的哈希是否已经生成来节省时间和资源。如果有，并且仍然在缓存中，那么它将被返回，而不需要再次运行哈希算法。

今天，我们将构建一个 **LRU** ( *最近最后使用的*)缓存，它存储固定数量的`strings`，并在缓存满时弹出最后使用的项目。

这不会是您想要在生产中运行的任何东西。但是它将清楚地展示使这种类型的缓存工作的`data structures`和`algorithms`。

**获取并运行最终结果:**

```
git clone [https://github.com/Lebonesco/go_lru_cache.git](https://github.com/Lebonesco/go_lru_cache.git)
go run main.go
```

让我们开始写代码吧！

我们将从定义我们的`data structures`开始。这些将包括`Node`、`Queue`、`Hash`和`Cache`。我们的`Queue`将是一个从`Hash`映射到的`Node`指针的双向链表。这将允许值的 **O(1)** **插入**和**删除**。👍

**注意:**我们现在是只是缓存`strings`，但是任何`data type`都可以替换它。

接下来，我们将为`Cache`和`Queue`设置我们的构造函数。尽管`Hash`开始时为空，但它需要初始化，否则会导致**“空指针错误”**。此外，我们为**头部**和**尾部**创建两个空的`Nodes`。当我们转向我们的`Add()`和`Remove()`方法时，这将更有意义。

到缓存的主代码上。

缓存有三个方法**，这三个方法是使其工作所必需的:`**Check()**`(从用户处接收字符串并返回结果)、`**Add()**`(将字符串添加到缓存中)、`**Remove()**`(从缓存中弹出字符串)。**

**在`Check()`内部，如果`string`已经存在于缓存中，我们首先移除它，然后再将它添加回去，这样`string`就被移到了`Queue`的前面。**

**`Add()`和`Remove()`都涉及类似的操作，重新分配`Queue`中的`Left`和`Right`指针。**

**太棒了，我们现在有一个工作缓存了！🎉🎉🎉**

**最后一步是添加一个`main()`和一些显示方法来演示我们的结果。**

**要查看运行中的代码，请运行:**

```
go run main.go
START CACHE
add: cat
1 - [{cat}]
add: blue
2 - [{blue} <--> {cat}]
add: dog
3 - [{dog} <--> {blue} <--> {cat}]
add: tree
4 - [{tree} <--> {dog} <--> {blue} <--> {cat}]
add: dragon
5 - [{dragon} <--> {tree} <--> {dog} <--> {blue} <--> {cat}]
add: potato
remove: cat
5 - [{potato} <--> {dragon} <--> {tree} <--> {dog} <--> {blue}]
add: house
remove: blue
5 - [{house} <--> {potato} <--> {dragon} <--> {tree} <--> {dog}]
remove: tree
add: tree
5 - [{tree} <--> {house} <--> {potato} <--> {dragon} <--> {dog}]
add: cat
remove: dog
5 - [{cat} <--> {tree} <--> {house} <--> {potato} <--> {dragon}]
```

**感谢您花时间阅读这篇文章。**

**如果你觉得它有帮助或有趣，请让我知道👏👏👏。**