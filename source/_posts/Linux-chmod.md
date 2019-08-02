---
title: Linux-权限管理
date: 2018-01-31 10:07:00
tags: Linux
---

#### 查看文件权限

```
    ls -l
```

![](/images/blog/linux/chmod-1.png)
![](/images/blog/linux/chmod-2.png)

#### 文件权限

- 读权限，表示你可以使用 cat <file name> 之类的命令来读取某个文件的内容；
- 写权限，表示你可以编辑和修改某个文件；
- 执行权限，通常指可以运行的二进制程序文件或者脚本文件，如同 Windows 上的 exe 后缀的文件，不过 Linux 上不是通过文件后缀名来区分文件的类型。

**一个目录同时具有读权限和执行权限才可以打开并查看内部文件，而一个目录要有写权限才允许在其中创建其它文件，这是因为目录文件实际保存着该目录里面的文件的列表等信息。**

#### 修改文件权限

```
    chmod 777 <file> //rwx 最高权限
    chmod go-rw <file> //g: group,o:others,u:user>; +:增加权限，-： 去掉权限
```

### 防火墙操作
#### 查看防火墙状态
```linux
firewall-cmd --state
systemctl status firewalld
```
#### 关闭、开启防火墙
```linux
systemctl start firewalld
systemctl stop firewalld
```
#### 禁止firewall开机启动
```linux
systemctl disable firewalld
```
####  查看已经开放的端口
```linux
firewall-cmd --list-ports
```
#### 开启端口
```linux
firewall-cmd --zone=public --add-port=80/tcp --permanent 
命令含义:
–zone #作用域
–add-port=80/tcp #添加端口，格式为：端口/通讯协议
–permanent #永久生效，没有此参数重启后失效
```
#### 关闭端口
```linux
firewall-cmd --add-port=80/tcp --permanent 
```

#### 重载生效刚才的端口设置
```linux
firewall-cmd --reload
```