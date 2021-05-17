# VMware WorkStation的网络连接

## 参考

https://www.jb51.net/article/105365.htm

https://blog.csdn.net/a745233700/article/details/90230490

## VMware WorkStation的网络连接有哪些设置

![image-20210425100014696](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210425100014.png)

![image-20210425100401376](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210425100401.png)

![image-20210425100959791](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210425100959.png)

## 常用的三者网络连接的模式

### 1.桥接模式(B):直接连接物理网络(Use bridged networking)







![桥接模式](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210425101610.png)

```
使用桥接模式的虚拟机的ip地址和主机的ip处于同一个网段,由路由器进行分配ip
所以采取桥接模式的虚拟机可以被和真机在同一局域网的其他真机访问到
```

#### 该模式有一个额外选项说明:复制物理网络连接状态(P) 

```
如果在笔记本电脑或其他移动设备上使用虚拟机，请选择勾选   复制物理网络连接状态(P)  的选项

勾选该选项后，当您在有线或无线网络之间进行移动时，该设置会导致 IP 地址续订。


举个例子说明
比如你在笔记本上使用VMware软件，开始你主机用有线连接的局域网，开启虚拟机系统（使用桥接），虚拟机系统获取的局域网地址为192.168.1.4。然后你把主机的有线拔掉，连接上同一局域网的wifi时，如果你选择了复制物理网络连接状态这个选项，那你的虚拟机系统的IP不会变化（还是192.168.1.4），如果你没有选择复制物理网络连接状态这个选项，那你的虚拟机系统的IP可能就会发生变化，比如变为192.168.1.5。

简单的说就是，当勾选该选项后，当你断网重新连回上次的网络后，虚拟机的ip地址依旧是上次分配到的ip不会改变

```



#### 真实主机同时进行有线上网和无线上网要指明桥接到哪个网卡

![image-20210514172159571](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210514172159.png)



### 2.NAT模式(N):用于共享主机的IP地址

![NAT模式](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210425101614.png)



```
在NAT模式中，主机网卡直接与虚拟NAT(Network Address Translation，网络地址转换)设备相连，然后虚拟NAT设备与虚拟DHCP服务器一起连接在虚拟交换机VMnet8上，这样就实现了虚拟机联网



VMnet8网卡 被禁用后，虚拟机仍然可以访问外网，但是真机无法ping通虚拟机

Vmnet8网卡的作用就是 主机网卡发送包给VMnet8网卡，然后VMnet8网卡将包通过Vmnet8交换机，最好到达对应的虚拟机


```





### 3.仅主机模式(H):与主机共享的专用网络

![Host-Only模式](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210425101818.png)



```
在此模式下，使用的虚拟网络是VMnet1，采用这种模式虚拟机处于一个独立的网段中。与NAT模式比较可以发现，此模式下虚拟机没有提供NAT服务，所以此时虚拟机无法上网，但可使用Windows系统提供的连接共享功能实现共享上网。虚拟机在此模式下，所谓“与主机共享一个私有网络”，是指主机能与此模式下的所有虚拟机互访，就像在一个私有的局域网内一样可以实现文件共享等功能。如果没有开启Windows的连接共享功能的话，除了主机外，虚拟机与主机所在的局域网内的所有其它电脑之间无法互访。
```





#### 该模式下默认不能连通外网需要进行额外的设置

#### 1.将能够上网的物理网卡的网络共享给VMnet1虚拟网卡

![image-20210425105006916](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210425105007.png)

#### 2.设置VMnet1虚拟网卡的ip地址，子网掩码等(ip地址不能和共享的网卡在同一个网段)

![image-20210425105231931](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210425105232.png)

#### 3.打开虚拟机设置虚拟机的ip地址，子网掩码，网关

![image-20210425105513387](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210425105513.png)





### Internet连接共享 的原理

Internet连接共享或ICS(internet connection sharing)是Windows计算机（Windows 98,2000，Me和Vista）的一项内置功能，它允许多台计算机通过一台计算机上的一个Internet连接连接到Internet。 这是一种局域网（LAN），它使用单台计算机作为其他设备连接到Internet的网关（或主机）。 连接到[网关计算机](https://zhcn.eyewated.com/什么是网络网关？/)或通过[ad-hoc无线网络](https://zhcn.eyewated.com/ad-hoc无线网络的特点和用途/)无线连接的[计算机](https://zhcn.eyewated.com/什么是网络网关？/)可以使用ICS。



　ICS即Internet连接共享(Internet Connection Sharing)的英文简称，是Windows系统针对家庭网络或小型的Intranet网络提供的一种Internet连接共享服务。它实际上相当于一种网络地址转换器，所谓网络地址转换器就是当数据包向前传递的过程中，可以转换数据包中的IP地址和TCP/UCP端口等地址信息。有了网络地址转换器，家庭网络或小型的办公网络中的电脑就可以使用私有地址，并且通过网络地址转换器将私有地址转换成ISP分配的单一的公用IP地址从而实现对Internet的连接。ICS方式也称之为Internet转换连接。



说到这里似乎让人糊涂了，那究竟两者有什么区别呢？在Windows 2000的帮助文件中ICS和NAT分别叫做Internet的转换连接和路由连接，其实说白了ICS就是NAT的简化版，使用ICS无需理解IP地址和路由方面的一些知识，并且提供一种局域网中使用Windows 2000路由器共享Internet的简化配置，不过ICS可能不允许局域网和Internet主机之间所有的IP通信，如《暗黑破坏神》一类的多玩家游戏、实时通讯及其他对等服务，如果在公用Internet上使用专用地址或同时使用同一端口号，这些应用程序就会中止。而NAT的配置需要有关于IP地址和路由配置方面的知识，它的配置比ICS要复杂，它允许在局域网和Internet主机间所有的IP通信。此外，ICS只能使用一个合法的公用IP地址，而NAT可以通过配置地址池的方式使用ISP提供的多个合法的公用IP地址供客户机共享。

```
Operation
ICS routes TCP/IP packets from a small LAN to the Internet. ICS provides NAT services, mapping individual IP addresses of local computers to unused port numbers in the sharing computer. Because of the nature of the NAT, IP addresses on the local computer are not visible on the Internet. All packets leaving or entering the LAN are sent from or to the IP address of the external adapter on the ICS host computer.

Typically, ICS can be used when there are several network interface cards installed on the host computer. In this case, ICS makes an Internet connection available on one network interface to be accessible to one other interface that is explicitly designated as the private network. ICS can also share dial-up (including PSTN, ISDN and ADSL connections), PPPoE and VPN connections.

Starting with Windows XP, ICS is integrated with UPnP, allowing remote discovery and control of the ICS host. It also has a Quality of Service Packet Scheduler component.[1] When an ICS client is on a relatively fast network and the ICS host is connected to the Internet through a slow link, Windows may incorrectly calculate the optimal TCP receive window size based on the speed of the link between the client and the ICS host, potentially affecting traffic from the sender adversely. The ICS QoS component sets the TCP receive window size to the same as it would be if the receiver were directly connected to the slow link. ICS also includes a local DNS resolver in Windows XP to provide name resolution for all network clients on the home network, including non-Windows-based network devices.
```



```
允许其他网络用户通过此计算机的 Internet连接来连接(N)
```

