# Syncthing

## Syncthing Discovery Server

下载 stdiscosrv 包

```bash
wget https://github.com/syncthing/discosrv/releases/download/*version*/stdiscosrv-linux-amd64-*version*.tar.gz
```

解压

```bash
tar -xzvf stdiscosrv-linux-amd64-*version*.tar.gz
```

## Syncthing Relay Server

下载strelaysrv

```bash
wget https://github.com/syncthing/relaysrv/releases/download/*version*/strelaysrv-linux-amd64-*version*.tar.gz
```

解压

```bash
tar -xzvf strelaysrv-linux-amd64-*version*.tar.gz
```

放入`/usr/bin/relaysrv`方便直接运行(顺便改个名)

```bash
mv strelaysrv-linux-amd64-*version*/strelaysrv /usr/bin/relaysrv
```

清理安装包

```bash
rm -rf strelaysrv-linux-amd64-*version* strelaysrv-linux-amd64-*version*.tar.gz
```

```bash
useradd relaysrv -s /bin/false
#创建存储配置用的目录并修改所有者
mkdir /etc/relaysrv
chown relaysrv /etc/relaysrv
```

## 配置Supervisor

在`/etc/supervisord.conf`文档最后加上下面这段设置

```bash
[program:relaysrv]
command=relaysrv -keys /etc/relaysrv -pools=""
autostart=true
autorestart=true
startsecs=10
stdout_logfile=/var/log/relaysrv.log
stdout_logfile_maxbytes=1MB
stdout_logfile_backups=10
stdout_capture_maxbytes=1MB
redirect_stderr=true
user = root

[program:discosrv]
command=discosrv /etc/discosrv
user = root
autostart=true
autorestart=true
redirect_stderr=true
startsecs=10
stdout_logfile=/var/log/discosrv.log
stdout_logfile_maxbytes=1MB
stdout_logfile_backups=2
stdout_capture_maxbytes=1MB

#program后面跟服务的名称
#command是程序的执行路径
#autorstart是表示应用程序随supervisor的启动而启动
#stdout_logfile是捕获标准输出。
```

也可以选择加上

```bash
[include]files = /etc/supervisor/conf.d/*.ini（此地址文件中放置上段设置）
```

relaysrv启动配置

```bash
-debug 启用调试输出-ext-address=<address> 可选的外部地址(将被上报)，能够通过端口转发来监听高权限端口(0-1024)然后外部可以连接这个端口-global-rate=<bytes/s> 全局限速，单位 bytes/s-keys=<dir> 用于存储 cert.pem 和 key.pem 的目录，默认是 "."(当前目录)-listen=<listen addr> 协议监听的地址，默认是 ":22067"-message-timeout=<duration> 等待消息到达的最大时间(默认 1m0s)-nat 使用UPnP/NAT-PMP来取得外部端口映射-nat-lease=<duration> NAT租赁时间，单位分钟(默认 60)-nat-renewal=<duration> NAT刷新频率，单位分钟(默认 30)-nat-timeout=<duration> NAT发现超时，单位秒(默认 10)-network-timeout=<duration> 客户端和中继之间网络操作的超时，如果在这个时间段内客户端和中继之间没有数据被接收到，那么连接将被终止。此外，如果在这段时间内任何被中继的客户端没有数据发送，这个会话也会被终止(默认 2m0s)-per-session-rate=<bytes/s> 每个会话的限速，单位 bytes/s-ping-interval=<duration> ping的发送间隔(默认 1m0s)-pools=<pool addresses> 中继服务器池的地址，使用逗号分隔多个(默认 "http://relays.syncthing.net/endpoint")。留空(-pools="")来禁止公布这个服务器到池中，以便作为私有中继。-protocol=<string> 监听协议，"tcp"来监听IPv4和IPv6，"tcp4"来监听IPv4，"tcp6"来监听IPv6(默认 "tcp")-provided-by=<string> 一个可选的描述字段来表示谁提供了这个中继(可以打打广告啥的)-status-srv=<listen addr> 提供状态服务的监听地址(默认 ":22070")，用于中继服务器池页面来展示服务器状态(传输了多少数据，有多少客户端在线等等)，留空(-status-srv="")来禁用这个功能-pools=""
```

带配置文件启动

```bash
supervisord -c /etc/supervisor/supervisor.conf
```
