#  TFTP和TTL刷机



## TFTP和TLL刷机的使用情景

路由器上电执行CPU(实际是一个SOC System on Chip的缩写，称为芯片级系统，也有称[片上系统](https://baike.baidu.com/item/片上系统)，意指它是一个产品，是一个有专用目标的集成电路，其中包含完整系统并有嵌入[软件](https://baike.baidu.com/item/软件)的全部内容 一般路由器的cpu里面就集成了2.4gwifi芯片，交换机芯片等）内部的程序从闪存加载Bootloader程序到内存中执行，然后通过Bootloader程序运行从闪存加载固件（操作系统）到内存

 



如果Bootloader程序中有tftp服务端程序，此时可以在计算机上用tftp刷机

强大的Bootloader程序中如果不仅有tftp服务端程序并且集成了web服务器如breed，那么我们可以在浏览器上进入控制台直接刷机





如果如果Bootloader程序没有tftp服务端程序，此时我们就需要TTL串口刷机

 

 

## TFTP刷机

当Bootloader程序中有tftp（Trivial File Transfer Protocol,简单文件传输协议）服务端程序

（如果有web服务程序就可以在浏览器直接进行刷写不需要在电脑上安装TFTP程序，下面介绍的是没有web服务程序但有tftp服务端程序的Bootloader）

 

我们可以使用TFTP刷机（不需要拆机）

此时路由器作为TFTP服务器

我们的电脑作为TFTP客户端

我们在电脑上运行TFTP程序以客户端的身份向服务器传送文件进行刷写

 

 

### 硬件准备：

1. 需要刷机的路由器

2. 网线

### 软件准备：

1. tftp程序（路径不要有中文）

![img](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20210101203409.jpg)

2. 需要刷入的固件，和tftp程序同一个目录

 

 

 

 

 

### 实际操作：

1.网线连接路由器和电脑（连接路由器的lan口）

2.将路由器挂起（ 在按住路由器的 RST（复位） 按键不松的情况下 再给路由器进行通电，大概三秒左右指示灯开始闪烁）

3.给电脑配置静态IP，保证和路由器在同一个网段（一般路由器的IP地址都为192.168.1.1，根据自己的情况而定）

4.打开 TFTP软件，选择TFTP客户端 按照下方图片进行设置

![img](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20210101203416.jpg)

5.填写好之后点击 TFTP 上面的上传按键（上传成功会有以下显示）

![img](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20210101203423.jpg)

6.上传成功之后路由器上面的指示灯会全部一起亮一次，待系统正常启动。

 



## TTL刷机

当Bootloader程序中没有tftp服务端程序

 

 

 

### TTL刷机介绍

我们使用TTL串口方式进行刷机（需要拆机）

什么是TTL串口方式刷机？

路由器的PCB板子上有串行通信的引脚可以通过串行通信调试的方式进行刷机，那前面的TTL是什么意思，TTL就是路由器的电路是TTL（晶体管-晶体管逻辑电路），遵循TTL电平标准

 

当要使用电脑对路由器进行串口调试的时候，需要一个USB转TTL模块

为什么需要USB转TTL模块

因为电脑的电平逻辑遵照USB的电平原则，路由器的电平逻辑遵照TTL的电平原则

USB转TTL模块的作用就是把电平转换到双方都能识别进行通信。

所以在用TTL串口方式刷机的时候，需要一个USB转TTL模块

 

在刷机的时候用TTL线（TTL线就是符合TTL电平标准的电线，杜邦线就符合TTL电平标准）连接USB转TLL模块和路由器上的串行通信引脚，然后把模块插在电脑上，完成电脑，USB转TTL模块 ，路由器的连接后，启动路由器，就可以在路由器上进入Bootloader命令模式通过命令获得TFTP服务端上的固件进行刷写

 

此时路由器作为TFTP客户端

我们的电脑作为TFTP服务端

我们在电脑上运行TFTP程序以服务端

 

 

 

 

 

 

 

 

### 硬件准备：

1. USB转TLL模块(ch341a编程器也可以进行TLL刷机)

![img](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20210101203430.jpg)

2. 代刷路由器

3. 网线

### 软件准备：

1. tftp程序（路径不要有中文）

 ![img](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20210101203438.jpg)

2. 串行通讯软件

 有很多支持串行通讯软件，比如putty，xshell，SecureCRT等，这里我使用xshell

 

3. 需要刷写的固件或者Bootloader程序（刷入不死Uboot(比如breed)后再用它刷机），和tftp程序同一个目录

 

### 实际操作：

#### 1.用TTL线（就是杜邦线）连接路由器和USB转TTL模块

按照下面方法接线：
 路由器上TTL接口1号针 – VCC （方形孔，远离电源接口）---空着，不要接线
 路由器上TTL接口2号针 – Tx  --- 接TTL线的RX针
 路由器上TTL接口3号针 – Rx  --- 接TTL线的TX针
 路由器上TTL接口4号针 – GND  --- 接TTL线的GND针

  用网线把路由器和电脑进行连接

==TTL串口只用于发送控制命令，固件下载是通过网线使用TFTP协议进行的==

==此时路由器不上电==



#### 2.把USB转TTL模块插入电脑，安装串口线驱动

win10自动安装了驱动，别的系统可能需要手动安装对应驱动，驱动正确安装后，在设备管理里面，可以看到COMx（x为数字，不同电脑，不同USB-TTL设备    x不同）出现；可以在属性中调节波特率

![img](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20210101203445.jpg)

![img](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20210101204554.jpg)

#### 3.给电脑配置静态IP，保证和路由器在同一个网段（一般路由器的网关IP地址都为192.168.1.1，刷斐讯官方系统的时候网关是192.168.2.1 根据自己的情况而定）

![img](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20210101204711.jpg)

 

#### 4.接下来确认TTL线端口号，在设备管理器-端口处找到端口号 (查看第二步的图片)

 

 

#### 5.在串口通讯程序（这里使用Xshell）中，配置相应的端口名字，波特率（波特率不合适等会界面会乱码），配置成功后点击连接，我们开启这个会话



![img](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20210101204852.jpg)

 

![img](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20210101204901.jpg)

#### 6.运行TFTP程序，选择TFTP服务器模式，选择对应的服务器IP地址，在程序目录下放置需要刷写的固件或者bootloader程序

 

#### 7.进入Bootloader命令模式



##### 不同Bootloader程序进入命令模式方式不同





###### Bootloader程序为 CFE进入命令模式：

在xsheel的会话界面，左手放在键盘的Ctrl+ C上。右手插入路由器电源，电源接通后，左手不停的按下Ctrl+C   

——中断CFE的自动运行命令，进入CFE命令环境。



###### Bootloader程序为 Uboot进入命令模式：

在xsheel的会话界面，左手放在键盘的4上。右手插入路由器电源，电源接通后，左手不停的按下4

——中断Uboot的自动运行命令，进入Uboot命令环境。



Uboot下按不同数字对应不同功能

```
Please choose the operation:
   1: Load system code to SDRAM via TFTP.
   2: Load system code then write to Flash via TFTP.
   3: Boot system code via Flash (default).
   4: Enter boot command line interface.
   7: Load Boot Loader code then write to Flash via Serial.
   9: Load Boot Loader code then write to Flash via TFTP.
```



#### 8.开启TTL可输入(如果TLL串口是不可输入的，需要进行这一步)

有些Uboot默认情况下 TTL 串口是不可输入的
 如果你不做任何设置，直接连上 TTL串口，会发现按什么都没反应。

需要修改 nvram 开启

启动的时候按4进入命令行模式

在命令行模式下输入

```
setenv uart_en 1
saveenv
```



![img](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20210101203504.jpg)

 

 



 

#### 9.开刷

##### CFE刷机：

CFE进入命令行模式后可以直接进行刷机了

![img](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20210101205539.jpg)

 

 



##### UBOOT刷机：

 

TTL串口可输入后，路由器断电，然后继续插上电源,电源接通后，左手不停的按下9

——进入刷bootloader模式（我们把原来的bootloader程序刷出breed，然后通过breed就可以在浏览器界面轻松刷机了）

输入Y，按照下图进行操作

 

![img](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20210101204933.jpg)

 

 

 

 

 

 