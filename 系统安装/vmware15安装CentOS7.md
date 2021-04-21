# vmware15安装CentOS7

## 参考

https://blog.csdn.net/nuoyanli/article/details/86503686

https://www.cnblogs.com/cannel/p/11104088.html

https://www.cnblogs.com/yogurtwu/p/10716992.html

![image-20210420185344239](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210420185351.png)

如果你是新手记住：除了SWAP分区外，其他分区的文件系统一律选择ext4类型,设备类型默认选LVM







此处设置自动创建分区的类型：Standard partition（标准分区）、Btrfs（一种已经废弃的文件系统）、LVM、LVM Thin Provisioning （自动精简LVM）

**Tips**：标准分区，分区是固定大小，LVM：普通的磁盘分区管理方式在逻辑分区划分好之后就无法改变其大小，当一个逻辑分区存放不下某个文件时，这个文件因为受上层文件系统的限制，也不能跨越多个分区来存放，所以也不能同时放到别的磁盘上。而遇到出现某个分区空间耗尽时，解决的方法通常是使用符号链接，或者使用调整分区大小的工具，但这只是暂时解决办法，没有从根本上解决问题。随着Linux的逻辑卷管理（LVM）功能的出现，这些问题都迎刃而解，用户在无需停机的情况下可以方便地调整各个分区大小，当然也可以使用LVM Thin Provisioning。某些分区固定只能是标准分区，即使选择LVM也会自动设置为标准分区。

 





## 设备类型使用标准分区

### 1.将/boot/efi 进行挂载 文件系统使用EFI system partition



### 2.将/ 进行挂载 文件系统使用ext4





