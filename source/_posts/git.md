---
title: 备忘一些git不常用的命令和坑爹的错误
date: 2018-02-09 09:07:00
tags: git
---
<style>
    ::selection{ color:#fff; background-color: #e26848; }
    .tx-explain { color:#666;margin-left:10px;  }
</style>

![](/images/blog/git/git.jpg)

<!-- more -->

## 日志统计
```
    git log --stat //显示每个（commit）中文件修改详情
```

## 比较提交
```
    git diff //查看修改的文件的内容
    git diff --stat // 功能同上，显示更友好
    git diff --cached //查看缓存区内和上次提交的内容的差别
```
