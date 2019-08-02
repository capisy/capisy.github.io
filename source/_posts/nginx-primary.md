---
title: nginx小记
date: 2019-03-30 15:03:41
tags: nginx
catalog: true
---

#### 安装
查看nginx是否安装
```linux
yum info nginx
```
官网下载
```linux
vi /etc/yum.repos.d/nginx.repo

[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key
```
查看安装源是否生效,安装
```linux
yum list|grep nginx
yum install nginx -y
```
安装成功，启动nginx
```linux
service nginx status  // 查看是否开启
servicee nginx start // 打开服务
```
查看nginx安装path
```linux
which nginx
```
#### 配置nginx项目目录
把root改成你想配置的目录
给自定义的目录设置755权限
更新配置
```linux
kill -HUP `cat /var/run/nginx.pid`
```
如果访问页面是403错误
查看SELinux设置状态
```linux
/usr/sbin/sestatus 
```
如果是enfing，修改为disabled
```linux
vi /etc/selinux/config
SELINUX = disabled
reboot
```

#### 配置本地域名
更改serve_name 
```linux
server_name mf.com;
```
更改window的host文件
```linux
10.0.1.188 mf.com // 10.0.1.188 为虚拟机的ip
```

#### 反向代理，解决前后端分离项目跨域问题
```nginx
location /api {
     rewrite  ^/api/(.*)$ /$1 break;
     proxy_pass http://api.kylin.eosbeijing.one:8880;
}
```

#### nginx配置,支持前端browser路由
```nginx
location / {
	root   /var/www/eos_game_build;
	try_files $uri $uri/ @router;
	index  index.html index.htm;
}
    
location @router{
   rewrite ^.*$ /index.html last;
}
```