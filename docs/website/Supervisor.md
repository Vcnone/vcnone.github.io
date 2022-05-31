# Supervisor

## 安装supervisor

### pip方式

```bash
pip install supervisor
```

### apt方式

```bash
apt-get install supervisor
```

## 配置supervisor

```bash
echo_supervisord_conf > /etc/supervisor/supervisord.conf(配置文件位置)
echo "supervisord" >> /etc/rc.local
```

在 `/etc/supervisor/supervisord.conf` 文档最后加上下面这段设置，服务配置放置在单独的文件中。

```bash
[include]files = /etc/supervisor/conf.d/*.ini
```

## 开机启动

### 配置systemctl

进入 `/lib/systemd/system` 目录，并创建 `supervisor.service` 文件，该文件内容如下所示。

```bash
root@VC:/lib/systemd/system#whereis supervisord
supervisord: *查到的地址*/supervisord
root@VC:/lib/systemd/system# whereis supervisorctl
supervisorctl: *查到的地址*/supervisorctl
# 创建文件
root@VC:/lib/systemd/system# vim supervisor.service

[Unit]
Description=supervisor
After=network.target

[Service]
Type=forking
ExecStart=*查到的地址*/supervisord -c *配置文件位置*/supervisord.conf
ExecStop=*查到的地址*/supervisorctl $OPTIONS shutdown
ExecReload=*查到的地址*/supervisorctl $OPTIONS reload
KillMode=process
Restart=on-failure
RestartSec=42s

[Install]
WantedBy=multi-user.target
```

### 刷新service

```bash
root@VC:/lib/systemd/system# systemctl enable supervisor.service
Created symlink from /etc/systemd/system/multi-user.target.wants/supervisor.service to /lib/systemd/system/supervisor.service.
root@VC:/lib/systemd/system# systemctl daemon-reload
```

### 修改文件权限为766

```bash
root@VC:/lib/systemd/system# chmod 766 supervisor.service
```

### supervisorctl常用命令：

```bash
supervisorctl status  #查看supervisord监控的所有进程的状态
supervisorctl celery status  #查看celery进程的状态
supervisorctl stop xxx  #停止某一个进程(xxx)，xxx为[program:theprogramname]里配置的值
supervisorctl start xxx  #启动某个进程
supervisorctl restart xxx  #重启某个进程
supervisorctl stop groupworker  #重启所有属于名为groupworker这个分组的进程
supervisorctl stop all  #停止全部进程，注：start、(start,restart同理)restart、stop都不会载入最新的配置文件
supervisorctl reload  #重启supervisor
supervisorctl update  #根据最新的配置文件，启动新配置或有改动的进程，配置没有改动的进程不会受影响而重启。当配置文件修改后，要执行这条命令。
#用stop停止掉的进程，即使配置文件设置了autorestart=true，用reload或者update都不会自动重启。
supervisorctl shutdown  # 关闭supervisord,会同时关闭supervisord监控的所有进程
```

## 配置


### Syncthing Discovery Server

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
```

### Syncthing Relay Server

```bash
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
```
