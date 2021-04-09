# Android开发工具相关

## 使用Android studio开发



## 安装Android studio

### 1.安装Android studio





### 2.环境变量配置

#### ANDROID_HOME和和ANDROID_SDK_ROOT和ANDROID_SDK_HOME

```
ANDROID_HOME，指向SDK安装目录。已弃用此方法，请改为使用ANDROID_SDK_ROOT
设置 SDK 安装目录的路径。设置后，该值通常不会更改，并且可以由同一台计算机上的多个用户共享。ANDROID_HOME 也指向 SDK 安装目录，但已弃用。如果您继续使用它，则需遵守以下规则：
如果定义了 ANDROID_HOME 并且其中包含有效的 SDK 安装，系统会使用 ANDROID_HOME 的值而不是 ANDROID_SDK_ROOT 中的值。
如果未定义 ANDROID_HOME，系统会使用 ANDROID_SDK_ROOT 中的值。
如果定义了 ANDROID_HOME，但其中不存在或不包含有效的 SDK 安装，系统会使用 ANDROID_SDK_ROOT 中的值。



ANDROID_SDK_HOME
存储所有配置和AVD内容的用户特定目录的根目录

在PATH环境变量添加

%ANDROID_SDK_ROOT%\tools
%ANDROID_SDK_ROOT%\platform-tools
```







## Android SDK目录详解

### add-ones文件夹

```
add-ones: 里面保存这一些附加的库，第三放公司为Android平台开发的附加功能系统。比如GoogleMaps等等第三方的。（一开始此包内容为空)
```

### bulid-tools文件夹

```
各个版本的SDK编译工具。
对应android studio build.gradle中的buildToolsVersion-- Build Tools Version()
```

### docs文件夹

```
SDK文档，对各种控件。类的官方说明。可以在里面找到所有的开发文档，index.html（为导航页）建议在火狐浏览器脱机状态下打开。
```

### extras文件夹

```
extras：该文件下存放了Google提供的USB驱动，Intel提供的硬件加速附件工具包。（后期存放了Android Support兼容包，使用兼容包版本时最好与SDK版本保持一致）
```

### platforms文件夹

```
platforms：里面是根据APILevel划分的SDK版本/平台，这个文件夹是SDK里面最重要的文件(每个平台的SDK真正的文件),这里就以Android6.0为例，进入后有一个android-23的文件夹，android-23进入后是Android6.0 SDK的主要文件，

其中
data:保存着一些系统资源，
skins:Android模拟器的皮肤，
templates:是工程创建的默认模板，
android.jar:是该版本的主要framework文件。


有时候我们在导入项目的时候发现导入后没有SDK，就是因为这里面没有我们导入项目编译时的SDK 包括android的平台。包含在android.jar库中。你必须指一个平台为你的编译目标。
project.properties里面将target改为platforms里面有的版本重新编译即可。这里面有SDK不同的版本，每个版本下面又有许多文件组成。还有就是如果你再布局中如果编写没有错误，但是视图预览不了，可能是由于你SDK选择的版本有问题。
```

### platform-tools文件夹

```
platform-tools：该文件夹下放了Android平台的相关开发和调试工具,比如adb.exe、sqlite3.exe等。platform-tools保存着一些通用工具，比如adb、和aapt、aidl、dx等文件

注意:这里和platforms目录中tools文件夹有些重复，主要是从android2.3开始这些工具被划分为通用了
```

### tools文件夹

```
tools：这个文件夹下存放了大量Android开发、调试的工具。包括测试、调试、第三方工具。模拟器、数据管理工具等。比如ddms用于启动Android调试工具，比如logcat、屏幕截图和文件管理器，而draw9patch则是绘制android平台的可缩放png图片的工具，而monkeyrunner则是一个不错的压力测试应用，模拟用户随机按键，mksdcard则是模拟器SD映像的创建工具，emulator是Android SDK模拟器主程序，不过从android 1.5开始，需要输入合适的参数才能启动模拟器，traceview作为android平台上重要的调试工具。
```

### sources文件夹

```
sources：这个文件夹下面存放的是Android的源代码。
```

### system-images文件夹

```
system-images：存放的是创建Android虚拟机时的镜像文件(已经编译好的镜像文件,模拟器可以直接加载)。从android-14开始将模拟器镜像文件整理在这里(原来放在platforms下)
```

### licenses文件夹

```
market_licensing作为AndroidMarket版权保护组件，一般发布付费应用到电子市场可以用它来反盗版。
```

### Android SDK Manager已经内置于Android Studio中，所以在“Tool”里是找不到了







## Android studio项目目录详解

### 参考：

https://blog.csdn.net/u013096088/article/details/78310901

https://blog.csdn.net/sinat_31311947/article/details/81084689

https://blog.csdn.net/u011240877/article/details/53798052



### .gradle文件夹(gradle运行时的记录文件夹)

```
.gradle文件夹，他是gradle运行的时候产生的一些记录性的文件。我们不需要关注。
```

### .idea文件夹(idea中项目的配置文件夹)

```
idea中关于项目的配置文件，不需要修改，跟开发编译也没关系，因为我们使用了gradle工具进行编译
```





### gradle文件夹   gradlew文件  gradlew.bat文件

```
在项目根目录下执行gradle wrapper 就能产生上述文件
在Android studio开发工具中，创建项目时会默认创建这些文件
```

![image-20210321155344074](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210321155351.png)





### gradle.properties文件(gradle读取的属性配置文件)

```
Gradle文件的全局性的配置，项目级别的Gradle配置文件
```



### 根目录下build.gradle文件(gradle的项目配置文件)

```
这是Android项目根目录下的build.gradle，此文件名是一个约定名字，gradle将依赖它来构建项目。一个项目可以包括多个工程，gradle可以构建多个工程，每个工程子目录都可以有build.gradle文件，如同make构建工具与Makefile文件的关系。我们做的大部分工作，也就是编写和组织build.gradle文件。
```



### local.properties文件(全局属性文件,默认存放了sdk路径)

```
local.properties文件在Android Studio中是用来配置SDK目录的,也可以在文件中配置一些本地化的变量。这个文件不会被提交到版本控制中。
```

### settings.gradle文件(项目由多模块组成时的配置文件)

```
Setting文件可以说是子项目（也可以说是Module）的配置文件，大多数setting.gradle的作用是为了配置子工程，在Gradle多工程是通过工程树表示的。

setting.gradle 文件在 初始化过程中被执行，构建器通过 setting.gradle 文件中的内容了解哪些模块将被 build，下面的内容表明当前项目中除了 app 模块还有另外一个叫做 “shixinlibrary” 的依赖模块：
include ‘:app’, ‘:shixinlibrary’
注意：单模块项目不一定需要有 setting 文件，但一旦有多个模块，必须要有 setting 文件，同时也要写明所有要构建的模块，否则 gradle 不会 build 不包括的模块。
```

例如： 在Android studio中指定相应的module能在主工程当中使用:

```javascript
include ':demo1app', ':demo2app'

project(':demo1app').projectDir = new File("Demo1\\demo1app") //对应Module的路径
project(':demo2app').projectDir = new File("Demo2\\demo2app")
```

项目工程如下：

![image-20210321160750515](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210321160750.png)

### .gitignore文件(git的配置文件)

```
.gitignore中配置哪些文件不加入版本控制中
```



### app文件夹(模块文件,在里面编写代码)

#### proguard-rules.pro文件

```
proguard是一个用来混淆java代码的开源项目，Android项目把它集成了进来，
```

#### build.gradle文件(gradle的模块配置文件)