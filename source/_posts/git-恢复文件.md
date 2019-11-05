---
title: git 恢复文件
abbrlink: 32f296c0
date: 2019-11-04 22:13:19
tags: [code,git,tech]
categories: [study,code]

---


平时提交代码的时候，一般也都是先从本地工作区提交代码
```
git add .
git commit -s
git push
```
一段代码的提交顺序：
```
工作区  ->  git add .  -> 暂存区 -> git commit -> 本地仓库 -> git push -> 远程仓库
```
被追踪的文件一共有五个状态：

* 未修改 origin
* 已修改 modified
* 已暂存 staged
* 已提交 committed
* 已推送 pushed

如何检查本地的修改，以及如何查看不同状态之间的修改，这就要用到` git diff `命令

直接使用` git diff `命令，能够查看已修改，未暂存的内容

使用 `git diff --cache` 来查看已暂存，未提交的内容

使用` git diff origin/master master `来查看已提交，未推送的差异。

```
工作区          暂存区           本地仓库                    远程仓库
    \          /     \          /         \                  /
     \        /       \        /           \                /
     git diff         git diff --cache     git diff origin/master master
```

<!-- more -->
图解
{% asset_img git-time.jpg This is an example image %}

在知道如何查看四个不同区之间的差异后，如何使用 git reset 来撤销呢？

# 撤销工作区修改

只是修改了文件，没有任何 git 操作，直接一个命令就可回退：


```
$ git checkout -- aaa.txt # aaa.txt为文件名
```
???
> 可以将 git add . 和 git checkout . 看做一对反义词，修改完成后，如果想 Git 往前进一步，让修改进入暂存区，执行 git add . 如果向后退则执行 git checkout .

# 撤销暂存区修改
如果已经执行了` git add`，意味着暂存区中已经有了修改，但是需要丢弃暂存区的修改，那么可以执行 `git reset`

```
git reset <file>
```

来单独将文件从暂存区中丢弃，将修改放到工作区。

对于从来没有被 Git 追踪过，是 **new file** 的文件，则需要使用：
```
git reset HEAD <file>
```
来将新文件从暂存区中取出放到工作区。

如果确定暂存区中的修改完全不需要，则可以使用
```
git reset --hard
```
**谨慎使用 –hard 命令**， 暂存区中所有修改都会被丢弃。**修改内容也不会被重新放到工作区**。

# 撤销本地提交

使用 `git add` 并且执行了` git commit `的修改,此时修改已进入本地仓库，撤销依然用` get reset `命令
```
git reset --hard origin/master
```
既然本地的修改已经不再需要，那么从远端将代码拉回来就行。

**不过不建议直接使用** ` git reset --hard origin/master `这样太强的命令，如果想要撤销本地最近的一次提交，可以使用
```
git reset --soft HEAD~1
```
将最近一次提交 `HEAD~1 `从本地仓库回退到暂存区, `--soft `不会丢弃修改，而是将修改放到暂存区，后续继续修改，或者丢弃暂存区的修改就可以随意了。如果要撤销本地两次修改，则改成` HEAD~2` 即可，其他同类。
*注意* 已经提交到远端的提交，不要使用 `git reset` 来修改，对于多人协作项目会给其他人带来很多不必要的麻烦。

# 撤销远端仓库修改

- 对于已经推送的修改，原则上是不要撤销的
- 在明确自己在做什么的情况下，可以使用 `git push -f `使用 `force` 选项来将本地库 `force` 覆盖远端仓库，强制 `push` 到远端。
> 对于个人，一个人使用的项目使用这样的方式，并没有太大问题，但是如果对于多人项目，如果你强行改变了远端仓库，别人再使用的时候就会出现很多问题，所以使用 git push -f 时一定要想清楚自己在做什么事情。
