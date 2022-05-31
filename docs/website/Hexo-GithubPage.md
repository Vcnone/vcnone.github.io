# Github Page

## 创建Github仓库

登录到[GitHub](https://github.com/)，创建新仓库，仓库名应该为：**用户名**.github.io。**用户名**为GitHub帐号名，这是固定写法。

## 配置ssh密钥

### 生成ssh密钥

如已有ssh密钥文件，跳过

```bash
ssh-keygen -t rsa -C "GitHub注册邮箱"
```

### 上传ssh密钥

打开[GitHub_Settings_keys](https://github.com/settings/keys) 页面，新建new SSH Key；

或打开仓库的Deploy keys页面，新建new deploy key；

其中Title为标题，Key填id_rsa.pub中内容

在Git Bash中检测GitHub公钥设置是否成功，输入 `ssh git@github.com` ：

```bash
PTY allocation request failed on channel 0
Hi *******! You've successfully authenticated, but GitHub does not provide shell access.
Connection to github.com closed.
```

如上则说明成功。

## 自定义域名

在DNS服务商添加一条CNAME解析

1. 主机记录：自定义域名
2. 记录值：**用户名**.github.io
3. 其余默认即可

本地博客文件夹 ，在 `blog/source` 目录下，创建一个 `CNAME` 文件，输入自定义域名。
注意：** `CNAME` 文件不是 `txt` 文件**。如果域名带有 `www` ，那么访问的时候必须带有 `www` 完整的域名才可以访问，但如果不带有 `www` ，以后访问的时候带不带 `www` 都可以访问。
