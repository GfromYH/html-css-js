### git游戏学习

<https://learngitbranching.js.org/>

git commit  提交修改记录

git branch <name> 创建分支

git checkout <name> 将节点移动到分支位name上

~~~javascript
简写
git checkout -b <name>
~~~

git merge <name> 将当前分支合并到名字位name分支上

git rebase <name>将当前分支合并到为name分支上 ，这个相比merge会更加的线性，但执行还是并行



+ HEAD

> （译者注：实际这些命令并不是真的在查看 HEAD 指向，看下一屏就了解了。如果想看 HEAD 指向，可以通过 `cat .git/HEAD` 查看， 如果 HEAD 指向的是一个引用，还可以用 `git symbolic-ref HEAD` 查看它的指向。但是该程序不支持这两个命令）

通过操作符**^**来寻找父节点

git checkout c4;git checkout HEAD^



------

>
>
> ### 强制修改分支位置
>
> 你现在是相对引用的专家了，现在用它来做点实际事情。
>
> 我使用相对引用最多的就是移动分支。可以直接使用 `-f` 选项让分支指向另一个提交。例如:
>
> ```
> git branch -f master HEAD~3
> ```
>
> 上面的命令会将 master 分支强制指向 HEAD 的第 3 级父提交。

## Git Reset

`git reset` 通过把分支记录回退几个提交记录来实现撤销改动。你可以将这想象成“改写历史”。`git reset` 向上移动分支，原来指向的提交记录就跟从来没有提交过一样。

git reset HEAD~n(当前位置向上移动次数)

![1563278844273](C:\Users\ZX50V\AppData\Roaming\Typora\typora-user-images\1563278844273.png)

## Git Revert

虽然在你的本地分支中使用 `git reset` 很方便，但是这种“改写历史”的方法对大家一起使用的远程分支是无效的哦！

为了撤销更改并**分享**给别人，我们需要使用 `git revert`。来看演示：

git revert HEAD



## Git Cherry-pick

本系列的第一个命令是 `git cherry-pick`, 命令形式为:

- `git cherry-pick <提交号>...`

如果你想将一些提交复制到当前所在的位置（`HEAD`）下面的话， Cherry-pick 是最直接的方式了。我个人非常喜欢 `cherry-pick`，因为它特别简单。

> 这里有一个仓库, 我们想将 `side` 分支上的工作复制到 `master` 分支，你立刻想到了之前学过的 `rebase` 了吧？但是咱们还是看看 `cherry-pick` 有什么本领吧。
>
>
>
> git cherry-pick C2 C4
>
>
>
> 这就是了！我们只需要提交记录 `C2` 和 `C4`，所以 Git 就将被它们抓过来放到当前分支下了。 就是这么简单!

如果不知道当前要复制的哈希值的话

> 交互式 rebase 指的是使用带参数 `--interactive` 的 rebase 命令, 简写为 `-i`
>
> 如果你在命令后增加了这个选项, Git 会打开一个 UI 界面并列出将要被复制到目标分支的备选提交记录，它还会显示每个提交记录的哈希值和提交说明，提交说明有助于你理解这个提交进行了哪些更改。
>
> 在实际使用时，所谓的 UI 窗口一般会在文本编辑器 —— 如 Vim —— 中打开一个文件。 考虑到课程的初衷，我弄了一个对话框来模拟这些操作。
>
>
>
>  Git示范
>
>
>
> 当你点击下面的按钮时，会出现一个交互对话框。对提交记录做个排序（当然你也可以删除某些提交），点击确定看结果
>
>
>
> git rebase -i HEAD~4
>
>
>
> Git 严格按照你在对话框中指定的方式进行了复制。



