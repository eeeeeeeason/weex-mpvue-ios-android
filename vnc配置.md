# 图形化界面服务器

1.安装VNC远程桌面

- 首先，通过以下命令安装比较的系统库

```
yum install -y xorg-x11-xauth xterm libXi libXp libXtst libXtst-devel libXext libXext-devel
```

- 安装Xvfb

```
yum install -y Xvfb
```

- 最后，通过以下命令安装 x11vnc

```
yum install -y x11vnc
```

2.启动 VNC 远程桌面命令如下，其中 12345 为端口号（如果已占用需要换另外的端口号，记得在腾讯云管理页面添加端口放行规则），CrekeNet 为 VNC 远程桌面连接密码：

```
x11vnc -rfbport 12345 -passwd CrekeNet -create -forever
```

3.下载

```
https://www.realvnc.com/en/connect/download/viewer/
```





