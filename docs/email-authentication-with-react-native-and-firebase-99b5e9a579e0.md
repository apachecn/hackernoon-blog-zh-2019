# 使用 React native 和 Firebase 进行电子邮件身份验证

> 原文：<https://medium.com/hackernoon/email-authentication-with-react-native-and-firebase-99b5e9a579e0>

## 有各种各样的方法来验证一个用户可能是通过使用脸书，谷歌，Twitter，LinkedIn 和电子邮件等。但是**电子邮件认证仍然是最流行的认证方式**。在本教程中，我们将带您使用 React native 和 Firebase 创建**电子邮件认证。**

![](img/db883dbc972d81607981a47bd6715641.png)

> 文章原载于[我的博客](https://kriss.io/posts/Email-Authentication-with-React-native-and-Firebase/)

将 React Native 与 firebase 一起使用使其易于实现。所以不浪费更多的时间，让我们开始吧

首先，利用世博会创造新的本土项目。

# 使用 Expo 通过 React native 和 Firebase 进行电子邮件身份验证:

在这篇文章中，我们在不接触 Xcode 或 Android studio 的情况下使用 expo for。打开您的终端或命令行，键入:

```
expo init fb-react-native-firebase
```

按下回车键后，你会看到如下所示的屏幕，在这里你可以输入应用程序和 slug 的名称。点击后，再次输入

![](img/abecd48d14e77c83d719a93e418de155.png)

安装过程完成后，您将看到如下所示的屏幕。

![](img/2204c2da88ef2e16fbab0d1ca557b757.png)

然后使用 cd app-name 转到如上所示的项目文件夹。之后，您可以通过简单的命令直接运行应用程序。

`**yarn start**`

我们要用 VS 代码开发这个项目。所以在 visual studio 代码中打开它，它们的 open terminal 运行 `**yarn start**`。

![](img/1c17b7b76c772f2491650a2ac0b4fb12.png)

世博会将在浏览器中打开一个新窗口，如下所示。在那里，您可以看到使用 android 模拟器或设备、iOS 模拟器和使用 QR 码扫描仪运行应用程序的选项。我们正在使用 Android 模拟器来展示这个应用程序。

![](img/1a6621171c27260c3bda5f029c4d48de.png)

然后打开你的模拟器，用它来打开 expo 应用程序。

![](img/10e8696f829ff4e6c3485eb9dbf5819d.png)

模拟器会占用大量的系统内存空间，这取决于您的设备或操作系统。我试着在 Mac 上使用 Android，它占用了太多空间。

所以现在我们准备编码。

# 用原生碱基快速形成自举

为了更快更流畅地设计我们的界面，我们使用 Native base 来开发这个应用程序。您可以查看原生基础以了解更多信息。所以**让我们继续下一步**

![](img/d16849a628e368587e96aad4bec1104d.png)

打开一个新的终端运行`**npm i native-base**`

下一步是将必要的本地基础组件导入到`**App.js**`

```
import { Container, Item, Form, Input, Button, Label } from "native-base";
```

并构造表单界面

```
render() {
    **return** (
      <Container>
        <Form>
          <Item floatingLabel>
            <Label>Email</Label>
            <Input autoCapitalize="none" autoCorrect={false} />
          </Item>
          <Item floatingLabel>
            <Label>Password</Label>
            <Input
              secureTextEntry={true}
              autoCapitalize="none"
              autoCorrect={false}
            />
          </Item>
          <Button full rounded success>
            <Text>Login</Text>
          </Button>
        </Form>
      </Container>
    );
  }
```

当您保存结果时，您可以立即在屏幕上看到它们。这就是所谓的热重装功能。

![](img/17bbbe28e8de24deb994eb9d78d49d9e.png)

顶部会出现一个表单。

现在，表单设计已经就绪。让我们转到主要的编程部分。

# 添加 firebase 来反应本地项目

让我们用 `npm i firebase`添加 firebase，在 App.js 上导入 firebase，然后导入到我们的项目中。

```
import * as firebase from "firebase";
```

下一步，您需要**创建一个 firebase 项目。转到 firebase console** 并进行如下操作:

![](img/b162b351e620e47b1c55dcf81542b1fb.png)

**现在获取配置 apikey，它对于从数据库中访问数据非常重要。**

![](img/bc0463c5aadd420cf8fb3ca67f79b27b.png)

并粘贴到 App.js 中，如下面的代码结构所示。

```
import * as firebase from "firebase";
import { Container, Item, Form, Input, Button, Label } from "native-base";var config = {
    apiKey: "AIzaSyDFdsjQWG8IFLXmviNqSiVZMw_ADFl5tpo",
    authDomain: "react-native-firebase-3bde9.firebaseapp.com",
    databaseURL: "https://react-native-firebase-3bde9.firebaseio.com",
    projectId: "react-native-firebase-3bde9",
    storageBucket: "react-native-firebase-3bde9.appspot.com",
     messagingSenderId: "269398778466"
};firebase.initializeApp(config);
```

现在，我们已经成功地将 firebase 添加到我们的项目中。

## 签约雇用

对于电子邮件身份验证，我们需要在 Firebase 控制台上激活电子邮件身份验证。

![](img/086ffc4027b831c9ba4177424ebc0ba9.png)

让我们跳回到 VS 代码，并添加一个注册按钮。

```
<Button full rounded success style={{ marginTop: 20 }}> <Text>Signup</Text>
</Button>
```

结果视图应该是

![](img/5eb1069b7eeec8ad806f9423c3e77bd3.png)

现在我们必须添加注册代码。

```
export default class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      email: "",
      password: ""
    };
  }
  SignUp = (email, password) => {
    try {
      firebase
          .auth()
          .createUserWithEmailAndPassword(email, password)
          .then(user => { 
                 console.log(user);
           });} catch (error) {
      console.log(error.toString(error));
    }
  };
```

我们创建了一个处理电子邮件和密码的状态。我们已经创建了处理 firebase 代码的表单和注册功能。

接下来，使用 onChangeText 向状态添加表单值。

```
<Item floatingLabel>
            <Label>Email</Label>
            <Input
              autoCapitalize="none"
              autoCorrect={false}
              onChangeText={email => this.setState({ email })}
            />
          </Item>
          <Item floatingLabel>
            <Label>Password</Label>
            <Input
              secureTextEntry={true}
              autoCapitalize="none"
              autoCorrect={false}
              onChangeText={password => this.setState({ password })}
            />
```

我们通过注册按钮的 onPress 事件触发注册功能。

```
onPress={() => this.SignUp(this.state.email, this.state.password)}
```

保存并尝试提交表单。

![](img/3c8b93965ede8681674aa953c45618a4.png)

提交数据后，转到 firebase 控制台，检查您输入的数据是否正确。

![](img/5142e092b67dd235df795273dfd833c7.png)

现在，我们已经在 Firebase 中以新用户身份成功添加了数据。

## 注册

对于登录，我们使用来自注册方法的代码，并更改 firebase 函数。

```
Login = (email, password) => {
    **try** {
      firebase
         .auth()
         .signInWithEmailAndPassword(email, password)
         .then(res => {
             console.log(res.user.email);
      });} **catch** (error) {
      console.log(error.toString(error));
    }
  };
```

之后需要使用**上的【状态变更】方法**获取用户数据。然后添加 **onPress 方法登录 Buton。**

```
onPress={() => this.LogIn(this.state.email, this.state.password)}
```

让我们尝试登录。

![](img/7b4b5b186ea4dfc0550cc345682b4812.png)

你好，我们终于成功了。我们从 firebase 用户数据库中获得了数据。

## 结论

在本教程中，您已经了解了如何使用 expo 和 firebase 设置 react native 项目。您还学习了如何使用 NativeBase kickstart 构建用户界面？之后，我们使用 React Native、Firebase 和 expo 创建了一个基本的电子邮件认证。

我们希望你能从中得到些什么。如果是的话，请与你的朋友和需要的人在一起。任何问题，请不要犹豫。