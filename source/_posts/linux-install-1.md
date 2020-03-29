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
