# 如何缓存一个 React 应用？

> 原文：<https://medium.com/hackernoon/how-to-cache-bust-a-react-app-6775081e09cc>

这篇文章也交叉发表在-

开发— [缓存破坏反应应用](https://dev.to/flexdinesh/cache-busting-a-react-app-22lk)

***TL；DR****—*[*SEM ver*](https://docs.npmjs.com/about-semantic-versioning)*您的应用程序，并在每个构建上生成一个不会被浏览器缓存的* `*meta.json*` *文件。当版本不匹配时，使缓存无效并硬重新加载应用程序。* ***注意:本帖中的例子和解释都是基于 React 的。但是这个策略将适用于任何 web 应用程序/框架。***

尽管缓存很棒，但缓存失效已经是一个长期的问题了。**使浏览器**中加载的网络应用的**缓存**失效是很难的**。但是**使**保存到**主屏幕**的网络应用的**缓存**失效**更加困难**。**

缓存的快速介绍-

**服务器缓存:** Web 服务器在第一次请求资源时缓存资源。第二次以后，资源由服务器缓存提供。除此之外还有很多——CDN、源服务器、边缘服务器等，但我们不会一一介绍。使服务器缓存失效非常简单，因为我们可以控制我们的服务器，并且在每次新部署时，我们可以自动或手动清除旧缓存。

**浏览器缓存:**浏览器也以自己的方式缓存资源。当一个站点第一次被加载到用户的浏览器中时，浏览器决定在本地缓存一些资源(主要是图像、js 和 css 等资产),下次用户访问同一个站点时，浏览器从本地缓存中提供资源。由于我们无法控制用户的浏览器，所以在过去清除用户浏览器中的缓存一直有点困难。有了缓存头和像 webpack 这样的构建工具，每次构建都会生成独特的块，这就变得更容易管理了，但是仍然存在缺陷。

以下是浏览器缓存的一些问题-

1.  **浏览器**倾向于**忽略缓存验证**如果站点在**同一个标签**中被刷新——如果用户锁定标签，即使服务器缓存被清除，站点也很有可能从浏览器缓存中被加载。
2.  如果你的应用程序正在注册一个**服务工作者**，那么服务工作者**缓存**将**失效**，除非用户在**新标签**中打开站点。如果该选项卡从未关闭，用户将永远无法使用服务工作人员缓存。
3.  如果用户**将**站点添加到手机/平板电脑的**主屏幕**，那么只有当用户明确**退出应用**时，浏览器**缓存**才会**失效**——这几乎等同于在浏览器中打开相同的标签页。我知道有些人几个月都不会退出他们的主屏幕应用程序。

理想情况下，缓存有助于更快地加载网站。禁用缓存不是答案。它也不可靠，因为你不能控制用户浏览器的行为。我们希望找到一种方法，在每次新版本的应用部署到服务器时，清除浏览器或服务人员缓存。

简单而有效的方法

*   永远不要部署
*   将应用版本捆绑到应用中
*   生成一个包含每次构建的应用版本的`meta.json`文件
*   加载时获取`meta.json`并比较版本
*   当版本不匹配时，强制清除缓存并硬重新加载

# 永远不要部署

用[永远不要用](https://docs.npmjs.com/about-semantic-versioning)给你所有的部署版本。我个人使用这三个 npm 命令来自动增加包的版本，并创建一个 git commit 和一个相应的版本标签。

*   `npm version patch` - *对于仅修复了错误的版本*
*   `npm version minor` - *针对具有新功能且未修复错误的版本*
*   `npm version major` - *主要版本或突破性功能*

记得用`--tag`属性- `git push origin master --tags`推送提交

# 将应用版本捆绑到应用中

在 webpack 构建(或相关构建工具)期间解析包版本，并在应用程序中设置一个全局变量，以便您可以在浏览器控制台中方便地检查版本，并使用它与最新版本进行比较。

```
import packageJson from '{root-dir}/package.json';
global.appVersion = packageJson.version;
```

一旦设置完成，您就可以通过键入`appVersion`在浏览器控制台中检查应用程序版本。

# 生成一个包含每次构建的应用版本的`meta.json`文件

运行一个脚本，在应用程序的`public`目录中生成一个`meta.json`文件。

添加一个`prebuild` npm 脚本，它将在每个`build`之前生成`meta.json`文件。

```
/* package.json */

{
    "scripts": {
        "generate-build-version": "node generate-build-version",
        "prebuild": "npm run generate-build-version",
        // other scripts
     }
}/* generate-build-version.js */

const fs = require('fs');
const packageJson = require('./package.json');

const appVersion = packageJson.version;

const jsonData = {
  version: appVersion
};

var jsonContent = JSON.stringify(jsonData);

fs.writeFile('./public/meta.json', jsonContent, 'utf8', function(err) {
  if (err) {
    console.log('An error occured while writing JSON Object to meta.json');
    return console.log(err);
  }

  console.log('meta.json file has been saved with latest version number');
});
```

在每次构建之后，一旦部署了应用程序，就可以使用路径`/meta.json`访问`meta.json`，并且可以像 REST 端点一样获取 json。它不会被浏览器缓存，因为浏览器不会缓存 XHR 请求。因此，即使您的包文件被缓存，您也将总是获得最新的`meta.json`文件。

因此，如果您的包文件中的`appVersion`小于`meta.json`中的`version`，那么我们知道**浏览器缓存是过时的，我们将需要使其无效**。

你可以用这个脚本来比较语义版本-

```
// version from `meta.json` - first param
// version in bundle file - second param
const semverGreaterThan = (versionA, versionB) => {
  const versionsA = versionA.split(/\./g);

  const versionsB = versionB.split(/\./g);
  while (versionsA.length || versionsB.length) {
    const a = Number(versionsA.shift());

    const b = Number(versionsB.shift());
    // eslint-disable-next-line no-continue
    if (a === b) continue;
    // eslint-disable-next-line no-restricted-globals
    return a > b || isNaN(b);
  }
  return false;
};
```

你也可以在我的 [GitHub 示例](https://github.com/flexdinesh/cache-busting-example/blob/ad03c264e2f52c71609726104e38ea3593520e07/src/CacheBuster.js#L6)中找到这段代码

# 加载时提取`meta.json`并比较版本

安装`App`后，读取`meta.json`并将当前版本与服务器中的最新版本进行比较。

当出现**版本不匹配时** = >强制**清除缓存**并在版本相同时硬重新加载= >渲染应用程序的其余部分

我已经构建了一个`CacheBuster`组件，它将强制清除缓存并重新加载站点。该逻辑将适用于大多数网站，但可以根据应用程序的定制情况进行调整。

```
/* CacheBuster component */
import packageJson from '../package.json';
global.appVersion = packageJson.version;

const semverGreaterThan = (versionA, versionB) => {
    // code from above snippet goes here
}

export default class CacheBuster extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      loading: true,
      isLatestVersion: false,
      refreshCacheAndReload: () => {
        console.log('Clearing cache and hard reloading...')
        if (caches) {
          // Service worker cache should be cleared with caches.delete()
          caches.keys().then(function(names) {
            for (let name of names) caches.delete(name);
          });
        }
        // delete browser cache and hard reload
        window.location.reload(true);
      }
    };
  }

  componentDidMount() {
    fetch('/meta.json')
      .then((response) => response.json())
      .then((meta) => {
        const latestVersion = meta.version;
        const currentVersion = global.appVersion;

        const shouldForceRefresh = semverGreaterThan(latestVersion, currentVersion);
        if (shouldForceRefresh) {
          console.log(`We have a new version - ${latestVersion}. Should force refresh`);
          this.setState({ loading: false, isLatestVersion: false });
        } else {
          console.log(`You already have the latest version - ${latestVersion}. No cache refresh needed.`);
          this.setState({ loading: false, isLatestVersion: true });
        }
      });
  }

  render() {
    const { loading, isLatestVersion, refreshCacheAndReload } = this.state;
    return this.props.children({ loading, isLatestVersion, refreshCacheAndReload });
  }
}
```

我们可以使用这个`CacheBuster`组件来控制`App`组件中的渲染

```
/* App component */
class App extends Component {
  render() {
    return (
      <CacheBuster>
        {({ loading, isLatestVersion, refreshCacheAndReload }) => {
          if (loading) return null;
          if (!loading && !isLatestVersion) {
            // You can decide how and when you want to force reload
            refreshCacheAndReload();
          }

          return (
            <div className="App">
              <header className="App-header">
                <h1>Cache Busting - Example</h1>
                <p>
                  Bundle version - <code>v{global.appVersion}</code>
                </p>
              </header>
            </div>
          );
        }}
      </CacheBuster>
    );
  }
}
```

你也可以在这里找到这两个组件的代码

CacheBuster—[CacheBuster . js](https://github.com/flexdinesh/cache-busting-example/blob/master/src/CacheBuster.js)

# 当版本不匹配时，强制清除缓存并硬重新加载

每次加载应用程序时，我们都会检查最新版本。根据应用程序版本是否过时，我们可以决定以不同的方式清除缓存。

举个例子，

*   您可以在渲染应用程序之前进行硬重新加载
*   你可以显示一个模式/弹出窗口，要求用户点击一个按钮并触发一个硬重载
*   当应用程序空闲时，你可以硬加载
*   您可以在几秒钟后用`setTimeout()`硬重新加载

你可以从这篇文章中找到完整的代码，以及这个报告中的一个工作示例— [缓存破坏示例](https://github.com/flexdinesh/cache-busting-example)

这是所有的乡亲。如果你对这种方法有任何反馈(好的和坏的)，请在评论中告诉我。

破坏缓存很有趣。🎉

*最初发表于*[](https://dineshpandiyan.com/cache-busting)**。**