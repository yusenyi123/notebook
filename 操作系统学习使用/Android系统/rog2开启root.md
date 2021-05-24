# rog2开启root

## 参考



https://www.youtube.com/watch?v=pgqboLXMSuk

https://www.youtube.com/watch?v=uGdSWUK9vNw

https://www.millet.vip/article/rog2-flash.html



## 手机打开USB调试

将手机用数据线连接到电脑，`注意手机接的是侧边的接口`，刷机都是连接侧边才能识别到手机

开机状态在**设置-系统-关于手机-软件信息-版本号**，多次点击版本号进入开发者模式，返回到**设置-系统-开发者模式**中打开USB调试，打开这个是为了方便电脑直接使用adb命令控制手机进入各种模式





## 1.解压asus官网下载的系统固件包(<strong style="color:red;">确保和当前的系统一致</strong>)，将payload.bin放到payload_dumper文件夹下

我是直接将系统回退到下载的固件包，这样保证手机的系统和下载的系统是一致的



固件下载地址

https://www.asus.com.cn/supportonly/ROG%20Phone%20II%20(ZS660KL)/HelpDesk_Download/





## 2.运行payload_dumper.py提取payload.bin中的boot.img

```
activate py38

pip install -r .\requirements.txt


#运行python 命令提取 boot.img
python payload_dumper.py payload.bin
```

## 3.手机上安装Magisk Manager



## 4.将boot.img传到手机上，在手机上运行Magisk Manager获得修复boot.img的修复文件,修复产生的文件会在手机的Download目录下

Magisk Manager修复前需要安装，需要翻墙和更换Magisk Manager的更新通道



## 5.安装ZS660KL_SIGNED_UnlockTool_9.1.0.10_190702_fulldpi.apk，<strong style="color:red;">拆掉SIM卡连接wifi后</strong>，运行该软件解锁手机的bootloader (<strong style="color:red;">解锁后将恢复出厂，清空原先所有数据</strong>)





## 6.在恢复出厂的手机里打开UBS调试

开机状态在**设置-系统-关于手机-软件信息-版本号**，多次点击版本号进入开发者模式，返回到**设置-系统-开发者模式**中打开USB调试，打开这个是为了方便电脑直接使用adb命令控制手机进入各种模式



## 7.手机进入fastboot模式，左侧接口连接电脑，然后使用在电脑上使用fastboot软件 将修复的boot.img刷入



电脑安装360手机助手，用数据线连接手机侧边接口，手机进入fastboot模式会在电脑上提示安装驱动，选择安装后，电脑上的fastboot程序才能识别到设备

https://www.androiddevtools.cn/

```
adb start-server
adb kill-server

adb devices

fastboot devices

#手机进入fastboot模式 刷入修改后的boot.img
#如何进入fastboot模式，在关机状态下，按音量+键  +  开机键
fastboot  flash  boot  ./magisk_patched-22100_0Fovh.img

```

![image-20210516105136968](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210516105144.png)











https://forum.xda-developers.com/t/recovery-3-4-0-0-i001d-official-unofficial-twrp-recovery-for-rog-phone-2-stable.4026801/

https://forum.xda-developers.com/t/magisk-the-magic-mask-for-android.3473445/

https://www.thecustomdroid.com/download-magisk-v21-stable/



https://www.thecustomdroid.com/asus-rog-phone-2-unlock-bootloader-twrp-root-guide/

https://www.thecustomdroid.com/download-install-twrp-recovery-android/

```

#手机处于fastboot模式，数据线连接电脑，电脑执行下列命令进入twrp界面
fastboot boot ./twrp-3.3.1-14-I001D-Pie-mauronofrio.img

#执行完上面命令手机会重启进入twrp的界面
```

