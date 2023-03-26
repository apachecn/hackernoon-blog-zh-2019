# 使用 RxJS 的 DIY Redux:第 3 部分

> 原文：<https://medium.com/hackernoon/diy-redux-with-rxjs-part-3-87eaf23d4092>

![](img/8404a87ec09016c896904ead515ef2e0.png)

Photo by [Steve Halama](https://unsplash.com/photos/iVGevPcaJzk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

*到目前为止，在之前的两篇帖子中，我已经涵盖了“如何用 RxJS 创建 Redux 库”和“如何编写 Redux 中间件”这两个主题。在开始这个系列的第三部分也是最后一部分之前，我建议你检查一下下面的前两部分:*

[](https://hackernoon.com/diy-redux-with-rxjs-rxdx-23163a87ade1) [## 带 RxJS 的 DIY Redux with RxJS

### 一个结合 RxJS 和所有 Redux 助手库的实验，创建一个“具有与 Redux 相似指纹的 React 的状态管理”。

hackernoon.com](https://hackernoon.com/diy-redux-with-rxjs-rxdx-23163a87ade1) [](https://hackernoon.com/diy-redux-with-rxjs-part-2-f9d4c53fa230) [## 使用 RxJS 的 DIY Redux:第 2 部分

### 将 RxJS 和所有广泛使用的 Redux 中间件合并为一个库的尝试

hackernoon.com](https://hackernoon.com/diy-redux-with-rxjs-part-2-f9d4c53fa230) 

在这一部分，我将创建一个 **HOC** (高阶组件)来连接 **RxDx** 和 **React。组件**。我假设你已经知道反应。组件，所以我不会再解释了。但是我需要解释一下特殊情况，这是我将在下面演示的核心。

# 这些高阶组件到底是什么？

基本上，HOC 是一个类**装饰器**。但是什么是班级装饰者呢？有时当我们编码时，我们开始觉得我们创建的所有类都有相同的基本属性。在那一点上，我们意识到我们在重复自己，这完全违背了**干**法则。所以我们开始试图找到一种方法来分离重复的部分。或者当我们编码时，我们需要在不改变实现的情况下向类中添加一个特性。在这两种情况下，我们的目的是实现类之间的代码共享。所以对我们上面描述的问题最好的答案是一个**类工厂函数**。

类工厂函数是返回类的函数。(欢迎来到 javascript 世界的怪异之处)。

```
// the most primitive description of a class factory function
function **giveMeClass**(someArguments) {
   return class **SomeDynamicallyGeneratedClass** {}
}
```

原始的类工厂函数还不是类装饰器。作为一个类装饰器，类工厂函数需要一个类作为参数，返回/产生的类将是输入类的扩展版本:

```
//enhance decorator
function **ehance**(targetClass) {
   return class **EnhancedClass** extends **targetClass** {
      constructor() {
         this.isEnhanced = true;
      }
}//usage in enviroment where experimental decorators allowed
@enhace
class **Normal** {}console.log(new Normal().isEnhanced) // true//usage in enviroment where experimental decorators not allowed
class **Normal** {}const **Enhanced** = enhance(Normal);console.log(new Enhanced().isEnhanced) // true
```

如前所述，特设也是装饰者，其工作方式与类装饰者完全相同。一个 **HOC** 接受一个 **React** 组件——它是一个类——并返回另一个 **React** 组件。关于特设的详细文档，您可以点击下面的链接，但是如果您有心情 TL；我将继续对它进行总结:

[](https://reactjs.org/docs/higher-order-components.html) [## 高阶组件-反应

### 高阶组件(HOC)是 React 中重用组件逻辑的一种高级技术。hoc 不属于…

reactjs.org](https://reactjs.org/docs/higher-order-components.html) 

现在是时候扩展上面的装饰器示例了，通过用一个 **React 组件**替换类输入和输出，使其成为一个**特设的**。我必须提到，使用 React 组件作为输入/输出为我们提供了两个机会，但是我将继续使用推荐的一个，用另一个组件包装原始组件:

```
// a primitive HOC which makes A component great nothing :)
function **makeGreat**(WrappedComponent) {
   return class **EnhancedComponent** extends **React.Component** {
      constructor() {
         this.state = {isGreat: true}
      } render() {
         return <**WrappedComponent** {...this.props, ...this.state} />
      }
   }
}// usage
class **LameComponent** extends **React.Component** {
   render() {
      return <div>{this.props.isGreat? 'Great': 'Lame'}</div>
   }
}const **GreatComponent** = **makeGreat**(LameComponent);<**LameComponent** /> // this will render <div>Lame</div>
<**GreatComponent** /> // this will render <div>Great</div>// alternative usage if experimental decorators are allowed
@makeGreat
class **LameComponent** extends **React.Component** {
   render() {
      return <div>{this.props.isGreat? 'Great': 'Lame'}</div>
   }
}<**LameComponent** /> // this will render <div>Great</div>
```

我相信我们现在需要更深入，所以系好安全带。在这一点上，你仍然不知道 **RxDx** 你可以从这个[链接](https://github.com/onerzafer/rxdx)来检查它，因为我将解释如何连接 **RxDx** 到 **React。组件**。

# 用 React 组件缝合 RxDx:一个真实的 HOC 示例

你还记得[零件 2](https://hackernoon.com/diy-redux-with-rxjs-part-2-f9d4c53fa230) 和**商店的**选择器**吗？这是它们派上用场的时刻，因为我们将使用它们作为 **RxDx** 和 **React 之间的粘合剂。组件**。获取一个选择器或一系列字符串，并返回一个可观察值，以允许我们在状态更新时得到通知。我们可以在 **componentDidMount** 中订阅所有这些可观测量，并且在每次更新时，我们可以使用 **forceUpdate** 重新呈现组件，为了防止内存泄漏，我们必须取消订阅 **componentWillUnmount:** 中的所有订阅**

```
class **SomeUglyComponent** extends **Component** {
   **componentDidMount**() {
      this.subscription = store
          .select(someSelector)
          .subscribe((update) => {
             this.setState({data: update});
             this.forceUpdate();
   }
   **componentWillUnmount**() {
      this.subscription.unsubscribe();
   }
   **render**() {
      return (<div>{ this.state.data }</div>);
   }
}
```

好吧，我不得不承认这是可行的，但是**有些难看的组件**并不难看，因为我们只有一个。让我们想象一下，我们必须实现这样的组件几十次，几百次。现在看起来更丑了吧？这个重复任务需要一个通用的解决方案，并且 **SomeUglyComponent** 应该尽可能简单，这样我们就可以把它叫做 **SimpleAndNiceComponent** 。期望的 **SimpleAndNiceComponent** 如下:

```
const **SimpleAndNiceComponent** = **connect**({
   data: store.select(someSlector)
})(({data}) => <div> {data} </div>);//or
@**connect**({
   data: store.select(someSlector),
})
class **SimpleAndNiceComponent** extends **Component** {
   **render**() {
      const {data} = this.props;
      return <div> {data} </div>;
   }
}
```

那么，我们如何实现这种简单性，以及如何将可观察物与道具联系起来呢？答案就藏在 **HOC** s 里面，我们需要比简单的 **HOC** 更进一步，我们需要的是 **HOC** 工厂。我将实现一个 **connect** 函数，它接受观察值并返回一个 **HOC** ，后者接受 **React。组件**并返回一个增强/包装的 **React。组件:**

为了清楚起见，上面的例子被简化了。当然，如果我们需要将它用于生产，我们必须更多地考虑*边缘用例*和*可调试性*以及*错误预防和恢复*策略。尽管这是一个简化，但这段代码将会完成这个任务，并允许我们用一个通用的连接机制编写更简单的组件，该机制具有 observables 的异步特性和 **React 的同步特性。组件**。

# 最后，最后，最后的话

是的，我们终于有了一个完全相同的库。我只能说，我在这个实验中学到了很多，我想分享它，希望我的旅程能帮助别人。下一步将是使用它，种植它，爱它！

如果你能读到这一点，并和我一起完成这个实验，我将非常高兴。非常感谢你！请不要忘记**鼓掌**或在下方留下你的**评论。**

如果你想为我的实验做贡献，并帮助我把它变成一个真正的图书馆，可以解决现实生活中的问题，不要犹豫，点击下面的链接，并作出贡献。你只会受到欢迎！

[](https://github.com/onerzafer/rxdx) [## onerzafer/rxdx

### 基于 rxjs 的 react 类 redux 库。在 GitHub 上创建帐户，为 onerzafer/rxdx 开发做出贡献。

github.com](https://github.com/onerzafer/rxdx)