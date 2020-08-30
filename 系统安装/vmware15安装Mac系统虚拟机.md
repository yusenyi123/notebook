# vmware15安装Mac系统虚拟机

## 1.解锁vmware15创建mac系统选项

参考：https://blog.csdn.net/qq_43901693/article/details/103723081

### 1.1安装vmware15，安装很简单，网上教程也很多

### 1.2停止vmware15的5个服务和相关进程，只有这些正在运行的活动和服务停止之后，才能使用解锁文件修改vmware15中的相关文件，从而解锁mac选项

![image-20200828230119061](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200828230126.png)



![image-20200828230309092](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200828230309.png)

### 

![image-20200828230655814](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200828230655.png)

### 1.3 打开unlocker文件夹，选择win-install.cmd文件以管理员身份运行

![image-20200828231158531](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200828231158.png)



### 1.4 命令运行完成后，安装mac系统选项解锁成功，==我们需要重新打开刚才被我们关闭的5个服务==，然后运行vmware15在创建虚拟机界面就能看到

![image-20200828232230130](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200828232230.png)

## 2.安装Mac操作系统

### 2.1修改vmx文件

![image-20200828233453960](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200828233454.png)

在smc.present = "TRUE"后添加smc.version = "0"

![image-20200828233759041](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200828233759.png)



## 2.2 启动虚拟机按步骤安装即可







### 3.开启  安装允许任何来源 选项



macOS 10.13允许任何来源开启方法：如果需要恢复允许“任何来源”的选项，即关闭系统的Gatekeeper，我们可以在“Launchpad”—“其他”—“终端”，在终端中输入命令：```sudo spctl --master-disable```  允许后就能打开允许从任何来源安装了，可以在安全性与隐私界面查看，如下图

![image-20200829003920927](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200829003921.png)

![image-20200829004014238](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200829004247.png)



![image-20200829003658669](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200829003701.png)