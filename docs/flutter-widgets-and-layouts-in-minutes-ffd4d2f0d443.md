# 在几分钟内弹出窗口小部件和布局-第一部分

> 原文：<https://medium.com/hackernoon/flutter-widgets-and-layouts-in-minutes-ffd4d2f0d443>

![](img/146f8cdbb31a3f58b74d72e9b50d14bf.png)

尝试过快速学习吗？。你觉得可能吗？

现在，介绍[**FlutterExamples**](https://github.com/TakeoffAndroid/FlutterExamples)**，**精选范例、设计、布局和部件的终极清单。

github—[https://github.com/TakeoffAndroid/FlutterExamples](https://github.com/TakeoffAndroid/FlutterExamples)

有什么特别的？

创建这个 repo 背后的想法是在几分钟内尽可能容易地更快地学习 flutter UI。那么，怎么做？我知道大家都很震惊。让我们看一些受欢迎的例子来分析这一点。

相信我！不解释！没有理论依据！

**容器**

![](img/89e61f991443004edbf1a41c5c62fb04.png)

```
Container(
 padding: const EdgeInsets.all(0.0),
 color: Colors.cyanAccent,
 width: 80.0,
 height: 80.0,
),
```

**居中**

![](img/4d430f53f8923c7274340525b854356a.png)

```
Center(child: Container(
  padding: const EdgeInsets.all(0.0),
  color: Colors.cyanAccent,
  width: 80.0,
  height: 80.0,
))
```

**对齐**

![](img/59a0b0573ce1a93018e6011c4583d04b.png)![](img/89fd1d234d6795953c1cfc0a8950c64b.png)![](img/c1817f3844a46c53d15c7f072c323aef.png)

Bottom Align (Left, center and right)

![](img/a8474e9e38019cbf252a197d75a2eec5.png)![](img/4d430f53f8923c7274340525b854356a.png)![](img/75d63a0ef655ce50937f2308802b40ac.png)

Center Align (Left, center and right)

![](img/dee185e9657b1f57a80866a2acd42028.png)![](img/5b1b9efab5b8005ae3b9ee08bba96cf0.png)![](img/3484ffce51c9d6aebd2d4278e3641a0a.png)

Top Align (Left, center and right)

```
Align(
  alignment: Alignment.center, 
  child: Container(
  padding: const EdgeInsets.all(0.0),
  color: Colors.cyanAccent,
  width: 80.0,
  height: 80.0,
))
```

**填充**

![](img/8fdfe49308cc22b5e7b16298c0d3b4cb.png)

```
Padding(
  padding: EdgeInsets.fromLTRB(24, 32, 24, 32) ,
  child: Container(
  padding: const EdgeInsets.all(0.0),
  color: Colors.cyanAccent,
  width: 80.0,
  height: 80.0,
))
```

**尺寸框**

![](img/d00965ec14e4a58e997583157d380e99.png)

```
SizedBox(
  width: 200.0,
  height: 100.0,
  child: Card(
    color: Colors.indigoAccent,
    child: Center(
        child: Text('SizedBox',
            style: TextStyle(color: Colors.white)
         )
       )
     )
   )
```

**展开**

![](img/facaf28190170a704f58385dc7abb6ce.png)

Row

![](img/3ad2a5b4ec1d24de6da1163ad3251246.png)

Column

```
Row(
  mainAxisAlignment: MainAxisAlignment.center,
  mainAxisSize: MainAxisSize.max,
  crossAxisAlignment: CrossAxisAlignment.center,
  children: <Widget>[
    Expanded(
      child: Container(color: Colors.cyan, height: 80),
      flex: 2,
    ),
    Expanded(
      child: Container(color: Colors.indigoAccent, height: 80),
      flex: 3,
    ),
    Expanded(
      child: Container(color: Colors.orange, height: 80),
      flex: 4,
    ),
  ],
),
```

**平面按钮**

![](img/f0beb59038e70c0a749bc53492daeced.png)

```
FlatButton(
  onPressed: () {
    debugPrint('I am Awesome');
  },
  textColor: Colors.white,
  color: Colors.blueAccent,
  disabledColor: Colors.grey,
  disabledTextColor: Colors.white,
  highlightColor: Colors.orangeAccent,
  child: Text('Flat Button'),
),
```

**凸起按钮**

![](img/9a8adc1715e4088e0d813d10bc34a9bd.png)

```
RaisedButton(
  onPressed: () {
    debugPrint('I am Awesome');
  },
  textColor: Colors.white,
  color: Colors.blueAccent,
  disabledColor: Colors.grey,
  disabledTextColor: Colors.white,
  highlightColor: Colors.orangeAccent,
  elevation: 4.0,
  child: Text('Raised Button'),
),
```

**图标按钮**

![](img/7cc893c2e06ac7a9d29e711ef0814029.png)

```
IconButton(
  onPressed: () {
    debugPrint("Starred Me!");
  },
  color: Colors.orangeAccent,
  icon: Icon(Icons.star),
  disabledColor: Colors.grey,
  highlightColor: Colors.black38,
),
```

**浮动动作按钮**

![](img/26aa5fc67a9f3cab7e7f44e690e1c058.png)

FAB (Default)

![](img/312664e79537aef2382e1c2b0f937d20.png)

FAB (Mini)

```
return Scaffold(
  floatingActionButton: new FloatingActionButton(
    mini: true,
    child: new Icon(Icons.add),
    onPressed: () {},
  ),
);
```

**文本字段**

**下线条样式**

![](img/13b65e179ded6f6628a9f3497bd69979.png)

```
TextField(
  decoration: InputDecoration(
  hintText: "Enter your name!",
  hintStyle: TextStyle(fontWeight: FontWeight.w300, color: Colors.blue),
  enabledBorder: new UnderlineInputBorder(
      borderSide: new BorderSide(color: Colors.blue)),
  focusedBorder: UnderlineInputBorder(
    borderSide: BorderSide(color: Colors.orange),
  ),
  ),
)
```

**外部线条样式**

![](img/57c36adb32de260399deb0ae76590c53.png)

```
TextField(
  decoration: InputDecoration(
  hintText: "Enter your name!",
  hintStyle: TextStyle(fontWeight: FontWeight.w300, color: Colors.blue),
  enabledBorder: new OutlineInputBorder(
      borderSide: new BorderSide(color: Colors.blue)),
  focusedBorder: OutlineInputBorder(
    borderSide: BorderSide(color: Colors.orange),
  ),
  ),
)
```

更多收藏的小工具可以在 [**这里**](https://github.com/TakeoffAndroid/FlutterExamples) 找到