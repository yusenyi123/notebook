# openwrt中继的一些思考



## 关于openwrt接口的概念

![image-20200824204253906](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827164131.png)



==这里的接口我通常理解成一个虚拟网卡，因为接口经常会和物理上的端口弄混==

正常情况下，每个接口都会设置端口，可以设置桥接端口（桥接端口的意思就是多个端口在物理上连接在了一起）

这里的端口也不仅仅只是物理端口，还包含虚拟端口（虚拟端口链接到一个物理端口，一个物理端口可以生成多个虚拟端口，虚拟端口是依靠程序来进行控制的）

==接口同连接在该接口设置的端口上的设备构成一个局域网，也就是说每个接口的ip应当属于不同的网段==

==正常情况下，不同的接口属于不同的局域网，这时候数据包会根据路由表  被转发进程 分配 流向不同接口的局域网==

==但当两个接口设置成同样的网段的时候，尽管路由表依然有路由记录，但转发进程不会进行转发了，因为转发进程只会转发不同网段的数据，两个接口设置同一个网段是不符合常规路由器中接口设置，在packet tracer中禁止一个路由器的任意两个接口的ip设置同一个网段，但在openwrt中运行接口可以设置同网段的ip，但是因为是同网段的原因，这些接口会被隔离，路由器的转发进程不会进行转发，转发进程只针对在不同网段直接数据包的流向==



==那如何实现在openwrt中两个同网段接口构成的局域网直接数据流通，使用relayd进程 就可以实现同网段接口数据包的流通，这个relayd进程的作用就好像一根导线 连通了两个交换机（这两个交换机就是那两个同网段接口构成的局域网，我把他们看成两个分离的交换机，但他们交换机的局域网网段是相同的）==

![image-20200824212127973](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827164148.png)

## padavan和openwrt无线中继的区别

openwrt使用这种方式进行无线中继，主路由器中的设备是可以访问到副路由器中的其他设备（无法访问副路由器对副路由器进行设置）



padavan的无线中继是全部设备真正的在同一个局域网中



## openwrt无线中继

在openwrt中，用无线网卡搜索wifi信号加入一个wifi的时候，会产生一个虚拟的wifi客户端端口，但这个wifi客户端端口不能和其他的端口进行桥接构成桥接端口,如果和其他端口一起设置了桥接端口那么这个端口就会断开和主路由器wifi的信号连接，（所谓桥接，作用类似在物理上这些端口互相连通就好像是一个交换机上的接口，桥接之后，连接在这些端口上的设备只要ip设置合理都是可以互相访问的，因为他们在物理上是连接在一起的）

按照下图设置，客户端的wifi断开就会断开

![image-20200824100457342](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827164154.png)

也正是因为不能使用桥接，导致这个虚拟的wifi客户端端口不能和本身路由器上的其他端口构成交换机，也就无法和连接在其他端口上的设备构成局域网了（这种情况下就只能变成无线桥接的方式，无线中继的思路就不对了）

![image-20200824105054337](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827164200.png)





一堆端口（真实的物理端口，或者由物理端口产生的虚拟端口（虚拟端口链接在一个真实的物理端口，也起到相同作用，是程序为了扩展端口能够把一个真实的物理端口扩展成许多个虚拟的物理端口））桥接的作用可以理解成是构成了一个交换机

openwrt中的一个网络就是一个虚拟网卡+连接在这个虚拟网卡下的端口

![image-20200824111541778](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827164206.png)

![image-20200824111123587](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827164217.png)



路由器根据虚拟网卡所在的局域网自动生成路由表，当一个数据包到来的时候就根据路由表进行查询

![image-20200824105251402](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827164211.png)

```

命令查看的路由表
root@OpenWrt:~# route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         192.168.1.1     0.0.0.0         UG    0      0        0 wlan1
192.168.1.0     0.0.0.0         255.255.255.0   U     0      0        0 wlan1
192.168.11.0    0.0.0.0         255.255.255.0   U     0      0        0 br-lan


路由记录讲解
0.0.0.0         192.168.1.1     0.0.0.0         UG    0      0        0 wlan1

如果数据包的目的网络是0.0.0.0 子网掩码是0.0.0.0    需要发送给网关192.168.1.1   该数据包从wlan1这个接口发出
```



## 刷机的时候刷坏了，使用编程器直接对闪存芯片进行刷写

win10 在联网的情况下当编程器插上的时候会自动安装驱动

![image-20200825114539561](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827164224.png)

![image-20200825114615331](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827164233.png)

![image-20200825103855295](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827164242.png)



![image-20200912211445831](C:\Users\sen\AppData\Roaming\Typora\typora-user-images\image-20200912211445831.png)