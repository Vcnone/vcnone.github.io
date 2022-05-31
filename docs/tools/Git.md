# Git

## Apt系

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

### 仓库目录

用于静态网页，假设在`/var/www`，需要更改目录权限

```bash
chown -R $USER:$USER /var/www
```

## Windows

### 安装

下载地址：[Git - Downloading Package](https://github.com/git-for-windows/git/releases) ，下载安装。

在命令行里输入`git --version`，查看git版本，测试是否安装成功

若安装失败，解决git安装问题。

[**廖雪峰**](https://weibo.com/liaoxuefeng)老师的[Git教程](https://www.liaoxuefeng.com/wiki/896043488029600)

### 配置

```bash
git config --global user.name "GitHub用户名"
git config --global user.email "GitHub注册邮箱"
```
