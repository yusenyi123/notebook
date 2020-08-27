# openwrt防火墙界面的设置

## 1. 基本设置中的转发规则会改变filter表中forward链的基本规则

### 当基本设置转发设置为拒绝

![image-20200823210001958](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163054.png)

------

![image-20200823210033832](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163101.png)

### 当基本设置转发设置同意

![image-20200823210132989](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163109.png)

------

![image-20200823210117126](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163115.png)





## 2. 区域设置中的选项对防火墙表链规则的改变

### full cone nat开启添加的规则

![image-20200823111642675](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163127.png)

### MSS钳制开启添加的规则

MSS（Maximum Segment Size，最大报文长度），是TCP协议定义的一个选项，MSS选项用于在TCP连接建立时，收发双方协商通信时每一个报文段所能承载的最大数据长度。

![image-20200823112901749](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163133.png)

------

![image-20200823112922998](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163140.png)





### IP动态伪装开启添加的规则

![image-20200823114949440](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163146.png)

------

![image-20200823114907311](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163152.png)



```
无线桥接模式自定义规则，此规则就表示数据包从wlan1流出时候（即数据包的来源mac地址是wlan1的mac地址）开启ip伪装

iptables -t nat  -A POSTROUTING -o wlan1 -j MASQUERADE
```



### 区域之间的转发设置

#### 1.区域对区域之间的规则，在filter表中的forward链增加规则，如果没有这样填写就会采取基本设置中转发的选项 

下图就表示允许newzone中接口出来的数据转发到newzone2中的接口

![image-20200823213710477](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163201.png)



------

上述选项产生的规则

![image-20200823212817355](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163206.png)





下图表示newzone2中接口没有设置允许转发的接口，采用基本设置中转发的策略，==不要被后面的reject给迷惑，后面的reject是表示该区域forward选项选择了reject==

![image-20200823213827509](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163212.png)



------

#### 2.转发/forward选项作用，在filter表中的forward链增加规则

==转发/FORWARD选项作用：对于入口是自身接口，出口是自身接口的数据包  该数据包如何处理==

![image-20200823214121244](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163220.png)

选项为同意时候的规则

![image-20200823212938351](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163226.png)



------

选项为拒绝时候的规则

![image-20200823213158885](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163232.png)

#### 3.入站数据/input 选项，在filter表中的input链增加规则



==入口:表示数据流入当前接口，即表示数据包的目的地址是当前接口==

==出口:表示数据流出当前接口，即表示数据包的源地址是当前接口==



==入站数据/input 选项作用：对于流入当前接口的数据，即数据包的目的mac地址是该接口  该数据包如何处理==



![image-20200823214719829](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163237.png)



------

![image-20200823214810958](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163244.png)

#### 4.出站数据/output 选项，在filter表中的output链增加规则

==出站数据/output 选项作用：对于流出当前接口的数据，即数据包的来源mac地址是该接口  该数据包如何处理==

![image-20200823214931191](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163250.png)



------

![image-20200823215003569](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163257.png)

## 3.状态栏中的防火墙和路由表中的内容

![image-20200823114550347](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163303.png)



















