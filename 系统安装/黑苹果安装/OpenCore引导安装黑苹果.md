# OpenCore引导安装黑苹果

## 参考

教程里涉及到的链接：
1、macOS镜像下载：

https://blog.daliansky.net/

https://www.applex.net/pages/macos/

文章列表：[https://blog.daliansky.net/categories/%E4%B8%8B%E8%BD%BD/](https://blog.daliansky.net/categories/下载/)

下载地址：https://mirrors.dtops.cc/iso/MacOS/daliansky_macos/





2、启动盘制作工具balenaEtcher下载：https://www.balena.io/etcher/
3、OpenCore下载：https://github.com/acidanthera/OpenCorePkg/releases
4、OpenCore驱动下载：https://dortania.github.io/OpenCore-Install-Guide/ktext.html
5、OpenCore编辑器：https://github.com/ic005k/QtOpenCoreConfig/releases
6、OpenCore排错：https://opencore.slowgeek.com/
7、磁盘精灵：https://www.diskgenius.cn/







https://www.tmstweaks.com/post/2020/asus-zephyrus-m15-hackintosh/#wifibluetooth

## EFI文件夹制作

### 1.Driver文件夹下内容

#### HfsPlus.efi (必备 )

看HFS卷（即macOS安装程序和恢复分区/映像）所需。请勿混用其他HFS驱动程序

#### OpenRuntime.efi (必备)

替换原先的AptioMemoryFix.efi,用作OpenCore的扩展，以帮助修补boot.efi以获得NVRAM修复和更好的内存管理。





##### 对应 legacy引导方式还需要额外的.efi文件(这里不使用legacy,不进行介绍)



### 2.Kexts文件夹下内容

#### VirtualSMC.kext(必备)

- 模拟真实Mac上的SMC芯片，没有此MacOS将无法启动
- 另一种选择是FakeSMC，它的支持可能更好，也可能更差，最常用于传统硬件。
- 需要OS X 10.6或更高版本



##### VirtualSMC额外的插件

###### SMCProcessor.kext

用于监视CPU温度，在基于AMD CPU的系统上不起作用

###### SMCSuperIO.kext

用于监视风扇速度，在基于AMD CPU的系统上不起作用

###### SMCBatteryManager.kext

用于测量笔记本电脑上的电池读数，台式电脑不能使用







#### Lilu.kext(必备)

一个用于修补许多进程的kext，这是AppleALC、WhateverGreen、VirtualSMC和许多其他kext所需的。没有Lilu，他们就不会工作。



##### Lilu的插件

###### CpuTscSync.kext

#### WhateverGreen.kext (图像必备)

用于图形补丁DRM，boardID，帧缓冲区修复等，所有GPU均可从此kext中受益。

#### AppleALC.kext (声卡必备)

用于AppleHDA修补，可支持大多数板载声音控制器

AMD 15h/16h可能有问题，Ryzen/ThreadRipper系统很少支持麦克风

#### RealtekRTL8111.kext (以太网卡驱动，我电脑以太网卡是Realtek)

电脑以太网卡:Realtek Gaming 2.5GbE Family Controller 

注意：有时最新版本的kext可能无法在您的以太网上正常工作。如果您看到此问题，请尝试旧版本。

### 3. ACPI文件夹下内容