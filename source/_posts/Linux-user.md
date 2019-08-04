---
title: 用户管理
date: 2018-01-30 10:07:00
tags: Linux
catalog: true
---

#### 查看用户

```
    who am i //输出用户名，终端序号，终端启动时间
    whoami //输出用户名
```

#### 创建用户

一般登录系统的时候都是以普通用户的身份登录的，要创建用户需要 root 权限。这里就要用到 sodu（switch user do）,使用 sudo 命名的两个前提条件

- 知道当前登录用户的密码
- 当前用户必须在 sudo 用户组

```
    su <user> //切换到用户user
    sudo <cmd> //可以以特权级别运行cmd命令，需要当前用户属于sudo组，且需要输入当前账户的密码
```

```
    sudo adduser yyy //新建一个叫yyy的新用户，同时为新用户创建home目录
```

#### 用户组 groups

```
    groups yyy // yyy:yyy
```

冒号之前表示用户，后面表示该用户所属的用户组。每次新建用户如果不指定用户组的话，会默认自动创建一个与用户名相同的用户组。

#### 将其他用户加入 sudo 用户组

默认情况下新创建的用户是不具有 root 权限的，也不在 sudo 用户组中，可以让其加入 sudo 用户组从而获取 root 权限

- 普通用户赋予 root 权限
  修改 /etc/sudoers 文件或 visudo 直接打开，找到下面一行，在 root 下面添加一行，如下所示：

  ```
    ## Allow root to run any commands anywhere
    root    ALL=(ALL)     ALL
    MF      ALL=(ALL)     ALL
  ```

  修改完毕，现在可以用 tommy 帐号登录，然后用命令 su - ，即可获得 root 权限进行操作。

- 使用 usermod 命令可以为用户添加用户组，使用该命令必须有 root 权限。可以使用 root 用户为其他用户添加用户组，或者在其他已经在 sudo 用户组的用户使用 sudo 命令获取权限来执行该命令

  ```
    su MF
    sudo usermod -G sudo yyy
    groups yyy
  ```

  usermod 命令
  用于修改用户的基本信息。usermod 命令不允许你改变正在线上的使用者帐号名称。当 usermod 命令用来改变 user id，必须确认这名 user 没在电脑上执行任何程序。你需手动更改使用者的 crontab 档。也需手动更改使用者的 at 工作档。采用 NIS server 须在 server 上更动相关的 NIS 设定。

  - c<备注>：修改用户帐号的备注文字；
  - d<登入目录>：修改用户登入时的目录；
  - e<有效期限>：修改帐号的有效期限；
  - f<缓冲天数>：修改在密码过期后多少天即关闭该帐号；
  - g<群组>：修改用户所属的群组；
  - G<群组>；修改用户所属的附加群组；
  - l<帐号名称>：修改用户帐号名称；
  - L：锁定用户密码，使密码无效；
  - s<shell>：修改用户登入后所使用的 shell；
  - u<uid>：修改用户 ID；
  - U:解除密码锁定。

#### 删除用户

```
    sudo deluser yyy --remove-home
```
