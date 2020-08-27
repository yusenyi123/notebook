# grub2菜单代码解析

## 菜单代码案例

```
if search --no-floppy -f  /ubuntu/UBT-small.vhd; then
menuentry "UBT-small.vhd" --class ubuntu {
	insmod gzio
	insmod part_msdos
	insmod part_gpt
	insmod ext2
	insmod ntfs
	insmod probe
	set vhdfile="/ubuntu/UBT-small.vhd"
	set root=(hd0,1)
	search --no-floppy -f --set=aabbcc  $vhdfile
	set root=${aabbcc}
	probe -u --set=ddeeff ${aabbcc}
	loopback lp0 $vhdfile
	linux	(lp0,1)/vmlinuz root=UUID=${ddeeff}  kloop=$vhdfile  kroot=/dev/mapper/loop0p1
	initrd	(lp0,1)/initrd.img
}
fi

```

上述代码表示 搜索文件/ubuntu/UBT-small.vhd 如果能够找到，那么变创建UBT-small.vhd名字的菜单属于ubuntu分类

```
search --no-floppy -f  /ubuntu/UBT-small.vhd

查找所用硬件存储设备(禁止查找软盘设备)找到首个根目录（每个分区依次作为根目录）里包含 ubuntu/UBT-small.vhd的分区，返回为分区号

search [--file|--label|--fs-uuid] [--set [var]] [--no-floppy] name
通过文件[--file]、卷标[--label]、文件系统UUID[--fs-uuid]来查找设备。

如果使用了 --set 选项，那么会将第一个找到的设备设置为环境变量"var"的值。默认的"var"是'root'。

可以使用 --no-floppy 选项来禁止查找软盘设备，因为这些设备非常慢。
```

~~~
probe -u --set=ddeeff ${aabbcc}

得到变量aabbcc表示的设备号的uuid赋值给变量ddeeff

probe [--set var] --driver|--partmap|--fs|--fs-uuid|--label device
提取"device"设备的特定信息。如果使用了 --set 选项，则表示将提取的结果保存在"var"变量中，否则将提取的结果直接显示出来。
~~~

```
loopback lp0 $vhdfile

将vhdfile变量表示的文件目录映射成device回环设备

loopback [-d] device file
将"file"文件映射为"device"回环设备。例如：

loopback loop0 /path/to/image
ls (loop0)/

可以使用 -d 选项，删除先前使用这个命令创建的设备。
```

```
linux	(lp0,1)/vmlinuz root=UUID=${ddeeff}  kloop=$vhdfile  kroot=/dev/mapper/loop0p1

表示装载指定的内核文件，并传递内核启动参数

常用参数
init=   ：指定Linux启动的第一个进程init的替代程序。
root=   ：指定根文件系统所在分区，在grub中，该选项必须给定。
ro,rw   ：启动时，根分区以只读还是可读写方式挂载。不指定时默认为ro。
initrd  ：指定init ramdisk的路径。在grub中因为使用了initrd或initrd16命令，所以不需要指定该启动参数。
rhgb    ：以图形界面方式启动系统。
quiet   ：以文本方式启动系统，且禁止输出大多数的log message。
net.ifnames=0：用于CentOS 7，禁止网络设备使用一致性命名方式。
biosdevname=0：用于CentOS 7，也是禁止网络设备采用一致性命名方式。
             ：只有net.ifnames和biosdevname同时设置为0时，才能完全禁止一致性命名，得到eth0-N的设备名。
             
             
kloop 参数 ：虚拟环回设备所在目录

kroot 参数： 虚拟磁盘中根文件所在分区  kroot=根分区挂载好以后在vhd-linux内部的名字
             
```



![image-20200811194114443](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163559.png)



==The ‘search.file’, ‘search.fs_label’, and ‘search.fs_uuid’ commands are aliases for ‘search --file’, ‘search --label’, and ‘search --fs-uuid’ respectively.==

==search --fs-uuid  和 search.fs_uuid 相同意思==

```
search.fs_uuid 5ef8bd72-a034-45c5-b578-9bab3ffcc063 root hd0,gpt2 

search --set=root --fs-uuid  5ef8bd72-a034-45c5-b578-9bab3ffcc063  --hint hd0,gpt2

--hint 表示优先选择满足提示条件的设备，若指定了多个hint条件，则优先匹配第一个hint，然后匹配第二个，依次类推。

fs-uuid 是卷uuid


(fd0)           ：表示第一块软盘
(hd0,msdos2)    ：表示第一块硬盘的第二个mbr分区。grub2中分区从1开始编号，传统的grub是从0开始编号的
(hd0,msdos5)    ：表示第一块硬盘的第一个逻辑分区
(hd0,gpt1)      ：表示第一块硬盘的第一个gpt分区
/boot/vmlinuz   ：相对路径，基于根目录，表示根目录下的boot目录下的vmlinuz，
                ：如果设置了根目录变量root为(hd0,msdos1)，则表示(hd0,msdos1)/boot/vmlinuz
(hd0,msdos1)/boot/vmlinuz：绝对路径，表示第一硬盘第一分区的boot目录下的vmlinuz文件

命令解释
从全部分区中寻找卷uuid为5ef8bd72-a034-45c5-b578-9bab3ffcc063的分区，优先判断hd0,gpt2该分区

```


