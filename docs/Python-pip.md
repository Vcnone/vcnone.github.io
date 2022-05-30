# pip

## python3.x安装pip

```bash
wget https://bootstrap.pypa.io/get-pip.py
python3 get-pip.py
```

## python2.x安装pip

```bash
python2.7 get-pip.py
```

## 检查setuptools是否安装成功：

```bash
easy_install --version
sudo apt-get clean
```

## 设置使用国内PIP源：

```bash
vim ~/.pip/pip.conf
[global]
index-url = http://mirrors.aliyun.com/pypi/simple/
trusted-host = mirrors.aliyun.com
```
