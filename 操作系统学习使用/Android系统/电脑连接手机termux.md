# 电脑连接手机termux

```
 
 # 在手机termux上安装ssh
 pkg install openssh
 
 # 手机上启动sshd，手机作为ssh服务器
 sshd


# 查看用户名
whoami

# 修改登录密码
password

#之后就可以在电脑上使用xshell使用查看到的用户名和修改的登录密码进行连接了
但是需要注意的是手机上termux开启的sshd服务用的是8022端口，而不是常用的22端口
```

![image-20210515224339101](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210515224346.png)





## 更改termux的安装源

```
cd  /data/data/com.termux/files/usr/etc/apt
cp -p sources.list sources.list.bak

nano sources.list 
```

原内容如下

```
#The main termux repository:
deb https://dl.bintray.com/termux/termux-packages-24 stable main
```

修改如下（使用清华源）

```
# The termux repository mirror from TUNA:
deb https://mirrors.tuna.tsinghua.edu.cn/termux stable main

```

```
#更新包列表
apt update 
#更新已经安装的包文件
apt upgrade 
```



## 启动root

```
apt install tsu
#开启root
tsu
```





## 设置手机ip

```
ip address add 192.168.43.100/24 dev wlan1
```

https://android.stackexchange.com/questions/213514/how-can-i-permanently-change-my-hotspot-tethering-ip-address

https://linux.cn/article-3144-1.html
