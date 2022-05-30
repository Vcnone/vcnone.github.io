# NVM-Windows

## nvm是什么

**nvm**全英文也叫**node.js version management**，是一个**nodejs**的版本管理工具。**nvm**和**n**都是**node.js**版本管理工具，为了解决**node.js**各种版本存在不兼容现象可以通过它可以安装和切换不同版本的**node.js**。

## nvm下载

在Github上下载[windows版本](https://github.com/coreybutler/nvm-windows/releases)。

## nvm安装

1. **卸载之前的node后安装nvm**, nvm-setup.exe安装版，直接运行nvm-setup.exe
2. 选择nvm安装路径(无空格与中文)
3. 选择nodejs路径
4. 确认安装即可
5. 安装完确认

打开CMD，输入命令nvm，安装成功则可以看到列出了各种命令，**nvm-windows 需要管理员权限**。

## nvm命令提示

```
- nvm arch：显示node是运行在32位还是64位。
- nvm install <version> [arch] ：安装node， version是特定版本也可以是最新稳定版本latest。可选参数arch指定安装32位还是64位版本，默认是系统位数。可以添加--insecure绕过远程服务器的SSL。
- **nvm list [available]** **：显示已安装的列表。可选参数available，显示可安装的所有版本。list可简化为ls。**
- nvm on ：开启node.js版本管理。
- nvm off ：关闭node.js版本管理。
- nvm proxy [url] ：设置下载代理。不加可选参数url，显示当前代理。将url设置为none则移除代理。
- nvm node_mirror [url] ：设置node镜像。默认是https://nodejs.org/dist/。如果不写url，则使用默认url。设置后可至安装目录settings.txt文件查看，也可直接在该文件操作。
- nvm npm_mirror [url] ：设置npm镜像。https://github.com/npm/cli/archive/。如果不写url，则使用默认url。设置后可至安装目录settings.txt文件查看，也可直接在该文件操作。
- nvm uninstall <version> ：卸载指定版本node。
- **nvm use [version] [arch]** **：使用制定版本node。可指定32/64位。**
- nvm root [path] ：设置存储不同版本node的目录。如果未设置，默认使用当前目录。
- nvm version ：显示nvm版本。version可简化为v。
```

## nvm常见问题

如果下载node过慢，请更换国内镜像源, 在nvm的安装路径下，找到 settings.txt，设置node_mirro与npm_mirror为国内镜像地址。

或在命令行中：

```bash
nvm node_mirror https://npmmirror.com/mirrors/node/
nvm npm_mirror https://npm.taobao.org/mirrors/npm/
```

> 来源：[http://nvm.uihtm.com/](http://nvm.uihtm.com/)
