# win10资源管理器视觉隐藏磁盘分区

## 参考：

https://jingyan.baidu.com/article/e75057f2f0abfcebc91a8938.html



## 该隐藏分区的效果

当有隐藏磁盘分区的需求时可以通过修改注册表来实现在不删除盘符的情况下使分区不在资源管理器中显示

但不会修改盘符以及影响分区中的数据，仅仅只是在资源管理器中不再显示,可以理解为视觉上的隐藏

适合在保证磁盘分区能正常使用的情况下让电脑使用者看不见某一些分区





## 隐藏单个盘符

### 1.隐藏分区的注册表,根据想要隐藏的盘符填写不同参数,示例代码隐藏的是F盘符（.reg文件   ANSI编码)

```
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer]
"NoDrives"=hex:20,00,00,00
```



![image-20210124135348672](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210206112835.png)







### 2.取消隐藏的注册表

```
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer]

"NoDrives"=hex:00,00,00,00
```





### 3.重启windows资源管理器之后设置生效



## 隐藏多个盘符

### 1.隐藏多个盘符的注册表

隐藏多个驱动器的方法也很简单，以隐藏C盘和D盘为例。查盘符表，C盘对应04000000，D盘对应08000000，然后把他们相加（**注意：这里要用十六进制而非十进制！**）

我们可以调用系统自带的计算器或是百度搜索一个在线计算器帮助我们计算。

04000000+08000000=0C000000

```
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer]

"NoDrives"=hex:0C,00,00,00
```



### 2.取消隐藏的注册表

```
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer]

"NoDrives"=hex:00,00,00,00
```

### 3.重启windows资源管理器之后设置生效