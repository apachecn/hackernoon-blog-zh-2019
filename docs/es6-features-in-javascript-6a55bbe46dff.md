# JavaScript 中的 ES6 特性

> 原文：<https://medium.com/hackernoon/es6-features-in-javascript-6a55bbe46dff>

![](img/6f29f573db11046555ee6f1f841873d3.png)

Photo by [Lorenzo](https://www.pexels.com/@lorenzocafaro) on [Pexels.com](http://www.pexels.com)

Es6 或也称为 EcamScript2015 为优秀的旧 JavaScript 带来了各种令人兴奋和有前途的功能。参考消息: **ECMAScript 是一种语言的规范。JavaScript 是该语言的一种实现。**

Es6 带来了许多有前途的特性，比如箭头函数、模板字符串、常量、let 等等。让我们从容易理解的特性开始，然后再看更复杂的特性。

# **const and let**

const(常量)是块范围的(仅在局部范围内可用)。你不能重新分配常量变量的值，但是常量的属性可以变异

```
>> const a = {namv : "Jhon Doe"}>> a = {  // this not allowedname : "Jen Doe"}>> a.name = "Jen Doe" // this allowed
```

let 也是块范围，但是您可以重新分配 let 变量的值，也可以改变它们的值。

# 模板字符串

模板字符串或模板文字用`(反勾号)括起来。你可以在模板字符串中嵌入变量，只需要用`${}`包围变量，多行字符串和更多的特性，但是为了让这个指南对初学者来说简单，我不打算讨论模板字符串的每一个特性，而是让我们通过比较普通字符串和模板字符串来理解大多数特性。

```
let a = 2
let b = 3String old = "two plus three is : \n" + (a+b)
>> two plus three is :
>> 5
String new = `two plus three is :
              $(a+b)`
>>two plus three is :
>>5
```

关于模板字符串的更多信息，请看一下[这个](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)链接。

# 箭头功能

箭头函数是用更快更简单的方式编写的一些常规函数。

例如对于匿名函数

```
function(e){ 
console.log(e)
return 1
}           
(e) => { // same as above
console.log(e)
return 1
}
```

命名函数

```
const fun1 = function(name){
console.log(`Hello ${name}`)
}
const fun2 = (name) {
console.log(`Hello ${name}`)
}
>>fun1("Jhon")
>>Jhon
>>fun2("Jhon")
>>Jhon
```

# 默认参数

Es6 为函数提供了默认参数。您可以设置参数的默认值，如果您传递该参数的值，它将被覆盖，如果您不传递该参数的默认值，该参数将采用默认值。为了更好的理解，我们举个例子。

```
const add = (a, b=5) =>{
      console.log(a+b);
}
>>add(3,4)
>>7
>>add(3)
>>8
```

# 进出口

在模块化至关重要地方，进出口更为重要。导入的常用语法是`import module from 'path/to/module'`。同样，您也可以导出任何模块。让我们举一个例子来进一步说明。假设有两个文件，一个是包含返回两个数和的方法的`sum.js`,另一个是我们想要对两个数求和的`index.js`。

```
---------------------------sum.js-----------------------------------export default function sum(a,b){
 return a+b
}---------------------------index.js---------------------------------import sum from './sum' // no need to provide format of file
console.log(sum(4,5))---------------------------console----------------------------------
>>9
```

有很多东西我在这里没有涉及到，你可以在这里[和](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)这里[找到更多关于进出口的信息。](https://developer.mozilla.org/en-US/docs/web/javascript/reference/statements/export)

# 班级

JavaScript 中的类不过是“特殊函数”。它没有向 JavaScript 引入新的面向对象的继承模型。引用 MDN blog 的话，“类主要是 JavaScript 现有的基于原型的继承的语法糖。”类给你的代码提供了良好的结构并保持整洁。

```
-------------------------Class Declaration-------------------------
class Person{
   constructor(name,age){ 
    this.name = name
    this.gender = gender 
    this.age = age
   }
}const Jhon = new Person("Jhon Doe", "Male", 30)
console.log(Jhon.age) // 30
Jhon.age = 25
console.log(Jhon.age) // 25
```

Extend 关键字用于继承。

```
class Animal{
....
}
class Dog extends Animal{
....
}
```

# 承诺

承诺是执行异步代码的 JavaScript 方式。Promise 采用两个参数 resolve 和 reject 来处理场景。

```
const myFirstPromise = new Promise((resolve, reject) => {
  // do something asynchronous which eventually calls either:
  //
  //   resolve(someValue); // fulfilled
  // or
  //   reject("failure reason"); // rejected
});function myAsyncFunction(url) {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open("GET", url);
    xhr.onload = () => resolve(xhr.responseText);
    xhr.onerror = () => reject(xhr.statusText);
    xhr.send();
  });
}
```

[这里的](https://codepen.io/pen/tour/welcome/start?editors=0012#0)是等待承诺的码潘例子。它等待 5 秒钟来执行异步代码。

这是所有的乡亲。为了简单起见，我们没有介绍 ES6 的很多特性。你可以随时在 MDN 博客上查找 ES6 的更多功能。

像往常一样，这里有一些随机编排的幽默来让你的一天开心起来。

![](img/b21569330a7a3ebe3c37ab19dacbc77d.png)