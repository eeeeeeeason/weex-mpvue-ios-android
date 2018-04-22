#配置gitlab服务器
###配置 CentOS 7.3 x64

- 更新并打开相关权限

  ```
  yum update -y
  yum install -y curl policycoreutils-python openssh-server  //安装sshd
  systemctl enable sshd	//启用sshd
  systemctl start sshd	//启动
  ```

- 打开 [/etc/sysctl.conf](undefined) 文件，在文件最后添加新的一行并按 `Ctrl + S` 保存：

  ```
  net.ipv4.ip_forward = 1	//配置防火墙
  systemctl enable firewalld
  systemctl start firewalld	//启用并启动防火墙：
  firewall-cmd --permanent --add-service=http //放通 HTTP
  systemctl reload firewalld	//重启
  ```

- 安装postfix

  ```
  yum install -y postfix
  ```

- 配置

  ```
  打开 /etc/postfix/main.cf 文件，在第 119 行附近找到 inet_protocols = all，将 all 改为 ipv4 并按 Ctrl + S 保存：
  ```

- 启用

  ```
  systemctl enable postfix 
  systemctl start postfix
  ```

- 配置交换分区

  ```
  dd if=/dev/zero of=/root/swapfile bs=1M count=2048	//新建2G大小交换分区
  mkswap /root/swapfile	//格式化并启用
  swapon /root/swapfile
  ```

- 添加自启用。打开 [/etc/fstab](undefined) 文件，在文件最后添加新的一行并按 `Ctrl + S` 保存：

  ```
  /root/swapfile swap swap defaults 0 0
  ```

- 软键源修改为清华大学

  在 `/etc/yum.repos.d` 目录下新建 [gitlab-ce.repo](undefined) 文件并保存。内容如下：

  ```
  [gitlab-ce]
  name=Gitlab CE Repository
  baseurl=https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el$releasever/
  gpgcheck=0
  enabled=1
  ```

- 安装gitlab

  ```
  yum makecache	//重新生成缓存
  yum install -y gitlab-ce //安装
  ```

- 初始化gitlab

  打开 [/etc/gitlab/gitlab.rb](undefined) 文件，在第 13 行附近找到 `external_url 'http://gitlab.example.com'`，将单引号中的内容改为自己的域名（带上协议头，末尾无斜杠），并按 `Ctrl + S` 保存。

  ```
  external_url 'http://easonnnnn.club'
  ```

- 初始化 GitLab

  ```
  sudo gitlab-ctl reconfigure
  ```

  ​


