---
title: jenkins自动部署
date: 2019-11-05 14:35:13
tags: jenkins
---
在代码push到仓库后,使用jenkins一键部署,完成自动拉取代码,编译,测试,打包,上传服务器等操作

# 一、下载jenkins
[jenkins中文官网](https://jenkins.io/zh/)
windows平台下载war包

# 二、启动
进入到jenkins下载目录

` java -jar jenkins.war `

<!-- more -->
# 三、配置
打开 http://localhost:8080/,
使用推荐的配置,创建管理用户,然后登录
登录之后有各种菜单,My Views 是默认的展示页,主要关注 
1. Manage Jenkins
2. 凭据

## 3-1.Manage Jenkins
主要关注 
Configure System,Global Tool Configuration,Manage Plugins

### 3-1-1.Configure System
这里面可以配置的选项
1. jenkins主目录(默认 C:\Users\Administrator\.jenkins)
2. 配置 Gitlab　的地址 (需要安装 Gitlab 插件)
3. 系统时间的格式化
4. jenkins的url,管理员的邮件地址
5. Github服务器
6. 配置远程服务器ip,私钥,remote directory工作目录(需要安装 Publish over SSH 插件)
   1. ip 地址
   2. 私钥,登录需要使用的私钥文件
   3. remote directory,工作目录就是jenkins上传到远程服务器的哪个目录,一般为webapps目录

### 3-1-2.Global Tool Configuration
1. Maven配置,文件路径一定是要全限定名,比如 settings.xml文件在 D:\maven\conf 目录下,那文件路径就是 D:\maven\conf\settings.xml
2. jdk配置,指定 java 的安装目录
3. Git配置,也需要全限定名,windows文件路径为 cmd目录下的 git.exe,不能是 bin目录下的git.exe
4. Maven配置,指定maven的安装目录
5. 还可以配置docker,ant,gradle

### 3-1-3.Manage Plugins
各种插件,需要安装插件才能使用功能
创建maven工程安装 Maven Integration
使用GitLab安装 GitLab
使用Git安装 Git
使用GitHub安装 GitHub
使用Gitee安装 Gitee

## 3-2.凭据
添加git,远程服务器登录关联的账号密码,私钥,GitLab token等
添加git私钥的时候,是包括注释的

配置后应用并保存

# 四、创建项目
配置好后,进入主界面,新建 Item,有各种类型的项目,输入项目名字,新建一个Maven项目
创建好后配置项目,重点关注 源码管理,构建触发器,Pre Steps,Build,Post Steps
## 4-1.源码管理
仓库地址和分支,表名工程是在哪个分支下面,需要选择在 凭据里面 添加的账号密码

## 4-2.构建触发器
在push代码后执行部署操作,需要服务器环境
github的选项为 Build when a change is pushed to GitLab. GitLab webhook URL: http://localhost:8080/project/xx
gitee的选项为 Gitee webhook 触发构建，需要在 Gitee webhook 中填写 URL: http://localhost:8080/project/xx

## 4-3.Pre Steps
在构建工程前可以上传文件到远程服务器或者执行相关命令
进入tomcat目录,删除和项目相关的war或jar文件,停止tomcat
```
cd /home/tomcat/apache-tomcat-8.5.45/war
rm -rf ./*.war
cd ..
./tomcat.sh stop
```

## 4-4.Build
Root POM 填写 pom.xml,根据pom文件构建
options 就是maven命令,clean install,package deploy等

## 4-5.Post Steps
构建后,这时需要将war或jar包上传到远程服务器了
Transfers配置上传哪个位置的文件,然后执行什么命令
1. source files 指定上传当前maven项目哪个目录下的文件到服务器,一般为 target/xx.war,xx为项目名
2. remove prefix 要移除的文件前缀,可指定为 target/,如果不指定,上传到远程服务器后会在远程工作目录新建 target文件夹
3. remote directory,这里是相对目录,如果在Configure System里面配置的远程工作目录为 /home/tomcat/webapps,那这里的 / 就是表示 webapps目录
4. 执行命令,进入tomcat根目录,启动tomcat等,命令是在 /home/tomcat/webapps 路径下执行的

```
source /etc/profile
cd /home/tomcat/apache-tomcat-8.5.45/war
unzip -o xx.war -d  ../webapps/xx
cd ..
./tomcat.sh start
```
配置好后应用并保存

# 五、测试构建
回到主菜单,可以看到我们创建的项目了,点击项目进入到构建面板
点击 Build Now,就会开始构建了,一般有以下几个步骤
1. 从git拉取代码到 jenkins 的工作空间,如果拉取成功,执行下一步
2. 在远程服务器运行 Pre Steps 指定的命令,如果执行成功,执行下一步
3. 开始执行 Build 配置的maven命令,如 clean install,如果执行成功,执行下一步
4. war包生成了,连接远程服务器上传war包到 远程工作目录,如果执行成功,执行下一步
5. 运行Post Steps 指定的命令,如果执行成功,整个自动部署就成功了

每一次构建都会有详细的日志记录,如果失败可以快速定位失败的原因


