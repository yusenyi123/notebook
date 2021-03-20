# eclipse插件开发

## 参考：

https://cloud.tencent.com/developer/article/1023297

https://www.cnblogs.com/xing901022/p/3903334.html

https://cloud.tencent.com/developer/article/1023156?from=article.detail.1023297

https://blog.csdn.net/it_man/article/details/1351938

https://wenku.baidu.com/view/1e61b08da0116c175f0e48be.html

https://sites.google.com/site/yeyanbojava/Home/os-ecplug



https://www.cnblogs.com/yunxiaguo/p/7372298.html

## Eclipse　RCP application（  RCP Rich Client Platform 富客户端平台）







## 开发一个插件



### 开发知识

#### 1.插件开发的是OverView界面(包含了插件的总览信息，可以在这个界面修改插件信息，测试插件，生成插件)

![image-20210304163434993](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210304163435.png)









#### 2.MANIFEST.MF文件   (保存插件的捆绑信息, 这些信息都对应着插件的overview页面的信息) 



#### 3.plugin.xml文件  (插件的详细设置文档，包含插件的扩展点信息，以及插件自己的信息)



#### 4.build.properties文件 (构建的元素列表,里面包括插件的源文件目录，生成文件目录，还有一些配置信息的引入)