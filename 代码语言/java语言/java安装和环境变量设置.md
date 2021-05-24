# java安装和环境变量设置

## 参考

https://my.oschina.net/whiteSpring/blog/4915721

https://blog.csdn.net/gulang03/article/details/81771343

## 不同系统不同cpu下jdk的选择

### jdk下载地址(在官网下载jdk需要注册一个oracle账号)

https://www.oracle.com/java/technologies/javase-downloads.html

### windows只有一类jdk(因为windows系统的电脑基本都是amd芯片或者intel芯片   这两个都是x86指令集架构)



### linux有两类jdk，所以需要 查看cpu架构，之后选择对应的linux 版本的jdk  (linux有两种cpu指令集架构，一种是arm芯片的arm指令集架构，另外一种是amd芯片或者intel芯片的x86指令集架构)

在linux系统上使用uname -a命令  (可以看到是x86架构的指令集，那么就下载linux x86的安装包)

```
[sen@localhost test]$ uname -a
Linux localhost.localdomain 3.10.0-1160.el7.x86_64 #1 SMP Mon Oct 19 16:18:59 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux

```



![image-20210423193047254](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210423193047.png)

## windows安装JDK时会跳出是否需要安装公有jre的提示(推荐不安装，jdk中包含了jre了)

有两种Java运行环境（JRE），公有JRE （public JRE)与私有JRE（private JRE)。JDK安装程序会安装私有JRE和一个可选的公有JRE。

私有JRE在JDK安装时包含，完全包含在JDK的安装路径下，仅对JDK可见，为JDK所用。

公有JRE在JDK安装时会提示是否额外安装，为系统中所有的Java程序共享，具有独立的安装程序。



与私有JRE不同，公有JRE的安装程序会对系统做一些修改来与OS和浏览器建立更密切的关系，主要有以下几个方面：

- 控制面板中的添加/删除程序列表就会出现公有JRE
- 控制面板中安装一个Java的控制面板项，可用它来设置一些与公有JRE相关的参数
- 在%SystemRoot%/system32下会出现java.exe,javaw.exe, javaws.exe
- 注册表做必要的添加或调整以将公有JRE注册到系统中。

公有JRE的卸载程序会做相应的清除工作来恢复一个干净的操作环境。

由于种种原因,公有JRE卸载失败，或者有时重装JRE也不能解决问题，为了恢复以前的环境，有时需要手工将安装程序所做的修改恢复回去，这时我们就需要了解安装程序对注册表做了哪些修改。





## windows环境变量的设置



```
windows下环境变量不区分大小，但是linux下是区分大小的，所以按照linux下的规范来

输入变量名JAVA_HOME   变量值:      你的jdk的路径

输入变量名CLASSPATH   变量值:    .;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;

在系统已经存在的Path环境变量中添加  %JAVA_HOME%\bin和%JAVA_HOME%\jre\bin

```

### JAVA_HOME环境变量设置

![1.java环境变量设置1](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210410230313.png)

### CLASSPATH环境变量设置

![1.java环境变量设置3](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210518094158.png)



### 在已经有的Path环境变量中添加新的路径

![1.java环境变量设置2](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210410230321.png)



## linux上jdk安装

```
#使用xftp软件将jdk-8u171-linux-x64.tar.gz 上传到/home/sen目录下

cd /home/sen
tar -xvf jdk-8u171-linux-x64.tar.gz 


# 修改/etc/profile文件设置系统环境变量
nano /etc/profile

cat /etc/profile
```

在/etc/profile文件末尾添加下列内容

```
export JAVA_HOME=/home/sen/jdk1.8.0_171
export JRE_HOME=${JAVA_HOME}/jre
export JAVA_PATH=${JAVA_HOME}/bin:${JRE_HOME}/bin
export PATH=$PATH:${JAVA_PATH}
export CLASSPATH=.:${JAVA_HOME}/lib/dt.jar:${JAVA_HOME}/lib/tools.jar
```

![image-20210521114351935](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210521114352.png)

```
# 让修改的文件生效
source /etc/profile

#测试是否生效
java -version

javac
```



## 第一个java程序

https://blog.csdn.net/gulang03/article/details/81771343

```java
public class Firstdemo{

 public static void main(String  [] args)
 {
  
     System.out.println("我的第一个java程序");
 
 }


}
```

在命令提示符上使用命令进行源代码的编译，然后运行

```
#编译源代码,javac是运行的程序，后面添加一个参数，参数值是需要编译的java源代码的路径
#这里给的路径是相对路径，即给出文件名，该文件位于当前命令运行所在目录下
#也可以给定绝对路径
javac  Firstdemo.java

#绝对路径
javac   C:\Users\sen\Desktop\测试\Firstdemo.java

#设置cmd窗口输出的编码格式为utf-8，默认是GBK,java程序运行输出的中文均使用utf-8编码，不进行修改运行输出的结果会乱码
#cmd窗口输出的编码切换成GBK编码
chcp 936
#cmd窗口输出的编码切换成utf-8编码
chcp 65001


#测试运行
java  Firstdemo
```





## 关于orcale jdk 收费情况(open jdk开源免费)

![img](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210521122134.jpeg)

