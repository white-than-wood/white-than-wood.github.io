---
title: think of git
date: 2022-05-15 05:40:05
tags: [git,github]
categories: git
---

# git命令

> git remote add origin git@github.com:white-than-wood/white-than-wood.github.io.git

从官方对于git remote的定义来看,意思是列出本地拥有的每个指定远程句柄的简称.简单来说本地与远程交流的一个简化链接,当在本地建立起一个已经初始化的git项目时,势必要与远程git库建立联系(远程git库本来也是为了保存代码、保证多人开发时代码的同步以及简化其流程性而存在的),那么此命令就是为了在远程git@github.com:white-than-wood/white-than-wood.github.io.git链接上面建立一个新远程句柄简称并进行关联,之后推送同步代码直接与此句柄简称关联就可以了,不需要再去copy使用github远程库上面的链接.

> git push --set-upstream origin master

在建立了与远程代码库的新远程句柄简称之后,首次推送同步代码,需要设置推送同步代码的上游远程分支,当首次设置之后,后续无需设置.
--set-upstream origin master就是设置推送同步代码的上游origin远程句柄简称的分支为master.

> git merge master --allow-unrelated-histories

当同一个仓库存在多个独立的分支并没有公共的上游交集分支时,会出现无法合并的情况(多出现于本地git初始化时默认主分支为master,而远程github默认主分支为main).

    fatal: refusing to merge unrelated histories

故此我们需要在人为确认合并分支安全的情况下,将多个独立的分支进行允许强制合并,也就是--allow-unrelated-histories.