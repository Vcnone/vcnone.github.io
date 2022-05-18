---
title: Ubuntu 20.04 lts 安装 LNMP
date: 2021-09-12 20:37:33
cover: https://img.wordpressjc.com/wp-content/uploads/2019/05/1558091755-lnmp.jpeg
tags: 
- [服务部署]
categories:
- [建站]
---

**参考**：[Debian9(Stretch) 下编译安装LNMP环境](https://blog.csdn.net/weixin_34232363/article/details/88728946 "Debian9(Stretch) 下编译安装LNMP环境")

------------

# 一、环境

- **Ubuntu 20.04 lts**

- [**Nginx-1.20.2**](https://nginx.org/download/nginx-1.20.2.tar.gz)
  
  - [**zlib-1.2.12**](http://zlib.net/zlib-1.2.12.tar.gz)
  - [**pcre-8.45**](https://versaweb.dl.sourceforge.net/project/pcre/pcre/8.45/pcre-8.45.zip)
  - [**openssl-1.1.1o**](https://www.openssl.org/source/openssl-1.1.1o.tar.gz)

- [**PHP-7.4.29**](https://www.php.net/distributions/php-7.4.29.tar.gz)

- [**MariaDB-10.3**](https://dlm.mariadb.com/2145683/MariaDB/mariadb-10.7.3/repo/ubuntu/mariadb-10.7.3-ubuntu-focal-amd64-debs.tar) 

# 二、 安装过程

按照MariaDB > Nginx > PHP的顺序安装。

## 2.1 MariaDB

### 2.1.1 安装

```bash
sudo apt update
sudo apt install mariadb-server
```

安装完成后 ，MariaDB 服务将会自动启动。输入以下命令验证数据库服务器是否正在运行。
输出结果将会显示服务已经启用，并且正在运行。

```bash
sudo systemctl status mariadb
```

### 2.1.2 维护

```bash
sudo mysql_secure_installation
```

根据脚本提示输入 root 密码：
由于没有设置 root 密码，所以这里仅仅输入回车"Enter"即可。
接下来，会提示是否为 MySQL root 用户设置密码：

```bash
Enter current password for root (enter for none):
Set root password? [Y/n] n
Remove anonymous users? [Y/n] Y 
Disallow root login remotely? [Y/n] Y 
Remove test database and access to it? [Y/n] Y 
Reload privilege tables now? [Y/n] Y
```

## 2.2 Nginx 源码安装

三个源码包： [**zlib-1.2.12**](http://zlib.net/zlib-1.2.12.tar.gz)(压缩库) [**pcre-8.45**](https://versaweb.dl.sourceforge.net/project/pcre/pcre/8.45/pcre-8.45.zip)(正则表达式库) [**openssl-1.1.1o**](https://www.openssl.org/source/openssl-1.1.1o.tar.gz)(加密库，如果要使用HTTPS，这个库是必须的)

### 2.2.1 创建用户

```bash
groupadd -r nginx
useradd -r -g nginx -s /bin/false -M nginx
```

### 2.2.2 编译安装

安装编译器

```bash
apt install build-essential 
```

编译安装

```
./configure --prefix=/etc/nginx --group=nginx --user=nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --with-compat --with-file-aio --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-pcre-jit --with-openssl-opt=no-nextprotoneg --with-debug --with-pcre=../pcre-8.45 --with-zlib=../zlib-1.2.12 --with-openssl=../openssl-1.1.1o
## --with-pcre=../pcre-8.45 --with-zlib=../zlib-1.2.12 --with-openssl=../openssl-1.1.1o填源码包路径
## 注意，如果是使用二进制包安装了zlib,pcre,openssl,及相应的开发库，不需要指定路径。
mkdir /var/cache/nginx
```

然后，`make` 和 `make install`

### 2.2.2 启动项

`vim /etc/sytemd/system/nginx.service`

```
[Unit]
Description=nginx - high performance web server
Documentation=http://nginx.org/en/docs/
After=network-online.target remote-fs.target nss-lookup.target
Wants=network-online.target

[Service]
Type=forking
PIDFile=/run/nginx.pid
ExecStartPre=/usr/sbin/nginx -t -c /etc/nginx/nginx.conf
ExecStart=/usr/sbin/nginx -c /etc/nginx/nginx.conf
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/bin/kill -s TERM $MAINPID

[Install]
WantedBy=multi-user.target
```

这个可以在nginx 官网找到，可以按照自己需求修改。注意路径修改成自己的安装路径。 `systemctl daemon-reload` `systemctl start nginx.service` 启动Nginx `systemctl enable nginx.service` 开机启动。

记得，如果中途修改了service文件，一定要先运行 systemctl daemon-reload重新加载守护进程文件。然后运行 systemctl start nginx.service重启服务。

## 2.3 PHP安装

### 2.3.1 安装

```
apt-get install php7.4 php7.4-fpm
```

### 2.3.2 配置Nginx支持

需要PHP的网页启用

1. 在`server{}`内，找到`index`开头的配置行，在该行中添加`index.php`。

2. 在`server{}`内，找到`location ~ \.php$ {}`，启用以下行

```
location ~ \.php$ {
   include snippets/fastcgi-php.conf;
   fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
}
```

待续~~~
