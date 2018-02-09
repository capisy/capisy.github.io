---
title: Linux- 文件操作
date: 2018-01-27 10:07:00
tags: Linux
---
<style>
    ::selection{ color:#fff; background-color: #e26848; }
    .tx-explain { color:#666;margin-left:10px;  }
</style>

![](/images/blog/linux/linux.jpg)

<!-- more -->

[资料来自实验楼linux基础教程](https://www.shiyanlou.com/courses/1)

## 常见操作
```
    man <command_name> //帮助文档

    pwd //查看当前所在的目录

    ls *.txt //查看当前目录下匹配格式的文件 
    ls -l //显示文件的详细信息
    ls -A //显示除了 .（当前目录）和 ..（上一级目录）之外的所有文件，包括隐藏文件（Linux 下以 . 开头的文件为隐藏文件）。
    ls -Al //查看一个目录的完整属性
    ls -dl //显示所有文件大小
```

## 文件操作
- 新建文件
```
    touch file //创建新文件
    touch file1 file3 //创建多个文件
    touch file_{1..5}.txt //按照索引创建文件 file_1.txt file_2.txt file_3.txt...
```
- 新建目录 (make)
```
    mkdir mydir
    mkdir -p father/son/grandson //使用-p参数，同时创建父目录（如果不存在父目录），可以同时创建一个多级目录
```
- 删除文件或文件夹 (remove)
```
    rm file
    rm -f file //-f参数，强制删除有权限限制的文件 
    rm -r mydir //删除文件夹 -r参数，表示递归（复制文件夹及文件夹内的文件） 
```
- 复制文件或文件夹(copy)
```
    cp file <path> 
    cp -r mydir <path> //复制文件夹 -r参数，表示递归（复制文件夹及文件夹内的文件）
```
- 移动文件 & 文件重命名 (move)
```
    mv file <path>  //移动（剪切）文件
    mv file file_new //文件重命名
```

## 字符匹配
**?**   <span class="tx-explain">匹配任意一个字符</span>
**[list]**  <span class="tx-explain">匹配 list 中的任意单一字符</span>
**[!list]** <span class="tx-explain">匹配 除list 中的任意单一字符以外的字符</span>
**[c1-c2]** <span class="tx-explain">匹配 c1-c2 中的任意单一字符 如：[0-9] [a-z]</span>
**{c1..c2}** <span class="tx-explain">匹配 c1-c2 中全部字符 如{1..10}</span>
**{string1,string2,...}** <span class="tx-explain">匹配 string1 或 string2 (或更多)其一字符串</span>

## 查看文件 cat(正序显示)， tac（倒序显示），nl(添加行号，并打印信息)
```
    cat file 
    cat -n file // -n 参数用于添加行号
```
```
    nl -b a file // eg
    // nl 重要参数
    -b : 指定添加行号的方式，主要有两种：
        -b a:表示无论是否为空行，同样列出行号("cat -n"就是这种方式)
        -b t:只列出非空行的编号并列出（默认为这种方式）
    -n : 设置行号的样式，主要有三种：
        -n ln:在行号字段最左端显示
        -n rn:在行号字段最右边显示，且不加 0
        -n rz:在行号字段最右边显示，且加 0
    -w : 行号字段占用的位数(默认为 6 位)
```

## 使用more 和 less 命令分页查看文件
```
    cp /etc/paswd
    more passwd
```
打开后默认只显示一屏内容，终端对不显示当前阅读的进度。
可以使用enter键向下滚动一行，使用space键向下滚动一屏

## tail（跟踪，尾巴）查看文件的头几行和末尾几行
```
    tail /etc/passwd
    tail -n 1 /etc/passwd //-n： 显示行数
```

## 查看文件类型 file
```
    file /bin/ls //可执行文件
```

## 文件打包和解压缩

- zip 命令（打包压缩）
1.压缩
```
    zip -r -q -l -o <打包后的文件名.zip> <被打包的目录>  //打包操作
    zip -r  -8 -q -o <打包的文件名_8.zip> <被打包的目录的绝对路径> -x ~、*.zip 
    du -h <打包后的文件名.zip>  //查看压缩包的大小
    file <打包后的文件名.zip> //查看压缩包的类型

    //zip 主要参数参数
    -r 表示递归打包包含子目录的全部内容
    -q 表示安静模式，即不向屏幕输出信息
    -o 表示输出的文件名
    -l 解决windows 系统和Linux系统在文本文件格式上的兼容性问题
    -8 压缩级别，数字越大表示压缩程度越高（体积小，耗时长）
```

2.unzip 命令（解压缩zip文件）
```
    unzip <解压缩的文件.zip>
    unzip -q <解压的文件.zip> -d <解压后，存储解压包的路径>

    unzip -l <压缩文件.zip> // 如果不想解压，只想查看压缩包的内容，可以使用 -l 参数
```
3.tar命令
```
    tar -cf <打包后的名字.tar> <打包文件或文件目录> //打包文件 
    -c  // 创建有个打包文件
    -f // 打包的文件名

    tar -xf <打包的文件名.tar> -C <已经存在的目录>  //解压打包文件

    tar -czf <打包压缩文件.tar.gz> <打包文件或文件目录> //打包并用gzip压缩文件
    tar -xzf <打包压缩文件.tar.gz>
```
4.压缩文件格式 
```
    *.tar.gz  //-z
    *.tar.xz //-J
    *.tar.bz2 //-j
```