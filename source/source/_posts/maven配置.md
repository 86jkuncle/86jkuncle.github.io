---
title: maven配置
date: 2020-05-15 10:54:29
tags: maven
---
1.配置本地仓库,下载的jar包保存的路径
```
<localRepository>D:/tools/apache-maven-3.5.4/repository</localRepository>
```
<!-- more -->
2.配置远程仓库,从这些仓库中拉取jar包
```
<mirror>
  <id>alimaven</id>
  <mirrorOf>central</mirrorOf>
  <name>aliyun maven</name>
  <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
</mirror>

<mirror>
  <id>repo2</id>
  <mirrorOf>sec</mirrorOf>
  <name>Human Readable Name for this Mirror.</name>
  <url>http://repo2.maven.org/maven2/</url>
</mirror>

<mirror>
  <id>uk</id>
  <mirrorOf>third</mirrorOf>
  <name>center-repo-in-uk</name>
  <url>http://uk.maven.org/maven2/</url>
</mirror>
```

3.配置编译的jdk版本
```
<profile>
	<id>jdk-1.8</id>  
	<activation>  
		<activeByDefault>true</activeByDefault>  
		<jdk>1.8</jdk>  
	</activation>  
	<properties>  
		<maven.compiler.source>1.8</maven.compiler.source>  
		<maven.compiler.target>1.8</maven.compiler.target>  
		<maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
	</properties>
</profile>
```