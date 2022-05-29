# pip

## 卸载旧版本

如果有旧的python3.x版本，可以先卸载，但建议不要卸载，因为有很多依赖，替换版本号，执行以下命令:

```bash
apt-get autoremove python3.5 python3.5-dev
```

## 安装步骤

### 补全相关组件：

```bash
sudo apt-get install dirmngr sudo gcc
```

### 编辑apt的源文件

```bash
vim /etc/apt/sources.list
```

添加以下源

```bash
deb http://mirrors.163.com/ubuntu/ bionic main
```

为源添加KEY

```bash
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 3B4FE6ACC0B21F32
```

让服务器使用 “稳定版” 的库成为默认软件库：

```bash
echo 'APT::Default-Release "stable";' | sudo tee -a /etc/apt/apt.conf.d/00local
```

更新源

```bash
sudo apt-get update
```

### 安装python3.6

```bash
sudo apt-get -t bionic install python3.6 python3.6-dev python3-distutils python3-pip
ln -s /usr/bin/python3.6 /usr/bin/python3
```

### 安装pip3.6

```bash
wget https://bootstrap.pypa.io/get-pip.py
python3 get-pip.py
```

python2.x安装pip

```bash
python2.7 get-pip.py
```

检查setuptools是否安装成功：

```bash
easy_install --version
sudo apt-get clean
```

设置使用国内PIP源：

```bash
vim ~/.pip/pip.conf
[global]
index-url = http://mirrors.aliyun.com/pypi/simple/
trusted-host = mirrors.aliyun.com
```
