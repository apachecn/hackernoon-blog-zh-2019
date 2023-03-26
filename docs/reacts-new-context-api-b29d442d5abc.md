# React 的新上下文 API

> 原文：<https://medium.com/hackernoon/reacts-new-context-api-b29d442d5abc>

![](img/98a124a84a5854e5cfb1b80faa5ed7ae.png)

## 介绍

最近 React 引入了一些新的很酷的特性，其中一个有趣且备受期待的特性是**新的** **上下文 API** 。16.3 版引入了新的上下文 API。上下文 API 在没有道具训练的状态管理中非常有用。让我们深入研究一下。

## 问题

在许多情况下，在我们的应用程序中，我们需要将组件的状态传递到两到三个级别。因此，当并非所有的组件都需要道具时，我们将道具传递到组件树的一层又一层。假设组件层次结构很复杂，那么状态管理对开发人员来说是一种开销。

## 解决办法

对于状态管理，有几个可用的库，如 Redux(最常用的和趋势)。但是 React 引入了上下文 API 来解决道具钻取的问题，让开发者的状态管理工作变得简单。

## 何时使用上下文

正如 React 建议的“如果你只想避免通过许多关卡传递一些道具。Context 旨在共享可被视为 React 组件树的“全局”数据，如当前已验证的用户、主题或首选语言。

## 如何使用上下文

所以现在如果你想在你的应用程序中使用上下文，首先创建上下文对象。

```
import { createContext } from 'react';const {Provider, Consumer } = React.createContext();
```

然后创建一个包装器组件，它将返回 Provider 组件，并且还将您想要从中访问上下文的所有组件添加为子组件。

```
class ProviderComponent extends Component {
    state = {
        title : “Vishal”,
    }render() {
        return ( 
        <Provider value={this.state}>
          <Website />
        </Provider>
        )
    }
}
```

我们的网站组件看起来像

```
const Website = () => (
  <div>
    <Header />
    <Footer />
  </div>
)
```

现在，如果我们想从提供者组件到标题组件访问标题，我们可以简单地使用上下文来消费提供者组件的状态，而无需适当的钻取。

```
const Header = () => (
  <div>
     <Consumer>
        {(context) => context.title}
      </Consumer>
  </div>
)
```

## 结论

ReactJS 的这个很酷的新特性可以帮助开发人员简化他们的工作。对于一个简单的 prop drilling 解决方案，context 可以很好地工作，但是对于具有复杂状态和缩减器的大型应用程序，Redux 可能是一个更好的解决方案。

感谢阅读。我希望你喜欢这篇文章🙏