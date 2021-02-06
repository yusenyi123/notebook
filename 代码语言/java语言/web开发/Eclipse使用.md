# Eclipse使用



## 1.Eclipse导出jar全解



### 参考：

https://blog.csdn.net/siguchou/article/details/108031635

https://blog.csdn.net/haoweilaizoule/article/details/52945122











### 第二个窗口界面中的配置

#### Select options for handling problems选项

![image-20210115234704538](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20210115234704.png)



#### Save the description of this JAR in the workspace选项步骤

![image-20210115233550804](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20210115233550.png)









### 第三个窗口界面中的配置

#### seal the JAR 和  seal some packages 选项

参考：https://blog.csdn.net/cunchi4221/article/details/107471235

```
seal the JAR
密封整个jar,也就是密封整个jar中的全部packages 

seal some packages
密封jar中指定的包


怎么理解密封？


JAR中的Package Sealing功能是在Java 1.2引入的。在生成Jar文件时我们可以指定是否将整个Jar或者其中某几个Package进行密封，如果是将Jar文件整个进行密封，那就意味着其内所有的Package都被密封了。Package一旦密封，那么Java 虚拟机一旦成功装载密封Package中的某个类后，其后所有装载的带有相同Package名的类必须来自同一个Jar文件，否则将触发Sealing Violation安全异常。

密封包意味着，如果任何程序正在使用该jar中被密封包中的类，那么如果在使用与该密封包同包名下的类，那么这个类不能来自其他jar中的同名包下，只能来自当前jar中这个被密封的包，从其他jar文件加载类的任何尝试都会抛出java.lang.SecurityException 



举例说明：
当前有个jar名字是test1.jar，该jar里面有一个com.itany.test的包 包下有一个dog类
这个com.itany.test包是密封

还有另外一个jar名字是test2.jar，该jar里面也有一个com.itany.test的包，包下有一个cat类


我们在编写程序的时候引入了test1.jar去使用在test.jar中com.itany.test下的dog类，引入test2.jar去使用com.itany.test下的cat类

这时候编译不会有错，但运行时会出错，因为test1.jar中的com.itany.test包被密封了，所以当我们要引入test2.jar下com.itany.test下的cat类的时候，因为cat类来自com.itany.test包下，但不是test1.jar中的com.itany.test包
由于密封的效果:如果是来自com.itany.test包下的类，必须来自test1.jar中的com.itany.test,但cat类来自test2.jar中的com.itany.test  所以运行报错抛出java.lang.SecurityException 




概括来说，有两种情况触发关于Sealing的安全异常：

检查当前试图加载的类对应的Package是否已经被JVM装载且密封。如果已经被装载且密封了，但被密封的Package与当前加载的类对应的Package不是来自同一个Jar文件，将触发安全异常。
检查当前试图加载的类对应的 Package是否已经被JVM装载且密封。如果已经被装载但没有被密封，但当前试图加载的类对应的Package确要试图进行密封操作，将触发安全异常。JVM不允许对已经装载但未密封的Package再进行密封操作。
Package Sealing的好处：
Package Sealing所能带来的好处主要是版本一致性. 我们知道Java 在运行时是严格按照classpath中定义的顺序进行装载和检查，尤其是现在Java开源包满天飞, 很有可能你的Java应用程序或者中间件的classpath中会在不同的Jar文件中包含同一个Package的不同版本。这会使得程序运行产生不一致性结果，很难发




```



![image-20210115231505246](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20210115233707.png)







## 2.tomcat服务器设置

![image-20210127000156856](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210206113115.png)