---
title: think of git
date: 2022-05-15 05:40:05
tags: [git,github]
categories: git
---

# git命令

> git remote add origin git@github.com:white-than-wood/white-than-wood.github.io.git

   从官方对于 git remote 的定义来看,意思是列出本地拥有的每个指定远程句柄的简称.简单来说本地与远程交流的一个简化链接,当在本地建立起一个已经初始化的 git 项目时,势必要与远程 git 库建立联系(远程 git 库本来也是为了保存代码、保证多人开发时代码的同步以及简化其流程性而存在的),那么此命令就是为了在本地建立一个句柄简称与远程链接 git@github.com:white-than-wood/white-than-wood.github.io.git 进行关联,实行通信,不需要再去 copy 使用 github 远程库上面的链接.

> git push --set-upstream origin master

   在本地句柄简称 origin 建立了与远程代码库的关联之后,首次推送同步代码,需要设置推送、同步代码的上游远程分支,当首次设置之后,后续无需设置. --set-upstream origin master 就是设置推送、同步代码的本地 origin 句柄简称的上游分支为 master.

> git merge master --allow-unrelated-histories

   当同一个仓库存在多个独立的分支并没有公共的上游交集父分支时,会出现无法合并的情况(多出现于本地 git 初始化时默认主分支为 master,而远程 github 默认主分支为 main ).

    fatal: refusing to merge unrelated histories

   故此我们需要在人为确认合并分支安全的情况下,将多个独立的分支进行允许强制合并,也就是 --allow-unrelated-histories.

> git remote set-url origin https://mirrors.aliyun.com/homebrew/brew.git

   修改本地句柄简称列表中 origin 所对应的远程仓库链接为 https://mirrors.aliyun.com/homebrew/brew.git.

# SSH公私钥安全关联

  本地设置与 github 远程仓库的 ssh 安全关联.

  > 步骤

   只有持有账号私钥的情况下才可以推送、同步代码到拥有公钥的 github 远程仓库,使用 ssh 命令产生公私钥.我这里用的是 rsa 的加解密方式,小伙伴们也可以选择自己喜欢的加密方式.
   
    #使用自己的 github 账号来作为 rsa 加解密的注释
    #-t: type,选用 rsa 的密钥类型
    #-b: byte,公私钥的长度大小4096比特
    #-C: comments,添加公私钥生成的注释
    ssh-keygen -t rsa -b 4096 -C '15866369958@qq.com'

   在 mac 下,生成之后,前往自己账号目录下查询 .ssh/id_rsa.pub,将 id_rsa.pub 文件里面的内容复制添加到 github 的账号 settings 设置下的 SSH and GPG keys.
   
   <img src="https://image.white-than-wood.zone/git/ssh_settings.png" style="width: 160px;float: left;"/>
   <img src="https://image.white-than-wood.zone/git/ssh_settings_SSHKeys.png" style="width: calc(100% - 180px);float: left;margin-left: 20px;"/>
   <div style="clear: both;display: block;"></div>

   添加成功之后,我们测试一下,将远程 github 库('git@github.com'开头链接)克隆到本地,如果可以拉取到本地,那就说明 ssh-keygen 设置与 github 远程仓库 ssh 安全关联生效.
   
  本地设置多个 ssh 私钥与多个 github 账号远程仓库建立安全关联.

  > 示例

   - ssh-add 交与 ssh-agent 代理高速缓存管理.

       - ssh-agent.

         ssh-agent 实际上是一个本地的高速缓存代理机制,可以在用户不输入任何密码短语的情况下,将 session 缓存中的私钥与远程仓库建立 ssh 安全关联并实行通信.
     
       - ssh-add.
     
         ssh-add 是将指定的 ssh 私钥置于 ssh-agent 的高速缓存中,执行时会在 ssh-agent 建立一个 session,并将 ssh 私钥放入其中.注意,此行为是一次性行为,也就是临时性行为,重启 ssh-agent 之后,会重置.
             
             #可用于查看 ssh-agent 的高速缓存中的私钥列表.
             ssh-add list
             #清空 ssh-agent 的高速缓存中的私钥列表.
             ssh-add -D
             #重启 ssh-agent.
             eval $(ssh-agent)
       
       - 步骤.

         使用 ssh-keygen 与 ssh-add 联合使用.
    
             ssh-keygen -t rsa -b 4096 -C 'dreamthen.99@gmail.com'
             #这时会出现需要你设置进行保存公私钥的文件名,默认还是为id_rsa
             Generating public/private rsa key pair.
             Enter file in which to save the key (/Users/yinwk/.ssh/id_rsa): id_rsa_ano
             #设置过后,公私钥就会以id_rsa_ano.pub以及id_rsa_ano文件进行保存

         一系列操作结束后,我们还需要将新生成的私钥交与 ssh-agent 代理高速缓存管理.

             ssh-add ~/.ssh/id_rsa_ano

         在成功交与 ssh-agent 管理后,我们重复上一个部分中'本地设置与 github 远程仓库的 ssh 安全关联'的后续操作即可.
     
       - 问题.
     
         使用此方法是有比较多的问题的,首先就是重启 ssh-agent 或者电脑之后,ssh-agent 中的高速缓存会重置,也就是会被清空掉.再就是对于同一个 ssh 域名下的链接建立 ssh 安全关联时,ssh-agent 会选择高速缓存列表中的一个缓存私钥来建立通信.举个🌰:
       
             #这两个 github 库有着同样的域名别称,也就是 @github.com,在建立通信时,如果将两个账号下生成的私钥都放入 ssh-agent 高速缓存中,ssh-agent 会默认选择高速缓存列表中先放入的 ssh 私钥进行通信.
             #只有在切换不同的账号项目开发时,将 ssh-agent 重启,并且将当前项目所对应的 ssh 私钥放入 ssh-agent 高速缓存中才可以建立 ssh 安全关联并且实行通信.
             git@github.com:white-than-wood/white-than-wood.github.io.git
             git@github.com:dreamthen/webpack-rebuild.github.io.git

       - 结论.
         
         ssh-add 交与 ssh-agent 代理高速缓存管理这种方法,本质也不是为了让多个 ssh 公私钥与多个 github 远程仓库建立安全关联而产生的,作用是为了避免每次与远程建立 ssh 安全关联时必须填写密码短语.并且我们在生成 ssh 密钥时,一般也是不设置密码短语的,都是直接回车(密码短语为空).这种每次必须手动向高速缓存中添加私钥的行为,不仅麻烦,而且是临时性的、不合理的.
   
   - 配置 .ssh/config 文件使不同本地项目 ssh 私钥永久性与远程不同 github 账号仓库建立安全关联.

     - .ssh/config.
     
       通过配置 config 文件来辅助管理 ssh,通常 .ssh/config 文件是不存在的,需要自己创建配置.
     
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
         #HostName: 对应的代码存储/协同网站域名,如 github.com,不同公司的 gitlab 域名不同
         #User: 对应的不同代码存储/协同网站域名账号名称
         #IdentityFile: 对应的不同代码存储/协同网站域名账号下的 ssh 私钥
         #IdentitiesOnly: 只能通过指定的 IdentityFile 来与远程建立 ssh 安全关联并实行通信
         ```
       
       - 配置本地不同账号的项目域名、账号名称以及 email 地址.
     
         进入不同github账号的项目下,修改 .git/config 文件,注意自定义的域名别称要与 .ssh/config 中的 Host 对应,因为每次建立 ssh 安全关联实行通信都会通过 .ssh/config 文件进行辅助管理.
           
             vim .git/config
       
           ```text
             #white-than-wood 账号下的项目
             [remote "origin"]
                     url = git@github.com-white-than-wood:white-than-wood/white-than-wood.github.io.git
                     fetch = +refs/heads/*:refs/remotes/origin/*
        
             [user]
                     name = white-than-wood
                     email = 1309777341@qq.com 

             #dreamthen 账号下的项目
             [remote "origin"]
                     url = git@github.com-dreamthen:dreamthen/webpack-rebuild.github.io.git
                     fetch = +refs/heads/*:refs/remotes/origin/*
        
             [user]
                     name = dreamthen
                     email = dreamthen.00@gmail.com
           ```
         
         也可以直接使用命令来进行修改 .git/config 中的内容.
             
             #white-than-wood 账号下的项目
             git remote set-url origin git@github.com-white-than-wood:white-than-wood/white-than-wood.github.io.git
             git config user.name 'white-than-wood'
             git config user.email '1309777341@qq.com'
       
             #dreamthen 账号下的项目
             git remote set-url origin git@github.com-dreamthen:dreamthen/webpack-rebuild.github.io.git
             git config user.name 'dreamthen'
             git config user.email 'dreamthen.00@gmail.com'
     
     - 测试自定义域名别称.
           
           # 若 Hi white-than-wood! You've successfully authenticated, but GitHub does not provide shell access. 说明测试与远程建立 ssh 安全关联实行通信身份验证通过.
           ssh -T git@github.com-white-than-wood
           # 若 Hi dreamthen! You've successfully authenticated, but GitHub does not provide shell access. 说明测试与远程建立 ssh 安全关联实行通信身份验证通过.
           ssh -T git@github.com-dreamthen
     
     - 结论
     
       此方法永久性的解决了不同的 github 账号下本地项目 ssh 私钥与远程建立安全关联并实行通信的问题.

   

    