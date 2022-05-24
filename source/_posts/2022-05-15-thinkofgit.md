---
title: think of git
date: 2022-05-15 05:40:05
tags: [git,github]
categories: git
---

# git命令

> git remote add origin git@github.com:white-than-wood/white-than-wood.github.io.git

   从官方对于git remote的定义来看,意思是列出本地拥有的每个指定远程句柄的简称.简单来说本地与远程交流的一个简化链接,当在本地建立起一个已经初始化的git项目时,势必要与远程git库建立联系(远程git库本来也是为了保存代码、保证多人开发时代码的同步以及简化其流程性而存在的),那么此命令就是为了在远程git@github.com:white-than-wood/white-than-wood.github.io.git链接上面建立一个新远程句柄简称并进行关联,之后推送、同步代码直接与此句柄简称关联就可以了,不需要再去copy使用github远程库上面的链接.

> git push --set-upstream origin master

   在建立了与远程代码库的新远程句柄简称之后,首次推送同步代码,需要设置推送、同步代码的上游远程分支,当首次设置之后,后续无需设置.
--set-upstream origin master就是设置推送、同步代码的上游origin远程句柄简称的分支为master.

> git merge master --allow-unrelated-histories

   当同一个仓库存在多个独立的分支并没有公共的上游交集分支时,会出现无法合并的情况(多出现于本地git初始化时默认主分支为master,而远程github默认主分支为main).

    fatal: refusing to merge unrelated histories

   故此我们需要在人为确认合并分支安全的情况下,将多个独立的分支进行允许强制合并,也就是--allow-unrelated-histories.

# SSH公私钥安全关联

> 本地设置与github远程仓库的ssh安全关联

   只有持有账号私钥的情况下才可以推送、同步代码到拥有公钥的github远程仓库,使用ssh命令产生公私钥.我这里用的是rsa的加解密方式,小伙伴们也可以选择自己喜欢的加密方式.
   
    #使用自己的github账号来作为rsa加解密的注释
    #-t: type,选用rsa的密钥类型
    #-b: byte,公私钥的长度大小4096比特
    #-C: comments,添加公私钥生成的注释
    ssh-keygen -t rsa -b 4096 -C '15866369958@qq.com'

   在mac下,生成之后,前往自己账号目录下查询.ssh/id_rsa.pub,将id_rsa.pub文件里面的内容复制添加到github的账号settings设置下的SSH and GPG keys.
   
   <img src="/images/ssh_settings.png" style="width: 160px;float: left;"/>
   <img src="/images/ssh_settings_SSHKeys.png" style="width: calc(100% - 180px);float: left;margin-left: 20px;"/>
   <div style="clear: both;display: block;"></div>

   添加成功之后,我们测试一下,将远程github库('git@github.com'开头链接)克隆到本地,如果可以拉取到本地,那就说明ssh-keygen设置与github远程仓库ssh安全关联生效.
   
> 本地设置多个ssh公私钥与多个github远程仓库建立安全关联

   * ssh-add交与ssh-agent代理管理
 
   当需要与远程多个github仓库建立安全关联时,需要设置多个ssh公私钥.这时需要使用ssh-keygen与ssh-add联合使用.
   
    ssh-keygen -t rsa -b 4096 -C 'dreamthen.99@gmail.com'
    #这时会出现需要你设置进行保存公私钥的文件名,默认还是为id_rsa
    Generating public/private rsa key pair.
    Enter file in which to save the key (/Users/yinwk/.ssh/id_rsa): id_rsa_ano
    #设置过后,公私钥就会以id_rsa_ano.pub以及id_rsa_ano文件进行保存
    
   一系列操作结束后,我们还需要将新生成的私钥交与ssh-agent管理. ssh-agent简单来说是一个ssh密钥管理器,作为本地与远程建立ssh安全关联的代理机制,而ssh-add就是将新生成的私钥交与ssh-agent管理.

    ssh-add ~/.ssh/id_rsa_ano

   在成功交与ssh-agent管理后,我们重复第一个部分中'单个ssh公私钥与单个github远程仓库建立安全关联'的后续操作即可.

   

    