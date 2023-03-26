# 使用 React 挂钩将类组件迁移到函数

> 原文：<https://medium.com/hackernoon/react-hooks-migrate-class-component-to-functional-and-use-hooks-36a59b130cdc>

![](img/03d37d9af23874a956ada5051ff52a58.png)

Photo by [Olliss](https://unsplash.com/@fomaolea?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们都知道 react 钩子已经准备好了，我们中的许多人已经开始使用它了。但是，如果我们有一个现有的项目，我们想移动到有类组件的 react hooks 呢？

这篇文章将指导你把一个现有的基于类的组件变成一个基于钩子的组件。这篇文章不打算深入钩子的细节。

这是文档中对`hooks`的描述。

> 钩子让我们不用写类就可以使用状态和其他 React 特性。

让我们从一个示例应用程序开始。这是一个简单的计数器应用程序。代码如下所示:

```
const initialData = {
  count: 0,
  maxLimit: 10
};class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: this.props.initialCount
    };
  }
  onIncrement = () => {
    this.setState({ count: this.state.count + 1 });
  };
  onDecrement = () => {
    this.setState({ count: this.state.count - 1 });
  };
  render() {
    return (
      <div>
        <button
          onClick={this.onDecrement}
          disabled={this.state.count === this.props.initialCount}
        >
          -
        </button>
        <div>{this.state.count}</div>
        <button
          onClick={this.onIncrement}
          disabled={this.state.count === this.props.maxLimit}
        >
          +
        </button>
      </div>
    )
  }
}class CounterApp extends React.Component {
  render() {
    const { initialData } = this.props;
    return (
      <React.Fragment>
        <h2>Counter Example</h2>
        <Counter
          initialCount={initialData.count}
          maxLimit={initialData.maxLimit}
        />
      </React.Fragment>
    )
  }
}ReactDOM.render(
  <CounterApp initialData={initialData} />,
  document.getElementById('root')
)
```

如你所见，我已经创建了一个简单的计数器应用程序。创建的类组件有:

*   `CounterApp`:这是主要的应用组件
*   `Counter`:实际计数器组件

请注意，这段代码是为这篇文章准备的，只是为了解释的目的。请忽略您可能看到的任何其他需要改进的地方。☺

因此，要将类组件转换为功能性/钩子组件，步骤如下:

## 首先选择最小的类组件，即自下而上的方法会更好。

1.  将类组件拥有的所有代码移动到 render 方法中并使其工作，例如事件处理函数或任何其他函数或代码。
2.  更新代码以使用`props` & `states`的对象分解。
3.  在`class`语法前添加一行，如下所示。

```
const Counter = () => {}
class Counter extends React.Component {
  /*...code goes here */
}
```

4.将 render 方法中的所有内容移动到更新的函数组件代码块中。

```
const Counter = () => {
  //...code inside render methods of class component code goes here
}
class Counter extends React.Component {
  //...Rest of the code
  render() {
    // This will be blank now
  }
}
```

5.将 props 的析构代码移到功能组件的括号内。从代码块中移除代码。

```
const Counter = ({ maxLimit, count }) => {
  /*...rest of the code */
}
```

6.更新组件以使用像`useState`这样的钩子或者任何其他可能需要的钩子。例如。

```
const Counter = ({ maxLimit, count }) => {
  //...Rest of the code
  const [count, setCount] = React.useState(count)
  //...Rest of the code
}
```

> *如果您有多个 state 属性，请将其更改为对所有属性单独使用挂钩。我在这里不解释如何写钩子的语法。请阅读相同的官方文档。*

7.最后，更新 return 语句代码块，使用新创建的 props(析构的)或使用钩子的状态。所做的任何其他更改都应相应纳入。并完全删除类组件代码行。

好吧，这可能看起来很多。让我们来研究一下现有的例子。

# 计数器组件

在 render 方法内移动`onIncrement`和`onDecrement`代码块。定义为`const`并将功能`this.onIncrement`和`this.onDecrement`分别更新为`onIncrement`和`onDecrement`。

在 render 方法中对`props`和`state`使用对象去结构化。在我们的例子中，您可以这样做:

```
const { maxLimit, count } = this.props
const { count } = this.state
```

现在，就在`class Counter extends React.Component {`行之前，创建一个类似如下的行:

```
const Counter = () => {}
```

从`render`方法中剪切所有内容，并将其粘贴到`const Counter = () => {}`的花括号中

将分解后的`props`移入括号中。第 2 行被注释掉，第 1 行现在有了道具`{maxLimit, initialCount}`。

```
const Counter = ({ maxLimit, count }) => {
  // const { maxLimit, initialCount } = this.props;
  const { count } = this.state
  // ....Rest of the code
}
```

接下来，我们需要改变我们的`this.state`。我们将在这里使用`useState`钩子。更新第 2 行的代码，如下所示:

```
const Counter = ({ maxLimit, initialCount }) => {
  const [count, setCount] = React.useState(initialCount);
  // ....Rest of the code
}
```

我们修改的代码是使用钩子的语法。

现在是时候更新我们的函数，使用我们用钩子定义的`setCount`。更改我们在`onIncrement`和`onDecrement`中的代码`this.setState`。更新后的代码如下所示:

```
const Counter = ({ maxLimit, initialCount }) => {
  // ....Rest of the code
  const onIncrement = () => {
    setCount(count + 1);
  };
  const onIncrement = () => {
    setCount(count - 1);
  };
  // ....Rest of the code
}
```

现在，相应地更新 return 语句的代码，以使用新的`count`状态(使用钩子创建)和去结构化的`props`。并且，完全删除`class Counter extends React.Component {}`代码。

`Counter`组件的最终代码应该如下所示:

```
const Counter = ({ maxLimit, initialCount }) => {
  const [count, setCount] = React.useState(initialCount);
  const onIncrement = () => {
    setCount(count + 1);
  };
  const onDecrement = () => {
    setCount(count - 1);
  };
  return (
    <div>
      <button
        onClick={onDecrement}
        disabled={count === initialCount}
      >
        -
      </button>
      <div>{count}</div>
      <button
        onClick={onIncrement}
        disabled={count === maxLimit}
      >
        +
      </button>
    </div>
  )
}
```

所以，你可能会注意到，最后没有使用`this`，代码看起来也干净了很多。

# CounterApp 组件

主`CounterApp`组件中没有太多东西，所以我们可以毫不费力地将其转换为功能组件。`CounterApp`的最终代码将如下所示:

```
const CounterApp = ({ initialData: {count, maxLimit} }) => {
  return (
    <>
      <h2>Counter Example</h2>
      <Counter initialCount={count} maxLimit={maxLimit} />
    </>
  )
}
```

> *疑惑*`*<>*`*`*</>*`*是什么？这是一种避免我们以前创建的额外元素层的方法。所以，用这个或者用文章开头所示的* `*React.Fragment*` *。**

*完整的计数器应用程序代码如下所示。*

```
*const initialData = {
  count: 0,
  maxLimit: 10
};
// added style to make it look better, in case you want to see
const counterStyles = {
  h1: {
    maxWidth: "185px",
    boxShadow: "5px 5px 4px #888888"
  },
  container: {
    display: "flex",
    alignItems: "center"
  },
  button: {
    width: "30px",
    borderRadius: "50%",
    borderStyle: "none",
    fontSize: "22px",
    backgroundColor: "khaki",
    height: "30px",
    paddingBottom: "5px",
    cursor: "pointer"
  },
  countDiv: {
    minWidth: "80px",
    padding: "0 15px",
    textAlign: "center",
    height: "30px",
    lineHeight: 2,
    border: "1px solid",
    borderRadius: "10px",
    margin: "0 5px"
  }
}const Counter = ({ maxLimit, initialCount }) => {
  // styles...
  const { container, button, countDiv } = counterStyles;
  const [count, setCount] = React.useState(initialCount);
  const onIncrement = () => {
    setCount(count + 1);
  };
  const onDecrement = () => {
    setCount(count - 1);
  };
  return (
    <div style={container}>
      <button
        style={button}
        onClick={onDecrement}
        disabled={count === initialCount}
      >
        -
      </button>
      <div style={countDiv}>{count}</div>
      <button
        style={button}
        onClick={onIncrement}
        disabled={count === maxLimit}
      >
        +
      </button>
    </div>
  );
};const CounterApp = ({ initialData: {count, maxLimit} }) => {
  return (
    <>
      <h2 style={counterStyles.h1}>Counter App: Hooks</h2>
      <Counter initialCount={count} maxLimit={maxLimit} />
    </>
  );
};ReactDOM.render(
  <CounterApp initialData={initialData} />,
  document.getElementById('root')
)*
```

# *摘要*

*通过这篇文章，我们学到了一些关于如何将基于类的组件转换成功能性的基于钩子的组件的技巧。我希望这将有助于您迁移现有的代码库。*

*如果有任何问题/顾虑，请随时联系我们。*

*快乐学习！*

**最初发表于*[*【https://www.elanandkumar.com】*](https://www.elanandkumar.com/blog/react-hooks-class-to-hooks-migration/)*。**