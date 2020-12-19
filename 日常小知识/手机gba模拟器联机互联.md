# 手机gba模拟器联机互联

## 1.局域网联机(很简单)

###  准备工具：

####      1. 安卓手机上my boy 模拟器(我使用的是1.8版本)

### 操作：

1.打开my boy模拟器，互联双方打开相同的游戏

![image-20201208125310771](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20201208125317.png)



2.可以选择wifi联机，也可以选择蓝牙联机，一方作为服务端，其他人作为客户端进行联机

这里要注意的是my boy 模拟器开启wifi服务端必须当前手机连接了wifi才能开启，不然会提示当前网络不可用

![image-20201208125350510](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20201208125350.png)







## 2.互联网联机（一般我们家里是没有公网ip的，所以需要一台云服务器进行内网穿透）

###   准备工具：

#### 1.安卓my boy模拟器(我使用的是1.8版本)

#### 2.一台云服务器(拥有公网ip)

#### 3.Termux 软件

Termux 是一个 Android 下一个高级的终端模拟器,开源且不需要 root，支持 apt 管理软件包，十分方便安装软件包，完美支持 Python、 PHP、 Ruby、 Nodejs、 MySQL等。随着智能设备的普及和性能的不断提升，如今的手机、平板等的硬件标准已达到了初级桌面计算机的硬件标准，用心去打造 DIY 的话完全可以把手机变成一个强大的极客工具。

**初始化**

第一次启动Termux的时候需要从远程服务器加载数据，然而可能会遇到这种问题：

```verilog
Ubable to install
Termux was unable to install the bootstrap packages.
Check your network connection and try again.
```

原因是Termux官方远程的服务器地址是: http://termux.net/bootstrap/，这个网站位于国外，国内连接网络不畅

![img](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20201208140947.png)

目前解决方法有两种：

1. VPN 全局代理 （成功率很高）

2. 如果你是 WiFi 的话尝试切换到运营商流量 （有一定成功率）

3. ① Google Play ② F-Droid ③ 酷安 根据这个顺序重复1、2操作

   

#### 4.frp软件(进行内网穿透)





### 操作：

#### 1.云服务器启动frp服务端

```shell
#查看云服务器cpu架构
uname -m
uname -a
#在云服务器上下载对应该服务器cpu架构的frp软件
wget https://github.com/fatedier/frp/releases/download/v0.33.0/frp_0.33.0_linux_amd64.tar.gz -O ./frp.tar.gz
#创建目录
mkdir ./frp
#解压并重命名文件夹
tar -xzvf frp.tar.gz -C ./frp --strip-components 1

#修改frp服务端的配置文件
nano ./frp/frps.ini


#运行frp服务端
nohup ./frps -c frps.ini >/dev/null 2>&1 &
```

##### 服务器端配置文件示例

```
[common]
#服务器端端口
bind_port=7000
#客户端连接凭证
privilege_token=5201314
#最大连接数
max_pool_count=10

#访问客户端web服务自定义的端口号
vhost_http_port = 5200

#服务器看板的访问端口
dashboard_port=7500
#服务器看板账户
dashboard_user=admin	
dashboard_pwd=admin

```



#### 2.手机上启动frp客户端

```shell
#下载openssh,可以开启ssh后,在电脑上使用xshell远程连接手机进行设置更加方便
pkg install openssh
#下载nano文本编辑工具
pkg install nano


#修改ssh认证密码
passwd

#开启ssh,开启后可以电脑使用xshell远程连接
sshd


#查看手机cpu架构，然后选择下载合适的frp软件
uname -m
uname -a

#下载frp软件，我手机是aarch64架构也就是arm64架构
wget https://github.com/fatedier/frp/releases/download/v0.33.0/frp_0.33.0_linux_arm64.tar.gz -O ./frp.tar.gz

#创建目录
mkdir ./frp
#解压并重命名文件夹
tar -xzvf frp.tar.gz -C ./frp --strip-components 1

#修改配置文件
nano ./frp/frpc.ini

#在手机上启动frp客户端（先在云服务器上启动frp服务端，云服务器frp服务端配置很简单）
nohup ./frp/frpc -c ./frp/frpc.ini >> 123.out  2>&1  &
```

##### 客户端配置文件示例

```
server_addr = 182.92.101.166
server_port = 7000
privilege_token=5201314

[mytcp1]
type = tcp
local_ip = 192.168.1.105
local_port = 6374
remote_port = 6374


#下列是web内网穿透例子,进行gba互联网联机写上面的配置文件就行
[web]
type = http         #访问协议
local_port = 8081   #内网web服务的端口号
custom_domains = repo.iwi.com   #外部访问的域名，只有访问该域名并且端口是服务器端配置的http端口就会进行内网穿透
```

#### 3. 打开my boy模拟器，进行跟局域网联机一样的操作，选择wifi互联模式，一个手机做wifi服务端，另外的就是wifi客户端，只是这里客户端填写服务器ip地址的时候填写云服务器的ip地址，端口填你内网穿透的时候配置文件中写的外部端口(remote_port)

==这里要注意的是my boy 模拟器开启wifi服务端必须当前手机连接了wifi才能开启，不然会提示当前网络不可用==

![客户端连接](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20201208144603.png)





![image-20201208152106519](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20201208152202.png)







# linux下nuhup和&的一些知识

## &介绍

1.后台运行，输出默认到屏幕

2.免疫SIGINT信号，比如Ctrl+c不会杀死程序

3.响应SIGHUP信号，关闭终端(session)发送SIGHUP信号，程序关闭

4.通过jobs和fg重新转到前台运行

 

 

## nohup介绍

1.不挂断运行，输出默认到$HOME/nohup.out文件，忽略输入

2.免疫SIGHUP信号，比如关闭终端(session)后不会杀死程序

3.响应SIGINT信号，Ctrl+c可以关闭程序

 ![image-20201208143347834](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20201208143347.png)





## nohup和&搭配使用

所以两个搭配在一起使用，不会因为Ctrl+c和关闭Shell让程序停止

nohup python -u sss.py &

nohup默认输出在当前命令运行目录下的nohup.out下，也可以进行修改

如

nohup python -u sss.py >123.out &

表示后台不挂断执行sss.py把输出内容覆盖到123.out

\> 表示覆盖123.out文件（清空123.out原先的内容）

nohup python -u sss.py >> 123.out &

\>> 表示追加到123.out文件（在原先123.out内容后开始添加）

 

nohup python -u sss.py >> 123.out  2>&1  &

把错误输出和标准输出都追加到123.out

## 2>&1解析

2>&1 是将标准出错重定向到标准输出，这里的标准输出已经重定向到了 123.out 文件，即将标准出错也输出到 123.out文件中

## 分析 2>&1

0 ,1,2分别代表stdin标准输入，stdout标准输出，stderr标准错误

\>表示重定向

对于2>&1的理解，2就是标准错误，1是标准输出，那么这条命令不就是相当于把标准错误重定向到标准输出么？是的。

为什么是&1而不是1，这里& 符号是什么？& 符号可以理解为引用（reference）。&1 就是对标准输出的引用。

 