## Git Command
---
基本用法



- [amend](https://www.atlassian.com/git/tutorials/rewriting-history)  
> 若想要更改一次提交，使用`git commit --amend`。git会使用与当前提交相同的父节点进行一次新提交，旧的提交会被取消。  某次需求开发提交中，由于某种原因导致提交的结果不是期望的，需要重新提交，但是又不希望前一次的提交使git提交历史树变长，那么可以使用`git commit --amend`来用暂存区的内容修正前一次的提交。 如果不使用带`--amend`的提交，结果是git提交历史树会变长,如下图所示：  

![](https://wac-cdn.atlassian.com/dam/jcr:a4de784b-3572-4d23-8c68-cea9ad4f205f/01.svg?cdnVersion=ht)

- [cherry-pick](http://pinkyjie.com/2014/08/10/git-notes-part-3/)  
> "复制"一个提交节点并且在当前分支做一次完全一样的提交  

![](https://marklodato.github.io/visual-git-guide/cherry-pick.svg)

>cherry-pick 和 merge的区别就是cherry-pick 不合并分支，而merge会把两个分支合并成一个分支。

- reset  (git reset ^HEAD)
> reset可以遗弃不再使用的提交，执行遗弃的时候，可以根据影响的范围指定不同的模式，可以指定是否复原索引或者工作树的内容。

![](https://marklodato.github.io/visual-git-guide/reset-commit.svg)
如果没有给出提交点的版本号，那么默认用HEAD，这样，分支指向不变，但是索引会回滚到最后一次提交，如果用--hard选项，工作目录也同样。
![](https://marklodato.github.io/visual-git-guide/reset.svg)


- [stash](https://git-scm.com/book/en/v1/Git-Tools-Stashing)

> 当你正在进行项目中某一部分的工作，里面的东西处于一个比较杂乱的状态，而你想转到其他分支上进行一些工作。问题是，你不想提交进行了一半的工作，否则以后你无法回到这个工作点。解决这个问题的办法就是git stash命令,“‘储藏”“可以获取你工作目录的中间状态——也就是你修改过的被追踪的文件和暂存的变更——并将它保存到一个未完结变更的堆栈中，随时可以重新应用。即`git stash`是首先备份当前工作区的内容， 然后从最近一次的提交中读取相关的内容，让工作区和上次提交的内容一致，同时将当前的工作区内容保存到Git栈中。
