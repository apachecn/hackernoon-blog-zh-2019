# 通过道具来反应路由器的链接组件

> 原文：<https://medium.com/hackernoon/pass-props-to-react-routers-link-component-2eebda7a63e1>

![](img/246c8c9431b07c8f42523875e15deb52.png)

通常，当使用 React Router 构建应用程序时，您需要通过链接组件向新路由传递属性。在这篇文章中，我们将分析这个过程是如何工作的。

有两种不同的方式将数据从`Link`组件传递到正在呈现的新路径。第一个是通过 URL 参数，第二个是通过`state`。

首先，我们来看看 URL 参数。如果你读过我们的 [URL 参数](https://tylermcginnis.com/react-router-url-parameters/)帖子，你会对这个例子很熟悉。假设我们负责构建呈现 Twitter 的[个人资料](https://twitter.com/tylermcginnis)页面的`Route`。如果使用 React 路由器创建，该路由可能如下所示

```
<Route path='/:handle' component={Profile} />
```

注意`handle`将是动态的。它可以是从`tylermcginnis`或`dan_abramov`到`realDonaldTrump`的任何东西。

因此，在我们的应用程序中，我们可能有一个类似这样的`Link`组件

```
<Link to='/tylermcginnis'>Tyler McGinnis</Link>
```

如果点击，用户将被带到`/tylermcginnis`并且`Profile`组件将能够从`props.match.params.handle`访问动态句柄(`tylermcginnis`)。

```
class Profile extends React.Component {
  state = {
    user: null
  }
  componentDidMount () {
    const { handle } = this.props.match.params fetch(`[https://api.twitter.com/user/${handle}`](https://api.twitter.com/user/${handle}`))
      .then((user) => {
        this.setState(() => ({ user }))
      })
  }
  render() {
    ...
  }
}
```

URL 参数很好，但是它们并不是真正用来从一个路由到另一个路由获取数据的，因为它们仅限于字符串。如果我们不只是想传递一个字符串，而是想传递一个稍微复杂一点的东西，比如一个对象或者一个数组，会怎么样？使用 URL 参数就没有办法做到这一点。这就把我们带到了从一个路由向另一个路由传递数据的第二种方式，那就是使用`state`。

回到我们之前的例子，如果当用户点击`Link`时，我们想传递用户从某条路线到`Profile`组件，该怎么办？React 路由器为我们提供了一种方法，API 如下所示

```
<Link to={{
  pathname: '/tylermcginnis',
  state: {
    fromNotifications: true
  }
}}>Tyler McGinnis</Link>
```

现在，为该路线呈现的组件(在本例中为`Profile`)将能够通过访问`props.location.state`来访问`fromNotifications`。

```
class Profile extends React.Component {
  state = {
    user: null
  }
  componentDidMount () {
    const { handle } = this.props.match.params
    const { fromNotifications } = this.props.location.state fetch(`[https://api.twitter.com/user/${handle}`](https://api.twitter.com/user/${handle}`))
      .then((user) => {
        this.setState(() => ({ user }))
      })
  }
  render() {
    ...
  }
}
```

概括地说，有两种方法可以将数据从一个`Link`传递到新的路由:URL 参数和`state`。URL 参数对字符串非常有用，但之后就失效了。通过让`Link`的`to`支持一个对象，你可以在`state`属性下传递任何你需要的数据，并且这些数据可以在`props.location.state`下的新路径中被访问。

*这篇文章最初发表在 tylermcginnis.com**的* [*上，作为他们*](https://tylermcginnis.com/react-router-pass-props-to-link/) [*React 路由器*](https://tylermcginnis.com/courses/react-router/) *课程的一部分。*