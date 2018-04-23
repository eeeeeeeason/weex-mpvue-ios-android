# nginx

- 安装

  ```
  yum install nginx -y
  ```

- 执行

  ```
  nginx
  ```

- 发现端口80被占用

  ```
  netstat -lnp|grep 80
  kill -9 3135
  ```

  ​

  ​