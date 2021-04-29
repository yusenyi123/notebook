# vmware15安装Mac系统虚拟机

## 参考：

https://blog.csdn.net/qq_43901693/article/details/103723081

https://bbs.kafan.cn/thread-2206082-1-1.html

## 1.解锁vmware15创建mac系统选项



### 1.1安装vmware15，安装很简单，网上教程也很多

### 1.2停止vmware15的5个服务和相关进程，只有这些正在运行的活动和服务停止之后，才能使用解锁文件修改vmware15中的相关文件，从而解锁mac选项

![image-20200828230119061](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200828230126.png)



![image-20200828230309092](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200828230309.png)

### 

![image-20200828230655814](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200828230655.png)

![image-20210428093510510](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210428093510.png)

```
总共需要关闭四个进程
分别是
1. Vmware Authorization

2. Vmware USB Arbitration

3.vmware-hostd.exe

4.Vmware Tray Process
```





### 1.3 打开unlocker文件夹，选择win-install.cmd文件以管理员身份运行

![image-20200828231158531](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200828231158.png)



> 执行该文件的某个命令会去下载一个com.vmware.fusion.zip.tar文件
>
> 因为unlocker需要com.vmware.fusion.zip.tar文件中的darwin.iso和darwinPre15.iso
> 但只是需要darwin.iso和darwinPre15.iso这两个文件
>
> 方法:
>
> 将 darwin.iso文件和darwinPre15.iso文件放到unlocker工具的tools文件夹下 (没有tools文件夹就新建一个)
>
> 运行时提示You already have downloaded the tools. Download again?[y/n]敲n就可以跳过了。
>
> 
>
> 所以我们不需要把com.vmware.fusion.zip.tar这个文件全部下载下来，com.vmware.fusion.zip.tar文件有600M多大小,网速差，下载很慢
>
> com.vmware.fusion.zip.tar文件下载地址，下载后解压获得darwin.iso文件和darwinPre15.iso文件
>
> https://softwareupdate.vmware.com/cds/vmw-desktop/fusion/12.1.1/17801503/core/com.vmware.fusion.zip.tar







> 

### 1.4 命令运行完成后，安装mac系统选项解锁成功，==我们需要重新打开刚才被我们关闭的5个服务==，然后运行vmware15在创建虚拟机界面就能看到

![image-20200828232230130](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200828232230.png)

## 2.安装Mac操作系统

### 2.1 修改vmx文件

![image-20200828233453960](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200828233454.png)

在smc.present = "TRUE"后添加smc.version = "0"

![image-20200828233759041](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200828233759.png)



### 2.2 启动虚拟机按步骤安装即可

```
使用磁盘分区工具的抹掉功能对硬盘进行分区(抹掉功能会划分两个分区一个EFI分区，在苹果处看不到，存放引导程序，一个苹果文件系统的分区，这个在后续安装中选择这个分区将操作系统存放)

之后安装将系统安装到抹掉功能使用后产生的一个分区上
```



参考

https://zhuanlan.zhihu.com/p/92827822

https://www.cnblogs.com/bad5/p/13251570.html

https://zhuanlan.zhihu.com/p/337036027

http://www.yishimei.cn/network/577.html

https://vircloud.net/operations/vm-ins-macos-new.html



![image-20210428142512444](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210428142512.png)

![image-20210428163853985](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210428163854.png)



![image-20210428142900607](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210428142900.png)



![image-20210428164101005](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210428164101.png)





#### 使用抹掉功能后，硬盘分区情况

![image-20210428143930929](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210428143931.png)

#### 将系统安装到使用磁盘分区抹掉功能产生的分区，系统安装完成后硬盘分区情况

![image-20210428171606508](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210428171606.png)

## 3.开启  安装允许任何来源 选项



macOS 10.13允许任何来源开启方法：如果需要恢复允许“任何来源”的选项，即关闭系统的Gatekeeper，我们可以在“Launchpad”—“其他”—“终端”，在终端中输入命令：```sudo spctl --master-disable```  允许后就能打开允许从任何来源安装了，可以在安全性与隐私界面查看，如下图

![image-20200829003920927](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200829003921.png)

![image-20200829004014238](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200829004247.png)



![image-20200829003658669](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200829003701.png)



### 之后就可以安装vmtools





## 4.mac os虚拟机联网

使用桥接模式，然后在虚拟机里手动设置ip，子网掩码，网关，dns地址



## 5.显示硬盘和显示隐藏文件夹

### 显示硬盘

https://jingyan.baidu.com/article/acf728fd7138b7f8e510a398.html

### 显示隐藏文件夹

```
#终端输出下面的命令显示隐藏文件夹

defaults write com.apple.finder AppleShowAllFiles -boolean true ; killall Finder
```



### finder(mac的资源管理器)只显示英文

https://nanc.xyz/record/twoway-of-localization.html

```
com.apple.finder.plist
defaults write com.apple.finder.plist AppleLanguages -array en
```





