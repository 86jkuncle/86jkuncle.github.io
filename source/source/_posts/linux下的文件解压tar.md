---
title: linux下的文件解压tar
date: 2020-05-15 11:02:15
tags: centos
---
1.文件解压到指定目录
文件 aaa.tar.gz 解压后默认解压到 aaa 文件夹,如果我想解压到 a 文件夹,命令
```
tar -zxvf ./aaa.tar.gz --strip-components=1 -C ./a/
```
<!-- more -->
2.防火墙开关命令
```
打开防火墙
systemctl start firewalld

关闭防火墙
systemctl stop firewalld

查看防火墙状态
systemctl status firewalld

开放80商品
firewall-cmd --zone=public --add-port=80/tcp --permanent
```
