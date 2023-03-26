# 一起破解 Git 和 Vim

> 原文：<https://medium.com/hackernoon/hacking-git-and-vim-together-272a5830708a>

![](img/687f69b4257388570ef3ecfa022aef24.png)

Hacking… just not the type of hacking you’re thinking

如今，在我作为程序员的工作中，我大量使用 Vim 和 Git。我说的很多是指**很多**！

Vim 的最新版本将终端带到了 Vim。因此，将它用作窗口管理器并为 Vim 提供专用缓冲区应该更容易。你做你的工作，你去 Vim 专用缓冲区并提交你所有的修改。
更好的是，Vim 已经有很多插件专门用于在 Vim 内部操作 Git。像[vim-逃犯](https://github.com/tpope/vim-fugitive)、 [git-vim](https://github.com/tpope/vim-git) 或 [vim-gitgutter](https://github.com/airblade/vim-gitgutter) 这样的插件浮现在脑海中。

但你知道我，我算是一个黑客…在某种意义上，我喜欢一起黑东西(不是邪恶的蒙面密码窃取者类型的黑客)。

我很快意识到，90%的时间我使用相同的 3 或 4 个基本 Git 命令，因为我们可以使用 bang(！)为了在 Vim 中从命令模式执行 shell 命令，我决定将几个命令和映射组合在一起，帮助我进行基本的 Git 使用，而不必离开 Vim，没有专用的终端缓冲区，也没有插件的开销。

我首先使用 bang 操作符在 Vim 中运行基本的 shell 命令。运行这些命令的结果显示在一个新窗口中。

像这样的命令

`*:!git diff
:!git commit
:!git log*`

属于这一类。至此，我也意识到，常用的、经常使用的命令可以也应该被映射。例如:

`*map <F3> :!git diff*`

这些是我在这个黑客项目中的第一次实验。但我很快意识到，涉及多个文件的命令，如 *add* 或 *commit* 以及可能有很长输出的命令，如 *diff* 会导致一些问题。

那是我开始使用 **%** 操作符的时刻。操作符包含了当前文件的路径。因此，类似以下的命令:

`*:!git diff %
:!git commit -m “Commit message nº1” %*`

现在只考虑当前文件。这样，我可以更好地管理我的提交，并在没有太大问题的情况下检查非常具体的差异。

另一个真正有用的命令是 *:r！*命令。这个命令一开始有点难以理解。在 Vim 帮助页面中，我们可以看到 *:r！*命令:

> 执行{cmd}并将其标准输出插入光标或指定行的下方。使用一个临时文件来存储命令的输出，然后将该输出读入缓冲区。

每当我想把一些输出粘贴到一个文件中时，我就使用这个命令。我习惯了使用类似 *:r 这样的命令！吉特差异*。这样，我在一个临时文件中保存了完整的 diff。

![](img/d2ead0c39f11d31afcb68773299ab14e.png)

Storing output into files to parse with shell commands can be really useful.

经过一些反复试验，我最终得到了几个映射，它们反映了我最基本的 Git 用法。我一直在用的那种操作。对于像合并分支、创建分支、存储或分支创建这样的事情，我仍然喜欢启动终端，边打字边思考。

`*map <F1> :! git status
map <F2> :! git diff %
map <F3> :! git add %
map <F4> :! git commit -m “This commit was automatically created with Vim” %
map <F5> :! git log — oneline — abbrev-commit*`

仅仅使用这 5 行代码，我就能够在 Vim 中合理地提高我的 Git 使用速度。

这对我很有效，这是一个很好的练习。我绝对推荐你尝试一下，但是也看看一些为这个效果创建的插件，因为它们有更多的功能和思想。

最后，我建议你测试所有的东西，但是只使用对你有效的东西！

![](img/053f3cacbb0c0a4d951a5d9075cfbc3b.png)

想了解更多关于 Vim 的知识？想学习如何将其用作 IDE？看看我的新书[一个叫做 Vim](https://leanpub.com/anidecalledvim) 的 IDE。从基本的 Vim 使用到文件查找、自动完成、文件管理器等等，它都有。