## tomcat不同系统下的安装

### 参考

https://my.oschina.net/zhangyongjun/blog/4640002



### 下载地址

https://tomcat.apache.org/download-90.cgi



## tomcat下载安装包的区别 (tomcat不区分linux和windows版本,因为他是java开发的)

### 两种压缩包格式(不包含windows服务的相关批处理脚本以及windows下的apr本地库)

1． Apache-tomcat-x.zip：tomcat的基础发布包，zip压缩包格式主要给windows系统使用

2． Apache-tomcat-x.tar.gz：内容与zip包相同，只是压缩格式不同，这种压缩包格式主要提供给linux系统使用 





### 以后三个是windows 的安装程序，安装就是把里面的内容解压到一个目录下（相比直接下载压缩包的方式，安装程序在安装的时候还可以设置开启一些服务）

3． Apache-tomcat-x.exe：windows的可执行安装包，功能和zip基本一致，适用windows快捷键以及系统服务形式启动

4． Apache-tomcat-x-windows-x86.zip：32位windows发布包，包含windows32位JVM配合使用的APR本地库（32和64位操作系统均合适）

5． Apache-tomcat-x-windows-x64.zip：64位windows发布包，包含windows64位JVM配合使用的APR本地库（只适合64位操作系统）

