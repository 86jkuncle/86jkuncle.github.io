---
title: tomcat配置ssl证书
date: 2020-05-15 11:06:57
tags: tomcat
---
将证书放入tomcat目录下的conf目录,证书有多种格式,一般配置jks
```
<Certificate certificateKeystoreFile="conf/1.jks"
             certificateKeystoreType="JKS"
             certificateKeystorePassword="123456" />
```
