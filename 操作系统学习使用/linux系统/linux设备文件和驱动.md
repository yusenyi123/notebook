# linux设备文件和驱动

## 参考:

https://www.kernel.org/

https://max.book118.com/html/2018/0604/170768892.shtm

## Linux系统中对于设备的访问是通过文件系统内的设备名称进行的。那些名称被称为特殊文件、设备文件，它们通常位于/dev目录下。



## 查看设备号的命令

```
Linux的设备管理是和文件系统紧密结合的，把设备和文件关联起来，这样系统调用可以直接用操作文件一样的方法来操作设备。各种设备都以文件的形式存放在/dev目录下，称为设备文件。应用程序可以打开、关闭和读写这些设备文件，完成对设备的操作，就像操作普通的数据文件一样。为了管理这些设备，系统为设备编了号，每个设备号又分为主设备号和次设备号。主设备号用来区分不同种类的设备，而次设备号用来区分同一类型的多个设备。对于常用设备，Linux有约定俗成的编号，如硬盘的主设备号是3。

查看主设备号：  cat /proc/devices
查看当前设备的主次设备号： ls -l /dev



```

```
buntu@ubuntu-virtual-machine:~/桌面$  ls -l /dev
总用量 0
crw-------  1 root   root     10, 175 3月  14 09:07 agpgart
crw-r--r--  1 root   root     10, 235 3月  14 09:07 autofs
drwxr-xr-x  2 root   root         620 3月  14 09:06 block
drwxr-xr-x  2 root   root         100 3月  14 09:06 bsg
crw-------  1 root   root     10, 234 3月  14 09:06 btrfs-control
drwxr-xr-x  3 root   root          60 3月  14 09:06 bus
lrwxrwxrwx  1 root   root           3 3月  14 09:07 cdrom -> sr0
lrwxrwxrwx  1 root   root           3 3月  14 09:07 cdrw -> sr0
drwxr-xr-x  2 root   root        3800 3月  14 09:07 char
crw--w----  1 root   tty       5,   1 3月  14 09:07 console
lrwxrwxrwx  1 root   root          11 3月  14 09:06 core -> /proc/kcore
crw-------  1 root   root     10,  59 3月  14 09:07 cpu_dma_latency
crw-------  1 root   root     10, 203 3月  14 09:06 cuse
drwxr-xr-x  8 root   root         160 3月  14 09:07 disk
drwxr-xr-x  2 root   root          60 3月  14 09:06 dma_heap
crw-rw----+ 1 root   audio    14,   9 3月  14 09:07 dmmidi
drwxr-xr-x  3 root   root         100 3月  14 09:07 dri
lrwxrwxrwx  1 root   root           3 3月  14 09:07 dvd -> sr0
crw-------  1 root   root     10,  62 3月  14 09:07 ecryptfs
crw-rw----  1 root   video    29,   0 3月  14 09:07 fb0
lrwxrwxrwx  1 root   root          13 3月  14 09:06 fd -> /proc/self/fd
crw-rw-rw-  1 root   root      1,   7 3月  14 09:07 full
crw-rw-rw-  1 root   root     10, 229 3月  14 09:07 fuse
crw-------  1 root   root    242,   0 3月  14 09:07 hidraw0
crw-------  1 root   root     10, 228 3月  14 09:07 hpet
drwxr-xr-x  2 root   root           0 3月  14 09:06 hugepages
crw-------  1 root   root     10, 183 3月  14 09:07 hwrng
lrwxrwxrwx  1 root   root          12 3月  14 09:06 initctl -> /run/initctl
drwxr-xr-x  4 root   root         260 3月  14 09:07 input
crw-r--r--  1 root   root      1,  11 3月  14 09:07 kmsg
drwxr-xr-x  2 root   root          60 3月  14 09:06 lightnvm
lrwxrwxrwx  1 root   root          28 3月  14 09:06 log -> /run/systemd/journal/dev-log
brw-rw----  1 root   disk      7,   0 3月  14 09:10 loop0
brw-rw----  1 root   disk      7,   1 3月  14 09:07 loop1
brw-rw----  1 root   disk      7,   2 3月  14 09:07 loop2
brw-rw----  1 root   disk      7,   3 3月  14 09:07 loop3
brw-rw----  1 root   disk      7,   4 3月  14 09:07 loop4
brw-rw----  1 root   disk      7,   5 3月  14 09:07 loop5
brw-rw----  1 root   disk      7,   6 3月  14 09:07 loop6
brw-rw----  1 root   disk      7,   7 3月  14 09:07 loop7
crw-rw----  1 root   disk     10, 237 3月  14 09:07 loop-control
drwxr-xr-x  2 root   root          60 3月  14 09:06 mapper
crw-------  1 root   root     10, 227 3月  14 09:07 mcelog
crw-r-----  1 root   kmem      1,   1 3月  14 09:07 mem
crw-rw----+ 1 root   audio    14,   2 3月  14 09:07 midi
drwxrwxrwt  2 root   root          40 3月  14 09:06 mqueue
brw-rw----  1 root   disk     43,   0 3月  14 09:07 nbd0
brw-rw----  1 root   disk     43,  32 3月  14 09:07 nbd1
brw-rw----  1 root   disk     43, 320 3月  14 09:07 nbd10
brw-rw----  1 root   disk     43, 352 3月  14 09:07 nbd11
brw-rw----  1 root   disk     43, 384 3月  14 09:07 nbd12
brw-rw----  1 root   disk     43, 416 3月  14 09:07 nbd13
brw-rw----  1 root   disk     43, 448 3月  14 09:07 nbd14
brw-rw----  1 root   disk     43, 480 3月  14 09:07 nbd15
brw-rw----  1 root   disk     43,  64 3月  14 09:07 nbd2
brw-rw----  1 root   disk     43,  96 3月  14 09:07 nbd3
brw-rw----  1 root   disk     43, 128 3月  14 09:07 nbd4
brw-rw----  1 root   disk     43, 160 3月  14 09:07 nbd5
brw-rw----  1 root   disk     43, 192 3月  14 09:07 nbd6
brw-rw----  1 root   disk     43, 224 3月  14 09:07 nbd7
brw-rw----  1 root   disk     43, 256 3月  14 09:07 nbd8
brw-rw----  1 root   disk     43, 288 3月  14 09:07 nbd9
drwxr-xr-x  2 root   root          60 3月  14 09:06 net
crw-rw-rw-  1 root   root      1,   3 3月  14 09:07 null
crw-------  1 root   root     10, 144 3月  14 09:06 nvram
crw-r-----  1 root   kmem      1,   4 3月  14 09:07 port
crw-------  1 root   root    108,   0 3月  14 09:07 ppp
crw-------  1 root   root     10,   1 3月  14 09:07 psaux
crw-rw-rw-  1 root   tty       5,   2 3月  14 09:10 ptmx
drwxr-xr-x  2 root   root           0 3月  14 09:06 pts
crw-rw-rw-  1 root   root      1,   8 3月  14 09:07 random
crw-rw-r--+ 1 root   root     10, 242 3月  14 09:07 rfkill
lrwxrwxrwx  1 root   root           4 3月  14 09:07 rtc -> rtc0
crw-------  1 root   root    248,   0 3月  14 09:07 rtc0
brw-rw----  1 root   disk      8,   0 3月  14 09:07 sda
brw-rw----  1 root   disk      8,  16 3月  14 09:07 sdb
brw-rw----  1 root   disk      8,  17 3月  14 09:07 sdb1
brw-rw----  1 root   disk      8,  18 3月  14 09:07 sdb2
crw-rw----+ 1 root   cdrom    21,   0 3月  14 09:07 sg0
crw-rw----  1 root   disk     21,   1 3月  14 09:07 sg1
crw-rw----  1 root   disk     21,   2 3月  14 09:07 sg2
drwxrwxrwt  2 root   root          40 3月  14 09:06 shm
crw-------  1 root   root     10, 231 3月  14 09:07 snapshot
drwxr-xr-x  3 root   root         200 3月  14 09:07 snd
brw-rw----+ 1 root   cdrom    11,   0 3月  14 09:07 sr0
lrwxrwxrwx  1 root   root          15 3月  14 09:06 stderr -> /proc/self/fd/2
lrwxrwxrwx  1 root   root          15 3月  14 09:06 stdin -> /proc/self/fd/0
lrwxrwxrwx  1 root   root          15 3月  14 09:06 stdout -> /proc/self/fd/1
crw-rw-rw-  1 root   tty       5,   0 3月  14 09:07 tty
crw--w----  1 root   tty       4,   0 3月  14 09:07 tty0
crw--w----  1 root   tty       4,   1 3月  14 09:07 tty1
crw--w----  1 root   tty       4,  10 3月  14 09:07 tty10
crw--w----  1 root   tty       4,  11 3月  14 09:07 tty11
crw--w----  1 root   tty       4,  12 3月  14 09:07 tty12
crw--w----  1 root   tty       4,  13 3月  14 09:07 tty13
crw--w----  1 root   tty       4,  14 3月  14 09:07 tty14
crw--w----  1 root   tty       4,  15 3月  14 09:07 tty15
crw--w----  1 root   tty       4,  16 3月  14 09:07 tty16
crw--w----  1 root   tty       4,  17 3月  14 09:07 tty17
crw--w----  1 root   tty       4,  18 3月  14 09:07 tty18
crw--w----  1 root   tty       4,  19 3月  14 09:07 tty19
crw--w----  1 ubuntu tty       4,   2 3月  14 09:07 tty2
crw--w----  1 root   tty       4,  20 3月  14 09:07 tty20
crw--w----  1 root   tty       4,  21 3月  14 09:07 tty21
crw--w----  1 root   tty       4,  22 3月  14 09:07 tty22
crw--w----  1 root   tty       4,  23 3月  14 09:07 tty23
crw--w----  1 root   tty       4,  24 3月  14 09:07 tty24
crw--w----  1 root   tty       4,  25 3月  14 09:07 tty25
crw--w----  1 root   tty       4,  26 3月  14 09:07 tty26
crw--w----  1 root   tty       4,  27 3月  14 09:07 tty27
crw--w----  1 root   tty       4,  28 3月  14 09:07 tty28
crw--w----  1 root   tty       4,  29 3月  14 09:07 tty29
crw--w----  1 root   tty       4,   3 3月  14 09:07 tty3
crw--w----  1 root   tty       4,  30 3月  14 09:07 tty30
crw--w----  1 root   tty       4,  31 3月  14 09:07 tty31
crw--w----  1 root   tty       4,  32 3月  14 09:07 tty32
crw--w----  1 root   tty       4,  33 3月  14 09:07 tty33
crw--w----  1 root   tty       4,  34 3月  14 09:07 tty34
crw--w----  1 root   tty       4,  35 3月  14 09:07 tty35
crw--w----  1 root   tty       4,  36 3月  14 09:07 tty36
crw--w----  1 root   tty       4,  37 3月  14 09:07 tty37
crw--w----  1 root   tty       4,  38 3月  14 09:07 tty38
crw--w----  1 root   tty       4,  39 3月  14 09:07 tty39
crw--w----  1 root   tty       4,   4 3月  14 09:07 tty4
crw--w----  1 root   tty       4,  40 3月  14 09:07 tty40
crw--w----  1 root   tty       4,  41 3月  14 09:07 tty41
crw--w----  1 root   tty       4,  42 3月  14 09:07 tty42
crw--w----  1 root   tty       4,  43 3月  14 09:07 tty43
crw--w----  1 root   tty       4,  44 3月  14 09:07 tty44
crw--w----  1 root   tty       4,  45 3月  14 09:07 tty45
crw--w----  1 root   tty       4,  46 3月  14 09:07 tty46
crw--w----  1 root   tty       4,  47 3月  14 09:07 tty47
crw--w----  1 root   tty       4,  48 3月  14 09:07 tty48
crw--w----  1 root   tty       4,  49 3月  14 09:07 tty49
crw--w----  1 root   tty       4,   5 3月  14 09:07 tty5
crw--w----  1 root   tty       4,  50 3月  14 09:07 tty50
crw--w----  1 root   tty       4,  51 3月  14 09:07 tty51
crw--w----  1 root   tty       4,  52 3月  14 09:07 tty52
crw--w----  1 root   tty       4,  53 3月  14 09:07 tty53
crw--w----  1 root   tty       4,  54 3月  14 09:07 tty54
crw--w----  1 root   tty       4,  55 3月  14 09:07 tty55
crw--w----  1 root   tty       4,  56 3月  14 09:07 tty56
crw--w----  1 root   tty       4,  57 3月  14 09:07 tty57
crw--w----  1 root   tty       4,  58 3月  14 09:07 tty58
crw--w----  1 root   tty       4,  59 3月  14 09:07 tty59
crw--w----  1 root   tty       4,   6 3月  14 09:07 tty6
crw--w----  1 root   tty       4,  60 3月  14 09:07 tty60
crw--w----  1 root   tty       4,  61 3月  14 09:07 tty61
crw--w----  1 root   tty       4,  62 3月  14 09:07 tty62
crw--w----  1 root   tty       4,  63 3月  14 09:07 tty63
crw--w----  1 root   tty       4,   7 3月  14 09:07 tty7
crw--w----  1 root   tty       4,   8 3月  14 09:07 tty8
crw--w----  1 root   tty       4,   9 3月  14 09:07 tty9
crw-------  1 root   root      5,   3 3月  14 09:07 ttyprintk
crw-rw----  1 root   dialout   4,  64 3月  14 09:07 ttyS0
crw-rw----  1 root   dialout   4,  65 3月  14 09:07 ttyS1
crw-rw----  1 root   dialout   4,  74 3月  14 09:07 ttyS10
crw-rw----  1 root   dialout   4,  75 3月  14 09:07 ttyS11
crw-rw----  1 root   dialout   4,  76 3月  14 09:07 ttyS12
crw-rw----  1 root   dialout   4,  77 3月  14 09:07 ttyS13
crw-rw----  1 root   dialout   4,  78 3月  14 09:07 ttyS14
crw-rw----  1 root   dialout   4,  79 3月  14 09:07 ttyS15
crw-rw----  1 root   dialout   4,  80 3月  14 09:07 ttyS16
crw-rw----  1 root   dialout   4,  81 3月  14 09:07 ttyS17
crw-rw----  1 root   dialout   4,  82 3月  14 09:07 ttyS18
crw-rw----  1 root   dialout   4,  83 3月  14 09:07 ttyS19
crw-rw----  1 root   dialout   4,  66 3月  14 09:07 ttyS2
crw-rw----  1 root   dialout   4,  84 3月  14 09:07 ttyS20
crw-rw----  1 root   dialout   4,  85 3月  14 09:07 ttyS21
crw-rw----  1 root   dialout   4,  86 3月  14 09:07 ttyS22
crw-rw----  1 root   dialout   4,  87 3月  14 09:07 ttyS23
crw-rw----  1 root   dialout   4,  88 3月  14 09:07 ttyS24
crw-rw----  1 root   dialout   4,  89 3月  14 09:07 ttyS25
crw-rw----  1 root   dialout   4,  90 3月  14 09:07 ttyS26
crw-rw----  1 root   dialout   4,  91 3月  14 09:07 ttyS27
crw-rw----  1 root   dialout   4,  92 3月  14 09:07 ttyS28
crw-rw----  1 root   dialout   4,  93 3月  14 09:07 ttyS29
crw-rw----  1 root   dialout   4,  67 3月  14 09:07 ttyS3
crw-rw----  1 root   dialout   4,  94 3月  14 09:07 ttyS30
crw-rw----  1 root   dialout   4,  95 3月  14 09:07 ttyS31
crw-rw----  1 root   dialout   4,  68 3月  14 09:07 ttyS4
crw-rw----  1 root   dialout   4,  69 3月  14 09:07 ttyS5
crw-rw----  1 root   dialout   4,  70 3月  14 09:07 ttyS6
crw-rw----  1 root   dialout   4,  71 3月  14 09:07 ttyS7
crw-rw----  1 root   dialout   4,  72 3月  14 09:07 ttyS8
crw-rw----  1 root   dialout   4,  73 3月  14 09:07 ttyS9
crw-rw----  1 root   kvm      10,  60 3月  14 09:07 udmabuf
crw-------  1 root   root     10, 239 3月  14 09:06 uhid
crw-------  1 root   root     10, 223 3月  14 09:07 uinput
crw-rw-rw-  1 root   root      1,   9 3月  14 09:07 urandom
crw-------  1 root   root     10, 240 3月  14 09:06 userio
crw-rw----  1 root   tty       7,   0 3月  14 09:07 vcs
crw-rw----  1 root   tty       7,   1 3月  14 09:07 vcs1
crw-rw----  1 root   tty       7,   2 3月  14 09:07 vcs2
crw-rw----  1 root   tty       7,   3 3月  14 09:07 vcs3
crw-rw----  1 root   tty       7,   4 3月  14 09:07 vcs4
crw-rw----  1 root   tty       7,   5 3月  14 09:07 vcs5
crw-rw----  1 root   tty       7,   6 3月  14 09:07 vcs6
crw-rw----  1 root   tty       7, 128 3月  14 09:07 vcsa
crw-rw----  1 root   tty       7, 129 3月  14 09:07 vcsa1
crw-rw----  1 root   tty       7, 130 3月  14 09:07 vcsa2
crw-rw----  1 root   tty       7, 131 3月  14 09:07 vcsa3
crw-rw----  1 root   tty       7, 132 3月  14 09:07 vcsa4
crw-rw----  1 root   tty       7, 133 3月  14 09:07 vcsa5
crw-rw----  1 root   tty       7, 134 3月  14 09:07 vcsa6
crw-rw----  1 root   tty       7,  64 3月  14 09:07 vcsu
crw-rw----  1 root   tty       7,  65 3月  14 09:07 vcsu1
crw-rw----  1 root   tty       7,  66 3月  14 09:07 vcsu2
crw-rw----  1 root   tty       7,  67 3月  14 09:07 vcsu3
crw-rw----  1 root   tty       7,  68 3月  14 09:07 vcsu4
crw-rw----  1 root   tty       7,  69 3月  14 09:07 vcsu5
crw-rw----  1 root   tty       7,  70 3月  14 09:07 vcsu6
drwxr-xr-x  2 root   root          60 3月  14 09:06 vfio
crw-------  1 root   root     10,  63 3月  14 09:07 vga_arbiter
crw-------  1 root   root     10, 137 3月  14 09:06 vhci
crw-------  1 root   root     10, 238 3月  14 09:06 vhost-net
crw-------  1 root   root     10, 241 3月  14 09:06 vhost-vsock
crw-------  1 root   root     10,  58 3月  14 09:07 vmci
crw-------  1 root   root     10,  57 3月  14 09:07 vsock
crw-rw-rw-  1 root   root      1,   5 3月  14 09:07 zero
crw-------  1 root   root     10, 249 3月  14 09:06 zfs
ubuntu@ubuntu-virtual-machine:~/桌面$ 


```





## ls查看到的基本信息详解





```
buntu@ubuntu-virtual-machine:~/桌面$  ls -l /dev
总用量 0
crw-------  1 root   root     10, 175 3月  14 09:07 agpgart
crw-r--r--  1 root   root     10, 235 3月  14 09:07 autofs
drwxr-xr-x  2 root   root         620 3月  14 09:06 block
drwxr-xr-x  2 root   root         100 3月  14 09:06 bsg


一条文件信息详解
crw-------  1 root   root     10, 175 3月  14 09:07 agpgart






LINUX中的七种文件类型
d 目录文件。
l 符号链接(指向另一个文件,类似于window下的快捷方式)；
s 套接字文件；
b 块设备文件,二进制文件；
c 字符设备文件；
p 命名管道文件；
- 普通文件。
```

### 文件类型为设备的文件信息详解

![image-20210314093137039](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210314093144.png)

### 文件类型为普通文件或者目录的文件信息详解

![img](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210314093152.jpeg)







## stat命令查看更详细的单个文件信息

### inode(index node 索引节点)







## mknod命令(make node)用于创建Linux中的字符设备文件和块设备文件。

```
mknod  [选项][文件名称] [文件类型] [主设备号] [次设备号]


sudo mknod   /dev/console2 c 100 2                 //创建字符设备 /dev/console2,主设备号为100,次设备号为2
sudo mknod -m 660  /dev/console2 c 100 2    //创建字符设备 /dev/console2,并设置权限为660(用户和组都可读写) ,主设备号为100,次设备号为2　

```









```
typedef unsigned int u32;

typedef u32 __kernel_dev_t;

typedef __kernel_fd_set		fd_set;
typedef __kernel_dev_t		dev_t;
```

```
/*  
MAJOR宏 提取主设备号  
MINOR宏 提取次设备号  
MKDEV宏 将指定主设备号和次设备号 转化为一个dev_t  
*/  
#define MINORBITS   20  
#define MINORMASK   ((1U << MINORBITS) - 1)  
#define MAJOR(dev)  ((unsigned int) ((dev) >> MINORBITS))  
#define MINOR(dev)  ((unsigned int) ((dev) & MINORMASK))  
#define MKDEV(ma,mi)    (((ma) << MINORBITS) | (mi))  
```



```
//静态分配   dev_t为unsigned int类型

//设备注册函数
//from: 注册的指定起始设备编号,比如:MKDEV(100, 0),表示起始主设备号100, 起始次设备号为0

//count:需要连续注册的次设备编号个数,比如: 起始次设备号为0,count=100,表示0~99的次设备号都要绑定在同一个file_operations操作方法结构体上

//*name:字符设备名称

int register_chrdev_region(dev_t from, unsigned count, const char *name)  
```



### cdev数据结构和操作

```
struct cdev {
       struct kobject    kobj;                   // 内嵌的kobject对象 
       struct module   *owner;                   //所属模块
       const struct file_operations  *ops;     //操作方法结构体
       struct list_head  list;      　　　　　　//与 cdev 对应的字符设备文件的 inode->i_devices 的链表头
       dev_t dev;      　　　　　　　　　　　　　　//起始设备编号,可以通过MAJOR(),MINOR()来提取主次设备号
       unsigned int count;              　　//连续注册的次设备号个数
};




//将dev(注册好的设备编号)放入cdev-> dev里,  count(次设备编号个数)放入cdev->count里，然后将cdev这个结构体添加到到cdev数组（cdev_map）中供系统管理。
int cdev_add(struct cdev *p, dev_t dev, unsigned count);

//fs/char_dev.c
456 int cdev_add(struct cdev *p, dev_t dev, unsigned count)    
457 {
458         int error;
459 
460         p->dev = dev;
461         p->count = count;
462 
463         error = kobj_map(cdev_map, dev, count, NULL,
464                          exact_match, exact_lock, p);
465         if (error)
466                 return error;
467 
468         kobject_get(p->kobj.parent);
469 
470         return 0;
471 }
```







### inode数据结构

```
//include/linux/fs.h
 596 /*
 597  * Keep mostly read-only and often accessed (especially for
 598  * the RCU path lookup and 'stat' data) fields at the beginning
 599  * of the 'struct inode'
 600  */
 601 struct inode {
 602         umode_t                 i_mode;
 604         kuid_t                  i_uid;
 605         kgid_t                  i_gid;
 606         unsigned int            i_flags;
 630         union {
 631                 const unsigned int i_nlink;
 632                 unsigned int __i_nlink;
 633         };
 634         dev_t                   i_rdev;
 635         loff_t                  i_size;
 636         struct timespec         i_atime;
 637         struct timespec         i_mtime;
 638         struct timespec         i_ctime;
 668         union {
 669                 struct hlist_head       i_dentry;
 670                 struct rcu_head         i_rcu;
 671         };
 672         u64                     i_version;
 673         atomic_t                i_count;
 674         atomic_t                i_dio_count;
 675         atomic_t                i_writecount;
 679         const struct file_operations    *i_fop; /* former ->i_op->default_file_ops */
 681         struct address_space    i_data;
 682         struct list_head        i_devices;
 683         union {
 684                 struct pipe_inode_info  *i_pipe;
 685                 struct block_device     *i_bdev;
 686                 struct cdev             *i_cdev;
 687                 char                    *i_link;
 688                 unsigned                i_dir_seq;
 689         };
 702         void                    *i_private; /* fs or device private pointer */
 703 }; 
```



### file数据结构，打开文件表中的数据结构

```
//include/linux/fs.h
 877 struct file {
 878         union {
 879                 struct llist_node       fu_llist;
 880                 struct rcu_head         fu_rcuhead;
 881         } f_u;
 882         struct path             f_path;
 883         struct inode            *f_inode;       /* cached value */
 884         const struct file_operations    *f_op;
 885 
 886         /*                                            
 887          * Protects f_ep_links, f_flags.
 888          * Must not be taken from IRQ context.
 889          */
 890         spinlock_t              f_lock;
 891         atomic_long_t           f_count;
 892         unsigned int            f_flags;
 893         fmode_t                 f_mode;
 894         struct mutex            f_pos_lock;
 895         loff_t                  f_pos;
 896         struct fown_struct      f_owner;
 897         const struct cred       *f_cred;
 898         struct file_ra_state    f_ra;f
 904         /* needed for tty driver, and maybe others */
 905         void                    *private_data;
 912         struct address_space    *f_mapping;
 913 } __attribute__((aligned(4)));  /* lest something weird decides that 2 is OK */
```



## 字符型设备驱动总结

![image-20210314123101605](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210314123101.png)



![image-20210314113837043](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210314113838.png)



```
1. insmod命令(install module)，用来载入模块，通过模式的方式在需要时载入内核，可使内核精简，高效。此类载入的模块，通常为设备驱动程序.
这里我们的模块写的就是驱动程序，会执行我们模块中写的init方法
2.在init 方法中我们创建了一个cdev结构体，该结构体对应一个设备号(设备号由主设备号和次设备号决定)，并且把这个结构体添加到了系统内存中的cdev数组（cdev_map）中供系统管理。


3. 我们使用mknod 创建一个设备文件 ，此时会指定主设备号，次设备号，创建设备文件的时候会创建inode(索引节点)保存在磁盘中
索引节点中保存了主设备号次设备号 等很多信息



4. 当我们用字节代码测试编写的驱动文件时，先使用open("设备文件路径"),此时会进行系统调用，每个进程都在内存中都有一个打开文件表，打开该设备文件时，会创建一个对应的file数据结构加入到打开文件表中

5. open("设备文件路径")时，根据该设备文件对应的inode节点中的主设备号和次设备号，查找内存中cdev_map数组，找到该主设备号次设备号对应的cdev数据结构，该数据结构中保存了file_operations结构体，然后会将file_operations结构体指针赋值给打开文件表中file结构体中的  const struct file_operations    *f_op;

6. 之后在测试代码中调用read()，write()时就能直接查找打开文件表中的file_operations结构体，执行结构体中对应的read()，write()
 
 
 
 
 
 
```





## 编译编写的字符型驱动

```
make，-C 选项的作用是指将当前工作目录转移到你所指定的位置。“M=”选项的作用是，当用户需要以某个内核为基础编译一个外部模块的话，需要在make modules 命令中加入“M=dir”，程序会自动到你所指定的dir目录中查找模块源码，将其编译，生成KO文件。


```



```
#include <linux/module.h>
#include <linux/moduleparam.h>
#include <linux/cdev.h>
#include <linux/fs.h>
#include <linux/wait.h>
#include <linux/poll.h>
#include <linux/sched.h>
#include <linux/slab.h>

#define BUFFER_MAX    (10)
#define OK            (0)
#define ERROR         (-1)

struct cdev *gDev;
struct file_operations *gFile;
dev_t  devNum;
unsigned int subDevNum = 1;
int reg_major  =  232;    
int reg_minor =   0;
char *buffer;
int flag = 0;
int hello_open(struct inode *p, struct file *f)
{
    printk(KERN_EMERG"hello_open\r\n");
    return 0;
}

ssize_t hello_write(struct file *f, const char __user *u, size_t s, loff_t *l)
{
    printk(KERN_EMERG"hello_write\r\n");
    return 0;
}
ssize_t hello_read(struct file *f, char __user *u, size_t s, loff_t *l)
{
    printk(KERN_EMERG"hello_read\r\n");      
    return 0;
}
int hello_init(void)
{
    
    devNum = MKDEV(reg_major, reg_minor);
    if(OK == register_chrdev_region(devNum, subDevNum, "helloworld")){
        printk(KERN_EMERG"register_chrdev_region ok \n"); 
    }else {
    printk(KERN_EMERG"register_chrdev_region error n");
        return ERROR;
    }
    printk(KERN_EMERG" hello driver init \n");
    gDev = kzalloc(sizeof(struct cdev), GFP_KERNEL);
    gFile = kzalloc(sizeof(struct file_operations), GFP_KERNEL);
    gFile->open = hello_open;
    gFile->read = hello_read;
    gFile->write = hello_write;
    gFile->owner = THIS_MODULE;
    cdev_init(gDev, gFile);
    cdev_add(gDev, devNum, 1);
    return 0;
}

void __exit hello_exit(void)
{
    cdev_del(gDev);
    unregister_chrdev_region(devNum, subDevNum);
    return;
}
module_init(hello_init);
module_exit(hello_exit);
MODULE_LICENSE("GPL");
```

### Makefile文件

```
ifneq ($(KERNELRELEASE),)
obj-m := helloDev.o
else
PWD := $(shell pwd)
#KDIR:= /lib/modules/4.4.0-31-generic/build
KDIR := /lib/modules/`uname -r`/build
all:
	make -C $(KDIR) M=$(PWD)
clean:	
	rm -rf *.o *.ko *.mod.c *.symvers *.c~ *~
endif
```







```
sudo insmod helloDev.ko

dmesg 

sudo rmmod helloDev


lsmod


echo 7 > /proc/sys/kernel/printk
```

## 驱动测试代码(编译后 管理员运行程序)

```
#include <unistd.h>
#include <fcntl.h>
#include <stdio.h>
#include <errno.h>
 
int main(void)
{
	char s[] = "hello";
 
	int ret = 0;
 
	int fd = open("/dev/hello",O_RDWR);
	printf("fd=%d\n",fd);
	printf("errno=%d\n",errno);
	
		write(fd,s,3);
	
	close(fd);
	printf("close ok...\n");
	return 0;
}
```



