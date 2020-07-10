---
title: nginx配置目录浏览
date: 2020-05-15 11:09:12
tags: nginx
---
配置在 http 节点下面
```
autoindex on;  # 开启目录文件列表
autoindex_exact_size on;  # 显示出文件的确切大小，单位是bytes(字节)
autoindex_localtime on;  # 显示的文件时间为文件的服务器时间
charset utf-8,gbk;  # 避免中文乱码

另外，如果希望请求文件是下载而不是显示内容，可以通过添加下面参数实现：

add_header Content-Disposition attachment;
```