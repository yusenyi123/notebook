# tensorflow2.1 gpu安装

## 参考

https://bbs.cvmart.net/articles/363

## 一些概念

### GPU

显卡是我们平时说的GPU，现在大多数的电脑使用NVIDIA公司生产的显卡；常见的型号有Tesla V100，GTX950M，GTX1050TI，GTX1080等。

### CUDA Driver(显卡驱动) 驱动向下兼容

这个是我们常说的显卡驱动，NVIDIA的显卡驱动程序。





### CUDA(**C**ompute **U**nified **D**evice **A**rchitecture，统一计算架构)根据后面的介绍可以理解成一种思想，或是一种软件体系(让编写的程序能够在gpu上运行)

CUDA是NVIDIA公司所开发的GPU编程模型，它提供了GPU编程的简易接口，基于CUDA编程可以构建基于GPU计算的应用程序。CUDA提供了对其它编程语言的支持，如C/C++，Python，Fortran等语言，这里我们选择CUDA C/C++接口对CUDA编程进行讲解



CUDA是显卡厂商NVIDIA推出的运算平台。CUDA™是一种由NVIDIA推出的通用并行计算架构，是一种并行计算平台和编程模型，该架构使GPU能够解决复杂的计算问题。

```
有人说：CUDA是一门编程语言，像C,C++,python 一样，也有人说CUDA是API。
官方说：CUDA是一个并行计算平台和编程模型，能够使得使用GPU进行通用计算变得简单和优雅。
运行CUDA应用程序要求系统至少具有一个具有CUDA功能的GPU和与CUDA Toolkit兼容的驱动程序。
查看CUDA版本命令：nvcc -V 或nvcc --version或cat /usr/local/cuda/version.txt

需要知道：CUDA和CUDA Driver显卡驱动不是一一对应的，比如同一台电脑上可同时安装CUDA 9.0、CUDA 9.2、CUDA 10.0等版本。
```

CUDA的产生就是英伟达公司为拥有并行计算需求的从业者能够使用GPU而提供的工具，因此我们先来讲讲什么是GPU，它和CPU之间的有什么异同。首先要明确一点，CPU和GPU都可以作为计算机的大脑，两者的组成部分是一致的，如下图所示：

> 绿色：计算单元
> 橙色：存储单元（硬盘+缓存）
> 黄色：控制单元
> ![img](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210325092829.jpeg)

从图中我们可以发现：

- CPU具有很强的控制器，也有很强大的缓存（Cache）
- GPU有很多控制器，很多的计算单元，每个控制器都配有缓存

从两者的架构来看，我们发现，这两者的设计逻辑是不一样的，CPU秉承着低延时性，而GPU的思路是高吞吐量。所谓低延时性，直白来说就是一台挖掘机（控制器），用它的爪子（缓存），一次就能够挖很多的资源（计算）。所谓高吞吐量，直白来说就是一个团的人（控制器），用他们的手（缓存），一次一个个的也能捧很多资源（计算）。所以CPU和GPU用两种不同的思路实现了计算能力最大化。而基于不同的设计思路，两者的应用场景也是大不相同，CPU适合用于逻辑控制，串行的计算场景，而GPU适合用于大规模的并行计算场景。对图像数据的应用就是一种并行计算场景，图像数据由很多像素点组成，各个像素相互独立，并行存在的。，因此GPU最早用于图像渲染等操作。而最近，利用深度学习解决图像问题的场景也很适合用GPU计算。

言归正传，为了让上述的GPU能够发挥作用，CUDA应运而生。CUDA的诞生就是为了让GPU能够有可用的编程环境，使得开发人员可以用程序控制GPU的硬件进行并行计算。所以本质上，CUDA是一个软件体系，如下图所示，该体系结构三部分组成：

言归正传，为了让上述的GPU能够发挥作用，CUDA应运而生。CUDA的诞生就是为了让GPU能够有可用的编程环境，使得开发人员可以用程序控制GPU的硬件进行并行计算。所以本质上，CUDA是一个软件体系，如下图所示，该体系结构三部分组成：

- CUDA驱动API
  可以通过直接操纵硬件来实现GPU的使用，但编程复杂，编程难度大，类似与汇编语言。（函数前缀为`cu`）
- CUDA运行时API
  对驱动API中的操作进行了一次封装，使用起来相对更友好，因此在编程过程中使用会比驱动API的频率要高。需要注意的是不可以和驱动API混合使用。（函数前缀为`cuda`）
- CUDA函数库（官方和第三方）
  为了实现更高级的功能，官方或者第三方开发者提供的针对于某个领域的高级函数库，使得普通开发人员能够快速上手实现定制化功能。比如我们安装CUDA时经常会要连带安装的CUDNN，这就是针对于卷积计算的CUDA函数库，使得深度学习开发者能够很容易的调用CUDA实现深度学习算法的构建。



### CUDA Toolkit(开发能够在gpu上运行的程序(也就是实现cud思想)的的工具包)

CUDA工具包的主要包含了CUDA-C和CUDA-C++编译器、一些科学库和实用程序库、CUDA和library API的代码示例、和一些CUDA开发工具。（通常在安装CUDA Toolkit的时候会默认安装CUDA Driver；但是我们经常只安装CUDA Driver，没有安装CUDA Toolkit，因为有时不一定用到CUDA Toolkit；比如我们的笔记本电脑，安装个CUDA Driver就可正常看视频、办公和玩游戏了）

#### conda安装的cudatoolkit 与Nvidia官方提供的cudatoolkit

```
实际上，Nvidia 官方提供安装的 CUDA Toolkit 包含了进行 CUDA 相关程序开发的编译、调试等过程相关的所有组件。

但对于 Pytorch 之类的深度学习框架而言，其在大多数需要使用 GPU 的情况中只需要使用 CUDA 的动态链接库支持程序的运行( Pytorch 本身与 CUDA 相关的部分是提前编译好的 )，不需要重新进行编译过程。在安装了 cudatoolkit 后，只要系统上存在与当前的 cudatoolkit 所兼容的 Nvidia 驱动，就可以直接运行。

在大多数情况下，上述 cudatoolkit 是可以满足 Pytorch 等框架的使用需求的。但对于一些特殊需求，如需要为 Pytorch 框架添加 CUDA 相关的拓展时( Custom C++ and CUDA Extensions )，需要对编写的 CUDA 相关的程序进行编译等操作，则需安装完整的 Nvidia 官方提供的 CUDA Toolkit.


总结
conda安装的cudatoolkit包含运行时的库
Nvidia官方提供的cudatoolkit包含编译，调试等其他工具
```



### cuDNN(NVIDIA CUDA® Deep Neural Network library )cuda的gpu加速库，额外的库

是NVIDIA专门针对深度神经网络中的基础操作而设计基于GPU的加速库。cuDNN为深度神经网络中的标准流程提供了高度优化的实现方式，例如convolution、pooling、normalization以及activation layers的前向以及后向过程。
CUDA这个平台一开始并没有安装cuDNN库，当开发者们需要用到深度学习GPU加速时才安装cuDNN库，工作速度相较CPU快很多。

CUDNN是基于CUDA的深度学习GPU加速库，有了它才能在GPU上完成深度学习的计算；
来自知乎的解释：CUDA看作是一个工作台，上面配有很多工具，如锤子、螺丝刀等。cuDNN是基于CUDA的深度学习GPU加速库，有了它才能在GPU上完成深度学习的计算。它就相当于工作的工具，比如它就是个扳手。但是CUDA这个工作台买来的时候，并没有送扳手。想要在CUDA上运行深度神经网络，就要安装cuDNN，就像你想要拧个螺帽就要把扳手买回来。这样才能使GPU进行深度神经网络的工作，工作速度相较CPU快很多。
基本上所有的深度学习框架都支持cuDNN这一加速工具，例如：Caffe、Caffe2、TensorFlow、Torch、Pytorch、Theano等。
Caffe可以通过修改Makefile.config中的相应选项来修改是否在编译Caffe的过程中编译cuDNN，如果没有编译cuDNN的话，执行一些基于Caffe这一深度学习框架的程序速度上要慢3-5倍（Caffe官网上说不差多少，明明差很多嘛）。Caffe对cuDNN的版本不是很严格，只要大于cuDNN 4就可以。
查看cuDNN版本：cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2









### 安装tensorflow2.1 gpu版本



#### tensorflow对应cuda版本

![image-20210325093341939](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210325093342.png)

#### 显卡驱动版本对应cuda版本(显卡驱动向下兼容)

![image-20210325091048646](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210325091055.png)



#### 此处我们演示安装tensorflow2.1.0

```
查看上面的关系对应表，
tensorflow2.1.0需要cudatoolkit10.1    cudnn7.6
cudatoolkit10.1 需要显卡驱动版本>=418.96
所以我们先更新我们的显卡驱动，然后安装cudatoolkit10.1和cudnn7.6
```





#### 1.因为显卡驱动向下兼容，所以更新显卡驱动到最新版本就可以安装当前任意版本cuda



#### 2.电脑安装anaconda  安装完后配置一些pip下载源和conda下载源 



#### 3.使用conda工具创建一个tensorflow的工作环境

```
conda create  -n  tf21  python=3.7
```

#### 4.进入创建的环境

```
activate tf21
```



#### 5.安装cudatoolkit10.1和cudnn7.6

```
cudatoolkit和cudnn conda包安装 （这是为了运行tensorflow gpu版本所需要的）
Tensorflow2.1  gpu版本所对应的包是cudatoolkit-10.1和cudnn-7.6.5-cuda10.1 不能装cudatoolkit-10.2 就算装了也需要把一些缺失的dll文件放到相应目录里面去



conda install --use-local D:\IDM下载\cudatoolkit-10.1.243-h74a9793_0.tar.bz2

conda install --use-local D:\IDM下载\cudnn-7.6.5-cuda10.1_0.tar.bz2

--use-local 是本地安装的意思，后面是安装文件的目录
因为这两个包直接安装下载太慢，我用IDM下载器从镜像网站上面下载下来进行本地安装
镜像源：
https://repo.anaconda.com/pkgs/main/win-64/
https://mirrors.bfsu.edu.cn/anaconda/pkgs/main/win-64/


```

#### 6.安装tensorflow cpu版本和tensorflow gpu版本

```
可以先安装一些所需要的包
pip install  h5py

pip install  numpy  pillow  scipy  pandas 



进行tensorflow的安装，他会安装一系列依赖库
我依旧是下载到本地进行安装
镜像源:
https://pypi.tuna.tsinghua.edu.cn/simple/tensorflow-cpu/
https://pypi.tuna.tsinghua.edu.cn/simple/tensorflow-gpu/

pip install D:\IDM下载\tensorflow-2.1.0-cp37-cp37m-win_amd64.whl
pip install D:\IDM下载\tensorflow_gpu-2.1.0-cp37-cp37m-win_amd64.whl

```

#### 7.验证安装是否成功

```
python
import tensorflow as tf


查看gpu版本能否使用
print(tf.test.is_gpu_available())

返回true表示可用
```

![image-20210325094454220](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210325094454.png)