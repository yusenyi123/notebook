# openwrtUSB手机4G上网

## 1 需要的软件包：kmod-usb-net kmod-usb-net-rndis kmod-usb-net-cdc-ether usbutils udev



```undefined
opkg update

opkg install kmod-usb-net kmod-usb-net-rndis kmod-usb-net-cdc-ether usbutils udev
```

## 2 配置

### 2.1手机通过usb线连接到路由器。

### 2.2在手机上，打开tether功能（usb共享网络）。

### 2.3在路由器端，lsusb，查看是否探测到你的手机：

```ruby
root@OpenWrt:/# lsusb

Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

Bus 002 Device 003: ID 1782:5d21 Spreadtrum Communications Inc.

Bus 002 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
```

![image-20210326215840272](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210326215840.png)







## 3.在路由界面进行配置

![image-20210326221002731](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210326221002.png)

![image-20210326215259635](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210326215901.png)

![image-20210326215413035](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210326215904.png)



![image-20210326215751908](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210326215752.png)