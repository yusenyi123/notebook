# 解包打包padavan系统



```
sudo nautilus
```

在用户目录下创建firmware文件夹

把modify文件夹下的文件  ，需要解包的固件  ， pppd文件都放在该文件夹下

![image-20200826122931537](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163018.png)

```
依次执行以下命令
sudo chmod 777 -R ~/firmware
cd ~/firmware
~/firmware/modify.sh e RT-AC54U-GPIO-30-xiaomimini-128M_3.4.3.9-099.trx

sudo chmod  777 -R squashfs-root/

mv ~/firmware/squashfs-root/usr/sbin/pppd  ~/firmware/squashfs-root/usr/sbin/pppd.bak
cp ~/firmware/pppd/pppd  ~/firmware/squashfs-root/usr/sbin/pppd
sudo chmod 777 ~/firmware/squashfs-root/usr/sbin/pppd
sudo chown root:root  ~/firmware/squashfs-root/usr/sbin/pppd




mkdir  ~/firmware/squashfs-root/usr/lib/pppd/2.4.7
cp   ~/firmware/pppd/rp-pppoe.so  ~/firmware/squashfs-root/usr/lib/pppd/2.4.7/rp-pppoe.so
sudo chmod 777 -R ~/firmware/squashfs-root/usr/lib/pppd/2.4.7
sudo chown root:root  -R  ~/firmware/squashfs-root/usr/lib/pppd/2.4.7

mkdir ~/firmware/squashfs-root/usr/local && mkdir ~/firmware/squashfs-root/usr/local/lib && mkdir ~/firmware/squashfs-root/usr/local/lib/pppd && mkdir ~/firmware/squashfs-root/usr/local/lib/pppd/2.4.7
cp   ~/firmware/pppd/rp-pppoe.so   ~/firmware/squashfs-root/usr/local/lib/pppd/2.4.7/
sudo chmod 777 -R ~/firmware/squashfs-root/usr/local 
sudo chown root:root -R ~/firmware/squashfs-root/usr/local

cd ~/firmware
~/firmware/modify.sh c newrom.trx

```

![image-20200826123059955](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163028.png)

![image-20200826125725375](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163034.png)

![image-20200826125930498](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163042.png)

