---
title: GithubPage——Github+Hexo
date: 2021-06-14 10:45:20
cover: https://pic1.zhimg.com/v2-fccdcf1f6e8a31fc59da10690b6237fc_1440w.jpg
tags:
- [博客]
categories:
- [建站]
---

------------

# 配置Github

登录到[GitHub](https://github.com/)，创建新仓库，仓库名应该为：**用户名**.github.io

**用户名**为GitHub帐号名，这是固定写法。

# 安装Git

下载地址：[Git - Downloading Package](https://github.com/git-for-windows/git/releases) ，下载安装。

在命令行里输入`git --version`，查看git版本，测试是否安装成功

若安装失败，解决git安装问题。

[**廖雪峰**](https://weibo.com/liaoxuefeng)老师的[Git教程](https://www.liaoxuefeng.com/wiki/896043488029600) 

# 配置Git

## 全局配置

```bash
git config --global user.name "GitHub用户名"
git config --global user.email "GitHub注册邮箱"
```

### 生成ssh密钥文件

```bash
ssh-keygen -t rsa -C "GitHub注册邮箱"
```

### 配置ssh密钥

打开[GitHub_Settings_keys](https://github.com/settings/keys) 页面，新建new SSH Key；或打开仓库的Deploy keys页面，新建new deploy key；

其中Title为标题，Key填生成的id_rsa.pub中内容

在Git Bash中检测GitHub公钥设置是否成功，输入 `ssh git@github.com` ：

```txt
PTY allocation request failed on channel 0
Hi *******! You've successfully authenticated, but GitHub does not provide shell access.
Connection to github.com closed.
```

如上则说明成功。

# 安装Node.js

下载地址：[Download | Node.js](https://nodejs.org/en/download/) ，下载安装。注意安装Node.js会包含**环境变量**及**npm**的安装。

安装后，在命令行中输入 `node -v`，检测Node.js是否安装成功 ；输入`npm -v`，检测npm是否安装成功

# 安装Hexo

以[Hexo](https://hexo.io/zh-cn/)作为个人博客网站的框架。

使用npm命令安装Hexo：

```bash
npm install hexo-cli -g
```

安装完成后，在上一级目录`D:\>`初始化博客，可以命名为blog，Hexo框架与以后发布的网页都在这个文件夹中。

```bash
hexo init blog
```

生成并预览博客

```bash
hexo g
hexo s
```

常用的Hexo 命令

`npm install hexo -g` #安装Hexo
`npm update hexo -g` #升级
`hexo init` #初始化博客

命令简写
`hexo n "我的博客"` == `hexo new "我的博客"` #新建文章
`hexo g` == `hexo generate` #生成
`hexo s` == `hexo server` #启动服务预览
`hexo d` == `hexo deploy` #部署

`hexo server` #Hexo会监视文件变动并自动更新，无须重启服务器
`hexo server -s` #静态模式
`hexo server -p 5000` #更改端口
`hexo server -i 192.168.1.1` #自定义 IP
`hexo clean` #清除缓存，若是网页正常情况下可以忽略这条命令

# 推送网站

- blog根目录里的_config.yml文件，称为**站点配置文件**
- 根目录里的themes文件夹，里面也有个_config.yml文件，称为**主题配置文件**

将我们的Hexo与GitHub关联起来，打开**站点配置文件**_config.yml，翻到最后修改

```yml
deploy:
    type: git
    repo: 在GitHub上创建仓库的完整路径
    branch: master
```

安装Git部署插件，输入命令：

```bash
npm install hexo-deployer-git --save
```

此时生成并部署博客

```bash
hexo clean
hexo g -d
```

博客就被部署到：**用户名**.github.io

# 绑定域名

基于阿里云，在**管理控制台**进入**域名解析**管理页面

添加一条CNAME解析

1. 主机记录：@
2. 记录值：**用户名**.github.io
3. 其余默认即可

本地博客文件夹 ，在blog/source目录下，创建一个CNAME文件，输入自定义域名。

注意：不是txt文件。如果域名带有www，那么访问的时候必须带有www完整的域名才可以访问，但如果不带有www，以后访问的时候带不带www都可以访问。

# 更换主题

更换不同的主题，主题传送门：[Themes](https://hexo.io/themes/)。

[Next](https://theme-next.org/)   [Butterfly](https://github.com/jerryc127/hexo-theme-butterfly)

> **参考**：[RunDouble's Blog](http://wu.run/2018/12/20/hexo-from-github-2-aliyun/)
