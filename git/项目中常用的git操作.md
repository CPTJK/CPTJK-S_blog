# 项目中常用的git操作

## git commit --amend

* 提交了一个commit,但是想要修改commit信息,直接执行`git commit --amend`,进入`vim`编辑,保存即可
* 提交了一个commit,但是意识到有个地方需要改一个,修改对应的地方,执行`git add <file>; git commit --amend`

## `git commit --amend` + `git push -f`只能用于自己私有的远端分支!!!`

* 提了MR,code review时候发现了问题,使用`git commit --amend`简单修改后, 重新`git push -f`强推到远端.
    > 这里要使用 -f (force)是因为`git commit --amend`会产生新的commit记录,使得`git push`不能进行`fast-forward`更新,默认情况下`git push`只允许`fast-forward`更新
* 

