# 真机安装mac系统

## 参考

https://blog.daliansky.net/undefined.html



https://www.bilibili.com/video/BV18V41187JZ?from=search&seid=11692296010694641491



https://www.youtube.com/watch?v=rHa2T3G_3gM



https://www.jianxiangyun.com/2018/12/30/845.html



https://heipg.cn/macos/macos-big-sur-developer-beta3-20a5323i-cdr.html

https://www.jianshu.com/p/9bfe2824d459

## MacOS镜像下载地址

文章列表：[https://blog.daliansky.net/categories/%E4%B8%8B%E8%BD%BD/](https://blog.daliansky.net/categories/下载/)

下载地址：https://mirrors.dtops.cc/iso/MacOS/daliansky_macos/



## 需要准备的工具

### 1.一个还不错的U盘

### <strong style="color:red;">下面两个是最关键的!!!!</strong>

### 2.对应本机电脑硬件配置的苹果系统镜像 (不能随便找一个镜像,要找本机能够安装的镜像)

### 3.对应本机电脑硬件配置的EFI文件





### 4.balenaEtcher-Portable-1.5.116.exe (制作苹果系统安装U盘的工具)

### 5.DiskGenius5.2_x64.exe (硬盘格式化和分区的工具)

### 6.BOOTICE64位.exe （系统启动修改工具)















## 安装步骤

### 1.去下面的网站找到自己机型对应的EFI文件

https://blog.daliansky.net/Hackintosh-long-term-maintenance-model-checklist.html

#### 我的机子是ASUS-FL8000UQ，所以需要找到适合ASUS-FL8000UQ电脑硬件的EFI文件(这里的EFI可以理解成驱动的结合)

根据网站找到对应的EFI文件的下载地址,这个地址里面有本机如何安装黑苹果的教程

https://github.com/KKKIIINNN/ASUS-FL8000UQ-Hackintosh

### 2.从上述网站找到的EFI文件里面都会使用教程,按照里面的教程对BIOS进行配置



#### 我电脑需要做的BIOS设置：(不同电脑需要进行的设置不一样,查看具体的教程)

- 1.关闭`Secure  Boot`   (关闭Secure  Boot也就是开启  CMS)（这是微软为了防止安装Windows操作系统的电脑改装linux而设置的，不关闭无法启动到四叶草）

- 2.关闭`FastBoot`   (开启CMS之后默认关闭FastBoot)

- 3.设置硬盘模式为AHCI，否则会出现磁盘工具读不到内置硬盘的情况；　

  ![image-20210502104110122](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210502104153.png)　

- 4.解锁`CFG Lock`

  1.将U盘格式化为FAT32格式；

  2.打开CFG文件夹，将里面的EFI文件复制到U盘中，重启按ESC键选择U盘启动；

  3.输入命令`setup_var_3 0x527 0x00`即可解锁CFG Lock。







### 3. 制作苹果系统安装U盘(balenaEtcher工具)





## 系统安装完成后,会在原先的ESP分区的EFI文件下创建一个APPLE文件夹(不会情况ESP分区原先的文件)



## 安装时出现的问题:这个安装macOS Mojave 应用程序副本已损坏，不能用来安装Mac OS

https://zhuanlan.zhihu.com/p/88597219







https://zhuanlan.zhihu.com/p/79714868





