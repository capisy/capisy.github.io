---
title: Linux- 安装软件
date: 2018-01-28 10:07:00
tags: Linux
---
<style>
    ::selection{ color:#fff; background-color: #e26848; }
    .tx-explain { color:#666;margin-left:10px;  }
</style>

![](/images/blog/linux/linux.jpg)

<!-- more -->

[资料来自实验楼linux基础教程](https://www.shiyanlou.com/courses/1)

- .deb安装包安装
```
    sudo dpkg -i packagename.deb

    -i：安装软件包；
    -r：删除软件包；
    -P：删除软件包的同时删除其配置文件；
    -L：显示于软件包关联的文件；
    -l：显示已安装软件包列表；
    --unpack：解开软件包；
    -c：显示软件包内文件列表；
    --confiugre：配置软件包。
```
- 终端命令安装
```
    sudo apt-get update
    sudo apt-get dist-upgrade //将系统升级到新版本
    sudo apt-get install softwarename //安装软件
    apt-get remove packagename //卸载软件
    apt-get –purge remove packagename //同上，同时删除配置文件
    apt-get autoclean apt //软件安装完成，清除软件包
```


## eg：图形字符命令banner
```
    $ sudo apt-get update //安装
    $ sudo apt-get install sysvbanner
    $ banner yjy //使用
```
![](/images/blog/linux/yjy.jpg)  &nbsp;&nbsp;**效果图展示**



