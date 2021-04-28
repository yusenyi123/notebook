# java安装和环境变量设置

## 参考

https://my.oschina.net/whiteSpring/blog/4915721

## 不同系统不同cpu下jdk的选择

### linux 查看cpu架构，之后选择对应的linux 版本jdk，windows只有一类jdk





使用uname -a命令

```
[sen@localhost test]$ uname -a
Linux localhost.localdomain 3.10.0-1160.el7.x86_64 #1 SMP Mon Oct 19 16:18:59 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux

```



![image-20210423193047254](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210423193047.png)





```
windows下环境变量不区分大小，但是linux下是区分大小的，所以按照linux下的规范来

输入变量名JAVA_HOME   变量值:      你的jdk的路径

输入变量名CLASSPATH   变量值:    .;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;

在系统已经存在的Path环境变量中添加 %JAVA_HOME%\bin和%JAVA_HOME%\jre\bin

```

## JAVA_HOME环境变量

![1.java环境变量设置1](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210410230313.png)

## CLASSPATH环境变量

![image-20210410230434470](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210410230434.png)



## Path环境变量中添加

![1.java环境变量设置2](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210410230321.png)