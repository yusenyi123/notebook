# openwrt配置文件详解

## 1.etc/config/network

```

config interface 'loopback'
	option ifname 'lo'
	option proto 'static'
	option ipaddr '127.0.0.1'
	option netmask '255.0.0.0'

config globals 'globals'
	option ula_prefix 'fd6e:2842:d344::/48'

config interface 'lan'
	option type 'bridge'
	option proto 'static'
	option netmask '255.255.255.0'
	option ip6assign '60'
	option _orig_ifname 'eth0 wlan0 wlan1-1'
	option _orig_bridge 'true'
	option ipaddr '192.168.1.88'
	option ifname 'eth0'

config interface 'wan'
	option ifname 'eth1'
	option proto 'dhcp'
	option type 'bridge'

config interface 'wan6'
	option ifname 'eth1'
	option proto 'dhcpv6'

config switch
	option name 'switch0'
	option reset '1'
	option enable_vlan '1'

config switch_vlan
	option device 'switch0'
	option vlan '1'
	option ports '1 2 3 4 0'

config interface 'wwan'
	option proto 'dhcp'


```

```
config interface 'lan'  
	option type 'bridge'
	option _orig_ifname 'eth0 wlan0 wlan1-1'
	option _orig_bridge 'true'
	
	option ifname 'eth0'
	option proto 'static'
	option netmask '255.255.255.0'
	option ip6assign '60'
	option ipaddr '192.168.1.88'
	

解析配置文件意义：
config interface 'lan'  创建一个接口名字为lan

option type 'bridge'
option _orig_ifname 'eth0 wlan0 wlan1-1' 
option _orig_bridge 'true'
设置bridge模式，eh0 wlan0 wlan1-1三个物理网卡进行桥接互联(这里的桥接互联表示统一采用该接口的配置，并且三者相连在一起，wlan1-1是由wlan1产生的一个虚拟无线网卡接口，一个无线网卡可以产生多个虚拟网卡接口)

option ifname 'eth0'
option proto 'static'
option netmask '255.255.255.0'
option ip6assign '60'
option ipaddr '192.168.1.88'


设置接口采取静态协议，然后设置一些静态协议的参数
	
```

![image-20200821183427203](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827164702.png)

```
root@LEDE:~# ifconfig 
br-lan    Link encap:Ethernet  HWaddr 0E:62:20:20:EE:D0  
          inet addr:192.168.1.88  Bcast:192.168.1.255  Mask:255.255.255.0
          inet6 addr: fd6e:2842:d344::1/60 Scope:Global
          inet6 addr: fe80::c62:20ff:fe20:eed0/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:23518 errors:0 dropped:0 overruns:0 frame:0
          TX packets:14214 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:2664044 (2.5 MiB)  TX bytes:5411371 (5.1 MiB)

br-wan    Link encap:Ethernet  HWaddr 46:F5:A1:9F:6A:32  
          UP BROADCAST MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

eth0      Link encap:Ethernet  HWaddr 0E:62:20:20:EE:D0  
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:58112 errors:0 dropped:85 overruns:0 frame:0
          TX packets:58285 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:7873557 (7.5 MiB)  TX bytes:32113673 (30.6 MiB)
          Interrupt:5 

eth1      Link encap:Ethernet  HWaddr 46:F5:A1:9F:6A:32  
          UP BROADCAST MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)
          Interrupt:4 

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:621 errors:0 dropped:0 overruns:0 frame:0
          TX packets:621 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1 
          RX bytes:50991 (49.7 KiB)  TX bytes:50991 (49.7 KiB)

wlan0     Link encap:Ethernet  HWaddr DE:88:4E:42:3D:C4  
          inet6 addr: fe80::dc88:4eff:fe42:3dc4/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:8 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:0 (0.0 B)  TX bytes:952 (952.0 B)

wlan1     Link encap:Ethernet  HWaddr 00:03:3F:13:03:28  
          inet addr:192.168.1.204  Bcast:192.168.1.255  Mask:255.255.255.0
          inet6 addr: fe80::203:3fff:fe13:328/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:437 errors:0 dropped:0 overruns:0 frame:0
          TX packets:25 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:56372 (55.0 KiB)  TX bytes:3400 (3.3 KiB)

wlan1-1   Link encap:Ethernet  HWaddr 02:03:3F:13:03:28  
          inet6 addr: fe80::3:3fff:fe13:328/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:398 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:0 (0.0 B)  TX bytes:44707 (43.6 KiB)
```

