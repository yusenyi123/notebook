# CentOS7无图像界面网络配置

## 参考

https://blog.csdn.net/jiangshuanshuan/article/details/80150706

https://blog.csdn.net/tuntun1120/article/details/65443757

## 安装完成后开启网络

```
#查看网络连接状况
ip addr

# 将/etc/sysconfig/network-scripts/ifcfg-ens33文件中的ONBOOT=no改为ONBOOT=yes
vi /etc/sysconfig/network-scripts/ifcfg-ens33

#查看配置的dns服务器是哪个
cat /etc/resolv.conf 

#重启网络服务
systemctl restart network

 #开机启动网卡
sudo systemctl enable network

#安装一下nano，比vim好用
yum install nano
```

```
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
#BOOTPROTO=dhcp
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens33
UUID=35da066d-954a-4d4b-b433-b0bc914f68b2
DEVICE=ens33
#ONBOOT=no

ONBOOT=yes         #开机自动联网
BOOTPROTO=static    #使用静态网络分配方式
IPADDR=192.168.109.100         #设置的静态固定IP地址
NETMASK=255.255.255.0      # 子网掩码
GATEWAY=192.168.109.2        #网关地址
DNS1=114.114.114.114   #DNS服务器  
```



## 安装图形界面

```
yum  groupinstall   "GNOME Desktop" "Graphical Administration Tools"
ln -sf /lib/systemd/system/runlevel5.target /etc/systemd/system/default.target
reboot
```

## 安装ssh服务器

```
#安装一下nano，比vim好用
yum install nano
#安装网络工具，可以使用ifconfig命令
yum -y install net-tools

# 安装ssh服务器
yum install openssh-server

# 按下图设置
nano /etc/ssh/sshd_config

# 启动ssh服务器服务，这样就可以使用xshell远程连接了
sudo service sshd start

sudo service sshd restart

systemctl enable sshd.service


# 查看是否启动成功
ps -e | grep sshd

# 查看防护墙状态
systemctl status firewalld.service

关闭防火墙： 
systemctl stop firewalld.service


启动防火墙：
systemctl start firewalld.service


关闭防火墙开启启动： 
systemctl disable firewalld.service


开启防火墙开机启动： 
systemctl enable firewalld.service
```

![image-20210427114730065](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210427114730.png)





## 静态网络地址分配配置

修改       /etc/sysconfig/network-scripts/ifcfg-你的网卡名字  该文件

默认网卡是ens33 所以我们修改  /etc/sysconfig/network-scripts/ifcfg-ens33 该文件

```
vim   /etc/sysconfig/network-scripts/ifcfg-ens33

nano  /etc/sysconfig/network-scripts/ifcfg-ens33
```



### 内容修改为

```
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
#BOOTPROTO=dhcp       #注释掉
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens33
UUID=35da066d-954a-4d4b-b433-b0bc914f68b2
DEVICE=ens33
#ONBOOT=no     #注释掉

#新增配置
ONBOOT=yes         #开机自动联网
BOOTPROTO=static    #使用静态网络分配方式
IPADDR=172.25.102.224         #设置的静态固定IP地址
NETMASK=255.255.255.0      # 子网掩码
GATEWAY=172.25.102.1        #网关地址
DNS1=114.114.114.114   #DNS服务器  
```

### 防火墙命令

```

# 重启网络生效
service network restart


查看防火墙状态： 
systemctl status firewalld.service

执行关闭命令： 
systemctl stop firewalld.service

再次执行查看防火墙命令：
systemctl status firewalld.service

执行开机禁用防火墙自启命令： 
systemctl disable firewalld.service

启动：
systemctl start firewalld.service

防火墙随系统开启启动： 
systemctl enable firewalld.service
```





## 静态网络分配报错解决(Restarting network (via systemctl):  Job for network.service failed because the control process exited with error code. See "systemctl status network.service" and "journalctl -xe" for details.)

https://cloud.tencent.com/developer/article/1354933

```
systemctl stop NetworkManager

chkconfig NetworkManager off 

service network restart
```

