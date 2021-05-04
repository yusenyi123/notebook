

# IDEA界面详解

## 1.创建一个web项目

### 1.1 创建一个空的项目(空的项目就是我们在eclipse中的工作空间)

IDEA中没有像eclipse中有工作空间的概念，但是我们也可以做到编写项目像eclipse那样一个工作空间，然后工作空间下编写多个项目

因为IDEA中在project下还存在一个module的概念，所以在使用IDEA工具进行项目编写的时候，IDEA中的空project就是我们在eclipse中的工作空间，每一个module就是一个项目

这样我们就做到了类似eclipse中一个工作空间下多个项目



### 1.2在空的项目下创建module(这个module就是我们在eclipse中的项目)



这里创建项目的时候需要指定Application Server



#### tomcat目录说明

- **bin**：主要存放脚本文件，例如比较常用的windows和linux系统中启动和关闭脚本
- **conf**：主要存放配置文件，其中最重要的两个配置文件是**server.xml和web.xml**
- **lib**：主要存放tomcat运行所依赖的包
- LICENSE：版权许可证，软件版权信息及使用范围等信息
- **logs**：主要存放运行时产生的日志文件，例如catalina.out(曾经掉过一个大坑)、catalina.{date}.log等
- NOTICE：通知信息，一些软件的所属信息和地址什么的
- RELEASE-NOTES：发布说明，包含一些版本升级功能点
- RUNNING.txt：运行说明，必需的运行环境等信息
- **temp**：存放tomcat运行时产生的临时文件，例如开启了hibernate缓存的应用程序，会在该目录下生成一些文件
- **webapps**：部署web应用程序的默认目录，也就是 war 包所在默认目录
- **work**：主要存放由JSP文件生成的servlet（java文件以及最终编译生成的class文件）





#### 疑惑：Tomcat Home 和Tomcat  base directory 是什么？

![image-20201225210336609](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20201225210345.png)

#### 解答：这是为了实现Tomcat 单机多实例部署而存在的概念



![image-20201225215927348](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20201225215927.png)

```
Tomcat home   就是CATALINA_HOME  
Tomcat base directory  就是CATALINA_BASE

上图中的 CATALINA_HOME 指Tomcat安装路径，CATALINA_BASE 指实例所在位置。
CATALINA_HOME 路径下只需要包含 bin 和 lib 目录，而 CATALINA_BASE 只存放 conf、webapps、logs 等这些文件，这样部署的好处在于升级方便，配置及安装文件间互不影响，在不影响 Tomcat 实例的前提下，替换掉 CATALINA_HOME 中的安装文件。


所以Tomcat home就是Tomcat安装路径
Tomcat base directory就是指定一个Tomcat实例路径（这个路径文件夹下包含conf、webapps、temp、logs、work）
Tomcat安装路径下就存在文件，所以Tomcat的安装路径也可以作为一个实例路径

所以我们在配置IDEA服务器的时候往往Tomcat home和Tomcat base directory 这两处填写相同的地址
这样项目启动的时候就会加载Tomcat安装路径下的配置文件来设定端口号
```

**配置 server.xml 端口**
同一个服务器部署不同 Tomcat 要设置不同的端口，不然会报端口冲突，所以我们只需要修改`conf/server.xml`中的其中前三个端口就行了。但它有四个分别是：

- **Server Port**：该端口用于监听关闭tomcat的shutdown命令，默认为8005
- **Connector Port**：该端口用于监听HTTP的请求，默认为8080
- **AJP Port**：该端口用于监听AJP（ Apache JServ Protocol ）协议上的请求，通常用于整合Apache Server等其他HTTP服务器，默认为8009
- Redirect Port：重定向端口，出现在Connector配置中，如果该Connector仅支持非SSL的普通http请求，那么该端口会把 https 的请求转发到这个Redirect Port指定的端口，默认为8443；



#### cmd中设置环境变量(set  setx  /m参数)

```sh
set是设置临时环境变量，只在当前终端生效
setx是设置永久环境变量，当前终端不生效

/m参数 是设置系统环境变量，没有/m是设置用户环境变量



查看当前可用的所有环境变量（＝系统变量＋用户变量）
set

查看某个环境变量，如PATH
set PATH

添加用户环境变量，如xxx=aa
set xxx=aa

将用户环境变量（如xxx）的值置为空
set xxx=

在某个环境变量（如PATH）后添加新的值（如d:\xxx）
set PATH=%PATH%;d:\xxx




设置永久用户环境变量 
setx path "C:\Java\jdk1.8.0_31\bin"


设置永久系统环境变量
setx path "C:\Java\jdk1.8.0_31\bin" /m





set CATALINA_HOME=E:\runenv\apache-tomcat-8.5.61
set CATALINA_BASE=E:\runenv\tomcat1
set CATALINA_BASE=E:\runenv\tomcat2
set PATH=%PATH%;%CATALINA_HOME%


cd  E:\runenv\apache-tomcat-8.5.61\bin
catalina.bat run


```



#### tomcat单机多实例实现

##### 1.创建如下的Tomcat实例目录，修改两个实例目录下的conf文件夹中的server.xml文件中的端口信息，两个实例监听不同的端口

![image-20201225225533499](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20201225225533.png)

##### 2.在cmd窗口设置CATALINA_BASE变量后运行tomcat



![image-20201226121355862](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20201226121356.png)



#### 创建Module时Content root 和Moudle file location 地址区别

![image-20201226145722999](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20201226145818.png)



## 2.IDEA project Structure界面设置详解

### Project页面设置

![image-20201226115152380](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20201226115200.png)

### Modules页面设置

![image-20201226125356905](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20201226125357.png)



### Libraries页面设置(jar包库，一个jar包库包含多个jar包，导入一个jar时默认会创建一个以该jar包为名字的jar包库，这个jar包库中包含导入的jar包)

![image-20201231213431113](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20201231214434.png)





![image-20201231214354115](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20201231214445.png)





#### 在编写代码的时候导入library（作用范围（scope）是compile ）在生成artifact时需要修复一下

修复就是把导入的包加入artifact目录结构中，在WEB-INF文件夹下新建一个lib文件夹，把jar包放入

![image-20201231215000366](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20201231215000.png)





### Facets(项目使用的框架)页面设置

![image-20201226144622157](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20201231214515.png)









### Artifacts(人造产品，也就是输出的最终部署的项目)页面设置

![image-20201226150951712](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20201226150951.png)







## 3.项目运行Tomcat配置界面设置

![image-20210102231020054](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20210102232328.png)





![image-20210102233112905](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20210102233113.png)