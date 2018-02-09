---
title: Linux- 用户管理
date: 2018-01-30 10:07:00
tags: Linux
---
<style>
    ::selection{ color:#fff; background-color: #e26848; }
    .tx-explain { color:#666;margin-left:10px;  }
</style>

![](/images/blog/linux/linux.jpg)  

<!-- more -->

[资料来自实验楼linux基础教程](https://www.shiyanlou.com/courses/1)

## 查看用户
```
    who am i //输出用户名，终端序号，终端启动时间
    whoami //输出用户名
```

## 创建用户
一般登录系统的时候都是以普通用户的身份登录的，要创建用户需要root权限。这里就要用到sodu（switch user do）,使用sudo命名的两个前提条件
- 知道当前登录用户的密码
- 当前用户必须在sudo用户组
```
    su <user> //切换到用户user
    sudo <cmd> //可以以特权级别运行cmd命令，需要当前用户属于sudo组，且需要输入当前账户的密码
```
```
    sudo adduser yyy //新建一个叫yyy的新用户，同时为新用户创建home目录
```

## 用户组 groups
```
    groups yyy // yyy:yyy
```
冒号之前表示用户，后面表示该用户所属的用户组。每次新建用户如果不指定用户组的话，会默认自动创建一个与用户名相同的用户组。

## 将其他用户加入sudo 用户组
默认情况下新创建的用户是不具有root权限的，也不在sudo用户组中，可以让其加入sudo用户组从而获取root权限

- 普通用户赋予root权限
    修改 /etc/sudoers 文件，找到下面一行，在root下面添加一行，如下所示：
    ```
    ## Allow root to run any commands anywhere
    root    ALL=(ALL)     ALL
    MF      ALL=(ALL)     ALL
    ```
    修改完毕，现在可以用tommy帐号登录，然后用命令 su - ，即可获得root权限进行操作。

- 使用usermod 命令可以为用户添加用户组，使用该命令必须有root权限。可以使用root用户为其他用户添加用户组，或者在其他已经在sudo用户组的用户使用sudo命令获取权限来执行该命令
```
    su MF
    sudo usermod -G sudo yyy
    groups yyy
```

usermod 命令
用于修改用户的基本信息。usermod命令不允许你改变正在线上的使用者帐号名称。当usermod命令用来改变user id，必须确认这名user没在电脑上执行任何程序。你需手动更改使用者的crontab档。也需手动更改使用者的at工作档。采用NIS server须在server上更动相关的NIS设定。 

    -c<备注>：修改用户帐号的备注文字；
    -d<登入目录>：修改用户登入时的目录；
    -e<有效期限>：修改帐号的有效期限；
    -f<缓冲天数>：修改在密码过期后多少天即关闭该帐号；
    -g<群组>：修改用户所属的群组；
    -G<群组>；修改用户所属的附加群组；
    -l<帐号名称>：修改用户帐号名称；
    -L：锁定用户密码，使密码无效；
    -s<shell>：修改用户登入后所使用的shell；
    -u<uid>：修改用户ID；
    -U:解除密码锁定。
 
## 删除用户
```
    sudo deluser yyy --remove-home
```