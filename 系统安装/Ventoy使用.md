# Ventoy使用

## 参考



## centos镜像下载

https://zhuanlan.zhihu.com/p/104118123

```
================== i386-pc ======================
GRUB4DOS:
kernel /ipxe.krn vdisk=/MyVdiskDir/Deepin.vdi.vtoy
initrd /vdiskchain

GRUB2:
linux16  (hd0,1)/test123/ipxe.krn vdisk=/MyVdiskDir/Deepin.vdi.vtoy
initrd16 (hd0,1)/test123/vdiskchain


================== x86-64-efi ======================
GRUB2:
chainloader (hd0,1)/test123/vdiskchain vdisk=/MyVdiskDir/Deepin.vdi.vtoy

rEFInd:
loader /vdiskchain vdisk=/MyVdiskDir/Deepin.vdi.vtoy

Systemd-boot:
efi /vdiskchain vdisk=/MyVdiskDir/Deepin.vdi.vtoy
```



## 1.安装centos系统







## 2.在系统中运行vtoyboot脚本

安装完成并启动到 Linux 系统中之后，执行 vtoyboot 脚本。这一步是为了在系统中做一些处理，以支持Ventoy启动。
vtoyboot 是配套 Ventoy 开发的一个项目，单独发布。从 https://github.com/ventoy/vtoyboot/releases 下载压缩包即可。

下载到 Linux 系统中，解压，然后以root权限执行里面的脚本 `sudo sh vtoyboot.sh` 脚本执行完之后，使用 `poweroff` 命令关机。
注意 vtoyboot 会经常更新以支持更多的 Linux 版本以及修复 BUG，所以请使用最新版本。

```
sudo sh vtoyboot.sh
```







grub2引导项

```
menuentry 'ventoycentos'  --class windows {
	insmod part_msdos
	insmod gzio
	insmod part_msdos
	insmod part_gpt
	insmod ext2
	insmod ntfs
	insmod probe
	insmod fat
	insmod chain
	insmod search
		
	   #set root=(hd1,1)
      #  set root=(hd1,gpt2)
	search --no-floppy -f --set=root /vhd/centos/centos79gui/vdiskchain
   chainloader  /vhd/centos/centos79gui/vdiskchain   vdisk=/vhd/centos/centos79gui/centos7gpt.vhd
}
```





1. 
