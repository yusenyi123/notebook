# Mac系统安装refind引导程序

## 参考

https://www.youtube.com/watch?v=C5G9tr1JS_Y

https://www.jianshu.com/p/fe78d2036192

## 1.关闭苹果电脑的SIP(System Integrity Protection 系统一致性保护)

```
苹果电脑的SIP开启状态时，我们无法修改EFI分区,因为我们需要改变启动时的引导程序，所以需要关闭SIP然后修改EFI分区下的文件

mac电脑开机的时 按command进入+R进入 恢复模式，在恢复模式下关闭苹果电脑的SIP


在恢复模式下打开终端，输入下列命令，关闭电脑的SIP
csrutil disable

#开启SIP
csrutil enable
```



```
和 Linux 和 Windows 电脑上常用的 GRUB 一样，rEFInd 引导程序能实现诸如自定义的文本或图形化界面、支持更多第三方操作系统或EFI Shell、设置操作系统引导模式、加载EFI驱动以支持更多硬件和文件系统、支持PXE网络引导等功能。

下载：http://www.rodsbooks.com/refind/getting.html

0、重启Mac时按住Option键，进入Recovery恢复分区。在其中运行终端，输入 csrutil disable 禁用系统完整性保护功能（SIP），否则第4步会无权操作。

1、打开一个终端窗口。加载ESP分区，可参考：https://www.office26.com/windows/mount-esp-in-windows-macos.html

2、在Finder中打开EFI分区，打开其中efi文件夹，新建一个 refind 文件夹，并将下载到的rEFInd文件夹中的所有文件释放到这个文件夹中。并根据当前系统的架构，删掉不需要的架构文件夹和文件。例如x64系统上就删除aa64和ia32相关drivers文件夹，以及refind_ia32.efi、refind_aa64.efi这些不需要的文件。

3、将refind.conf-sample文件复制一份，并重命名为refind.conf。（可以使用文本编辑器编辑病配置该配置文件）

4、在终端窗口中运行 sudo bless –mount /Volumes/ESP –setBoot –file /Volumes/ESP/efi/refind/refind_x64.efi –shortform 命令，作用是在电脑的固件NVRAM中写入使用rEFInd作为默认引导程序的信息。

重新启动即可看到Mac采用rEFInd代替了苹果默认的引导程序了，并且还可以自动检测到多种操作系统，具备很多扩展功能！
```

