# adb学习

## ADB(Android Debug Bridge 安卓调试桥)简介

Android Debug Bridge (adb) 是一个Android的命令行工具。可以用来连接模拟器或实际的移动设备。比如 adb logcat, adb shell。Dalvik Debug Monitor Server(DDMS) 后台也是运行的adb来实现监控调试移动设备。

总体而言，adb有两个用途：

- **监控连接设备** ：adb会监控所有已经连接设备(包括模拟器)，譬如设备所处的状态：ONLINE，OFFLINE, BOOTLOADER或RECOVERY。
- **提供操作命令** ：adb提供了很多命令(`adb shell`，`adb pull`)，来实现对设备的操控。这些操作命令在adb的体系里面，都称为“服务”。





Android Debug Bridge (adb) 是一个通用命令行工具，其允许我们与模拟器或连接的 Android 设备进行通信。它可为各种设备操作提供便利，如安装和调试应用，并提供对 Unix shell（可用来在模拟器或连接的设备上运行各种命令）的访问。该工具是一个C/S架构实现的程序，包括三个组件：

- **ADB Client**：运行在PC上，通过在命令行执行adb，就启动了ADB Client程序，Client本质上就是Shell,用来发送命令给Server。发送命令时，首先检测PC上有没有启动Server，如果没有Server，则自动启动一个Server，然后将命令发送到Server，并不关心命令发送过去以后会怎样。
- **ADB Server**：运行于PC的后台进程，用于管理ADB Client和Daemon间的通信
- **ADB Daemon** (即adbd) ：运行在模拟器或移动设备上的后台服务。当Android系统启动时，由init程序启动adbd。如果adbd挂了，则adbd会由init重新启动。

您可以在 `android_sdk/platform-tools/` 中找到 `adb` 工具。







## 几个问题

一、**PC上为什么要有一个ADB Server，而不是ADB Client 和 ADB Daemon 直接通信呢？**

因为 ADB 是一个需要支持多对多架构的工具，一个PC可以连接多台手机设备或虚拟机，一个手机也可以同时连接多台PC。就需要一个统一的Sever管理多个设备的连接。

```
java -jar jeb3_19_keygen_0.2.0b.jar 61641164873316763
adb connect 127.0.0.1:7555
```





