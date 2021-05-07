# 机械革命钛钽plus黑苹果安装

## 参考

https://zhuanlan.zhihu.com/p/347899851

## 1.制作USB安装盘

### 1.1 准备必要的文件

### OpenCore

我们使用OpenCore对mac OS进行引导（现在的黑苹果主流是Clover），需要到这个项目的[github release页](https://link.zhihu.com/?target=https%3A//github.com/acidanthera/OpenCorePkg/releases)下载其相关文件：

![img](https://pic3.zhimg.com/80/v2-8cdc82f8947fe050af9a4d69546541be_1440w.jpg)

可以直接下RELEASE版（下载最新版即可，截图中的版本为撰写教程时的最新版），如果安装过程中报错，建议改为用DEBUG版，这样可以看到更详细的报错日志

### ProperTree

由于配置OpenCore的大部分工作都通过编辑config.plist文件来完成，所以我们需要一个.plist文件编辑器，这里推荐使用[ProperTree](https://link.zhihu.com/?target=https%3A//github.com/corpnewt/ProperTree) 。

- **命令行安装方法如下**：

```powershell
git clone https://github.com/corpnewt/ProperTree
cd ProperTree
.\ProperTree.bat
```

![img](https://pic1.zhimg.com/80/v2-95f401d2c9a276f9f8f18d464c4a32fc_1440w.jpg)

- **非命令行安装方法如下**：

首先进入ProperTree的[GitHub仓库主页](https://link.zhihu.com/?target=https%3A//github.com/corpnewt/ProperTree) ，点击屏幕中间偏右的绿色“Code”按钮，然后点击“Download ZIP”

![img](https://pic2.zhimg.com/80/v2-102f99cf58eb531c69f2fc38f5abcd5d_1440w.jpg)

然后解压缩文件夹，双击“ProperTree.bat”文件即可打开编辑器

![img](https://pic2.zhimg.com/80/v2-bc566d594a697c45aeb9d30c81d2c3c1_1440w.jpg)

### GibMacOS (下载原装mac os)

接下来我们需要使用到[GibMacOS](https://link.zhihu.com/?target=https%3A//github.com/corpnewt/gibMacOS)，这个软件可以帮助我们在windows/linux系统上下载指定版本的原装mac OS系统（mac OS系统用户也可以用它来下载指定版本的系统）。

- **命令行安装方法如下：**

```powershell
git clone https://github.com/corpnewt/gibMacOS
cd gibMacOS
./gibMacOS.bat
```

![img](https://pic4.zhimg.com/80/v2-bf4d5cde4e55fa00c7d2415a474f69a3_1440w.jpg)

- **非命令行安装方法同ProperTree，这里不再赘述**

现在我们准备好了所有必要的文件，可以开始制作USB安装盘了

## 1.2 制作USB安装盘

#### 1.插入U盘（注意提前备份好U盘里的文件，制作USB安装盘会抹掉U盘里原来的内容）

#### 2.进入之前下载好的OpenCorePkg里的Utilities文件夹

#### 3.在该文件夹下打开命令行，输入如下下载Catalina(mac OS 10.15)的代码：

```powershell
# Catalina(10.15)
python macrecovery.py -b Mac-00BE6ED71E35EB86 -m 00000000000000000 download
```

注意，要运行该命令需要提前在电脑中安装好Python

#### 4. 等待一会儿后下载完成：

![img](https://pic4.zhimg.com/80/v2-78d5650f3dd92d169a94bc9fae1446cb_1440w.jpg)

#### 5. 格式化U盘

##### 这里有两种方式：

##### 方法1.使用系统磁盘管理器（只支持UEFI系统）：

右键单击“我的电脑”，点击“管理”，在打开的窗口里点击磁盘管理，找到插入的U盘

![img](https://pic2.zhimg.com/80/v2-270fff9912b19b97791b30f4d795c085_1440w.jpg)

右键格式化为FAT32格式

##### 方法2.使用Rufus（存储容量大于16GB的U盘使用方法一无法格式化为FAT32格式，只能借助外部软件）：

[下载Rufus](https://link.zhihu.com/?target=https%3A//rufus.ie/)
下载完成后双击.exe文件打开，配置如下，然后点击开始

![img](https://pic2.zhimg.com/80/v2-8f5f47df699637e1afca88c5a18d75d5_1440w.jpg)

#### 6. U盘格式化完成后进入U盘根目录，创建一个名为com.apple.recovery.boot的文件夹：

#### ![img](https://pic1.zhimg.com/80/v2-8a7eb1b11e4225b16824ed7f21baeed8_1440w.jpg)

#### 7. 把之前下载的mac OS系统文件复制到文件夹中：

![img](https://pic3.zhimg.com/80/v2-257e5dd101602634d8f2e32f6cec99f2_1440w.jpg)