# 真机启动centos7VHD

```
sudo yum -y install libfuse-dev virtualbox pkg-config
cd /home/sen

git clone https://github.com/NyaMisty/dracut-vdfuseloop.git


如果dir2目录不存在，则可以直接使用
cp -r dir1 dir2
即可。
如果dir2目录已存在，则需要使用
cp -r dir1/. dir2

#将90vdfuseloop文件夹复制到/usr/lib/dracut/modules.d文件夹下
cp -r /home/sen/dracut-vdfuseloop/.   /usr/lib/dracut/modules.d

nano /etc/dracut.conf.d/10-debian.conf


sudo dracut -f  initramfs
```

https://www.ventoy.net/cn/plugin_vtoyboot.html



ventoy方式可以引导在vhd中lvm默认分区安装的centos系统

![image-20210522190455632](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210522190502.png)









![image-20210522222741205](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210522222741.png)



## centos7.9最小安装的分区模式   /挂载的分区设备类型选择标准分区   无法ventoy引导

![image-20210522223431335](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210522223431.png)







## centos7.9最小安装的分区模式  /挂载的分区设备类型选择lvm，文件系统xfs，/boot挂载的分区设备类型选择标准分区，文件系统xfs        可以使用ventoy引导 

![image-20210522225641813](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210522225642.png)

![image-20210522224920865](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210522224920.png)





## /挂载的分区设备类型选择lvm,文件系统为ext4，/boot挂载的分区设备类型选择标准分区，文件系统为ext4   ventoy可以进行引导



![image-20210522231452831](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210522231453.png)
