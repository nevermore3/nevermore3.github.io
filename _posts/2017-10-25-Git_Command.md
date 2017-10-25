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
