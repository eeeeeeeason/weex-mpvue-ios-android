#配置git服务器
###配置 CentOS 7.3 x64
-  安装相关依赖和编译工具
    ```
    yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel
    ```

-  安装编译工具

   ```
   yum install gcc perl-ExtUtils-MakeMaker
   ```

-  下载git并存放

   ```
   cd /usr/local/src
   wget https://www.kernel.org/pub/software/scm/git/git-2.10.0.tar.gz
   ```

-  解压编译安装

   ```
   tar -zvxf git-2.10.0.tar.gz
   cd git-2.10.0
   make all prefix=/usr/local/git
   make install prefix=/usr/local/git
   ```

-  配置环境变量

   ```
   echo 'export PATH=$PATH:/usr/local/git/bin' >> /etc/bash
   source /etc/bashrc
   git --version
   ```

-  创建git账号，密码

   ```
   useradd -m gituser
   passwd gituser
   ```

-  创建git仓库并初始化

   ```
   mkdir -p /data/repositories
   cd /data/repositories/ && git init --bare test.git
   ```

-  配置权限

   ```
   chown -R gituser:gituser /data/repositories
   chmod 755 /data/repositories
   ```

-  编辑用户表 /etc/passwd将最后一行关于 `gituser` 的登录 shell 配置改为 git-shell 的目录

   ```
   gituser:x:500:500::/home/gituser:/usr/local/git/bin/git-shell
   ```

-  自己用 git clone xxxxxxxxxxxxxxxxxxxxxx

