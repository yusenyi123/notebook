# win7原版映像中添加usb3.0驱动

```
dism /参数名:参数值


//将boot.wim挂载在我们创建的windows文件夹中。
dism /mount-wim /wimfile:boot.wim /index:2 /mountdir:windows
//将drivers目录下的所有驱动添加到windows文件中映像中
dism /image:windows /add-driver:drivers /recurse
//挂载在windows下的文件保存并卸载
dism /unmount-wim /mountdir:windows /commit
​
dism /mount-wim /wimfile:install.wim /index:4 /mountdir:windows
dism /image:windows /add-driver:drivers /recurse
dism /unmount-wim /mountdir:windows /commit
```

