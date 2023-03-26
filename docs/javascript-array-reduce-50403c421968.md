# JavaScript Array.reduce()

> 原文：<https://medium.com/hackernoon/javascript-array-reduce-50403c421968>

我有多讨厌你？让我来数一数……

![](img/7be4823320c5d1cf2d8f62c70fde43fa.png)

[Array.reduce()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce) 方法已经存在一段时间了。像 [map()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) ， [filter()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) 和 [find()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find) 一样，它通过调用回调函数对一组值进行操作。

这里有一个来自[developer.mozilla.org 站点](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)的例子:

```
const array1 = [1, 2, 3, 4];
const reducer = (accumulator, currentValue) => accumulator + currentValue;// 1 + 2 + 3 + 4
console.log(array1.reduce(reducer));
// expected output: 10// 5 + 1 + 2 + 3 + 4
console.log(array1.reduce(reducer, 5));
// expected output: 15
```

看起来很简单，对吧？继续抱怨:

# 0.它不会做你所期望的事情

让我们再来看看这个:

```
const array1 = [1, 2, 3, 4];
const reducer = (accumulator, currentValue) => accumulator + currentValue;// expected: 1 + 2 + 3 + 4 === 10
```

现在我将修改 reducer 函数，为数组的每个值加 1，然后将其与前一个值相加。因为我将 1 加到 4 个数组元素上，所以我希望得到 14 的和。

```
const array1 = [1, 2, 3, 4];
const reducer = (accumulator, currentValue) => accumulator + currentValue + 1;// (1+1) + (2+1) + (3+1) + (4+1) = 2+3+4+5 = 14
console.log(array1.reduce(reducer));// unexpected output: **13**
```

等等…什么？

原来数组的第一个元素是初始累加器值。所以，代替

```
(1+1) + (2+1) + (3+1) + (4+1) = 2+3+4+5 = 14
```

它实际上是:

```
1 + (2+1) + (3+1) + (4+1) = 2+3+4+5 = 13
```

所有[那些](https://www.airpair.com/javascript/javascript-array-reduce) **减少** [的例子](https://www.w3schools.com/jsref/jsref_reduce.asp) [在线](https://medium.freecodecamp.org/reduce-f47a7da511a9)那些使用[求和](https://appdividend.com/2018/10/16/javascript-reduce-example-tutorial/)函数作为[插图](https://codeburst.io/learn-understand-javascripts-reduce-function-b2b0406efbdc)的[会误导](https://www.codingame.com/playgrounds/5474/a-look-at-the-array-reduce-function)你。当然，您可以得到正确的答案:只需添加一个累加器初始值作为 reduce 方法的第二个参数:

```
console.log(array1.reduce(reducer, **0**));
// desired output: 14
```

# 1.它被错误地命名

名字 *reduce* 会让人认为它将一个元素数组简化为这些元素的子集。这不是它真正做的。事实上，它可以做与减少相反的事情:

```
const array1 = [1, 2, 3, 4];  // four elements
const increaser = (accumulator, currentValue) => {
  accumulator.push(currentValue);
  accumulator.push(currentValue*currentValue);
  return accumulator;
};console.log(array1.reduce(increaser, []));// output: Array [1, 1, 2, 4, 3, 9, 4, 16] (8 elements)
```

它甚至不需要返回一个数组:

```
const array1 = [1, 2, 3, 4];
const transformer = (accumulator, currentValue) => {
  accumulator[currentValue] = currentValue*currentValue;
  return accumulator;
};console.log(array1.reduce(transformer, {}));
// output: Object { 1: 1, 2: 4, 3: 9, 4: 16 }
```

# 2.回调参数的顺序很奇怪

我们来比较一下 map()、filter()、find()和 reduce()的回调:

**映射**:(当前值，索引，数组，this) = > {}

**过滤器**:(当前值，索引，数组，this) = > {}

**找到**:(当前值，索引，数组，this) = > {}

**reduce:** (累加器，当前值，索引，数组)= > {}

您的回调将获得累加器作为第一个**参数，而不是当前值；此外，也没有采用**这个**参数的参数。**

# 3.错误:myVar.reduce 不是函数

我已经被 [lodash](https://lodash.com/docs/4.17.11#reduce) 惯坏了，它把数组和对象都当做集合，集合有 map、find、filter、reduce 方法。JavaScript 不会这样简化:

```
var obj1 = {my:"dog", has: "fleas"};

const values = (accumulator, currentValue) => accumulator.push(currentValue);

console.log(obj1.reduce(values, []));// > **Error: obj1.reduce is not a function**
```

正如达纳·卡维常说的，“不，不要这样做。这是不明智的。”(没错，我就是那么老。)

您仍然可以使用 Object.values 或 Object.entries 做类似的事情:

```
var obj1 = {my:"dog", has: "fleas"};

const values = (acc, [key, value]) => {acc.push(value); return acc}console.log(Object.entries(obj1).reduce(values, []));// > Array ["dog", "fleas"]
```

# 4.累加器有时是可选的；结果有时是随机的

很容易忘记累加器的初始值，即使你需要一个。让我们以上面例子为例，做一个修改:

```
var obj1 = {my:"dog", has: "fleas"};

const values = (acc, [key, value]) => {acc.push(value); return acc}console.log(Object.entries(obj1).reduce(values));// **> Array ["my", "dog", "fleas"]**
```

你注意到代码的不同了吗？很难发现。更糟糕的是，除了第一个值无效之外，您还获得了一个值数组。还好我有一天要考试！

# 5.未定义？啊？

哦，你忘记归还蓄电池了。你真傻。我从不犯这种错误:

```
var obj1 = {my:"dog", has: "fleas"};

const dog = (acc, [key, value]) => {
  if (key === 'my') {
    acc.push(value);
    return acc;
  }
}console.log(Object.entries(obj1).reduce(dog, []));// > undefined
```

您必须始终归还累加器:

```
var obj1 = {my:"dog", has: "fleas"};

const dog = (acc, [key, value]) => {
  if (key === 'my') {
    acc.push(value);
  }
  return acc;
}console.log(Object.entries(obj1).reduce(dog, []));
```

# 所以我推荐什么来代替 reduce()？

我*总是*使用 Array.reduce()，你也应该这样。只知道它的怪癖。这就像一只小吉娃娃咬你的脚后跟，如果你不注意，它有时会咬你，但它大多是无害的(比吉娃娃有用得多)。

![](img/89c353912c79a327f37f711873ef8060.png)

快乐减！