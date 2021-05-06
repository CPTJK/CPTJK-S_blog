# git rebase

## 概述

在git中,有两种方式来整合不用分支

## git rebase -i

`git rebase -i`是`git rebase --interactive`的简写, 字面意思理解就是**交互式的rebase**
这个命令可以做很多事情,一句话总结就是重写提交历史

使用`git rebase -i <某一次提交>`,必须指定你想重写那一次提交后的所有历史.举个例子,如果你想修改最后三次的提交,那么命令是:
```git
git rebase -i HEAD~3 //注意实际上HEAD~4是从现在往前的第四个提交.
```

运行上面的命令,会在文本编辑器上给你一个提交列表:

```git
pick 1d1372e chore(comment-limit): 使用userService
pick c84fcca refactor(city-picker): 迁移CityPicker ts
pick 4938647 refactor(fileupload): 迁移FileUpload到ts
# 注意上面看到的提交记录的顺序跟你使用git log看到的提交记录的顺序是反的
# Rebase 2b12c29..4938647 onto 2b12c29 (3 commands)
#
# Commands:
# p, pick <commit> = use commit 直接使用对应的提交记录
# r, reword <commit> = use commit, but edit the commit message 使用提交记录,但是会停下来,让你修改commit信息
# e, edit <commit> = use commit, but stop for amending 使用提交记录,会停下来让你修改提交内容(对这个提交做amend)
# s, squash <commit> = use commit, but meld into previous commit 使用提交记录,但是这个提交的内容会被合并到前一个提交记录中,不会单独成一个提交记录. 不过commit 信息默认会跟上一条记录一起保留,你可以停下来编辑commit 信息
# f, fixup <commit> = like "squash", but discard this commit's log message 跟squash的区别就是不保留commit 信息
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit 丢弃这个commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified). Use -c <commit> to reword the commit message.
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
"~/Documents/fe-console/.git/rebase-merge/git-rebase-todo" 29L, 1302C
```