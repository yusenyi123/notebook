# openwrt源码文件夹详解



## 总目录结构

![openwrt目录结构](https://images0.cnblogs.com/blog/563391/201409/141300297779715.png)

## 原始目录文件夹

源码下载后的文件夹

![image-20200911102000836](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200911102008.png)



![原始目录](https://img-blog.csdn.net/20151213153906453)

#### 1. scripts

存放了一些脚本,使用了bash,python,perl等多种脚本语言.编译过程中,用于第三方软件包管理的feeds文件也是在这个目录当中.在编译过程中,使用到的脚本也统一放在这个目录中.

#### 2. tools

编译时,主机需要使用一些工具软件,`tools` 里包含了获取和编译这些工具的命令.软件包里面有Makefile文件,有的还包含了patch.每个Makefile当中都有一句`$(eval $(call HostBuild))`,这表明编译这个工具是为了在**主机**上使用的.

#### 3. config

存放着整个系统的配置文件

#### 4. docs

包含了整个宿主机的文件源码的介绍, 里面还有Makefile为目标系统生成docs.使用`make -C docs/`可以为目标系统生成文档.

#### 5. toolchain

交叉编译链,这个文件中存放的就是编译交叉编译链的软件包.包括:`binutils,gcc,libc`等等.

#### 6. target

openwrt的源码可以编译出各个平台适用的二进制文件,各平台在这个目录里定义了firmware和kernel的编译过程。

#### 7. package

存放了openwrt系统中适用的软件包,包含针对各个软件包的Makefile。openwrt定义了一套Makefile模板.各软件参照这个模板定义了自己的信息，如软件包的版本、下载地址、编译方式、安装地址等。在二次开发过程中,这个文件夹我们会经常打交道.
事实上,通过`./scripts/feed update -a和./scripts/feed install -a`的软件包也会存放在这个目录之中.

#### 8. include

openwrt的Makefile都存放在这里。文件名为 *.mk 。这里的文件上是在Makefile里被include的,类似于库文件.这些文件定义了编译过程.

#### 9. 其他

主要目录就是前面提及的8个,剩下的是单个文件.

##### 9.1 Makefile

> 在顶层目录执行`make`命令的入口文件.

##### 9.2 rules.mk

> 定义了Makefile中使用的一些通用变量和函数

##### 9.3 Config.in

> 在`include/toplevel.mk`中我们可以看到,这是和`make menuconfig`相关联的文件.

##### 9.4 feeds.conf.default

> 是下载第三方一些软件包时所使用的地址

##### 9.5 LICENSE & README

> 即软件许可证和软件基本说明.其中README描述了编译软件的基本过程和依赖文件.

##### 9.6 BSDmakefile

> 针对BSD make的Makefile文件

至此我们把原始目录大致浏览了一遍,下面我们看看生成目录.





## 安装,编译后的文件夹

![image-20200911103456378](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200911103456.png)

![](https://img-blog.csdn.net/20151213171412350)

#### 1. feeds

openwrt的附加软件包管理器的扩展包索引目录.有点绕,简单来说就是下载管理软件包的.默认的`feeds`下载有`packages、management、luci、routing、telephony`。如要下载其他的软件包，需打开源码根目录下面的feeds.conf.default文件，去掉相应软件包前面的#号，然后更新源:
`./scripts/feeds update -a`
安装下载好的包:
`./scripts/feeds install -a`

#### 2. dl

在编译过程中使用的很多软件,刚开始下载源码并没有包含,而是在编译过程中从其他服务器下载的,这里是统一的保存目录

#### 3. build_dir

==软件包都解压到build_dir/里，然后在此编译==

在前面的原始目录中,我们提到了host工具,toolchain工具还有目标文件.openwrt将在这个目录中展开各个软件包,进行编译.所以这个文件夹中包含3个子文件夹:

##### 3.1 host

> 在该文件夹中编译主机使用的工具软件

##### 3.2 toolchain-XXX

> 在该文件夹中编译交叉工具链

##### 3.3 target-XXX

> 在此编译目标平台的目标文件,包括各个软件包和内核文件.

#### 4. staging_dir

==最终安装目录。tools, toolchain被安装到这里==

用于保存在`build_dir`目录中编译完成的软件.所以这里也和`build_dir`有同样的子目录结构.
比如,在`target-XXX`文件夹中保存了目标平台编译好的头文件,库文件.在我们开发自己的ipk文件时,编译过程中,预处理头文件,链接动态库,静态库都是到这个子文件夹中.



#### 5. bin

保存编译完成后的二进制文件,包括:完整的bin文件,所有的ipk文件.

#### 6.tmp

从名字来看,是临时文件夹.在编译过程中,有大量中间临时文件需要保存,都是在这里.

#### 7.logs

这个文件夹,有时可以看到,有时没有.这是因为这个文件夹保存的是,编译过程中出错的信息,只有当编译出错了才会出现.我们可以从这里获取信息,从而分析我们的软件编译为什么没有完成.



#### 上述文件夹产生顺序

我们在编译前面安装非官方包产生feeds文件夹，然后编译的时候会下载软件产生dl文件夹，编译产生 build_dir文件夹，staging_dir文件夹保存 build_dir目录中编译完成的软件,编译完成的文件存放在bin文件夹



 feeds->dl-> build_dir->staging_dir->bin



至此我们把openwrt的目录结构大体浏览了一遍.