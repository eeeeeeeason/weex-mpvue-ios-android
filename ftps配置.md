# ftps配置

1.登录后

```
yum install vsftpd -y
```

2.启动vsftpd

```
service vsftpd start
netstat -nltp | grep 21
此时，访问 ftp://119.29.148.232 可浏览机器上的 /var/ftp 目录了。
```

3.配置FTP权限

```
匿名访问和切换根目录都会给服务器带来安全风险
，我们把这两个功能关闭。
编辑 /etc/vsftpd/vsftpd.conf
# 禁用匿名用户
anonymous_enable=NO
# 禁止切换根目录
chroot_local_user=YES
# 重启
service vsftpd restart
```

4.创建FTP用户

```
useradd ftpuser		//用户名
echo "ggW9ogOE" | passwd ftpuser --stdin	//密码
usermod -s /sbin/nologin ftpuser		//限制访问只能从ftp进行
```

5.分配主目录

```
mkdir -p /data/ftp/pub	//创建上传目录
echo "Welcome to use FTP service." > /data/ftp/welcome.txt	//登录欢迎文件
chmod a-w /data/ftp && chmod 777 -R /data/ftp/pub	//设置访问权限
usermod -d /data/ftp ftpuser	//设置为用户的主目录
```

主机：ftp://ftpuser:0QqOQcE7@139.199.170.159