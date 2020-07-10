---
title: idea2020.1.1不能debug
date: 2020-06-01 02:16:35
tags: idea
---
idea 2020.1.1社区版 在 jetty-runner 1.4.13下不能debugger,在vm args 下添加
```
-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=1044 (端口号可以改)
```
使用debug模式启动,再手动点击左下角的 Attach Debugger 按钮就可以了