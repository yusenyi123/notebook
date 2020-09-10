# 编译openwrt

编译环境ubuntu系统下载

https://mirrors.tuna.tsinghua.edu.cn/ubuntu-releases/

## 安装编译所需软件包

```
 在软件更新中切换下载源，然后运行下列命令
 
sudo apt-get update
 
ubuntu 16.04:
sudo apt-get install gcc g++ binutils patch bzip2 flex bison make autoconf gettext texinfo unzip sharutils libncurses5-dev ncurses-term zlib1g-dev gawk asciidoc libz-dev git-core uuid-dev libacl1-dev liblzo2-dev pkg-config libc6-dev curl libxml-parser-perl ocaml-nox


可选：
sudo apt-get install gcc g++ build-essential asciidoc  binutils bzip2 gawk gettext git libncurses5-dev libz-dev patch flex bison make autoconf texinfo unzip sharutils subversion ncurses-term zlib1g-dev ccache upx lib32gcc1 libc6-dev-i386 uglifyjs git-core gcc-multilib p7zip p7zip-full msmtp libssl-dev libglib2.0-dev xmlto qemu-utils automake libtool  -y

-y 参数：即当安装包的时候会询问y/n，这个参数是所有询问默认y，下边不再提醒，在终端输入以上命令时，直接确定，不再要求确认。

sudo apt-get install subversion g++ zlib1g-dev build-essential git python python3 python3-distutils libncurses5-dev gawk gettext unzip file libssl-dev wget libelf-dev ecj fastjar java-propose-classpath


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







## make参数讲解







```
检测编译环境并根据更新自动调整编译配置文件
make defconfig

预下载编译所需的软件包
make download -j8 V=s


检查文件完整性
find dl -size -1024c -exec ls -l {} \;
此命令可以列出下载不完整的文件（根据我多次编译的经验得出小于1k的文件属于下载不完整），如果存在这样的文件可以使用find dl -size -1024c -exec rm -f {} \;命令将它们删除，然后重新执行make download下载并反复检查，确认所有文件完整可大大提高编译成功率，避免浪费时间。
```







```

make menuconfig 
执行完上述命令会进入菜单选项，在菜单选项进行选择

make  V=99

-jN
N为数字，表示开启多线程编译，如make -j6 ，开启6个线程同时编译，对于编译内核有效快速，其他作用比较小


make V=99  V为verbose(冗长)的缩写  make v=s和v=99都不是make的标准参数，是编译openwrt时候特有的，
v=s和V=99作用相同：表示输出所有调试信息

```

