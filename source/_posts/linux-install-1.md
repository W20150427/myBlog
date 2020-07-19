---
title: linux
date: 2020-03-29
tags: [linux]
categories: linux
---
linux 安装设置问题
<!-- more -->
# <span style="color:#0366d6;">CentOS8 中文输入法</span>
$ sudo dnf install ibus-libpinyin.x86_64 -y
>dnf是centos 8新的包管理工具，yum依旧保留
查看【设置】的【Region&Language】里的【输入源】的【汉语(中国)】里面，有没有添加【汉语(智能拼音)】,若没有则重启下机器，再来添加。
# <span style="color:#0366d6;">文件和文件权限</span>
文件名
>
文件名前多一个“.”表示文件是一个隐藏文件
单一文件名字是255个字节，以一个ASCII英文字符占用一个字节来说
英文字母大概是255个，中文是128个

文件权限
>r 可读写文件的内容
w 可以写入文件，但不可以删除文件
x 该文件可以被执行

目录权限
>r 读取目录中的内容
w 修改目录中的内容，可以删除目录中的文件
x 访问目录，读取目录中的文件名字

修改文件的权限
>chgrp 修改文件所在的用户组
chown 修改文件拥有者
chmod 修改文件的权限

ls -l
>-rwxrwxrwx 显示的结果第一个字符为文件的类型
-表示为常规文件：纯文本文件，二进制文件，数据文件
d 表示文目录
l 表示为link文件
b 区块文件（block）
c 字符设备文件（character）


