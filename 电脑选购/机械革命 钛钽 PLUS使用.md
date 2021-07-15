# 机械革命 钛钽 PLUS使用

## 1.设备硬件信息

![image-20210504155101498](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210504155101.png)



![image-20210504172329466](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210504172342.png)



![image-20210505094537937](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210505094557.png)

## 2.设备BIOS详解 (开机的时候按F2)

### BIOS界面介绍

#### Main界面 (BIOS设备主要信息查看界面)

![image-20210504150455996](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210504150520.png)

#### Advanced界面 （BIOS高级设置界面)

![image-20210504152339523](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210504152404.png)

##### 关于OS Support选项的详解(图片中解释错误)

```
OS Support 有两个选项一个 是legacy OS 模式和 UEFI OS模式

选legacy OS模式   会同时支持MBR分区的系统和GUID的分区的系统，当选legacy OS模式 后,Secure Boot 会被设置成Disaabled无法修改，Launch CSM  会被设置成Enable 无法被修改，即可以同时进行MBR分区引导程序的引导和UEFI引导程序的引导

选UEFI模式后，Secure Boot可以设置Disaabled和Enable    Launch CSM 可以设置Disaabled和Enable


所以我推荐OS Support模式选legacy OS模式 ,这样就能兼容启动多种系统
```





#### Security界面(安全设置界面)

![image-20210504152534193](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210504152603.png)

#### Boot界面(系统引导程序设置界面)

![image-20210504153800458](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210504153800.png)

#### Exit界面(保存设置和退出界面)

![image-20210504154045277](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210504154045.png)