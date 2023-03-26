# 在 JavaScript 中删除对象属性

> 原文：<https://medium.com/hackernoon/deleting-object-properties-in-javascript-8608ae570fea>

最近我在做一个用例，我有一个如下的对象

```
let obj1 = {
 name: "Dr Strange",
 stone: "Time",
 status: "alive"
}
```

这里，API 被设计成只有当`status`是`Time`时，我们才需要发送`stone`。也因为某些原因，我得到了上述格式的输入对象。

我想编辑上面的对象，只是删除`stone` 属性完全如果`status !== 'active'`。所以我过去常常做的是

```
//incorrect way
if(obj1.status !== 'active')
 obj1.stone= undefined;
```

我的一个朋友告诉我，这似乎是错误的，因为我试图通过将它设置为`undefined`来删除属性。所以我问了正确的方法，但是他想不出其他更好的方法。我用的就是这种方式，当时就提前了。

最近在 JavaScript 中碰到了以前不知道的`delete`关键字。我还意识到，当我们将任何属性设置为`undefined`时，我们实际上并没有删除该属性。

这里有一个[的例子来证明](https://stackoverflow.com/questions/3390396/how-to-check-for-undefined-in-javascript?noredirect=1&lq=1)。现在，当我们 console.log 上述对象时

```
console.log(obj1) // {name: "Vision", stone: "Time", alive: undefined}
```

所以属性`alive`是未定义的，但它仍然存在于一个对象中。所以正确的做法是:

```
let obj1 = {
 name: "Dr Strange",
 stone: "Time",
 alive: true
}
delete obj1.alive;
obj1 // {name: "Vision", stone: "Soul"}
```

所以现在属性从对象中被完全删除，而不是拥有值为`undefined`的。这不仅从对象中移除了属性，还通过删除其引用节省了内存。

但是不要担心奇异博士他正躲在过去的某个地方，所以不妨在游戏结束时来拯救复仇者联盟。