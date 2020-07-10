---
title: act通过maven命令行创建项目
date: 2019-10-25 14:15:57
tags: act
---
```
mvn archetype:generate -B 
-DgroupId=com.mycom.helloworld 
-DartifactId=helloworld 
-DarchetypeGroupId=org.actframework 
-DarchetypeArtifactId=archetype-quickstart 
-DarchetypeVersion=1.8.28.1
```
<!-- more -->
***
开启跨域
```
cors=true
cors.origin=*
cors.headers=Content-Type,X-HTTP-Method-Override,Location,Authorization
cors.option.check=false
cors.max_age=-1
```

文档
[act作者oschina博客](https://my.oschina.net/greenlaw110)
[act在线文档](https://github.com/actframework/act-doc/tree/master/cn)
[依赖注入Genie](https://github.com/osglworks/java-di/blob/master/doc/container.md)