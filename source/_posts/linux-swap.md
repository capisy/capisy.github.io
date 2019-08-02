---
title: Linux-swap&kxsw
date: 2018-01-30 10:07:00
tags: Linux
catalog: true
---

#### 查看磁盘空间

```linux
df -hl // 查看磁盘使用情况
```

```linux
du  -sh <文件夹名>  // 查看文件夹大小
du  -sh *  // 查看当前文件夹内所有文件大小
```

#### 查看内存使用情况

```linux
free -hl
```

#### 内存不够用，设置 swap

```linux
mkdir -p /opt/temp

dd if=/dev/zero of=/opt/temp/swap bs=1024 count=2048000

mkswap -f /opt/temp/swap

swapon  /opt/temp/swap

echo "/opt/temp/swap  swap  swap   defaults  0 0" >>/etc/fstab

reboot
```

#### kxsw

```linux
yum -y install wget
```

> 安装

```linux
cd /usr/local/src

wget --no-check-certificate -O shadowsocks.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh

chmod +x shadowsocks.sh

./shadowsocks.sh 2>&1 | tee shadowsocks.log
```

> 配置

```linux
{
    "server":"0.0.0.0",
    "server_port":9906,
    "local_address":"127.0.0.1",
    "local_port":1080,
    "password":"heheda~",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open":false
}
```

> 扩展

```linux
启动：/etc/init.d/shadowsocks start
停止：/etc/init.d/shadowsocks stop
重启：/etc/init.d/shadowsocks restart
状态：/etc/init.d/shadowsocks status
配置文件路径：/etc/shadowsocks.json
卸载方法：/usr/local/src/shadowsocks.sh uninstall
```

> bbr（可选，提升不大）

```linux
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh

chmod +x bbr.sh

./bbr.sh
```
