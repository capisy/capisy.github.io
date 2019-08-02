---
title: 备忘Git不常用的命令
date: 2018-02-09 09:07:00
tags: git
catalog: true
---

#### 生成 ssh

```
ssh-keygen -t rsa -C "925697@163.com"
```

#### warning: LF will be replaced by CRLF

```
 git config --global core.autocrlf true
```

#### 日志统计

```
    git log --graph // 打印提交信息
    git log --stat // 打印提交统计
```

#### 比较提交

```
    git diff //查看修改的文件的内容
    git diff --stat // 功能同上，显示更友好
    git diff --cached //查看缓存区内和上次提交的内容的差别
```

#### 新建一个空分支

```
git checkout --orphan <new branch>
```

#### 克隆 github 上项目的非 master 分支

```
git branch -a
git checkout -b <name> origin/<name>
```
