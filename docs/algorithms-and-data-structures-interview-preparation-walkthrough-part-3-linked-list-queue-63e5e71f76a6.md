# 算法和数据结构面试准备和演练—第 3 部分，链表、队列和堆栈

> 原文：<https://medium.com/hackernoon/algorithms-and-data-structures-interview-preparation-walkthrough-part-3-linked-list-queue-63e5e71f76a6>

![](img/002f245e98d21912fd417089e2bee6cc.png)

(Source: [fotolia.com](https://www.fotolia.com/tag/queue))

在我之前的帖子: [**算法和数据结构面试准备&演练—第二部分**](/@victorlin_38374/algorithms-and-data-structures-interview-preparation-walkthrough-part-2-array-and-string-80f28e095ca8) 中，我们谈到了如何从头实现一个静态数组，将其扩展为动态数组，并讨论了摊销的含义。

在这篇文章中，我想谈谈链表、队列和堆栈，因为它们都是线性数据结构，这些元素是按顺序组织的。

## 链表

与 Array 类似，它也以线性方式存储元素。但是，它不会将它们存储在内存中连续的位置；它使用相互连接但分散在各处的节点。一个**节点**包含指向其他节点的值和指针，这取决于它是一个单向还是双向链表。在下面的代码片段中，我将实现节点和单链表。

```
function Node(value = null){
  this.value = value;
  this.next = null;
}function LinkedList(){
  this.head = null;
  this.tail = null;
  this.length = 0;
}// Time Complexity: O(1)
// Auxiliary Space Complexity: O(1)
LinkedList.prototype.**append** = function(value) {
  let newNode = new Node(value);
  if (this.head === null) {
    this.head = this.tail = newNode;
  } else if (this.head === this.tail) {
    this.tail = newNode;
    this.head.next = this.tail;
  } else {
    this.tail.next = newNode;
    this.tail = newNode;
  }
  this.length += 1;
};// Time Complexity: O(n)
// Auxiliary Space Complexity: O(1)
LinkedList.prototype.**insert** = function(value, index) {
  if (index > this.length || index < 0) return;let prev = new Node(value);
  let node = this.head;
  if (this.head === null && index === 0) {
    this.head = this.tail = prev;
    this.length += 1;
    return;
  } else if (this.head === this.tail) {
    if (index === 0) {
      prev.next = this.head;
      this.head = prev;
    } else {
      this.head.next = prev;
      this.tail = prev;
    }
    this.length += 1;
    return;
  }while (index > 1) {
    node = node.next;
    index--;
  }let nextNode = node.next;
  node.next = prev;
  prev.next = nextNode;
  this.length += 1;return;
};// Time Complexity: O(n)
// Auxiliary Space Complexity: O(1)
LinkedList.prototype.**delete** = function(index) {
  if (index > this.length || index < 0 || this.length === 0) return;
  if (index === 0 && this.length === 1) {
    this.head = this.tail = null;
    this.length = 0;
    return;
  } else if (this.length === 2) {
    if (index === 0) {
      this.head.next = null
      this.head = this.tail;
    } else {
      this.tail = this.head;
      this.head.next = null;
    }
    this.length = 1;
    return;
  }
  let node = this.head;
  while (index !== 0) {
    if (node === this.tail) return;
    node = node.next;
    index--;
  }
  node.next.next = null;
  node.next = node.next.next;
  this.length -= 1;
};// Time Complexity: O(n)
// Auxiliary Space Complexity: O(1)
LinkedList.prototype.**contains** = function(value) {
  let node = this.head;
  while (node !== null) {
    if (node.value === value) return true;
    node = node.next;
  }
  return false;
};
```

我为单链表实现 append(value)，insert(value，index)，delete(index)，contains(value)。对于 append()，因为我们知道它总是在尾部附加一个新节点，我们只需要将 tail.next 指向新节点，并将 tail 指向新节点。时间复杂度为 O(1)。在单链表中，从头部删除()也是 O(1)。然而当涉及到带有某个索引的 insert()、delete()或 contains()时，最坏的情况是我们需要遍历整个列表来找到正确的位置，时间复杂度是 O(n)。

在一个双向链表中，除了节点中的 next 属性，我们还有 prev 属性可以从尾部开始以相反的顺序跟踪。从其中移除最后一个元素的时间复杂度是 O(1)，因为现在可以在两个方向上遍历列表，所以可以从 tail 和 point tail.prev 中移除最后一个元素，并指向最后一个-1 元素。使用双向链表更容易也更有效，但是你需要小心维护节点中的 next 和 prev 指针。如何重新分配 next 和 prev 指针是链表相关[面试](https://hackernoon.com/tagged/interview)题中的关键。

链表的优点是我们可以动态地扩展和收缩它，因为我们不连续地在内存中存储节点。但是，我们需要额外的内存来存储指针，访问节点必须按顺序读取。

链表可以创建许多其他的数据结构:队列、栈、图、哈希表。

**什么时候**应该使用链表？

●主要需要的是在你的集合中间插入和删除——不需要重新分配内存中的节点，只需要修改指针。

●分割和连接是常见操作——原因同上。

●不需要搜索和访问值—我们不想要 O(n)搜索和访问时间。

●不需要随机访问。

可以从链表创建其他数据结构:

●排队

●堆栈

●图表

●哈希表

我们将在另一篇文章中讨论图和散列表。

## 长队

Queue 是一个线性的[数据结构](https://hackernoon.com/tagged/data-structure)，它使用先进先出(FIFO)规则添加和删除元素。我们可以从数组或链表中实现队列。在下面的实现中，为了简单起见，我们使用数组。然而，*最好使用双向链表来获得固定的时间复杂度。*

常用方法:

○入队—将元素添加到队列中

○dequee—从队列中移除并返回第一个元素

○ peek —返回第一个元素，但不从队列中删除

```
function Queue(){
  this.store = [];
}Queue.prototype.enqueue = function(num) {
  this.store.push(num);
}Queue.prototype.deque = function() {
  this.store.shift();
}Queue.prototype.peek = function() {
  this.store[0];
}
```

使用队列的好处是当我们想要强制执行 FIFO 限制时，也能够动态扩展和收缩。在多个用户之间共享资源(即 CPU)和处理异步操作(I/O 缓冲区)是我们现在使用队列的两个常见例子。

## 堆

一种线性数据结构，使用后进先出(LIFO)规则添加和删除元素。与队列类似，我们可以从数组或链表中实现队列。

常用方法:

推送—将元素添加到堆栈中

○ pop —从堆栈中移除并返回最后一个元素

○ peek —返回最后一个元素，但不从堆栈中删除

```
function Stack(){
  this.store = [];
}Stack.prototype.push = function(num) {
  this.store.push(num);
}Stack.prototype.pop = function() {
  this.store.pop();
}Stack.prototype.peek = function() {
  this.store[this.store.length - 1];
}
```

使用队列的好处是，当我们想要强制执行 LIFO 限制时，还可以像队列一样使用链表来动态扩展和收缩。然而，它限制了对值的访问，并且在一些现代语言(Ruby、Python、JavaScript)中已经过时了。对于某些语言，动态数组已经包含了与堆栈相同的方法。

实现堆栈的一些例子:撤销/重做特性，浏览器上的后退按钮，以及计算表达式。

有各种各样与数组、链表、堆栈和队列相关的面试问题，因为它们是非常基本的，并且被大多数面试问题所涵盖。我将列举一些例子供您研究:

1.  [回文](https://www.geeksforgeeks.org/tag/palindrome/)
2.  [数组旋转](https://www.geeksforgeeks.org/array-data-structure/#rotation)
3.  [分类和搜索](https://www.geeksforgeeks.org/array-data-structure/#searchSort)
4.  对[堆栈](https://www.geeksforgeeks.org/stack-data-structure/#operations) / [队列](https://www.geeksforgeeks.org/queue-data-structure/#operations)的操作

我发现没有人真正阅读我为我的思考过程创建的编码问题帖子，所以我会发布其他网站的链接来准备面试问题。

## 在你走之前—

没有比在 Medium 上给我一个 follow([**Victor Lin**](/@victorlin_38374))更好的支持我的方式了。让我知道我应该多写！

你知道你可以放弃 50 吗👏通过按下👏按钮？如果你*真的*喜欢这篇文章，那就试试吧！