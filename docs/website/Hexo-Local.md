# [Hexo](https://hexo.io/zh-cn/)

## 安装

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

## 推送网站

- blog根目录里的_config.yml文件，称为**站点配置文件**
- 根目录里的themes文件夹，里面也有个_config.yml文件，称为**主题配置文件**

将我们的Hexo与远程Git仓库关联起来，打开**站点配置文件**_config.yml，翻到最后修改，可通过多deploy，实现多端同步部署

```yml
deploy:
    github: git@github.com:******/******.github.io.git,Hexo
    gitee: git@gitee.com:******/******.git,master
    server: git@[域名/IP]:/home/git/hexoBlog.git,master
    #格式：[部署名]: 用户名@仓库位置,分支名
    #这里的@前的git是云服务器的git用户，也可以是root什么的，与云端对应
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

博客就被部署到deploy中列出的远程仓库

## 常用的Hexo 命令

`npm install hexo -g` #安装Hexo
`npm update hexo -g` #升级
`hexo init` #初始化博客

命令简写

```bash
hexo n "我的博客" == hexo new "我的博客" #新建文章
hexo g == hexo generate #生成
hexo s == hexo server #启动服务预览
hexo d == hexo deploy #部署
hexo server #Hexo会监视文件变动并自动更新，无须重启服务器
hexo server -s #静态模式
hexo server -p 5000 #更改端口
hexo server -i 192.168.1.1 #自定义 IP
hexo clean #清除缓存，若是网页正常情况下可以忽略这条命令
```

## 更换主题

主题传送门：

[官方主题](https://hexo.io/themes/) or [Next](https://theme-next.org/) or [Butterfly](https://github.com/jerryc127/hexo-theme-butterfly)

> [RunDouble's Blog](http://wu.run/2018/12/20/hexo-from-github-2-aliyun/)
>
> [HEXO部署到云服务器详细指南](https://www.jianshu.com/p/70bf58c48010 "HEXO 部署到云服务器详细指南")
>
> [Hexo教程：Hexo 博客部署到腾讯云教程](https://www.jianshu.com/p/271449df801f "Hexo教程：Hexo博客部署到腾讯云教程")
