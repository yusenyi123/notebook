# 编译openwrt



## ==编译前准备：保证能够科学上网==

### ==因为会下载很多文件，那些文件需要科学上网才行，不然编译会出错，大部分出错就是网络问题导致下载的文件没有下载到==





## 编译环境ubuntu18系统下载，虚拟机安装

https://mirrors.tuna.tsinghua.edu.cn/ubuntu-releases/



## 安装编译所需软件包

```
 在软件更新中切换下载源，然后运行下列命令
 
sudo apt-get update
 
ubuntu 16.04:
sudo apt-get install gcc g++ binutils patch bzip2 flex bison make autoconf gettext texinfo unzip sharutils libncurses5-dev ncurses-term zlib1g-dev gawk asciidoc libz-dev git-core uuid-dev libacl1-dev liblzo2-dev pkg-config libc6-dev curl libxml-parser-perl ocaml-nox


lede大openwrt编译可选：
sudo apt-get install gcc g++ build-essential asciidoc  binutils bzip2 gawk gettext git libncurses5-dev libz-dev patch flex bison make autoconf texinfo unzip sharutils subversion ncurses-term zlib1g-dev ccache upx lib32gcc1 libc6-dev-i386 uglifyjs git-core gcc-multilib p7zip p7zip-full msmtp libssl-dev libglib2.0-dev xmlto qemu-utils automake libtool  -y

-y 参数：即当安装包的时候会询问y/n，这个参数是所有询问默认y，下边不再提醒，在终端输入以上命令时，直接确定，不再要求确认。




ubuntu 18.04:
sudo apt-get install subversion build-essential libncurses5-dev zlib1g-dev gawk git ccache gettext libssl-dev xsltproc zip



```



## 下载openwrt

```
git clone https://git.lede-project.org/source.git lede

git clone git://github.com/openwrt/openwrt.git

git clone https://github.com/openwrt/openwrt.git



获取某个发布版源码，使用如下命令：
git clone https://www.github.com/openwrt/openwrt -b chaos_calmer

git clone https://www.github.com/openwrt/openwrt -b  openwrt-18.06

git clone -b lede-17.01 https://github.com/openwrt/openwrt.git source

(请于Github上验证发布版名称.)
```



## 安装非官方的包（luci等）

```
进入openwrt目录执行
./scripts/feeds update -a
./scripts/feeds install -a
```





## 编译步骤

```



进入菜单选项，选择编译数据，完成后生成编译配置文件
make menuconfig 


预下载编译所需的软件包，这一步非常重要！！！，如果下载错误后续不能正常编译(需要科学上网)
make -j5 download V=s


可选：检查文件完整性
find dl -size -1024c -exec ls -l {} \;

如果存在这样的文件可以使用
find dl -size -1024c -exec rm -f {} \ ;命令将它们删除，然后重新执行make download下载并反复检查，确认所有文件完整可大大提高编译成功率，避免浪费时间。


make -j1 V=99






```

## 我编译花费了三个小时！！！！耐心等待吧，注意网络通畅可以科学上网，不然就会编译出错



## make参数讲解

```
检测编译环境并根据更新自动调整编译配置文件，根据本机编译环境生成默认配置文件
make defconfig

进入菜单选项，选择编译选项，第一次编译选择架构就行了，选择完成后退出保存生成编译配置文件
make menuconfig 


预下载编译所需的软件包
make download -j8 V=s

-jN
N为数字，表示开启多线程编译，如make -j6 ，开启6个线程同时编译，对于编译内核有效快速，其他作用比较小


编译固件，输出所有调试信息,V=s和V=99作用相同，V为verbose(冗长)的缩写，表示输出所有调试信息
make  V=99


检查文件完整性
find dl -size -1024c -exec ls -l {} \;
此命令可以列出下载不完整的文件（根据我多次编译的经验得出小于1k的文件属于下载不完整），如果存在这样的文件可以使用find dl -size -1024c -exec rm -f {} \;命令将它们删除，然后重新执行make download下载并反复检查，确认所有文件完整可大大提高编译成功率，避免浪费时间。
```

