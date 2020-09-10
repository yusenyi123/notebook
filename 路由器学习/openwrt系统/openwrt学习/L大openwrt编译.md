# L大openwrt编译

## 准备

### 1.选择Ubuntu 18 LTS x64虚拟机环境

### 2. 能够科学上网（下载包需要用到，非常关键，不然会编译错误）





## 安装所需软件

```

sudo apt-get update 



sudo apt-get -y install build-essential asciidoc binutils bzip2 gawk gettext git libncurses5-dev libz-dev patch python3 python2.7 unzip zlib1g-dev lib32gcc1 libc6-dev-i386 subversion flex uglifyjs git-core gcc-multilib p7zip p7zip-full msmtp libssl-dev texinfo libglib2.0-dev xmlto qemu-utils upx libelf-dev autoconf automake libtool autopoint device-tree-compiler g++-multilib antlr3 gperf wget swig rsync


```

## 下载源码，选择需要编译的架构

```
git clone https://github.com/coolsnowwolf/lede
cd lede
./scripts/feeds update -a
./scripts/feeds install -a
make menuconfig
```



## 下载dl库（需要科学上网，这一步非常关键）

```


make -j8 download V=s

-j8是指使用8个线程下载，理论上是数字越大下载越快，但似乎有个上限，实测5线程以上其实速度相差不了多少，在网络好的情况下，基本在5分钟以内能下载完。


检查文件完整性
find dl -size -1024c -exec ls -l {} \;

如果存在这样的文件可以使用
find dl -size -1024c -exec rm -f {} \ ;命令将它们删除，然后重新执行make download下载并反复检查，确认所有文件完整可大大提高编译成功率，避免浪费时间。
```



## 编译

```
make V=s -j1
V=s：输出详细日志，用于编译失败时找出错误。
-j1：使用单线程编译（首次编译单线程成功率更高），其实不写也行，会自动选择线程
```

