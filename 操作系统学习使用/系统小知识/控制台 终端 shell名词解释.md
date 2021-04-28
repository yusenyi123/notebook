# 控制台 终端 shell名词解释

## 参考：

https://www.eet-china.com/mp/a46011.html

https://www.jianshu.com/p/ba84970508e3

https://www.zhihu.com/question/21711307/answer/118788917

https://www.cnblogs.com/sparkdev/p/11460821.html

https://www.cnblogs.com/MrVolleyball/p/10024540.html

https://www.shuzhiduo.com/A/nAJv4Axndr/

https://www.junmajinlong.com/shell/fd_duplicate/

https://www.cnblogs.com/sparkdev/p/11605804.html

https://program-think.medium.com/%E6%89%AB%E7%9B%B2-linux-unix-%E5%91%BD%E4%BB%A4%E8%A1%8C-%E4%BB%8E-%E7%94%B5%E4%BC%A0%E6%89%93%E5%AD%97%E6%9C%BA-%E8%81%8A%E5%88%B0-shell-%E8%84%9A%E6%9C%AC%E7%BC%96%E7%A8%8B-162a133fa651

https://zdyxry.github.io/2020/04/25/%E5%9C%A8%E7%BB%88%E7%AB%AF%E8%BE%93%E5%85%A5%E5%91%BD%E4%BB%A4%E5%90%8E%E7%B3%BB%E7%BB%9F%E5%81%9A%E4%BA%86%E4%BB%80%E4%B9%88/

https://blog.csdn.net/w1857518575/article/details/82670522

https://segmentfault.com/a/1190000016129862



## 终端(terminal  一个广泛的概念)

### 终端历史理解终端

对于什么是终端，我们先看一个定义。可以看出终端就是一个输入输出设备，简单的可以理解为鼠标，键盘和显示器。但是这个好像跟Linux中终端的概念有些出入，下面听我来娓娓道来。

> a combination of a keyboard and output device (such as a video display unit) by which data can be entered into or output from a computer or electronic communications system.

但如果想理解了解什么是终端，还需要从“远古”时期说起。在1970年之前，那个时候还没有个人电脑。那个使用只有**大型机和小型机**，也就是衣柜那么大的计算机。当时比较著名的计算机如DPD-7和GE-45等。

当时Ken Thompson和Dennis Ritchie（就是下图中的两位大神）负责在DPD-7上面开发一个新的操作系统，没错，就是UNIX操作系统。为了提高计算机的使用效率，他们打算让这个操作系统支持多个用户同时使用这台计算机。



![img](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210422085922.png)

但是，当时的显示器是一个非常贵的设备，不太可能每个人都有一个显示器。因此两个人想出了一个变通的方法。他们选择了便宜的**电传打字机**来做终端设备。这个电传打字机（TeleType）就是ASR33，就是下图这个设备。



![img](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210422085918.png)

这个设备通过键盘将信息输入计算机当中，而计算机的输出则是通过上面的纸打印出来。这样UNIX就成为世界上第一个支持多用户的操作系统，而ASR33则成为第一个Unix终端。后来，缩写TTY也就是用来表示Unix或者Linux终端了。

随着技术的发展和硬件价格的不断降低，终端也变得越来越先进和便宜。1970年，DEC发明了VT05视频终端。就是下面这个东东，可以看出她有个小显示器。也越来越像现在的键盘显示器了。



![img](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210422085900.png)

聊到这里我们知道了**，所谓终端，其实就是一个物理设备，也就是计算机的输入输出设备**。

### 总结:终端，其实就是一个物理设备，也就是计算机的输入输出设备



## tty(teletypewriter  电传打字机 )是终端的一种，最初的终端设备就是tty,因为历史原因tty也被沿用到现在作为linux中虚拟终端的名字







## 控制台(console)  终端的一种，直接集成在主机上的终端

在上个世纪70年代，终端是通过线缆连接在主机上的。同时，在主机上还有一种特殊终端，它是直接集成在主机上的。这个特殊的终端被称为控制台。这个终端的特点是只能被管理员使用。每一个计算机只有一个控制台，它在外观上与普通终端并没有太大的差异，但最重要的是控制台可以做一些普通终端不能做的事情。



比如当操作系统出现启动失败的时候，它会打印一些信息到控制台上，但终端并不会收到该信息。另外，当操作系统以单用户模式启动的时候，我们就只能通过使用控制台来登录了。这个时候其它终端是没有权限登录的。



![img](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210422090229.png)



![image-20210422090508352](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210422090508.png)









## 虚拟终端(软件仿真的终端)

前面我们了解到终端实际上是硬件设备，但是前面我们在Ubuntu上可以用菜单打开一个终端（Open Terminal）。其实，我们打开的这个窗口也是一个终端，我们称这个终端为**终端模拟器**，它是用软件的方式来模拟一个终端设备。有的时候我们又称它为**虚拟终端**。





### 1.物理的terminal

在历史上的客户机/服务器连接中，客户机是一种独立的硬件设备（包含键盘和显示屏）。一个客户机就作为一个terminal，用以连接、访问远程的服务器。

客户机与服务器的通信协议：

Telnet
SSH
客户机与服务器之间的文件传输的方式：

FTP
SCP



常见的terminal就是DEC公司的VT(Video-Terminal)系列，如：VT102, VT220等。逐渐，DEC的VT100/VT102成为访问各种计算机系统的标准的termianl type。

https://en.wikipedia.org/wiki/VT100



#### 常见的terminal type(终端设备类型)



还有：vt100, vt102, xterm, pterm, wyse60, ansi, ansi-bbs等。







### 2.软件仿真的terminal

随着PC的出现，独立硬件的terminal过时了。在PC上，往往由软件仿真terminal以连接访问服务器。于是，VT102 terminal （VT102  终端）就变成了VT102 emulator (VT102 仿真程序 )。

常见的terminal仿真软件有Putty等。而且，一个仿真软件可以仿真多种不同类型的terminal。

对于Linux服务器来说，其所支持的terminal type （终端类型），可以通过如下命令查看：

```
ll /usr/share/terminfo/v/
```





### 在linux中一切皆文件的概念，默认会有7个虚拟终端,每个虚拟终端都会对应一个设备文件









## shell(Shell是一个应用程序，它实现了用户对操作系统访问的接口)

Shell其实就是一个应用程序，它实现了用户对操作系统访问的接口。比如我们常见的管理文件，用户和网络资源等等，都是通过Shell来完成的。



Shell是一个应用程序，同时它又有很多具体的实现，比较常见的包括Bash、Zsh、 Csh和Ksh等等。

![img](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210422091529.png)

我们前面了解到终端是个物理设备，它被用户用来输入和现实信息，而目前我们使用的虚拟终端则是对物理设备的模拟。Shell则是用来执行用户命令的。这样我们现在就很容易理解终端和Shell的关系。



如果我们通过桌面版打开一个虚拟终端的话，那么终端和Shell的关系如下图所示。

![img](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210422091532.png)



如果我们不是通过本地的设备连接的，而是通过网络来访问计算机的话，那么其关系如下图所示。可以看出，这里面有个pty的组件起了比较关键的作用，它建立了两者之间的关联。

![img](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210422091534.png)





### 硬件终端工作原理

从历史上看，终端刚开始就是终端机，配有打印机，键盘，带有一个串口，通过串口传送数据到主机端，然后主机处理完交给终端打印出来。电传打字机(teletype)可以被看作是这类设备的统称，因此终端也被简称为 TTY(teletype 的缩写)。
如下图所示(下图来自互联网)：

![img](https://img2018.cnblogs.com/blog/952033/201909/952033-20190904181510246-1896309756.png)

**UART (Universal Asynchronous Receiver/Transmitter 通用异步收发传输器) 驱动**
如上图所示，物理终端通过电缆连接到计算机上的 UART(通用异步接收器和发射器)。操作系统中有一个 UART 驱动程序用于管理字节的物理传输。

**行规范**
上图中内核中的 Line discipline(行规范)用来提供一个编辑缓冲区和一些基本的编辑命令(退格，清除单个单词，清除行，重新打印)，主要用来支持用户在输入时的行为(比如输错了，需要退格)。

**TTY 驱动**
TTY 驱动用来进行会话管理，并且处理各种终端设备。

UART 驱动、行规范和 TTY 驱动都位于内核中，它们的一端是终端设备，另一端是用户进程。因为在 Linux 下所有的设备都是文件，所以它们三个加在一起被称为 "TTY 设备"，即我们常说的 TTY。



### 从软件仿真终端到伪终端

后来的终端慢慢演变成了键盘 + 显示器。如果我们要把内容输出到显示器，只要把这些内容写入到显示器对应的 TTY 设备就可以了，然后由 TTY 层负责匹配合适的驱动完成输出，这也是 Linux 控制台的工作原理(下图来自互联网)：

![img](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210422095610.png)



上图中，TTY 驱动和行规范的行为与前面的示例类似，但不再有 UART 或物理终端。相反，软件仿真出视频终端，并最终被渲染到 VGA 显示器。注意，这里出现了软件仿真终端，它们是运行在内核态的。显示器和 vSphere Client "Virtual Machine Console" 中的 tty1-tty6 都是**软件仿真终端**：

![img](https://img2018.cnblogs.com/blog/952033/201909/952033-20190904181715828-649376016.png)



### shell和虚拟终端的关系

```
在linux中虚拟终端也就是通过程序模拟处理的终端，当我们打开一个终端的时候就开始运行终端程序，获得一个界面，这个界面就是模拟的物理终端的效果，我们在界面上进行输入就可以看做是对一台物理终端进行了输入
当我们在界面中输入的时候，即虚拟终端接受到了我们的输入，在linux中虚拟终端对应一个设备文件，即我们对这个设备文件进行了input操作，那么就会调用相应的该设备文件对应的驱动程序

我的猜测是
这个设备文件的驱动程序中的代码是将输入的参数获得后，执行shell程序，shell程序
```







/dev/tty1-/dev/tty6 是这些仿真终端在文件系统中的表示，程序通过对这些文件的读写实现对仿真终端的读写。

如果我们在用户空间也进行终端仿真，情况会变得更加灵活，下图是 xterm 及其克隆的工作方式(下图来自互联网)：

![img](https://img2018.cnblogs.com/blog/952033/201909/952033-20190904181756676-1523246503.png)

为了便于将终端仿真移入用户空间，同时仍保持 TTY 子系统(TTY 子系统指 TTY 驱动和行规范)的完整，伪终端被发明了出来(pseudo terminal 或 pty)。伪终端在内核中分为两部分，分别是 master side 和 在 TTY 驱动中实现的 slave side。注意上图中的 xterm，这是一个运行在用户态的终端仿真程序，比如 Ubuntu Desktop 中的 GNOME Terminal：

![img](https://img2018.cnblogs.com/blog/952033/201909/952033-20190904181833526-1105253964.png)





### 终端设备接入系统，系统为终端设备设置对应文件描述符的操作方式（0,1,2）

在Linux系统中，一切设备都看作文件。而每打开一个文件，就有一个代表该打开文件的文件描述符。程序启动时默认打开三个I/O设备文件：标准输入文件stdin，标准输出文件stdout，标准错误输出文件stderr，分别得到文件描述符 0, 1, 2。

#### 系统为每一个接入终端都配备默认的文件描述符

```
[root@localhost pts]# ll /dev/std*
lrwxrwxrwx. 1 root root 15 4月  22 10:48 /dev/stderr -> /proc/self/fd/2
lrwxrwxrwx. 1 root root 15 4月  22 10:48 /dev/stdin -> /proc/self/fd/0
lrwxrwxrwx. 1 root root 15 4月  22 10:48 /dev/stdout -> /proc/self/fd/1
[root@localhost pts]# 
```

#### 系统为pts/0接入终端默认绑定的文件描述符

```
[root@localhost pts]# ll /proc/self/fd
总用量 0
lrwx------. 1 root root 64 4月  22 11:20 0 -> /dev/pts/0
lrwx------. 1 root root 64 4月  22 11:20 1 -> /dev/pts/0
lrwx------. 1 root root 64 4月  22 11:20 2 -> /dev/pts/0
lr-x------. 1 root root 64 4月  22 11:20 3 -> /proc/4635/fd
[root@localhost pts]# 

```

#### 得出系统设备、特殊文件描述符、终端的链接关系为

```
/dev/stderr   ->  /proc/self/fd/2  -> /dev/pts/0
/dev/stdin    ->  /proc/self/fd/0  -> /dev/pts/0
/dev/stdout   ->  /proc/self/fd/1  -> /dev/pts/0 
```











## Linux系统中的tty、pty和pts

前面我们从概念层面对终端、控制台和shell等进行了介绍。但是这些概念在Linux操作系统中是怎样的呢？它们之间的关系又是怎样的呢？

```
 
pty（ Pseudo Terminal 虚拟终端):
但是如果我们远程telnet到主机或使用xterm时不也需要一个终端交互么？是的，这就是虚拟终端pty(pseudo-tty)。

pts/ptmx(pts/ptmx结合使用，进而实现pty): 在Xwindows模式下的伪终端。
pts(pseudo-terminal slave)是pty的实现方法，与ptmx(pseudo-terminal master)配合使用实现pty。
```

### TTY和PTS的区别

对用户空间的程序来说，他们没有区别，都是一样的；从内核里面来看，pts的另一端连接的是ptmx，而tty的另一端连接的是内核的终端模拟器，ptmx和终端模拟器都只是负责维护会话和转发数据包；再看看ptmx和内核终端模拟器的另一端，ptmx的另一端连接的是用户空间的应用程序，如sshd、tmux等，而内核终端模拟器的另一端连接的是具体的硬件，如键盘和显示器。

![image-20210422105432983](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210422105433.png)



前面已经介绍过**tty**，它是一个终端，也就是一个输入输出设备的集合。而目前在Linux中都是通过虚拟终端来与计算机交互的，因此在Linux中tty其实就是虚拟终端，可以将其理解为一个软件。如果我们同时按住Ctrl+Alt+F5就可以切换到虚拟终端5，具体如下。



![img](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210422094036.png)

在Linux操作系统中，软件的整体架构要复杂一些，这是因为Linux不仅仅要支持虚拟终端，还有能够支持键盘显示器的物理外围设备，还要支持通过telnet或者ssh等网络的形式的连接。如下图给出了一个完整的示例。



![img](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210422094032.png)

为了支持不同类型的接入方式，在Linux实现了一个伪终端的概念，也就是pty。其中p是pseudo的缩写。



伪终端分为两部分，如上图所示，包括master和slave两部分。其中master实现了对不同接入方式的适配，它实现对来自不同设备或者软件消息的解析，将结果传输给slave；而slave端其实就是一个虚拟终端，它实现了与shell的交互，对于shell来说，pts(pseudo-terminal slave)是一个终端设备。



可能还是不太好理解，我们举一个具体的例子，比如telnet实现对远程计算机的管理，其实在客户端就是发送的各种字符串，通过网络发送给telnet守护进程，然后telnet守护进程调用master的功能实现解析。



今天我们主要从概念和架构的层面介绍了终端、控制台和Shell等概念，并介绍了Linux操作系统中常见的诸如tty、pty和pts等名称。后面我们以一个具体的实例来让大家更加清楚的理解上述架构。



```
历史发展的角度理解名词
控制台（console）
在早期的电脑上，往往具有带有大量开关和指示灯的面板，可以对电脑进行一些底层的操作（可以拨动开关，调节寄存器等），这个面板就叫做Console。其概念来自于管风琴的控制台。一台电脑通常只能有一个Console，很多时候是电脑主机的一部分，和CPU共享一个机柜。



在早期，计算机由于比较昂贵，一个机构里的一台主机都是很多人共享的，前提是每个人都有一个键盘和显示器，而这些键盘和显示器就叫称为终端。不过，有一种终端比较特殊，那就是主机自带的键盘和显示器，不需要连线的，它主要是给管理员管理系统的，它叫做控制台。
控制台也可以理解成一种特殊的终端，那就是主机自带的键盘和显示器，不需要连线的，它主要是给管理员管理系统的，它叫做控制台。


特点：主机的一部分


终端（terminal  一个广泛的概念）
终端就是一个输入输出设备


tty（teletypewriter  电传打字机  终端的一种，最初的终端
```





### 图形化界面中对应终端的是pty

### 远程ssh连接对应的终端是pty



### 本地非图形化界面登录对应的终端是tty







```shell
ubuntu@ubuntu-virtual-machine:~/桌面$ cd
ubuntu@ubuntu-virtual-machine:~$ cd /dev
ubuntu@ubuntu-virtual-machine:/dev$ ls
agpgart          loop1         null      tty16  tty45      ttyS15   vcs2
autofs           loop2         nvram     tty17  tty46      ttyS16   vcs3
block            loop3         port      tty18  tty47      ttyS17   vcs4
bsg              loop4         ppp       tty19  tty48      ttyS18   vcs5
btrfs-control    loop5         psaux     tty2   tty49      ttyS19   vcs6
bus              loop6         ptmx      tty20  tty5       ttyS2    vcsa
cdrom            loop7         pts       tty21  tty50      ttyS20   vcsa1
cdrw             loop8         random    tty22  tty51      ttyS21   vcsa2
char             loop9         rfkill    tty23  tty52      ttyS22   vcsa3
console          loop-control  rtc       tty24  tty53      ttyS23   vcsa4
core             mapper        rtc0      tty25  tty54      ttyS24   vcsa5
cpu_dma_latency  mcelog        sda       tty26  tty55      ttyS25   vcsa6
cuse             mem           sda1      tty27  tty56      ttyS26   vcsu
disk             midi          sda2      tty28  tty57      ttyS27   vcsu1
dmmidi           mqueue        sg0       tty29  tty58      ttyS28   vcsu2
dri              nbd0          sg1       tty3   tty59      ttyS29   vcsu3
dvd              nbd1          shm       tty30  tty6       ttyS3    vcsu4
ecryptfs         nbd10         snapshot  tty31  tty60      ttyS30   vcsu5
fb0              nbd11         snd       tty32  tty61      ttyS31   vcsu6
fd               nbd12         sr0       tty33  tty62      ttyS4    vfio
full             nbd13         stderr    tty34  tty63      ttyS5    vga_arbiter
fuse             nbd14         stdin     tty35  tty7       ttyS6    vhci
hidraw0          nbd15         stdout    tty36  tty8       ttyS7    vhost-net
hpet             nbd2          tty       tty37  tty9       ttyS8    vhost-vsock
hugepages        nbd3          tty0      tty38  ttyprintk  ttyS9    vmci
hwrng            nbd4          tty1      tty39  ttyS0      udmabuf  vsock
initctl          nbd5          tty10     tty4   ttyS1      uhid     zero
input            nbd6          tty11     tty40  ttyS10     uinput   zfs
kmsg             nbd7          tty12     tty41  ttyS11     urandom
lightnvm         nbd8          tty13     tty42  ttyS12     userio
log              nbd9          tty14     tty43  ttyS13     vcs
loop0            net           tty15     tty44  ttyS14     vcs1
ubuntu@ubuntu-virtual-machine:/dev$ tty
/dev/pts/0
ubuntu@ubuntu-virtual-machine:/dev$ 
```



```shell
ubuntu@ubuntu-virtual-machine:/dev$ ls -ahl
总用量 4.0K
drwxr-xr-x  18 root   root        4.4K 4月  22 09:47 .
drwxr-xr-x  20 root   root        4.0K 8月  11  2020 ..
crw-------   1 root   root     10, 175 4月  22 09:49 agpgart
crw-r--r--   1 root   root     10, 235 4月  22 09:49 autofs
drwxr-xr-x   2 root   root         640 4月  22 09:47 block
drwxr-xr-x   2 root   root          80 4月  22 09:46 bsg
crw-------   1 root   root     10, 234 4月  22 09:46 btrfs-control
drwxr-xr-x   3 root   root          60 4月  22 09:46 bus
lrwxrwxrwx   1 root   root           3 4月  22 09:49 cdrom -> sr0
lrwxrwxrwx   1 root   root           3 4月  22 09:49 cdrw -> sr0
drwxr-xr-x   2 root   root        3.6K 4月  22 09:47 char
crw--w----   1 root   tty       5,   1 4月  22 09:49 console
lrwxrwxrwx   1 root   root          11 4月  22 09:46 core -> /proc/kcore
crw-------   1 root   root     10,  59 4月  22 09:49 cpu_dma_latency
crw-------   1 root   root     10, 203 4月  22 09:46 cuse
drwxr-xr-x   7 root   root         140 4月  22 09:46 disk
crw-rw----+  1 root   audio    14,   9 4月  22 09:49 dmmidi
drwxr-xr-x   3 root   root         100 4月  22 09:46 dri
lrwxrwxrwx   1 root   root           3 4月  22 09:49 dvd -> sr0
crw-------   1 root   root     10,  62 4月  22 09:49 ecryptfs
crw-rw----   1 root   video    29,   0 4月  22 09:49 fb0
lrwxrwxrwx   1 root   root          13 4月  22 09:46 fd -> /proc/self/fd
crw-rw-rw-   1 root   root      1,   7 4月  22 09:49 full
crw-rw-rw-   1 root   root     10, 229 4月  22 09:49 fuse
crw-------   1 root   root    242,   0 4月  22 09:49 hidraw0
crw-------   1 root   root     10, 228 4月  22 09:49 hpet
drwxr-xr-x   2 root   root           0 4月  22 09:46 hugepages
crw-------   1 root   root     10, 183 4月  22 09:49 hwrng
lrwxrwxrwx   1 root   root          12 4月  22 09:46 initctl -> /run/initctl
drwxr-xr-x   4 root   root         260 4月  22 09:46 input
crw-r--r--   1 root   root      1,  11 4月  22 09:49 kmsg
drwxr-xr-x   2 root   root          60 4月  22 09:46 lightnvm
lrwxrwxrwx   1 root   root          28 4月  22 09:46 log -> /run/systemd/journal/dev-log
brw-rw----   1 root   disk      7,   0 4月  22 09:49 loop0
brw-rw----   1 root   disk      7,   1 4月  22 09:49 loop1
brw-rw----   1 root   disk      7,   2 4月  22 09:49 loop2
brw-rw----   1 root   disk      7,   3 4月  22 09:49 loop3
brw-rw----   1 root   disk      7,   4 4月  22 09:49 loop4
brw-rw----   1 root   disk      7,   5 4月  22 09:49 loop5
brw-rw----   1 root   disk      7,   6 4月  22 09:49 loop6
brw-rw----   1 root   disk      7,   7 4月  22 09:49 loop7
brw-rw----   1 root   disk      7,   8 4月  22 09:49 loop8
brw-rw----   1 root   disk      7,   9 4月  22 09:49 loop9
crw-rw----   1 root   disk     10, 237 4月  22 09:49 loop-control
drwxr-xr-x   2 root   root          60 4月  22 09:46 mapper
crw-------   1 root   root     10, 227 4月  22 09:49 mcelog
crw-r-----   1 root   kmem      1,   1 4月  22 09:49 mem
crw-rw----+  1 root   audio    14,   2 4月  22 09:49 midi
drwxrwxrwt   2 root   root          40 4月  22 09:46 mqueue
brw-rw----   1 root   disk     43,   0 4月  22 09:49 nbd0
brw-rw----   1 root   disk     43,  32 4月  22 09:49 nbd1
brw-rw----   1 root   disk     43, 320 4月  22 09:49 nbd10
brw-rw----   1 root   disk     43, 352 4月  22 09:49 nbd11
brw-rw----   1 root   disk     43, 384 4月  22 09:49 nbd12
brw-rw----   1 root   disk     43, 416 4月  22 09:49 nbd13
brw-rw----   1 root   disk     43, 448 4月  22 09:49 nbd14
brw-rw----   1 root   disk     43, 480 4月  22 09:49 nbd15
brw-rw----   1 root   disk     43,  64 4月  22 09:49 nbd2
brw-rw----   1 root   disk     43,  96 4月  22 09:49 nbd3
brw-rw----   1 root   disk     43, 128 4月  22 09:49 nbd4
brw-rw----   1 root   disk     43, 160 4月  22 09:49 nbd5
brw-rw----   1 root   disk     43, 192 4月  22 09:49 nbd6
brw-rw----   1 root   disk     43, 224 4月  22 09:49 nbd7
brw-rw----   1 root   disk     43, 256 4月  22 09:49 nbd8
brw-rw----   1 root   disk     43, 288 4月  22 09:49 nbd9
drwxr-xr-x   2 root   root          60 4月  22 09:46 net
crw-rw-rw-   1 root   root      1,   3 4月  22 09:49 null
crw-------   1 root   root     10, 144 4月  22 09:46 nvram
crw-r-----   1 root   kmem      1,   4 4月  22 09:49 port
crw-------   1 root   root    108,   0 4月  22 09:49 ppp
crw-------   1 root   root     10,   1 4月  22 09:49 psaux
crw-rw-rw-   1 root   tty       5,   2 4月  22 09:50 ptmx
drwxr-xr-x   2 root   root           0 4月  22 09:46 pts
crw-rw-rw-   1 root   root      1,   8 4月  22 09:49 random
crw-rw-r--+  1 root   root     10, 242 4月  22 09:49 rfkill
lrwxrwxrwx   1 root   root           4 4月  22 09:49 rtc -> rtc0
crw-------   1 root   root    249,   0 4月  22 09:49 rtc0
brw-rw----   1 root   disk      8,   0 4月  22 09:49 sda
brw-rw----   1 root   disk      8,   1 4月  22 09:49 sda1
brw-rw----   1 root   disk      8,   2 4月  22 09:49 sda2
crw-rw----   1 root   disk     21,   0 4月  22 09:49 sg0
crw-rw----+  1 root   cdrom    21,   1 4月  22 09:49 sg1
drwxrwxrwt   2 root   root          40 4月  22 09:46 shm
crw-------   1 root   root     10, 231 4月  22 09:49 snapshot
drwxr-xr-x   3 root   root         200 4月  22 09:46 snd
brw-rw----+  1 root   cdrom    11,   0 4月  22 09:49 sr0
lrwxrwxrwx   1 root   root          15 4月  22 09:46 stderr -> /proc/self/fd/2
lrwxrwxrwx   1 root   root          15 4月  22 09:46 stdin -> /proc/self/fd/0
lrwxrwxrwx   1 root   root          15 4月  22 09:46 stdout -> /proc/self/fd/1
crw-rw-rw-   1 root   tty       5,   0 4月  22 09:49 tty
crw--w----   1 root   tty       4,   0 4月  22 09:49 tty0
crw--w----   1 root   tty       4,   1 4月  22 09:49 tty1
crw--w----   1 root   tty       4,  10 4月  22 09:49 tty10
crw--w----   1 root   tty       4,  11 4月  22 09:49 tty11
crw--w----   1 root   tty       4,  12 4月  22 09:49 tty12
crw--w----   1 root   tty       4,  13 4月  22 09:49 tty13
crw--w----   1 root   tty       4,  14 4月  22 09:49 tty14
crw--w----   1 root   tty       4,  15 4月  22 09:49 tty15
crw--w----   1 root   tty       4,  16 4月  22 09:49 tty16
crw--w----   1 root   tty       4,  17 4月  22 09:49 tty17
crw--w----   1 root   tty       4,  18 4月  22 09:49 tty18
crw--w----   1 root   tty       4,  19 4月  22 09:49 tty19
crw--w----   1 ubuntu tty       4,   2 4月  22 09:49 tty2
crw--w----   1 root   tty       4,  20 4月  22 09:49 tty20
crw--w----   1 root   tty       4,  21 4月  22 09:49 tty21
crw--w----   1 root   tty       4,  22 4月  22 09:49 tty22
crw--w----   1 root   tty       4,  23 4月  22 09:49 tty23
crw--w----   1 root   tty       4,  24 4月  22 09:49 tty24
crw--w----   1 root   tty       4,  25 4月  22 09:49 tty25
crw--w----   1 root   tty       4,  26 4月  22 09:49 tty26
crw--w----   1 root   tty       4,  27 4月  22 09:49 tty27
crw--w----   1 root   tty       4,  28 4月  22 09:49 tty28
crw--w----   1 root   tty       4,  29 4月  22 09:49 tty29
crw--w----   1 root   tty       4,   3 4月  22 09:49 tty3
crw--w----   1 root   tty       4,  30 4月  22 09:49 tty30
crw--w----   1 root   tty       4,  31 4月  22 09:49 tty31
crw--w----   1 root   tty       4,  32 4月  22 09:49 tty32
crw--w----   1 root   tty       4,  33 4月  22 09:49 tty33
crw--w----   1 root   tty       4,  34 4月  22 09:49 tty34
crw--w----   1 root   tty       4,  35 4月  22 09:49 tty35
crw--w----   1 root   tty       4,  36 4月  22 09:49 tty36
crw--w----   1 root   tty       4,  37 4月  22 09:49 tty37
crw--w----   1 root   tty       4,  38 4月  22 09:49 tty38
crw--w----   1 root   tty       4,  39 4月  22 09:49 tty39
crw--w----   1 root   tty       4,   4 4月  22 09:49 tty4
crw--w----   1 root   tty       4,  40 4月  22 09:49 tty40
crw--w----   1 root   tty       4,  41 4月  22 09:49 tty41
crw--w----   1 root   tty       4,  42 4月  22 09:49 tty42
crw--w----   1 root   tty       4,  43 4月  22 09:49 tty43
crw--w----   1 root   tty       4,  44 4月  22 09:49 tty44
crw--w----   1 root   tty       4,  45 4月  22 09:49 tty45
crw--w----   1 root   tty       4,  46 4月  22 09:49 tty46
crw--w----   1 root   tty       4,  47 4月  22 09:49 tty47
crw--w----   1 root   tty       4,  48 4月  22 09:49 tty48
crw--w----   1 root   tty       4,  49 4月  22 09:49 tty49
crw--w----   1 root   tty       4,   5 4月  22 09:49 tty5
crw--w----   1 root   tty       4,  50 4月  22 09:49 tty50
crw--w----   1 root   tty       4,  51 4月  22 09:49 tty51
crw--w----   1 root   tty       4,  52 4月  22 09:49 tty52
crw--w----   1 root   tty       4,  53 4月  22 09:49 tty53
crw--w----   1 root   tty       4,  54 4月  22 09:49 tty54
crw--w----   1 root   tty       4,  55 4月  22 09:49 tty55
crw--w----   1 root   tty       4,  56 4月  22 09:49 tty56
crw--w----   1 root   tty       4,  57 4月  22 09:49 tty57
crw--w----   1 root   tty       4,  58 4月  22 09:49 tty58
crw--w----   1 root   tty       4,  59 4月  22 09:49 tty59
crw--w----   1 root   tty       4,   6 4月  22 09:49 tty6
crw--w----   1 root   tty       4,  60 4月  22 09:49 tty60
crw--w----   1 root   tty       4,  61 4月  22 09:49 tty61
crw--w----   1 root   tty       4,  62 4月  22 09:49 tty62
crw--w----   1 root   tty       4,  63 4月  22 09:49 tty63
crw--w----   1 root   tty       4,   7 4月  22 09:49 tty7
crw--w----   1 root   tty       4,   8 4月  22 09:49 tty8
crw--w----   1 root   tty       4,   9 4月  22 09:49 tty9
crw-------   1 root   root      5,   3 4月  22 09:49 ttyprintk
crw-rw----   1 root   dialout   4,  64 4月  22 09:49 ttyS0
crw-rw----   1 root   dialout   4,  65 4月  22 09:49 ttyS1
crw-rw----   1 root   dialout   4,  74 4月  22 09:49 ttyS10
crw-rw----   1 root   dialout   4,  75 4月  22 09:49 ttyS11
crw-rw----   1 root   dialout   4,  76 4月  22 09:49 ttyS12
crw-rw----   1 root   dialout   4,  77 4月  22 09:49 ttyS13
crw-rw----   1 root   dialout   4,  78 4月  22 09:49 ttyS14
crw-rw----   1 root   dialout   4,  79 4月  22 09:49 ttyS15
crw-rw----   1 root   dialout   4,  80 4月  22 09:49 ttyS16
crw-rw----   1 root   dialout   4,  81 4月  22 09:49 ttyS17
crw-rw----   1 root   dialout   4,  82 4月  22 09:49 ttyS18
crw-rw----   1 root   dialout   4,  83 4月  22 09:49 ttyS19
crw-rw----   1 root   dialout   4,  66 4月  22 09:49 ttyS2
crw-rw----   1 root   dialout   4,  84 4月  22 09:49 ttyS20
crw-rw----   1 root   dialout   4,  85 4月  22 09:49 ttyS21
crw-rw----   1 root   dialout   4,  86 4月  22 09:49 ttyS22
crw-rw----   1 root   dialout   4,  87 4月  22 09:49 ttyS23
crw-rw----   1 root   dialout   4,  88 4月  22 09:49 ttyS24
crw-rw----   1 root   dialout   4,  89 4月  22 09:49 ttyS25
crw-rw----   1 root   dialout   4,  90 4月  22 09:49 ttyS26
crw-rw----   1 root   dialout   4,  91 4月  22 09:49 ttyS27
crw-rw----   1 root   dialout   4,  92 4月  22 09:49 ttyS28
crw-rw----   1 root   dialout   4,  93 4月  22 09:49 ttyS29
crw-rw----   1 root   dialout   4,  67 4月  22 09:49 ttyS3
crw-rw----   1 root   dialout   4,  94 4月  22 09:49 ttyS30
crw-rw----   1 root   dialout   4,  95 4月  22 09:49 ttyS31
crw-rw----   1 root   dialout   4,  68 4月  22 09:49 ttyS4
crw-rw----   1 root   dialout   4,  69 4月  22 09:49 ttyS5
crw-rw----   1 root   dialout   4,  70 4月  22 09:49 ttyS6
crw-rw----   1 root   dialout   4,  71 4月  22 09:49 ttyS7
crw-rw----   1 root   dialout   4,  72 4月  22 09:49 ttyS8
crw-rw----   1 root   dialout   4,  73 4月  22 09:49 ttyS9
crw-rw----   1 root   kvm      10,  60 4月  22 09:49 udmabuf
crw-------   1 root   root     10, 239 4月  22 09:46 uhid
crw-------   1 root   root     10, 223 4月  22 09:49 uinput
crw-rw-rw-   1 root   root      1,   9 4月  22 09:49 urandom
crw-------   1 root   root     10, 240 4月  22 09:46 userio
crw-rw----   1 root   tty       7,   0 4月  22 09:49 vcs
crw-rw----   1 root   tty       7,   1 4月  22 09:49 vcs1
crw-rw----   1 root   tty       7,   2 4月  22 09:49 vcs2
crw-rw----   1 root   tty       7,   3 4月  22 09:49 vcs3
crw-rw----   1 root   tty       7,   4 4月  22 09:49 vcs4
crw-rw----   1 root   tty       7,   5 4月  22 09:49 vcs5
crw-rw----   1 root   tty       7,   6 4月  22 09:49 vcs6
crw-rw----   1 root   tty       7, 128 4月  22 09:49 vcsa
crw-rw----   1 root   tty       7, 129 4月  22 09:49 vcsa1
crw-rw----   1 root   tty       7, 130 4月  22 09:49 vcsa2
crw-rw----   1 root   tty       7, 131 4月  22 09:49 vcsa3
crw-rw----   1 root   tty       7, 132 4月  22 09:49 vcsa4
crw-rw----   1 root   tty       7, 133 4月  22 09:49 vcsa5
crw-rw----   1 root   tty       7, 134 4月  22 09:49 vcsa6
crw-rw----   1 root   tty       7,  64 4月  22 09:49 vcsu
crw-rw----   1 root   tty       7,  65 4月  22 09:49 vcsu1
crw-rw----   1 root   tty       7,  66 4月  22 09:49 vcsu2
crw-rw----   1 root   tty       7,  67 4月  22 09:49 vcsu3
crw-rw----   1 root   tty       7,  68 4月  22 09:49 vcsu4
crw-rw----   1 root   tty       7,  69 4月  22 09:49 vcsu5
crw-rw----   1 root   tty       7,  70 4月  22 09:49 vcsu6
drwxr-xr-x   2 root   root          60 4月  22 09:46 vfio
crw-------   1 root   root     10,  63 4月  22 09:49 vga_arbiter
crw-------   1 root   root     10, 137 4月  22 09:46 vhci
crw-------   1 root   root     10, 238 4月  22 09:46 vhost-net
crw-------   1 root   root     10, 241 4月  22 09:46 vhost-vsock
crw-------   1 root   root     10,  58 4月  22 09:49 vmci
crw-------   1 root   root     10,  57 4月  22 09:49 vsock
crw-rw-rw-   1 root   root      1,   5 4月  22 09:49 zero
crw-------   1 root   root     10, 249 4月  22 09:46 zfs
ubuntu@ubuntu-virtual-machine:/dev$ 
```

```shell
ubuntu@ubuntu-virtual-machine:/dev$ cd pts/
ubuntu@ubuntu-virtual-machine:/dev/pts$ ls -ahl
总用量 0
drwxr-xr-x  2 root   root      0 4月  22 09:46 .
drwxr-xr-x 18 root   root   4.4K 4月  22 09:47 ..
crw--w----  1 ubuntu tty  136, 0 4月  22 09:52 0
c---------  1 root   root   5, 2 4月  22 09:46 ptmx
ubuntu@ubuntu-virtual-machine:/dev/pts$ 
```





## unix98实现的sshd pty（主要分为登陆阶段和执行命令阶段）





### 登陆阶段

/dev/ptmx文件属性, /dev/pts/0 文件属性

```
crw-rw-rw-   1 root   tty       5,   2 4月  22 09:50 ptmx
ubuntu@ubuntu-virtual-machine:/dev/pts$ ls -ahl
总用量 0
drwxr-xr-x  2 root   root      0 4月  22 09:46 .
drwxr-xr-x 18 root   root   4.4K 4月  22 09:47 ..
crw--w----  1 ubuntu tty  136, 0 4月  22 09:52 0
c---------  1 root   root   5, 2 4月  22 09:46 ptmx
```





（1）当进程ssh client请求与sshd建立登陆连接的时候，经过TCP握手以及tls握手之后，确认是一个合法的请求，sshd会fork()一个子进程出来专门服务于这条连接，这里是3126进程

```
[root@localhost ~]# ps -ef | grep sshd
root       894     1  0 Nov25 ?        00:00:00 /usr/sbin/sshd -D
root      3126   894  0 Nov25 ?        00:00:00 sshd: root@pts/0
```

（2）可以看出sshd`894`进程创建了一个子进程`3126`用来处理这条TCP连接。子进程`3126`对`/dev/ptmx`设备文件调用open()操作即打开文件，那么这个进程就会在内存的该进程打开文件表中添加关于`/dev/ptmx`文件的文件描述符，子进程`3126`在`/dev/pts`下创建了一个字符设备文件`/dev/pts/0`，将关于`/dev/ptmx`文件的文件描述符中的内容指向这个创建的字符设备`/dev/pts/0`，后续对`/dev/ptmx`进行read()，write()操作时，根据查找文件描述符上的对应关系，其实是对`/dev/pts/0`进行read(),write()操作



/dev/ptmx 是一个字符设备文件，当进程打开 /dev/ptmx 文件时，进程会同时获得一个指向 pseudoterminal master(ptm)的文件描述符和一个在 /dev/pts 目录中创建的 pseudoterminal slave(pts) 设备。通过打开 /dev/ptmx 文件获得的每个文件描述符都是一个独立的 ptm，它有自己关联的 pts，ptmx(可以认为内存中有一个 ptmx 对象)在内部会维护该文件描述符和 pts 的对应关系，对这个文件描述符的读写都会被 ptmx 转发到对应的 pts。我们可以通过 lsof 命令查看 ptmx 打开的文件描述符：

```
#查看/dev/ptmx被多个进程打开的情况
$ sudo lsof /dev/ptmx
```

![img](https://img2018.cnblogs.com/blog/952033/201909/952033-20190929083729833-1042513533.png)

```
#这里使用strace跟踪sshd主进程和它创建的子进程，然后打开另外一个shell登陆服务器
[root@localhost ~]# strace -p 894 -ff -o sshd
strace: Process 894 attached
strace: Process 3126 attached
strace: Process 3127 attached
strace: Process 3128 attached
strace: Process 3129 attached
strace: Process 3130 attached
strace: Process 3131 attached
strace: Process 3132 attached
strace: Process 3133 attached
strace: Process 3134 attached
strace: Process 3135 attached
strace: Process 3136 attached
strace: Process 3137 attached
strace: Process 3138 attached
strace: Process 3139 attached
strace: Process 3140 attached
[root@localhost ~]# grep ptmx ./sshd.*
./sshd.3126:open("/dev/ptmx", O_RDWR)               = 8

```

（3）子进程`3126`会再fork()一个子进程`3128`这个一个bash程序运行创建的进程，子进程`3128`的3个描述符（标准输入，标准输出，标准错误）都定向到/dev/pts/0 这个设备文件，并开始循环读取标准输入描述符，即根据设置的映射关系，循环读取/dev/pts/0 这个设备文件，当有其他进程向/dev/pts/0 这个设备文件写入数据时，就会被该`3128`这个bash程序的进程所接收到，这个bash进程就会解释读到的数据执行相应的命令

```
[root@localhost ~]# ps -ef | grep 3126
root      3126   894  0 03:16 ?        00:00:00 sshd: root@pts/0
root      3128  3126  0 03:16 pts/3    00:00:00 -bash
[root@localhost ~]# ls -l /proc/3128/fd
total 0
lrwx------ 1 root root 64 Nov 26 03:16 0 -> /dev/pts/0
lrwx------ 1 root root 64 Nov 26 03:16 1 -> /dev/pts/0
lrwx------ 1 root root 64 Nov 26 03:16 2 -> /dev/pts/0
lrwx------ 1 root root 64 Nov 26 03:22 255 -> /dev/pts/0
```

至此，通信流程大概是这样：

```
                         +----------------+
+------------+           |                |
| ssh client +---------->|      sshd      |
+----+-------+           |                |
     |                   +--------+-------+
     |                            |
     |                            |
     |                         fork()
     |                            |
     |                            |
     |                            v
     |                       +----+-----+     fork()    +----------+      +-----+
     +---------------------->|pid: 3126 |-------------->|pid: 3128 |----->|bash |
                             +-+--------+               +----------+      +-----+
                               |                              ^
                               |                              |
                       +-------+                              |
                +------|--------------------------------+     |
                |      |                 +-----------+  |     |
                |      v                 |           |  |     |
                |  +---------+    fd=8   +-----------+  |     |
                |  |/dev/ptmx|---------->|/dev/pts/0 |--------+
                |  +---------+           +-----------+  |
                |                        |           |  |
                |                        +-----------+  |
                +---------------------------------------+
```

### 执行命令阶段

（1）当ssh client发出一个`ls`命令，通过TCP连接将该字符串发送给`3126`进程，`3126`进程接收`ls`字符串后，对`/dev/ptmx`文件调用write()操作，这时查找该进程下`/dev/ptmx`文件的文件描述符能够得知需要向`/dev/pts/0`这个文件进行write()操作，那么该字符串就被写入到`/dev/pts/0`这个设备文件

(2)`3128`bash进程是在循环读取标准输入文件描述符的，标准输入文件描述符被定向到了`/dev/pts/0`这个设备文件，即`3128`bash进程不断在循环读取`/dev/pts/0`设备文件中的内容，那么当其他进程向写入/dev/pts/0 这个设备文件数据时，就会被该`3128`这个bash程序的进程所接收到，这个bash进程就会解释读到的数据执行相应的命令，执行命令后会将运行结果对标准输出描述符所对应的文件执行write()操作，因为bash进程的标准输出描述符也会定向到了`/dev/pts/0`设备文件，即会对`/dev/pts/0`这个设备文件调用write()操作

(3)）`3126`进程对`/dev/ptmx`文件调用read()操作，根据前面设定的对应关系，其实是对`/dev/pts/0`调用read()操作，那么`3126`进程就能得到bash进程运行命令后的结果，然后返回给远程客户端

