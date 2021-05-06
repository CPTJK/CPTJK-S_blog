# 理解git

## commit

git commit 对应一次提交记录,这个记录保存的是你当前仓库的所有文件的快照,就像是把整个目录复制,然后再粘贴一样, 但是实际上比复制粘贴要优雅许多.

git 希望提交记录尽可能的轻量, 因此在你每次进行提交时, 它并不会盲目的复制整个目录.条件允许的情况下,它会将当前版本与仓库中的上一个版本进行对比,并把所有的差异打包到一起作为一个提交记录.

git 保存了提交的历史记录,大多数提交记录上面都有父节点. 维护提交历史记录对项目成员都有好处.

关于提交记录太深入的东西我们不继续探讨,可以把提交记录看作是项目快照. **提交记录非常轻量,可以快速地在这些提交记录之间切换**

![image](/git/static/commit.png)
## branch

git 中的分支非常轻量,本质上是一个指针,简单的指向某个提交记录.仅此而已.所以有一个原则: **早建分支,多用分支**,因为创建再多的分支也不会造成存储上的开销,按照逻辑分解工作到不同的分支要比维护那些特别臃肿的分支简单很多.

使用分支其实就相当于说:**我想基于这个提交以及它所有的父提交进行新的工作.**

![image](/git/static/branch.png)

## HEAD

HEAD也是一个指针,指向的是当前工作的分支,上图中的`*`就代表`HEAD指针`,`git check <branchName>`实际上就是让HEAD指针指向相应的分支

1. rebase 提取了相应的commit补丁和修改,应用在了另外一个commit的基础上
2. fast-forward:是一种现象

## merge

我们新建了一个分支,在这个分支上面开发了某个新功能,开发完成后,需要合并回主线.有两种方法可以达到我们的目的,第一种是`merge`

`merge`会产生一个特殊的提交记录,它有两个父节点.翻译成自然语言就是说,*这个节点能把两个父节点本身以及它们所有的祖先都包含进来.*

![image](/git/static/merge1.png)

如上图: 这个时候我们执行 `git merge bugfix`命令,将得到:

![image](/git/static/merge2.png)

**c4就是`git merge`产生的那个特殊的提交记录**
但是这时候,我们注意到`bugfix`还停留在`c2`,如果要让`bugfix`与`main`同步, 我们可以先切换到`bugfix`,然后再merge `main`.
执行`git checkout bugfix; git merge main`,将得到:

![image](/git/static/fastForward.png)

这个时候,`bugfix`也同样指向了`c4`,但是我们之前说,`git merge`命令会产生一个新的`commit 记录`,要解释这个现象,就要引入一个新的概念 `fast-forward`

## fast-forward

`fast-forward`不是一个命令,它描述了一个现象: **git 只需要简单的移动指针就可以达到我们想要的结果**, `fast-forward`不只会发生在`git merge`时候, 也可能发生在`git rebase`的时候. 后者我们后面会讲解到.

**什么时候会`fast-forward`?** 从数学的角度来讲, 就是????

## rebase

第二种合并分支的方式是`git rebase`. `rebase`实际上是**取出一系列的提交记录,然后'复制'它们,然后再另一个地方逐个的放下去**

`rebase`的优势是可以创造更线性的提交历史,如果只允许使用`rebase`,而不用`merge`的话,代码库的提交历史将会变得异常清晰.

![image](/git/static/rebase.png)

如上图,我们现在执行`git rebase main`命令,将得到:

![image](/git/static/rebase2.png)

同样的,我们想让`main`跟`bugfix`保持同步,我们可以执行命令`git checkout main; git rebase bugfix`

![image](/git/static/fastForward2.png)

注意,这时候同样产生了`fast-forward`现象
