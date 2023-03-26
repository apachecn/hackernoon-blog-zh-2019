# 解构 React 挂钩背后的魔力

> 原文：<https://medium.com/hackernoon/deconstructing-the-magic-behind-react-hooks-33ca987e5307>

![](img/b005473d30484687605a859f861b44b5.png)

React v16.8.0 已经发布，这意味着 React 挂钩现在可以在 React 的稳定版本中使用。这是我一直期待的事情，自从我第一次开始在 alpha 版本中使用钩子。我相信 hooks 会给 React 组件的编写和组合方式带来翻天覆地的变化，这是一个更好的变化。

如果你还没有听说过 React 钩子，你可能应该去 React 文档网站上阅读一下[关于钩子](https://reactjs.org/docs/hooks-overview.html)的概述。接下来，你可能还想读一读丹·阿布拉莫夫写的这篇关于钩子的优秀文章[。](https://overreacted.io/react-as-a-ui-runtime/)

## 当心:这里有魔法！

我最近一直在谈论钩子，很多时候，有人指出，虽然钩子似乎是解决副作用和 React 组件中的状态管理问题的一个很好的解决方案，但它似乎也有点像一个黑盒，用无法识别的魔法让它实际工作。

这些人中的大多数认为钩子具有魔力是因为:

*   钩子需要在每次渲染时以相同的静态顺序调用。
*   挂钩只能从其他挂钩或函数组件中调用。
*   这个`useState`钩子似乎跟踪了组件的`state`，而这个组件从来没有被传递给它。

虽然如果你在看表演，魔术可能会很有趣，但当你写软件时，它肯定不有趣，而且对于你的生活来说，不知道为什么你的库会做一些奇怪的，意想不到的，违反所有逻辑的事情。因此，让我们来解构这种魔力，看看在每一个案例的幕后到底发生了什么。

## 让拆伙开始吧！

钩子需要在每次渲染时以相同的静态顺序被调用。

人们注意到的第一个因素是(正如在钩子的[规则中提到的)，钩子对它们的调用顺序非常挑剔。具体来说，React 要求在每次渲染时，钩子的调用顺序必须相同。这意味着你不能在循环或条件中调用钩子，而是必须在最顶层调用它们。](https://reactjs.org/docs/hooks-rules.html)

这背后的原因很简单。考虑函数组件中定义的几个挂钩，如下所示:

```
const [firstName, setFirstName] = useState('John'); 
const [lastName, setLastName] = useState('Doe');
const [age, setAge] = useState(28);
```

如您所见，这是一个普通的 ES6 对函数`useState`的调用。`useState`函数得到的唯一信息是传递给它的初始值(不完全是，但我们稍后会回到这个问题)。然后，`setState`调用以`[value, setter]`的形式返回一个数组。

在随后的渲染中，相同的`useState`方法将返回特定属性的更新状态值。为了做到这一点，`useState`方法必须以某种方式跟踪每个被请求的属性。然而，我们绝对没有向`useState`方法传递任何类型的`key`。

那么`useState`究竟如何知道哪个调用返回哪个状态呢？Well `useState`使用 render 函数内部的调用序列号来跟踪状态。所以在上面的例子中，对`useState`的第一次调用将总是返回`firstName`及其 setter 的值，第二次调用将总是返回`lastName`及其 setter 的值，以此类推。

如果像我一样，你可能会想，应该有一个更好的方法来实现它，而不是依赖于调用顺序，这看起来很糟糕。Sebastian Markbage 在 React Hooks RFC 上留下了一个极好的评论，除了其他事情之外，还提到了这个问题。你也应该去看看。

总之，简而言之，如果你使用钩子，你绝对必须在每次渲染时以相同的顺序调用钩子，否则会发生不好的事情，因为 React 认为你想要获得或设置某个状态值，而你想要的是完全不同的。

理解这一点的一个简单方法是将`useState`视为维护一个将调用顺序链接到状态值的键值映射。在第一次渲染时，`useState`将键值映射初始化为:

```
const [firstName, setFirstName] = useState('John'); // internal state: {0: 'John'}
const [lastName, setLastName] = useState('Doe'); // internal state: {0: 'John', 1: 'Doe'}
const [age, setAge] = useState(28); // internal state: {0: 'John', 1: 'Doe', 2: 28}
```

在第二次渲染时，`useState`已经有了一个键值映射，所以它没有在上面设置值，而是根据调用顺序检索值。请注意，`useState`实际上并没有维护一个简单的键-值映射，实际的实现要比这复杂一些。但是为了理解调用顺序的重要性，您可以将它视为一个有效的键值映射。

挂钩只能从其他挂钩或函数组件中调用。

钩子的另一个重要规则是，你只能从一个 React 函数组件或者其他钩子中调用一个钩子。如果你仔细想想，这是完全有意义的——如果你的钩子必须直接或间接地获取或设置一个组件的状态，或者触发副作用，那么它需要链接到一个特定的 React 组件，该状态将属于该组件。否则，React 如何知道钩子试图访问哪个组件的状态呢？

到目前为止，一切似乎都很好。这个没什么神奇的，对吧？如果你想看到魔法，你需要打破这个规则，从一个普通的 Javascript 函数内部调用一个钩子。

```
function catchMeIfYouCan(){ 
    const [firstName, setFirstName] = useState('John'); 
};
```

运行这段代码会立即抛出一个错误:

```
Uncaught Error: Hooks can only be called inside the body of a function component.
```

现在等一下！如果`useState`调用只是一个普通的函数调用，并且 Javascript 中的一个函数在分析调用它的方法方面实际上做不了什么，那么 React 是如何知道从 React 组件外部调用了`useState`方法的呢？魔法？不完全是，但是由于这与我们列表中的下一个魔法项目有关，我将一起解决它们。

这个`useState`钩子似乎在跟踪组件的`state`，而这个组件从来没有被传递给它。

考虑两个组件`Student`和`Teacher`，它们都具有状态属性`firstName`、`lastName`和`age`。

```
class StudentComponent extends React.Component{
    constructor(props){
        super(props);
        this.state = {
            firstName: 'Bobby',
            lastName: 'Tables',
            age: 8
        }
    }
    render(){
        return <div>{this.state.firstName} {this.state.lastName} ({this.state.age})</div>
    }
}class TeacherComponent extends React.Component{
    constructor(props){
        super(props);
        this.state = {
            firstName: 'John',
            lastName: 'Keating',
            age: 34
        }
    }
    render(){
        return <div>{this.state.firstName} {this.state.lastName} ({this.state.age})</div>
    }
}
```

在上面的例子中，在两个组件中，`state`是一个实例属性，因此可以从任何类方法中访问。因为它是一个实例属性，而不是一个公共共享对象，所以值也被正确封装，并且`TeacherComponent`不能访问`StudentComponent`的状态，反之亦然。

现在让我们使用`useState`钩子将这些组件转换成它们对应的功能组件。

```
function StudentComponent(props){
    const [firstName, setFirstName] = useState('Bobby');
    const [lastName, setLastName] = useState('Tables');
    const [age, setAge] = useState(8); return <div>{firstName} {lastName} ({age})</div>
};function TeacherComponent(props){
    const [firstName, setFirstName] = useState('John');
    const [lastName, setLastName] = useState('Keating');
    const [age, setAge] = useState(34); return <div>{firstName} {lastName} ({age})</div>
};
```

如果你注意到这里，在两个组件中，我们调用相同的`useState`方法，这是从 React 库导入的方法，并且在两个组件之间共享。像我们之前讨论的一样，`useState`只使用调用顺序来跟踪状态值。

鉴于此，如果我们先渲染`StudentComponent`，设置`firstName`、`lastName`和`age`状态属性，然后紧接着渲染`TeacherComponent`，那么`TeacherComponent`应该可以访问由`StudentComponent`设置的状态值，对吗？

实际上没有。即使在使用钩子的时候，React 也能确保`state`值总是被正确封装，并且不会被错误的组件访问。否则，生活就不会令人愉快，因为每个组件都能够访问所有其他组件的状态，不是吗？

但是假设我们不再使用`this`来封装组件实例中的状态，React 如何为我们执行这种封装呢？答案其实很简单。但是在开始之前，我们需要稍微了解一下 React 的内部。或者说，反应世界的内部。

## 使用 React 纤维获得更好的渲染性能

在 React 的最初版本中，呈现组件是作为单个连续动作发生的事情。在 React 应用程序中，对应用程序状态的更改会触发整个应用程序的重新呈现。然而，在实践中，对于任何重要的应用程序，重新渲染整个应用程序都会导致糟糕的性能。

为了提高性能，React 执行了相当多的优化。这些优化大多发生在`Reconciliation`阶段。协调基本上是 React 通过在每个组件实例上执行`render`方法来同步虚拟 DOM 和在浏览器上构建的实际 DOM 的过程。

在 React Fiber(React 的 v16 中发布)之前，React 依靠调用堆栈来管理渲染。每当一个组件需要重新呈现时，它就会被添加到调用堆栈中，一旦堆栈中的其他项目完成呈现，该组件就会呈现。然而，当堆栈上有项目时，没有其他工作可以做。渲染会阻塞所有其他功能，并且渲染不能被分解成更小的块来运行。

为了解决这个问题，引入了光纤。Fiber 基本上是调用栈的一个定制的重新实现，它允许将一个大任务分割成更小的工作单元，根据需要暂停和恢复这些工作单元，如果不再需要的话，还可以删除这些工作单元。

为了实现这一点，fiber 在内部将 React 组件的每个实例引用为“Fiber”对象。纤程对象是一个 JavaScript 对象，包含关于组件、其输入和输出的信息。它类似于堆栈框架，但也对应于组件的实例。

这实际上意味着，组件的每个实例在 React 中被内部表示为一个“纤程”，这些纤程有许多属性，React 使用这些属性来跟踪类似于`state`和`props`的事情。

那么所有这些与将`state`封装在功能组件中有什么关系呢？

每个组件实例都是一个纤维，纤维有很多特定于反应的内部属性。在类组件和功能组件中，组件的实际实例被表示为纤程，在 React 内部之外是不可访问的。

当我们在类组件中调用`this.setState`时，纤程不会立即将部分状态合并到预先存在的状态中，而是创建一个 updateAction，并将该动作排队。动作排队的队列对于每个单独的纤程是特定的，并且要在纤程上执行的大多数动作是排队的，而不是直接运行的。将动作排队允许纤程调度和控制执行。

当我们在函数组件中调用`useState`钩子时，或者当我们使用由`useState`钩子返回的 setter 方法时，会发生完全相同的事情。要执行的相应动作在代表该组件的特定实例的纤程上排队。

类组件和功能组件之间的唯一区别是如何识别对应于特定组件实例的特定纤程。在类组件中，类组件实例本身有一个属性`_reactInternalFiber`，它直接给出相应的纤程。这是在最初构造组件时由 React 设置的。

但是，在函数组件中，不存在这样的键。回到我们的例子:

```
function StudentComponent(props){
    const [firstName, setFirstName] = useState('Bobby');
    const [lastName, setLastName] = useState('Tables');
    const [age, setAge] = useState(8); return <div>{firstName} {lastName} ({age})</div>
};function TeacherComponent(props){
    const [firstName, setFirstName] = useState('John');
    const [lastName, setLastName] = useState('Keating');
    const [age, setAge] = useState(34); return <div>{firstName} {lastName} ({age})</div>
};
```

React 如何封装两个组件实例之间的数据？到目前为止，我们知道解决方案包括为每个组件实例维护单独的纤程，并且这个纤程是在组件第一次构造时设置的。但是`useState`如何知道哪个组件试图访问状态并返回适当的状态呢？

React 用来识别调用组件实例的解决方案非常简单。在 React 中，在任何给定的时间点，当前只能有一个组件正在渲染。React 还知道组件渲染何时开始，何时结束。因此，当一个组件实例将要开始呈现时，React 会设置一个标志`currentlyRenderingFiber`，指向该实例。当组件完成渲染时，该标志被置空。

因为钩子函数只能从函数组件内部调用，而函数组件基本上只是类组件的渲染方法，所以可以保证无论何时调用钩子，我们都将处于渲染过程中，并且`currentlyRenderingFiber`将指向正在渲染的纤程。组件实例的`state`作为这个纤程上的一个属性被维护，这就是`useState`用来获取正确组件实例的状态。

还记得我们之前看到的从普通 Javascript 函数调用钩子时的错误吗？

```
Uncaught Error: Hooks can only be called inside the body of a function component.
```

那么 React 是如何知道这不是从函数组件内部调用的呢？好吧，React 只在组件渲染为功能组件时设置了`currentlyRenderingFiber`、`dispatcher`等一些项。当您从一个普通的 javascript 对象调用钩子时，这些项没有被设置，这时 React 抛出错误。

## 魔法被驱散了！

这就是 React Hooks 背后的全部魔力。既然我们已经确切地知道了幕后发生了什么，我们应该能够更好地理解钩子，它们是如何工作的，以及为什么我们需要坚持编写钩子的特殊规则。

我很想听听你对我在这篇文章中所写的东西的看法。所以，请在下面发表你的评论。

*原载于*[*asleepysamurai.com*](https://asleepysamurai.com/articles/deconstructing-the-magic-behind-react-hooks?import=medium)*。*