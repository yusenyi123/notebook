# Typora+github-云笔记本2.0版本

## 1.优化原因

原先的云笔记搭建方式是图片和笔记都存放在本地，然后一起推送到远程仓库，但这样做有个问题，当我们写了比较多的笔记，这时候图片就会占据远程仓库中大量的空间，对于免费用户，github一个仓库1G容量（超过1G会收到邮件），gitee一个仓库500M容量（gitee免费用户总容量5G）

==所以我们要把图片和笔记分别放在不同的地方进行存储，笔记里面写文件的网络地址就可以了==

## 2.优化方式

### 2.1准备软件

#### PicGo软件，一款把图片上传到图床的软件，typora现在的版本可以和这个该软件配合轻松自动上传

如图所示，现在版本可以选择上传服务

![image-20200827174334310](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827174334.png)





#### Typora图像设置中的参数讲解

#### 1.允许根据YAML设置自动上传图片

![image-20200827170518823](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827170518.png)该参数表示根据md文件开头的Front-matter中的配置来设置自动上传

##### 参数说明：

原先版本的typora开启图片自动上传需要在Front-matter中进行设置

如在 macOS 上必须开启这个选项，同时在文章的顶部写下如下的 YAML 配置：

![image-20200827170802640](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827170802.png)

------

![image-20200827184053267](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827184053.png) 这样才可以开启自动上传图片的功能。应该是 Typora 的一个 bug ，现在的版本已经修复了

==现在的版本已经不需要在Front-matter中进行配置来实现自动上传！！！==



==自动上传效果==

------

![typora-upload-image-gif-v2](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827171623.gif)

------



##### Front-matter介绍

Front-matter 是文件最上方以 `---` 分隔的区域，用于指定个别文件的变量，举例来说：

```
---
title: Hello World
date: 2013/7/13 20:46:25
---
```

以下是预先定义的参数，您可在模板中使用这些参数值并加以利用。

| 参数         | 描述                 | 默认值       |
| :----------- | :------------------- | :----------- |
| `layout`     | 布局                 |              |
| `title`      | 标题                 | 文章的文件名 |
| `date`       | 建立日期             | 文件建立日期 |
| `updated`    | 更新日期             | 文件更新日期 |
| `comments`   | 开启文章的评论功能   | true         |
| `tags`       | 标签（不适用于分页） |              |
| `categories` | 分类（不适用于分页） |              |
| `permalink`  | 覆盖文章网址         |              |



------



#### 2.插入时自动转义图片URL                                                     

  ![image-20200827184215523](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827184215.png)

作用:![image-20200827171811826](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827171811.png)

==也就是把路径中的中文进行转义，因为某些软件可能无法识别中文会产生乱码==

### 2.2 实际步骤

#### 1.typora图像设置如下图所示

![image-20200827174334310](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827174334.png)



#### 2.PigGo配置github视频说明：https://www.bilibili.com/video/BV1TE411w758?t=73