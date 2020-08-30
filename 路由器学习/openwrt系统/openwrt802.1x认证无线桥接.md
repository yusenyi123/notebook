# openwrt802.1x认证无线桥接

参考：http://www.openwrt.pro/post-209.html

## 需要解决的问题：

主要就是openwrt路由器默认的wifi桥接的时候没有802.1x认证的选项

## 解决步骤



### 一些知识

```
wpad wpa_supplicant -b br-wan -i wlan0-1 -D nl80211 -c /etc/staff.conf

wpa_supplicant [ -BddfhKLqqtuvW ] [ -iifname ] [ -cconfig file ] [ -Ddriver ] [ -PPID_file ]
[ -foutput file ]

-b br_ifname,Optional bridge interface name. (Per interface)
 可选网桥接口名称。每个接口（每个接口）

-i  ifname,Interface to listen on. Multiple instances of this option can be present, one per interface, separated by -N option (see below).
要监听的接口。这个选项可以有多个实例，每个接口一个，用-N选项分隔（见下文）。

-D driver,Driver to use (can be multiple drivers: nl80211,wext). (Per interface, see the available options below.)
要使用的驱动程序(可以是多个驱动程序：nl80211、Wext)。(每个接口，请参阅下面的可用选项。)

-f output file,Log output to specified file instead of stdout.
将输出记录到指定文件，而不是标准输出。

-c filename,Path to configuration file. (Per interface)
配置文件的路径

-C ctrl_interface,Path to ctrl_interface socket (Per interface. Only used if -c is not).
Ctrl_interface套接字的路径(每个接口。仅在-c不是时使用)。
```

### 步骤

进行无线桥接802.1X认证需要安装两个软件包：wpa_supplicant与wpa_cli

wpa_supplicant进行认证，wpa_cli进行认证后的操作（查看状态、注销等）。

wpa_supplicant包含在wpad中，但由于OpenWrt已经默认安装wpad-mini，我们需要先卸载wpad-mini再进行安装。
路由器联网情况下运行下列命令

```
opkg update
opkg remove wpad-mini
opkg install wpad
opkg install wpa-cli
```

在/etc/config/wireless文件中添加

```

模板
config wifi-iface 'stacfg'
        option device 'radio0'
        option mode 'sta'
        option network 'wan'        
        option ssid 'UMRnet_staff'
        option encryption 'wpa2'
        option identity 'yourname@staff.uni-marburg.de'
        option password 'yourpassword'
        option eap_type 'peap'
        option auth 'MSCHAPV2'

模板中需要修改的地方
config wifi-iface 'stacfg'   
        option device 'radio0'
        option mode 'sta'
        option network 'wan'        
        option ssid 'UMRnet_staff'      #修改成需要连接的wifi名字
        option encryption 'wpa2         #需要连接的wifi的加密方式
        option identity 'yourname@staff.uni-marburg.de'    #连接wifi时候你的用户名
        option password 'yourpassword'        #连接wifi时候你的密码
        option eap_type 'peap'     #该wifi设置的802.1x的扩展认证协议类型（英語：Extensible Authentication Protocol）
        option auth 'MSCHAPV2'    ##该wifi设置的802.1x的认证协议类型

参数根据实际情况修改上面的配置


        
```

修改完成后执行

```
/etc/init.d/network restart
```

关闭路由器重启
