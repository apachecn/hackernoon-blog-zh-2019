# 从多个存储库转移到一个 lerna-js 单一存储库

> 原文：<https://medium.com/hackernoon/moving-from-multiple-repositories-to-a-lerna-js-mono-repo-d0fff3538c7e>

![](img/2a4a18ca7e6af4f1bdacc2ea5d22259d.png)

在 [mitter.io](http://mitter.io/) 中，我们有几个面向公众的`npm`包需要发布，我们最近转移到了由 Lerna 管理的单一回购结构，而不是为每个包都有单独的存储库。今天，我想分享一下我们在这一迁移过程中的经验，以及我们对新 monorepo 结构的设置。我们的所有软件包要么是 SDK 目标，要么是 SDK 目标的依赖项:

*   `@mitter-io/core`—[mitter . io](http://mitter.io/)SDK 的核心功能
*   `@mitter-io/models` -类型脚本模型(类、类型别名、接口等。)对于 SDK
*   `@mitter-io/web`-web SDK
*   `@mitter-io/react-native`-React 原生 SDK
*   `@mitter-io/node`-node.js SDK(用于 node . js 后端)
*   `@mitter-io/react-scl`-react js 应用的标准组件库

我们所有的包都是用 TypeScript 编写的，类型与包本身捆绑在一起，我们不分发单独的类型包(你通常看到的以`@types/`开头的包)。我们使用 [rollup](https://rollupjs.org/guide/en) 以 UMD 和 ES5 模块格式捆绑这些包。除此之外，我们使用 [TypeDoc](https://typedoc.org/) 生成我们的文档，然后在 **AWS S3** 的公共桶上发布。

在使用 Lerna 之前，我们的每个包都有一个单独的存储库，当我们只有 web SDK 时，它工作得很好。随着我们的发展，越来越多的开发人员开始开发 SDK，我们的设置开始面临一些问题:

1.  鉴于大多数 SDK 逻辑驻留在`@mitter-io/core`中，几乎所有发生在`core`包和所有其他包中的更改都必须更新以指向新版本。因此，即使有一个 bug 需要修复，比如说 React Native，更改也会在`core`中进行，但更新现在需要反映在所有其他目标中，即`web`、`node`和`react-native`。开发人员错过目标是很常见的。
2.  几乎 SDK 中的每个变化都会导致 5 个包中至少 3 个的变化。
3.  我们看到了跨包保持相同版本的巨大好处(使开发人员更容易猜测目标的最新版本)，但是手动跟踪变得很麻烦。
4.  `npm link`(或者`yarn link`，如果你喜欢的话)有自己的一套问题，确保所有的依赖关系都被链接，然后取消链接以使用来自`npm`的正确链接，并返回到本地链接进行开发。
5.  跨包运行脚本(例如，发布 typescript 文档)是很常见的，我们使用脆弱的符号链接和 bash 脚本来管理它们。

大约在那个时候，我们遇到了 L [erna](https://lernajs.io/) ，它似乎非常适合我们的要求。

我们决定遵循最简单的方法，尽量使用默认值。根据我们的经验，迁移到 Lerna 是一件轻而易举的事。首先创建一个新的 Lerna repo:

```
mkdir my-new-monorepo && cd my-new-monorepo
git init .
lerna init
```

回答几个简单的问题(我们总是使用默认设置)，就万事俱备了。将我们的旧包从它们的回购转移到新包(我们担心这会是一个巨大的痛苦)比预期的要容易得多:

```
lerna import ~/projects/my-single-repo-package-1 --flatten
```

> ***注意****`*--flatten*`*可能需要，也可能不需要，但是我们面临没有它的问题。**

*Lerna 的神奇之处在于它带来了所有的 git 提交(你可能会丢失一些关于`--flatten`的历史)，因此对于新的 repo，历史看起来像是一直在这个 monorepo 中进行开发。这是绝对必要的，因为在转移到 monorepo 之后，你将需要为你发现的一个 bug 找个人。*

*有了 Lerna，我们现在可以管理所有软件包的单一存储库，其目录结构如下所示:*

```
*packages/
 core/
 models/
 node/
 react-native/
 web/
lerna.json
package.json*
```

*要发布已更改的包，我们现在只需:*

```
*lerna boostrap
lerna publish*
```

*你不必每次都做`lerna bootstrap`；除非这是你第一次检查回购。它所做的只是在这个 repo 下安装每个包的所有依赖项。*

*与此同时，我们还决定简化我们的流程，并在`npm`生命周期本身中添加了所有打包任务。一定要注意，这和 Lerna 没有任何关系；无论回购结构如何，这都是任何 npm 包中应该有的东西。对于每个包，以下脚本出现在单独的`pacakge.json`中:*

```
*"scripts": {
 ...
 "prepare": "yarn run build",
 "prepublishOnly": "./../../ci-scripts/publish-tsdocs.sh"
 ...
}*
```

*这用 **typescript** 编译器构建了这个包，用 **rollup** 捆绑它，并用 **typedoc** 生成文档:*

```
*"scripts": {
 ...
 "build": "tsc --module commonjs && rollup -c rollup.config.ts && typedoc --out docs --target es6 --theme minimal --mode file src"
 ...
}*
```

*拥有一个单一的 repo 结构还允许您将公共脚本保存在一个地方，以便更改可以应用于所有的包(我们还应该将构建脚本移动到一个单独的脚本中，因为它现在已经变成了一个相当复杂的 bash 命令)。*

*除了发布之外的开发人员流程是不变的。开发人员在 GitLab 上创建一个问题(或被分配一个问题)，为该问题创建一个新的分支，然后在代码审查后将更改合并到 master。发布生命周期现在遵循一个非常结构化的过程:*

1.  *当一个里程碑完成后，我们计划发布一个新的版本，其中一个开发人员(负责那个特定的版本)通过运行`lerna version`创建一个新版本。*
2.  *Lerna 为确定下一个版本提供了一个非常有用且易于使用的提示*

```
*(master) mitter-js-sdk ツ lerna version --force-publish
 lerna notice cli v3.8.1
 lerna info current version 0.6.2
 lerna info Looking for changed packages since v0.6.2
 ? Select a new version (currently 0.6.2) (Use arrow keys)
 ❯ Patch (0.6.3)
 Minor (0.7.0)
 Major (1.0.0)
 Prepatch (0.6.3-alpha.0)
 Preminor (0.7.0-alpha.0)
 Premajor (1.0.0-alpha.0)
 Custom Prerelease
 Custom Version*
```

*一旦选择了新版本，Lerna 就会更改包的版本，在远程 repo 中创建一个标记，并将更改推送到我们的 GitLab 实例。除此之外，开发人员不需要做任何其他事情。我们的 CI 设置为构建所有名称类似于语义版本号的标记。*

> ****注意*** *我们用* `*--force-publish*` *运行* `*lerna version*` *，因为我们希望所有的包都有完全相同的版本血统。所以有时我们会有不同版本之间没有区别的包。根据您的偏好，您可以选择不这样做。**

*我们使用 GitLab 的集成 CI 来构建、测试和发布我们所有的项目(JS 和 Java)。对于新的 JS monorepo，我们有两个阶段:*

*构建阶段非常简单，运行以下两个脚本:*

```
*lerna bootstrap
lerna run build*
```

*这个阶段在每一次提交时运行，从本质上验证包的健全性。另一方面，发布阶段运行以下内容:*

```
*git checkout master
lerna bootstrap
git reset --hard
lerna publish from-package --yes*
```

> **我们发现我们必须做一个* `*git checkout master*` *和一个* `*git reset --hard*` *因为 GitLab 克隆(或获取，取决于配置)了回购，然后检查将要构建的提交。这将工作目录设置为“分离头”模式，即 ref* `*HEAD*` *不指向任何地方。Lerna 使用* `*HEAD*` *计算出当前版本的包，并在分离的 head 状态下出错。**

*我们还需要运行`lerna publish from-package`而不是`lerna publish`，因为执行简单的`lerna publish`会让 Lerna 抱怨当前版本已经发布，因为元数据是在开发者本地运行`lerna version`时更新的。`from-package`参数告诉 Lerna 为给定的包发布 npm 中当前没有的所有版本。如果由于某种原因发布失败，并且您正在重试管道，这也会有所帮助。*

*发布阶段被配置为仅在匹配以下 regex [credit](https://github.com/semver/semver/issues/232) 的标签上运行:*

```
*^v(0|[1-9]\d*)\.(0|[1-9]\d*)\.(0|[1-9]\d*)(-(0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*)(\.(0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*))*)?(\+[0-9a-zA-Z-]+(\.[0-9a-zA-Z-]+)*)?$*
```

*这有点花哨，对于大多数团队和大多数目的来说，简单的`^v*$`应该就可以了。:)*

***你可以在[https://github.com/mitterio/js-sdk](https://github.com/mitterio/js-sdk)查看我们的 monorepo(这是我们内部 GitLab repo 的镜像)。***

***当运行公共脚本时(就像我们发布 typescript 文档一样)，了解运行脚本的包的细节是非常有用的。这适用于 npm 生命周期中的脚本，以及可能使用`lerna run`或`lerna exec`运行的脚本。对于 npm 中给定的包，npm 使用环境变量使整个`package.json`对脚本可用。因此，对于具有以下`package.json`的给定包:***

```
***{
 "name": "@mitter-io/core",
 "version": "0.6.28",
 "repository": {
 "type": "git"
 }
}***
```

***运行任何生命周期脚本时，以下变量将可用:***

```
***npm_package_name=@mitter-io/core
npm_package_version=0.6.28
npm_package_repository_type=git***
```

*****怪癖/问题*****

***在新的设置中，我们仍然在做一些事情(其中一些是问题，而另一些我们可能不知道得更好):***

*   ***不确定这是否可能，但我们希望能够为我们所有的软件包提供通用的生命周期脚本。在根目录`package.json`中声明这些是不起作用的。***
*   ***在没有向 npm 发布任何东西的情况下，完全测试您的 Lerna 设置是非常困难的。不确定某处是否有`--dry-run`。***
*   ***Lerna 有办法为`devDependencies`保留一个公共的配置块，这样所有的`devDependencies`对于每个子包都是相同的版本。这是一个很酷的功能，但我们需要一些时间来剔除所有常见的功能。***
*   ***同样的情况也适用于其他依赖项，所以虽然我们不想要一个通用的`dependencies`配置块，但是有一种方法来表达跨项目可用的变量会很好。例如，在我们的 Java/Kotlin monorepo 中，我们使用`gradle.properties`来包含像`springBootVersion`、`springCoreVersion`等变量。，然后由各个 gradle 脚本使用。***

***我们对 monorepos 的看法最近在 monorepos 上有一场相当激烈的辩论，我们是否会看到大量的人再次加入这一潮流，这很容易让人想起微服务风靡一时的时代。***

***我们在这里遵循的结构是拥有多个单一回购，这并不是我们第一次管理单一回购。我们的整个平台和后端是一个 monorepo，包含私有的、可部署的代码和发布到 bintray 的多个面向公众的包。我们的主网站也运行在 Spring 后端，前端捆绑了支持热重装的 web pack(`webpack watch`)等等。我们从未决定在整个组织内采用单一回购，因为工具根本不存在。***

***将我们的大部分 Java 代码放在单个 repo 中非常有用，因为`gradle`提供了 Java monorepo 所需的所有工具，而`lerna`和 npm 生命周期为 JS SDK 的 monorepo 提供了工具。因此，简单地说，一旦你确定了回购中的变化范围，单一回购就很棒。对于我们的 Java 后端，我们看到一个特性有多个跨项目的 MRs，这使我们倾向于只为这个特定的项目转移到 monorepo，而我们所有的其他代码仍然在单独的 repos 中。一旦我们看到 JS SDKs 也出现了类似的模式，我们就转向了 Lerna。***

***请注意，我们是一个大约 9 名工程师的小团队；所以对我们有效的方法可能对不同规模的团队无效。我们最想指出的是，采用任何解决方案都不必是二元的，要么我们按规定做，要么根本不做。***

***我们看到的单一回购的一些动机肯定适用于我们，但很多动机并不适用。例如，如果我们的整个代码库被转移到一个单独的 repo 中，我们就不能抽出时间来构建工具——不管我们可能体验到或没有体验到好处。因此，这场辩论实际上并不是关于“单一回购”——就其本身而言，它只不过是一种新的目录结构。它们的处方是缓解某些问题，就像每一个“银弹”一样，也有警告。***

***这场辩论是关于软件行业面临的共同问题以及通常采取的解决方案；“普通”是关键词。你偏离“普通”应用程序的地方是你开始创新、改变和构建的地方。***

****原载于 2019 年 1 月 22 日*[*【medium.com】*](/mitterio/multirepo-to-lerna-js-monorepo-80f6657cb443)*。****