# 使用 Draftjs 在 Reactjs 中制作自己的富文本编辑器

> 原文：<https://medium.com/hackernoon/make-your-own-rich-text-editor-in-reactjs-using-draftjs-25a0b4fb05e5>

![](img/ccd877c01f48304d7e3ae3838cb3575f.png)

Photo by [Jakob Owens](https://unsplash.com/@jakobowens1?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 为什么了解富文本编辑器很重要？

你可能已经在 instagram、facebook 和其他平台上看到了类似上面的介绍部分。它们支持很多很酷的功能，比如表情符号、标签、@提及和其他东西。甚至还有其他类型的样式(如粗体、斜体、引号等)。你有没有想过如何在自己的 react 应用中实现这一点？如果是，那么你就在正确的地方，因为在阅读完这篇文章之后，你将能够像那样制作你自己的富文本编辑器。哦，😄正如最后一行和标题所示，这篇文章是为那些了解 React 基础知识的人准备的，这是我保证的唯一先决条件。因此，在阅读本文之前，请确保您已经熟悉 React。

# 让我们开始制作自己的富文本编辑器

在真正开始编写 React 代码之前，我们需要 React 生态系统，这意味着所有这些 Webpack 配置，以及 babel transpiler 的配置，这是一个重要的部分，但从头开始做所有这些事情将会使本文致力于的富文本编辑器黯然失色。感谢脸书的开源项目 [create-react-app](https://github.com/facebook/create-react-app) ，使用它我们可以通过在你的终端上写一个简单的命令来开始制作 React App。

因此，让我们使用 create-react-app 来制作 React 应用程序。我不会在我的系统中全局安装这个包，而是使用 npx(它会自动从 npm 注册表中为您安装一个同名的包，并调用它。完成后，已安装的软件包将不会出现在您的 globals 中的任何位置，因此从长远来看，您不必担心污染)。

```
npx create-react-app myapp
cd myapp
npm start
```

嘣，你的 react app 托管在 localhost:3000/。

您将拥有这样的目录结构

```
my-app
├── README.md
├── node_modules
├── package.json
├── .gitignore
├── public
│   ├── favicon.ico
│   ├── index.html
│   └── manifest.json
└── src
    ├── App.css
    ├── App.js
    ├── App.test.js
    ├── index.css
    ├── index.js
    ├── logo.svg
    └── serviceWorker.js
```

所以最后，我们准备好实际编码我们的富文本编辑器(兴奋的❕ ❕)。那我们要做什么？检查我的[富文本编辑器](http://myrte.surge.sh)。尝试键入( :、@或任何链接)您可以清楚地看到富文本编辑器的威力。您可能还见过一些变量，如( **organization.name、organization.email 和许多其他变量)。**只要你点击它们，你就会看到车把的小胡子语法。不要害怕，如果你不知道车把。这不是本文所必需的。我实现了它们，在我的一个项目中，我需要制作一个界面，每个组织可以为一些动作触发器设计他们自己的电子邮件模板。

# 草稿基础-js

Draft-js 中使用的基本组件是一个编辑控件。如果我们将文本输入与编辑控件进行比较，那么文本输入的值就是文本字符串。在一个编辑控件中，我们需要的不仅仅是简单的文本，我们需要一个保存文本、光标位置等的对象。除了文本，我们还使用了 EditorState 对象。要成功使用编辑器控件，必须知道几个关键事项:

*   EditorState 控件是不可变的。这意味着您永远不会更改它或它的任何属性。相反，如果您想要更改像文本这样的内容，您可以使用一个方法(具体来说是 onChange())返回一个全新的 EditorState，它是旧 EditorState 和您的新更改的副本(请记住 React 中的“State”对象，就像我们使用 setState()方法来更改状态而不是直接改变对象一样)。然后，新的 EditorState 被提供给编辑控件，您可以在其中看到您的更改。
*   EditorState 控件有子对象，显示编辑器中所表示内容的实际状态，包括文本、光标/选择、样式等。

## 定义

*   ***编辑器状态:*** 编辑器中所代表的对象负责当前状态
*   ***ContentState:*** 编辑器中负责文本的对象。内容由内容块组成。
*   ***selection state:***负责光标的对象和光标的选择区域
*   ***实体:*** 可以添加到部分文本中的元数据

这些是你在代码和草稿中随处可见的一些基本术语——js[docs](https://draftjs.org/docs/getting-started)也是如此。

## 我们的项目

让我们回到我们的反应应用程序。你可以从 [Github](https://github.com/dsc712/Rich-Text-Editor) 中克隆这个项目进行并排评审，但是我会推荐你自己制作。项目中的 App.js 是我们的根组件。看起来像这样

```
*import* React, { Component } *from* 'react';
*import* MyEditor *from* './Editor'*class* App *extends* Component {
  render() {
    *return* (
        <MyEditor/>
    );
  }
}*export default* App;
```

现在，我的编辑是从哪里来的？它来自我们的另一个组件 Editor.js。现在，在制作这个组件之前，让我们安装 draft-js 和 draft-js-plugins-editor。等等，你也可以在没有 npm 库的情况下实现你的编辑器。那么我们为什么要进口它呢？答案是，它给了你一种在编辑器中添加许多即插即用扩展的方式，就像(提及、表情符号、链接、内嵌工具栏等)。该编辑器仅基于 draft-js 编辑器构建。因为这个库只给编辑。对于其他类型的东西，我们仍然需要 draft-js。更多细节请看这个。

```
npm install draft-js draft-js-plugins-editor --save
```

好了，现在我们为 Editor.js 做好了准备

```
*import* React, { Component } *from* 'react';
*import* { convertToRaw, EditorState, RichUtils } *from* 'draft-js';
*import* Editor *from* 'draft-js-plugins-editor';
```

进口这些东西。

```
*class* MyEditor *extends* Component { constructor(props) {
        *super*(props);
        *this*.state = { editorState: EditorState.createEmpty() };
      *this*.onChange = (editorState) => *this*.setState({editorState});
    }handleKeyCommand = ( command, editorState ) => {*let* newState;
      newState = RichUtils.handleKeyCommand( editorState, command );
      *if*( newState ) {
         *this*.onChange(newState);
         *return* "handled"
      }*return* 'non-handled'};<Editor
      *editorState*={ *this*.state.editorState }
      *onChange*={ *this*.onChange }
      *handleKeyCommand*={ *this*.handleKeyCommand }/>}
```

我们已经使用 EditorState.createEmpty()方法初始化了编辑器的空状态，并将其添加到 MyEditor 组件的状态中。因此，当编辑器状态更改时，组件会重新呈现。编辑器组件有 3 个 props 第一个是 editorState，下一个是 onChange，说明是一个[控制的](https://stackoverflow.com/questions/42522515/what-are-controlled-components-and-uncontrolled-components-in-reactjs-and-what)组件，第三个是 handleKeyCommand，它使用 RichUtils 为选中的状态提供富文本编辑。尝试在编辑器中输入 cmd + b、cmd +i、cmd + u 和 cmd + z，看看其中的神奇之处。您也可以在这里定制命令检查[。这就是富 utils 为我们做的。现在，让我们添加一些 draft-js 插件，看看其中的神奇之处。](https://stackoverflow.com/questions/42311815/how-to-create-custom-key-bindings-in-draft-js)

**提外挂**

导入插件及其 CSS 文件。

```
*import* createMentionPlugin, { defaultSuggestionsFilter } *from* 'draft-js-mention-plugin';
*import* 'draft-js-mention-plugin/lib/plugin.css';
```

创建一个 reference . js 文件，向其中输入一些数据，然后从该文件中导出数据。

**提提. js**

```
*const* mentions = [
    {
        name: 'Matthew Russell',
        link: 'https://twitter.com/mrussell247',
        avatar: 'https://pbs.twimg.com/profile_images/517863945/mattsailing_400x400.jpg',
    },
    {
        name: 'Julian Krispel-Samsel',
        link: 'https://twitter.com/juliandoesstuff',
        avatar: 'https://avatars2.githubusercontent.com/u/1188186?v=3&s=400',
    },
    {
        name: 'Jyoti Puri',
        link: 'https://twitter.com/jyopur',
        avatar: 'https://avatars0.githubusercontent.com/u/2182307?v=3&s=400',
    },
    {
        name: 'Max Stoiber',
        link: 'https://twitter.com/mxstbr',
        avatar: 'https://pbs.twimg.com/profile_images/763033229993574400/6frGyDyA_400x400.jpg',
    },
    {
        name: 'Nik Graf',
        link: 'https://twitter.com/nikgraf',
        avatar: 'https://avatars0.githubusercontent.com/u/223045?v=3&s=400',
    },
    {
        name: 'Pascal Brandt',
        link: 'https://twitter.com/psbrandt',
        avatar: 'https://pbs.twimg.com/profile_images/688487813025640448/E6O6I011_400x400.png',
    },
];*export default* mentions;
```

很好，现在将这个文件导入到 MyEditor 组件中。

```
*import* mentions *from* './mentions';
```

现在初始化这个插件

```
*// mention plugin ( do it globally out of MyEditor class )
const* mentionPlugin = createMentionPlugin();
*const* { MentionSuggestions } = mentionPlugin;
```

现在将这个组件作为兄弟组件添加到 MyEditor 中，并将这两个组件包装到一个 div 标签或 react 片段中。

```
<div>
<MyEditor ... />
<MentionSuggestions
    *onSearchChange*= { *this*.onSearchChange }
    *suggestions*= { *this*.state.suggestions }
    *onAddMention*= { *this*.onAddMention }
/>
</div>
```

在构造函数中修改这一行，

```
*this*.state = {editorState: EditorState.createEmpty(), suggestions: mentions};
```

实现这两个方法

```
onSearchChange = ({ value }) => {
    *this*.setState({
        suggestions: defaultSuggestionsFilter(value, mentions),
    });
}; onAddMention = () => { // add logic after mentioning someone here };
```

太好了，你已经准备好在你的编辑器中添加@ mentions 了。尝试键入@。这个插件需要一些额外的工作，其他可用的插件非常直接和易于使用。只需导入它们，初始化它们，最后作为兄弟组件添加到 MyEditor 中。就这样，你已经准备好使用它们了。查看我的 [GitHub repo 中提供的示例代码。](https://github.com/dsc712/Rich-Text-Editor)

现在让我们看看我是如何添加变量和块变量组件的，它们本质上是原子的，也就是说，在退格时整个块被删除。这样模板中的变量就不会被污染。检查 [repo 中的 Variable.js 和 BlockVariable.js 文件。](https://github.com/dsc712/Rich-Text-Editor)代码很简单，过一遍就清楚了。

其中使用的主要概念是

1.  [修改器](https://draftjs.org/docs/api-reference-modifier)
2.  [AtomicBlockUtils](https://draftjs.org/docs/api-reference-atomic-block-utils)
3.  [实体](https://draftjs.org/docs/advanced-topics-entities)

我希望这篇文章能帮助你开始使用 draft-js。Draft-js 有很多东西，你要努力去发现。你越深入其中，你就越能和你的编辑玩得开心。我会试着多写几篇关于 draft-js 高级主题的文章。

一些入门资源:

1.  [草稿-js 单据](https://draftjs.org/docs/getting-started)
2.  我发现的[好资源](https://draft-js-samples.now.sh/Home)之一。

如果你喜欢，请鼓掌。是的，如果你有任何疑问，可以在这里提问，我会尽力帮助你。快乐编码😃。