# 用单体组件设计管理反应模式

> 原文：<https://medium.com/hackernoon/managing-react-modals-with-singleton-component-design-5efdd317295b>

*(本帖原帖到我个人* [*博客*](https://www.techynovice.com/manage-react-modal-with-singleton-component-design/) *)*

在开发应用程序时，控制应用程序中所有类型的模态(信息模态、定制警告、图像灯箱等等)是一个令人沮丧的问题。当应用程序很简单时，放置一个单独的`<Modal />`组件并有一个状态来切换它的开/关就足够了。然而，当你在应用程序中有不同的地方呈现相同的模态或者不同种类的模态时，事情就变得复杂了。

对此已经有了一些设计解决方案，比如使用 React Context API ( [这篇博客](/@BogdanSoare/how-to-use-reacts-new-context-api-to-easily-manage-modals-2ae45c7def81)就是一个例子)或者操纵`react-navigation`的`StackNavigator`(如[这篇博客文章](https://blog.brainsandbeards.com/better-modals-in-react-native-8ea6fb207146?gi=23e52db335d3)中所述)。在这篇文章中，我想分享我自己使用 singleton `<Modal />` 组件解决这个问题的设计。我们将从该解决方案中获得一个命令式 API，如:

```
SomeModal.show(title, content, { ...options })
SomeModal.hide()
```

(请注意，这有点类似于没有导航道具的`react-navigation`向导[导航)](https://reactnavigation.org/docs/en/navigating-without-navigation-prop.html)

`<SomeModal />`是一个 react 组件，它在顶层呈现如下:

```
class App extends Component {
  render() {
    // Container is your own wrapper component
    return (
      <Container>
        {/* Your real app */}
        <SomeModal />
      </Container />
    );
  }
}
```

那么当我们调用`show()`和`hide()`方法时`SomeModal`是如何知道该做什么的呢？首先，我们将定义组件方法`__show()`和`__hide()`，然后在它的构造函数中，我们通过使用`this`保存它对静态属性`SomeModal`的引用(因为构造函数中的`this`将是对呈现组件的引用)。该引用将可以访问`SomeModal`的所有组件方法。我们现在可以创建两个静态方法`show(), hide()`并使用它:

```
class SomeModal extends Component {
  static show() {
    SomeModal.__singletonRef.__show();
  }

  static hide() {
    SomeModal.__singletonRef.__hide();
  }

  constructor(props) {
    super(props);
    SomeModal.__singletonRef = this;
  }

  __show() { ... }
  __hide() { ... }
}
```

当做这个单例组件时，我们需要确保我们不会在应用程序中使用这个组件两次，因为引用将是不正确的。在我看来，当我们在应用程序中有一个定义好的模态风格，并且我们想在任何地方显示和隐藏它时，这是最好的选择。这也允许在 React 组件之外显示和隐藏模态。

如果你有不同种类的情态动词呢？在我自己的项目中，对于每一种模态，我都制作一个单独的 React 组件，并将其渲染到顶层，如下所示:

```
<Container>
  {...}
  <FeedbackModal />
  <LinkPreviewModal />
  <AlertModal />
</Container />
```

希望你发现这种处理情态动词的方式很有用。请让我知道你的想法！

黑客快乐！