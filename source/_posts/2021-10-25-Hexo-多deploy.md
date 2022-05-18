---
title: Hexo同时部署GitHub Page与云服务器
date: 2021-10-25 11:00:33
cover: https://pic1.zhimg.com/v2-55d7911a24217554d57c7827c51f06e1_720w.jpg
tags:
- [博客]
categories:
- [建站]
---

# 准备

核心是在原本Github Page推送的基础上，增加一条推送向云服务器的`deploy`，适用于增加更多部署。

## GitHub Page

本地已经配安装置好git、hexo，见{% post_link GithubPage ‘此处’ %}

## 服务器

已搭建好LNMP框架下的网页目录，见{% post_link LNMP搭建 此处 %}

# 搭建

## 本地git

```
ssh-keygen -t rsa
```

密钥默认位置：C盘用户文件夹下`.ssh`中，如已有则不需要生成。

## 服务器git

### 安装git

```bash
apt-get install git
```

### 创建git用户及ssh设置

```bash
sudo adduser git
su git
cd ~
mkdir .ssh && chmod 700 .ssh
```

```
touch .ssh/authorized_keys && chmod 600 .ssh/authorized_keys
```

在`authorized_keys`中写入本地生成的公钥

### git仓库

将`home`中的git用户文件夹作为仓库目录

```bash
git init --bare hexoBlog.git
```

在 `/home/git/hexoBlog.git` 下，有一个自动生成的 `hooks` 文件夹。我们需要在里边新建一个新的钩子文件 `post-receive`。**记得赋予执行权限。**

```bash
vim /home/git/hexoBlog.git/hooks/post-receive
```

在该文件中添加两行代码

```bash
#!/bin/bash
git --work-tree=/var/www --git-dir=/home/git/hexoBlog.git checkout -f
#--work-tree是网页目录，--git-dir是git仓库目录
```

## Nginx托管目录

假设在`/var/www`，需要更改目录权限

```bash
chown -R $USER:$USER /var/www
```

## Hexo

本地的 Hexo 博客所在文件，修改站点配置文件，增加一条推送向云服务器的`deploy`。

```bash
deploy:
    repo:
    #增加一行
        hexo: git@ip:/home/git/hexoBlog.git,master
        #这里的@前的git是云服务器的git用户，也可以是root什么的，与云端对应
```

> **参考**：[HEXO部署到云服务器详细指南](https://www.jianshu.com/p/70bf58c48010 "HEXO 部署到云服务器详细指南")，[Hexo教程：Hexo 博客部署到腾讯云教程](https://www.jianshu.com/p/271449df801f "Hexo教程：Hexo博客部署到腾讯云教程")
