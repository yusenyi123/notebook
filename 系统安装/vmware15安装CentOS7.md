# vmware15安装CentOS7

## 参考

https://blog.csdn.net/nuoyanli/article/details/86503686

https://www.cnblogs.com/cannel/p/11104088.html

https://www.cnblogs.com/yogurtwu/p/10716992.html

https://www.cnblogs.com/liuhouhou/p/8975812.html

此处设置自动创建分区的类型：Standard partition（标准分区）、Btrfs（一种已经废弃的文件系统）、LVM、LVM Thin Provisioning （自动精简LVM）

**Tips**：标准分区，分区是固定大小，LVM：普通的磁盘分区管理方式在逻辑分区划分好之后就无法改变其大小，当一个逻辑分区存放不下某个文件时，这个文件因为受上层文件系统的限制，也不能跨越多个分区来存放，所以也不能同时放到别的磁盘上。而遇到出现某个分区空间耗尽时，解决的方法通常是使用符号链接，或者使用调整分区大小的工具，但这只是暂时解决办法，没有从根本上解决问题。随着Linux的逻辑卷管理（LVM）功能的出现，这些问题都迎刃而解，用户在无需停机的情况下可以方便地调整各个分区大小，当然也可以使用LVM Thin Provisioning。某些分区固定只能是标准分区，即使选择LVM也会自动设置为标准分区。

 





## vmware设置固件类型为UEFI(因为我系统要安装到GPT格式的分区)

![image-20210427111717485](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210427112012.png)





## 系统安装需要设置的地方



![image-20210425155559273](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210425155559.png)

## 软件选择(系统环境的选择,即安装的系统默认安装了哪些软件)

![image-20210425155403441](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210425155403.png)



## 系统安装采用自定义分区，设备类型使用标准分区

### 1.将/boot/efi 进行挂载 文件系统使用EFI system partition

![image-20210425143553448](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210425143553.png)



![image-20210425143621549](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210425143621.png)

### 2.将/ 进行挂载 文件系统使用ext4

![image-20210425143644174](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210425143644.png)







## 安装ssh(如果选择的系统环境没有安装，就额外安装)

#### 1. 安装openssh-server

```
# 查看是否安装
yum list installed | grep openssh-server
# 没有安装就执行下列命令进行安装
yum install -y openssl openssh-server
```

#### 2. 修改配置文件

用vim打开配置文件/etc/ssh/sshd_config

![img](http://static.oschina.net/uploads/space/2016/0515/123831_NYoU_160089.png)

将上图的PermitRootLogin，RSAAuthentication，PubkeyAuthentication的设置打开。

启动ssh的服务：

```
systemctl start sshd.service
```

设置开机自动启动ssh服务

```
systemctl enable sshd.service
```

设置文件夹~/.ssh的访问权限：

authorized_keys文件存储的是客户端的公共密钥。

```
$ cd ~
$ chmod 700 .ssh                                                                                                
$ chmod 600 .ssh/*                                                                                              
$ ls -la .ssh                                                                                                   
total 16
drwx------. 2 root root   58 May 15 00:23 .
dr-xr-x---. 8 root root 4096 May 15 00:26 ..
-rw-------. 1 root root  403 May 15 00:22 authorized_keys
-rw-------. 1 root root 1766 May 15 00:21 id_rsa
-rw-------. 1 root root  403 May 15 00:21 id_rsa.pub
```

