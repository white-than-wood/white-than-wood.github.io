---
title: think of homebrew
date: 2022-05-18 22:54:10
tags: [homebrew]
categories: homebrew
---

# download homebrew

> 最近在下载homebrew时,发现其总是出现连接被拒绝的情况

    curl: (7) Failed to connect to raw.githubusercontent.com port 443: Connection refused

   后续是发现其脚本需要到 raw.githubusercontent.com 上拉取代码,原因是 github 的一些域名的 DNS 解析被污染，导致 DNS 解析过程无法通过域名取得正确的IP地址.

> 使用修改本机hosts文件,建立域名与IP的映射关系,当访问hosts文件列表中的域名时,依次尝试在其映射的IP进行请求,绕过DNS

   使用<a href='https://www.ipaddress.com/'>https://www.ipaddress.com/</a>查找域名所对应的IP地址.

   ![](/images/ipaddress.png)

   使用switchHosts修改mac的hosts文件.

   ![](/images/switchhosts.png)

   PS: 使用switchHosts无法对原有的hosts文件进行修改,只能添加新的hosts文件对原有的hosts文件进行合并覆盖,添加好后,将其配置的switch开关打开,允许其合并覆盖.

> 再次下载,被拒绝的情况不存在了,但是出现了新的异常情况

    curl: (35) LibreSSL SSL_connect: SSL_ERROR_SYSCALL in connection to raw.githubusercontent.com:443

   究其根本原因,在于国内网络环境对于境外服务器的种种限制,只用解决这一问题才能真正意义上解决 GitHub push/pull 网络错误的问题.所以我查找到了一条可以彻底解决的路,使用国内镜像,就跟npm的淘宝镜像相同,homebrew在国内也有多条镜像途径.

    /bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"

   ![](/images/homebrew_mirror.png)

   选择任意一个镜像进行下载,最好是用'梯子'🐶.有时候用'梯子'也下载的非常慢,好在重新进行下载时,会在原来downloaded的基础之上进行下载.下载好之后,重启终端命令行工具,或者执行一下source .bash_profile,使得配置文件在修改了环境变等配置的情况下进行重置.

> brew install

   ![](/images/homebrew_install_git.png)

   这样就可以愉快快捷的下载任意在homebrew上的资源了!PS: 每下载完一次资源,还是最好执行一下source ~/.bash_profile,使得配置文件在修改了环境变量等配置的情况下进行重置.

> fatal: not in a git directory 
> Error: Command failed with exit 128: git

   当出现这种错误时,就说明本地与远程并没有建立关联,并没有添加origin句柄简称映射远程的镜像链接,所以我们需要重新设置一下.

   * Bash终端配置
    
    # 替换brew.git:
    cd "$(brew --repo)"
    git remote set-url origin https://mirrors.aliyun.com/homebrew/brew.git

    # 替换homebrew-core.git:
    cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
    git remote set-url origin https://mirrors.aliyun.com/homebrew/homebrew-core.git

    # 应用生效
    brew update

    # 替换homebrew-bottles:
    echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.aliyun.com/homebrew/homebrew-bottles' >> ~/.bash_profile
    source ~/.bash_profile

   * Zsh终端配置

    # 替换brew.git:
    cd "$(brew --repo)"
    git remote set-url origin https://mirrors.aliyun.com/homebrew/brew.git
    # 替换homebrew-core.git:
    cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
    git remote set-url origin https://mirrors.aliyun.com/homebrew/homebrew-core.git
    # 应用生效
    brew update
    # 替换homebrew-bottles:
    echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.aliyun.com/homebrew/homebrew-bottles' >> ~/.zshrc
    source ~/.zshrc

   
