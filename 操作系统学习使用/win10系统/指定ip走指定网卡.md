# 指定ip走指定网卡

https://blog.fooleap.org/configuring-ip-address-by-specifying-gateway.html

```
ping es.huobipool.com

8.136.41.145

# 设置全部ip的总体路由  metric优先级值越小优先级越高。
route delete 0.0.0.0
route add  0.0.0.0 mask 0.0.0.0 172.25.102.1 metric 10

route delete 8.136.41.145 mask 255.255.255.255 192.168.43.125 
route add 8.136.41.145 mask 255.255.255.255 192.168.43.125 metric 3
route add 8.136.41.145 mask 255.255.255.255 192.168.43.102 metric 3


route change 0.0.0.0 mask 0.0.0.0 172.25.102.1 metric 10

route add 172.25.102.1 mask 255.255.0.0  172.25.102.1 metric 3

route add 8.136.41.145 mask 255.255.255.255 192.168.43.125 metric 3

route add 8.136.41.145 mask 255.255.255.255 192.168.43.189 metric 3


route  change 8.136.41.145 mask 255.255.255.255 192.168.43.125 metric 2



#测试配置的路由是否起到作用
tracert bilibili.com
tracert baidu.com
tracert es.huobipool.com
tracert 8.136.41.145
```

![image-20210514141458078](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210514141458.png)

