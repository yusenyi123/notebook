# vmware15安装Mac系统

## 1.解锁vmware15创建mac选项

参考：https://blog.csdn.net/qq_43901693/article/details/103723081

### 1.1安装vmware15，安装很简单，网上教程也很多

### 1.2停止vmware15的5个服务和相关进程，只有停止之后才能使用解锁文件进行解锁

![image-20200828230119061](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200828230126.png)



![image-20200828230309092](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200828230309.png)

### 

![image-20200828230655814](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200828230655.png)

### 1.3 打开unlocker文件夹，选择win-install.cmd文件以管理员身份运行解锁文件

![image-20200828231158531](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200828231158.png)



### 1.4 命令运行完成后，解锁成功，我们重新打开刚才被我们关闭的5个服务后，运行vmware15在创建虚拟机界面就能看到

![image-20200828232230130](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200828232230.png)

## 2.安装Mac操作系统

### 2.1修改vmx文件

![image-20200828233453960](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200828233454.png)

在smc.present = "TRUE"后添加smc.version = "0"

![image-20200828233759041](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200828233759.png)