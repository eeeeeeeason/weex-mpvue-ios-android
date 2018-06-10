## centos下环境部署

- 需要环境node,mongodb,redis,nginx

- node

  - wget https://nodejs.org/dist/v6.10.3/node-v6.10.3-linux-x64.tar.xz //下载6.10.3随LTS自己修改，譬如现在是8.11.2

  - tar xvJf node-v6.10.3-linux-x64.tar.xz  //解压

  - mv node-v6.10.3-linux-x64 /usr/local/node-v6 //移动到/usr/local/node-v6目录

  - ln -s /usr/local/node-v6/bin/node /bin/node //配置 `node` 软链接到 [/bin](undefined) 目录

  - ln -s /usr/local/node-v6/bin/npm /bin/npm //配置npm软连接到/bin 目录

  - echo 'export PATH=/usr/local/node-v6/bin:$PATH' >> /etc/profile //配置环境变量

  - source /etc/profile //生效环境变量

  - npm install forever -g //安装进程管理模块

    - 问题1：

      ```
      ser "root" does not have permission to access the dev dir "/blog-server/node_modules/hiredis/.node-gyp/8.11.2"
      ```

      解决办法：sudo npm install --unsafe-perm 

    - pm2进程管理用于服务宕机重启

- mongodb参考链接(https://www.cnblogs.com/benjamin77/p/8465394.html)

  -  配置MongoDB的yum源
  ```
    vim /etc/yum.repos.d/mongodb-org-3.4.repo
    #添加以下内容：
    [mongodb-org-3.4]  
    name=MongoDB Repository  
    baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/  
    gpgcheck=1  
    enabled=1  
    gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc

    #这里可以修改 gpgcheck=0, 省去gpg验证
    [root@localhost ~]# yum makecache      
  ```

  - 安装mongodb,外网访问要关防火墙

  - ```
    yum -y install mongodb-org
    ```

  - 启动命令

    ```
    systemctl start mongod.service
    ```

  - 配置远程访问

    编辑mongod.conf注释**bindIp**,并重启mongodb.(这句配置代表只能本机使用，所以需注释

    ```
    vim /etc/mongod.conf
    ```

    ```
    systemctl restart mongod.service //记得重启
    ```

- redis

  - wget http://download.redis.io/releases/redis-4.0.2.tar.gz //下载

  - cd  redis-4.0.2

  - yum install gcc-c++

  - make , 需要先装gcc-c++进行编译

  - make PREFIX=/usr/local/redis install

  - 安装完毕还需要配置，复制编译目录中的redis.conf到安装文件目录

    ```
    cp redis.conf /usr/local/redis
    ```

    ```
    cd /usr/local/redis
    vim redis.conf
    daemonize no 改为yes 允许后端启动
    找到requirepass 配置密码
    ```

  - 启动

    ```
    ./bin/redis-server ./redis.conf 根据配置启动
    ```

    ​