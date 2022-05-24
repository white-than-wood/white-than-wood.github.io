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
   
> 本地设置多个ssh私钥与多个github账号远程仓库建立安全关联

   - ssh-add交与ssh-agent代理高速缓存管理.

       - ssh-agent.

         ssh-agent实际上是一个本地的高速缓存代理机制,可以在用户不输入任何密码短语的情况下,将session缓存中的私钥与远程仓库建立ssh安全关联并实行通信.
     
       - ssh-add.
     
         ssh-add是将指定的ssh私钥置于ssh-agent的高速缓存中,执行时会在ssh-agent建立一个session,并将ssh私钥放入其中.注意,此行为是一次性行为,也就是临时性行为,重启ssh-agent之后,会重置.
             
             #可用于查看ssh-agent的高速缓存中的私钥列表.
             ssh-add list
             #清空ssh-agent的高速缓存中的私钥列表.
             ssh-add -D
             #重启ssh-agent.
             eval $(ssh-agent)
       
       - 步骤.

         使用ssh-keygen与ssh-add联合使用.
    
             ssh-keygen -t rsa -b 4096 -C 'dreamthen.99@gmail.com'
             #这时会出现需要你设置进行保存公私钥的文件名,默认还是为id_rsa
             Generating public/private rsa key pair.
             Enter file in which to save the key (/Users/yinwk/.ssh/id_rsa): id_rsa_ano
             #设置过后,公私钥就会以id_rsa_ano.pub以及id_rsa_ano文件进行保存

         一系列操作结束后,我们还需要将新生成的私钥交与ssh-agent代理高速缓存管理.

             ssh-add ~/.ssh/id_rsa_ano

         在成功交与ssh-agent管理后,我们重复上一个部分中'本地设置与github远程仓库的ssh安全关联'的后续操作即可.
     
       - 问题.
     
         使用此方法是有比较多的问题的,首先就是重启ssh-agent或者电脑之后,ssh-agent中的高速缓存会重置,也就是会被清空掉.再就是对于同一个ssh域名下的链接建立ssh安全关联时,ssh-agent会选择高速缓存列表中的一个缓存私钥来建立通信.举个🌰:
       
             #这两个github库有着同样的域名,也就是@github.com,在建立通信时,如果将两个账号下生成的私钥都放入ssh-agent高速缓存中,ssh-agent会默认选择高速缓存列表中先放入的ssh私钥进行通信.
             #只有在切换不同的账号项目开发时,将ssh-agent重启,并且将当前项目所对应的ssh私钥放入ssh-agent高速缓存中才可以建立ssh安全关联并且实行通信.
             git@github.com:white-than-wood/white-than-wood.github.io.git
             git@github.com:dreamthen/webpack-rebuild.github.io.git

       - 结论.
         
         ssh-add交与ssh-agent代理高速缓存管理这种方法,本质也不是为了让多个ssh公私钥与多个github远程仓库建立安全关联而产生的,作用是为了避免每次与远程建立ssh安全关联时必须填写密码短语.并且我们在生成ssh密钥时,一般也是不设置密码短语的.这种每次必须手动向高速缓存中添加私钥的行为,不仅麻烦,而且是临时性且不合理的.
   
   - 配置.ssh/config文件使不同本地项目ssh私钥永久性与远程不同github账号仓库建立安全关联.

     - .ssh/config.
     
       通过配置config文件来辅助管理ssh,通常.ssh/config文件是不存在的,需要自己创建配置.
     
           touch ~/.ssh/config
     
     - 步骤.
       
       - 配置config.
       
         配置多个域名对应不同的ssh私钥,与本地自定义的域名别称建立联系.
     
             vim ~/.ssh/config
       
         ```text
         #github
         Host github.com-white-than-wood
              HostName github.com
              User white-than-wood
              IdentityFile "~/.ssh/id_rsa"
              IdentitiesOnly yes
       
         #github
         Host github.com-dreamthen
              HostName github.com
              User dreamthen
              IdentityFile "~/.ssh/id_rsa_ano"
              IdentitiesOnly yes
       
         #Host: 自定义的域名别称,要与本地不同账号的项目域名关联
         #HostName: 对应的代码存储/协同网站域名,如github.com,不同公司的gitlab域名不同
         #User: 对应的不同代码存储/协同网站域名账号名称
         #IdentityFile: 对应的不同代码存储/协同网站域名账号下的ssh私钥
         #IdentitiesOnly: 只能通过指定的IdentityFile来与远程建立ssh安全关联
         ```
       
       - 配置本地不同账号的项目域名、账号名称以及email地址.
     
         进入不同github账号的项目下,修改.git/config文件,注意自定义的域名别称要与.ssh/config中的Host对应,因为每次建立ssh安全关联实行通信都会通过.ssh/config文件进行辅助管理.
       
           ```text
             #white-than-wood账号下的项目
             [remote "origin"]
                     url = git@github.com-white-than-wood:white-than-wood/white-than-wood.github.io.git
                     fetch = +refs/heads/*:refs/remotes/origin/*
        
             [user]
                     name = white-than-wood
                     eamil = 1309777341@qq.com 

             #dreamthen账号下的项目
             [remote "origin"]
                     url = git@github.com-dreamthen:dreamthen/webpack-rebuild.github.io.git
                     fetch = +refs/heads/*:refs/remotes/origin/*
        
             [user]
                     name = dreamthen
                     eamil = dreamthen.00@gmail.com
           ```
     
     - 测试自定义域名别称.
           
           # 若 Hi white-than-wood! You've successfully authenticated, but GitHub does not provide shell access. 说明测试与远程建立ssh安全关联实行通信身份验证通过.
           ssh -T git@github.com-white-than-wood
           # 若 Hi dreamthen! You've successfully authenticated, but GitHub does not provide shell access. 说明测试与远程建立ssh安全关联实行通信身份验证通过.
           ssh -T git@github.com-dreamthen
     
     - 结论
     
       此方法永久性的解决了不同的github账号下本地项目ssh私钥与远程建立安全关联并实行通信的问题.

   

    