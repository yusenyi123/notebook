# linux文件目录详解

/ 根目录，第一层目录，所有其他目录的根，一般根目录下只存放目录。包括：/bin,

/boot, /dev, /etc, /home, /lib, /mnt, /opt, /proc, /root, /sbin, /sys, /tmp, /usr, /var



![image-20200910110556408](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200910110556.png)

## /bin (Binary 二进制）

commandsin this dir are all system installed user commands 系统指令

/bin是系统的一些指令。bin为binary的简写主要放置一些系统的必备执行档例如:cat、cp、chmod df、dmesg、gzip、kill、ls、mkdir、more、mount、rm、su、tar等



## /sbin（System Binary 系统二进制）

/sbin一般是指超级用户指令。主 要放置一些系统管理的必备程式例如:cfdisk、dhcpcd、dump、e2fsck、fdisk、halt、ifconfig、 ifup、 ifdown、init、insmod、lilo、lsmod、mke2fs、modprobe、quotacheck、reboot、 rmmod、 runlevel、shutdown等





## /usr （Unix System Resource Unix系统资源）

==首先注意usr 指 Unix System Resource（Unix系统资源），而不是user==

==以前的usr是user的缩写，是曾经的HOME目录，然而现在已经被/home取代了，现在usr被称为是Unix System Resource，即Unix系统资源的缩写。==

###  1./usr/bin

 是你在后期安装的一些软件的运行脚本。主要放置一些应用软体工具的必备执行档例如c++、g++、gcc、chdrv、diff、dig、du、 eject、elm、free、gnome*、 gzip、htpasswd、kfm、ktop、last、less、locale、m4、make、 man、mcopy、ncftp、 newaliases、nslookup passwd、quota、smb*、wget等。



###   2./usr/sbin

放置一些用户安装的系统管理的必备程式例如:dhcpd、httpd、imap、in.*d、inetd、lpd、named、netconfig、nmbd、samba、sendmail、squid、swap、tcpd、tcpdump等



然后通常：
 /usr/bin下面的都是系统预装的可执行程序，会随着系统升级而改变。
 /usr/local/bin目录是给用户放置自己的可执行程序的地方，推荐放在这里，不会被系统升级而覆盖同名文件。

## /boot （boot 启动）

**这里存放系统启动需要的文件**，你可以看到grub文件夹，它是常见的开机引导程序。我们不应该乱动这里的文件。

## /dev （device 设备）

dev是device的缩写，这里存放着所有的**设备文件**。在 Linux 中，所有东西都是以文件的形式存在的，包括硬件设备。

比如说，sda,sdb就是我电脑上的两块硬盘，后面的数字是硬盘分区：

鼠标、键盘等等设备也都可以在这里找到。



## /etc  （etcetera 额外的）

这个目录经常使用，存放很多程序的**配置信息**，比如包管理工具 apt：

在/etc/apt中就存放着对应的配置，比如说镜像列表（我配置的阿里云镜像）：

如果你要修改一些系统程序的配置，十有八九要到etc目录下寻找



## /lib（library  库）

lib是 Library 的缩写，包含 bin 和 sbin 中可执行文件的依赖，类似于 Windows 系统中存放dll文件的库。

也可能出现lib32或lib64这样的目录，和lib差不多，只是操作系统位数不同而已。



## /media （media 媒体）

这里会有一个以你用户名命名的文件夹，**里面是自动挂载的设备**，比如 U 盘，移动硬盘，网络设备等。

比如说我在电脑上插入一个 U 盘，系统会把 U 盘自动给我挂载到/media/fdl这个文件夹里（我的用户名是 fdl），如果我要访问 U 盘的内容，就可以在那里找到。



## /mnt （mount 挂载）

这也是和设备挂载相关的一个文件夹，一般是空文件夹。media文件夹是系统自动挂载设备的地方，这里是你**手动挂载设备**的地方。

比如说，刚才我们在dev中看到了一大堆设备，你想打开某些设备看看里面的内容，就可以通过命令把设备挂载到mnt目录进行操作。

不过一般来说，现在的操作系统已经很聪明了，像挂载设备的操作几乎都不用你手动做，系统应该帮你自动挂载到media目录了



## /opt （option  选项）

opt是 Option 的缩写，这个文件夹的使用比较随意，一般来说我们自己在浏览器上下载的软件，安装在这里比较好。当然，包管理工具下载的软件也可能被存放在这里。



## /proc （process 运行）

proc是process的缩写，这里存放的是全部**正在运行程序的状态信息**。

你会发现/proc里面有一大堆数字命名的文件夹，这个数字其实是 Process ID（PID），文件夹里又有很多文件。

前面说过，Linux 中一切都以文件形式储存，类似/dev，这里的文件也**不是真正的文件**，而是程序和内核交流的一些信息。比如说我们可以查看当前操作系统的版本，或者查看 CPU 的状态：

如果你需要调试应用程序，proc目录中的信息也许会帮上忙



## /root （root 根）

这是超级用户的家目录，普通用户需要授权才能访问。

区别一下 root 用户和根目录的区别哈，root 用户就是 Linux 系统的超级用户（Super User），而根目录是指 / 目录，整个文件系统的「根部」



## /srv （service  服务）

srv是service的缩写，主要用来**存放服务数据。**

对于桌面版 Linux 系统，这个文件夹一般是空的，但是对于 Linux 服务器，Web 服务或者 FTP 文件服务的资源可以存放在这里。



## /tmp （temporary  临时文件）

tmp是temporary的缩写，存储一些程序的**临时文件**。

临时文件可能起到很重要的作用。比如经常听说某同学的 Word 文档崩溃了，好不容易写的东西全没了，Linux 的很多文本编辑器都会在/tmp放一份当前文本的 copy 作为临时文件，如果你的编辑器意外崩溃，还有机会在/tmp找一找临时文件抢救一下。

比如上图的VSCode Crashes应该就是 VScode 编辑器存放临时文件的地方。

当然，tmp文件夹在系统重启之后会自动被清空，如果没有被清空，说明系统删除某些文件失败，也许需要你手动删除一下。



## /var （variable 变量）

var是variable的缩写，这个名字是历史遗留的，现在该目录最主要的作用是**存储日志（log）信息**，比如说程序崩溃，防火墙检测到异常等等信息都会记录在这里。

日志文件不会自动删除，也就是说随着系统使用时间的增长，你的var目录占用的磁盘空间会越来越大，也许需要适时清理一下。



## /home（home 家）

最后说home目录，这是普通用户的家目录。在桌面版的 Linux 系统中，用户的家目录会有下载、视频、音乐、桌面等文件夹，这些没啥可说的，我们说一些比较重要的隐藏文件夹（Linux 中名称以.开头就是隐藏文件）。

其中.cache文件夹存储应用缓存数据，.config文件夹存储了一部分应用程序的配置，比如说我的 Chrome 浏览器配置就是那里面。但是还有一部分应用程序并不把配置储存在.config文件夹，而是自己创建一个隐藏文件夹，存放自己的配置文件等等信息，比如你可以看到 Intellij 的配置文件就不在.config中。

最后说.local文件夹，有点像/usr/local，里面也有bin文件夹，也是存放可执行文件的。比如说我的 python pip 以及 pip 安装的一些工具，都存放在~/.local/bin目录中。**但是，存在这里的文件，只有该用户才能使用。**

这就是为什么，有时候普通用户可以使用的命令，用 sudo 或者超级用户却被告知找不到该命令。因为有的命令是特定用户家目录里的，仅被添加到了该用户的PATH环境变量里，只有他可以直接用。你超级用户想用当然可以，但是得写全绝对路径才行。







# 最后总结

如果修改系统配置，就去/etc找，

如果修改用户的应用程序配置，就在/home的隐藏文件里找

如果某个程序崩溃了，可以到/val/log中尝试寻找出错信息，到/tmp中寻找残留的临时文件。

### 系统可运行命令目录

你在命令行里可以直接输入使用的命令，其可执行文件一般就在以下几个位置：

```
/bin    
/sbin
/usr/bin
/usr/sbin
/usr/local/bin
/usr/local/sbin
/home/USER/.local/bin
/home/USER/.local/sbin
```

如果你写了一个脚本/程序，想在任何时候都能直接调用，可以把这个脚本/程序添加到上述目录中的某一个。



### /bin和/sbin区别

​    1）从命令功能来看，/sbin 下的命令属于基本的系统命令，如shutdown，reboot，用于启动系统，修复系统，/bin下存放一些普通的基本命令，如ls,chmod等，这些命令在Linux系统里的配置文件脚本里经常用到。

  2）从用户权限的角度看，/sbin目录下的命令通常只有管理员才可以运行，/bin下的命令管理员和一般的用户都可以使用。

  3）从可运行时间角度看，/sbin,/bin能够在挂载其他文件系统前就可以使用

 /bin和/sbin目录是在系统启动后挂载到根文件系统中的，所以/sbin,/bin目录必须和根文件系统在同一分区；



### /usr/bin,/usr/sbin和/sbin,/bin目录的区别

  1) /bin,/sbin目录是在系统启动后挂载到根文件系统中的，所以/sbin,/bin目录必须和根文件系统在同一分区；

  2）/usr/sbin存放的一些非必需的系统命令，/usr/bin 存放一些用户命令

  3）usr/bin /usr/sbin 可以和根文件系统不在一个分区。



### /usr/bin,/usr/sbin和/usr/local/bin,/usr/local/sbin的区别

通常/usr/bin下面的都是系统预装的可执行程序，会随着系统升级而改变。

/usr/local/bin目录是给用户放置自己的可执行程序的地方(后续安装的和系统无关的软件)



==两个目录下有相同的可执行程序，/usr/local/bin优先于/usr/bin。==



### 上述命令目录下的程序

```
/bin      是系统的一些指令。bin为binary的简写主要放置一些系统的必备执行档例如:cat、cp、chmod df、dmesg、gzip、kill、ls、mkdir、more、mount、rm、su、tar等。    
/sbin     一般是指超级用户指令。主要放置一些系统管理的必备程式例如:cfdisk、dhcpcd、dump、e2fsck、fdisk、halt、ifconfig、ifup、 ifdown、init、insmod、lilo、lsmod、mke2fs、modprobe、quotacheck、reboot、rmmod、 runlevel、shutdown等。   
/usr/bin　是你在后期安装的一些软件的运行脚本。主要放置一些应用软体工具的必备执行档例如c++、g++、gcc、chdrv、diff、dig、du、eject、elm、free、gnome*、 gzip、htpasswd、kfm、ktop、last、less、locale、m4、make、man、mcopy、ncftp、 newaliases、nslookup passwd、quota、smb*、wget等。
/usr/sbin 放置一些用户安装的系统管理的必备程序例如:dhcpd、httpd、imap、in.*d、inetd、lpd、named、netconfig、nmbd、samba、sendmail、squid、swap、tcpd、tcpdump等。    
如果新装的系统，运行一些很正常的诸如：shutdown，fdisk的命令时，悍然提示：bash:command not found。那么首先就要考虑root 的$PATH里是否已经包含了这些环境变量。    
可以查看PATH，echo $PATH，如果是：PATH=$PATH:$HOME/bin则需要添加成如下：PATH=$PATH:$HOME/bin:/sbin:/usr/bin:/usr/sbin
```









