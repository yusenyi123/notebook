# win10系统安装完后的步骤

## 1. 关闭win10自带杀毒软件，断网使用下载的镜像管理员方式安装office2019，安装完成后使用kms工具进行激活

为什么要先进行这一步，首先office2019十分好用，对于pdf也可以直接进行编辑，虽然我很想支持wps，可是wps好多工具需要收费呀

但是如果我们不先进行安装office2019，后面联网安装的时候可能会出现问题，一旦出现问题不容易修复就需要重新安装系统，这样就会很麻烦

在刚装完系统后，断网，用管理员身份运行office2019安装程序基本可以稳稳安装成功

安装成功后使用管理员身份运行kms-神龙激活工具进行office2019的激活，激活的时候不管你安装的什么版本都会变成kms版本，有效期是180天，时间到了需要重新运行激活工具进行激活

现在一些视频，教程说可以永久激活office2019都是骗人的，win10系统可以永久激活，office2019不行

![image-20200820192808649](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163753.png)

## 2.关闭win10自带杀毒软件然后使用激活工具进行激活

彻底关闭win10杀毒软件: 1.联想开发的小工具  2.安装火绒安全卫士（无捆绑，无广告，占用小），安装后win10自带杀毒就会关闭



win10数字激活:HWIDGen_62.01_汉化版.exe（关闭win10自带杀毒后使用  数字激活永久激活）

![image-20200819202723414](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163801.png)

## 3. 安装驱动

现在的新的win10版本安装完成后，只要电脑联网会自动默默安装驱动，过一段时间电脑就会把大部分驱动都安装完，极个别硬件的驱动安装不上可以使用我给的两款无广告单文件的驱动精灵

![image-20200819202545070](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163808.png)

## 4. 安装net 框架和C++运行库

### 4.1 安装.net框架

新的win10系统安装完成后自带.net4.8的框架，但我们依旧要把.net3.5的框架(部分软件需要.net3.5框架)也一起安装上打开，

在控制面板-程序-卸载程序-启用或关闭windows 功能   

选中框架后按确定，然后会跳出让你更新

![image-20200819203203896](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163814.png)

### 4.2 安装c++运行库

许多软件需要各个版本的C++运行库才能正常运行，我们需要把各个版本的C++运行库一起安装上

使用DirectX.Repair_4.0这个给工具一键全部安装

![image-20200819203534636](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827163822.png)

