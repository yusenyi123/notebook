# Typora+github  打造自己的云笔记本

## 1. Typora介绍

Typora 是一款**支持实时预览的 Markdown 文本编辑器**。它有 OS X、Windows、Linux 三个平台的版本，并且由于仍在测试中，是**完全免费**的。下载地址：https://www.typora.io/

### 关于 Markdown

Markdown 是用来编写结构化文档的一种纯文本格式，它使我们在双手不离开键盘的情况下，可以对文本进行一定程度的格式排版。

由于目前还没有一个权威机构对 Markdown 的语法进行规范，各应用厂商制作时遵循的 Markdown 语法也是不尽相同的。其中比较受到认可的是 [GFM 标准](https://github.github.com/gfm/)，它是由著名代码托管网站 [GitHub](https://github.com/) 所制定的。Typora 主要使用的也是 GFM 标准。同时，你还可以在 `文件 - 偏好设置 - Markdown 语法偏好 - 严格模式` 中将标准设置为「更严格地遵循 GFM 标准」。具体内容你可以在官方的 [这篇文档](http://support.typora.io/Strict-Mode/) 中查看。



## 2.Typora安装和设置



### 2.1 图像设置，在这里设置插入图片的保存规则，选相对路径，在你的笔记目录下建一个文件夹专门存放图片，然后把放图片那个目录设置隐藏，这样在下面菜单中就不会显示了，都是我们的笔记看着很清爽

![image-20200815115209812](../assets/Typora Git  云笔记.assets/image-20200815115209812.png)

![image-20200815115251679](../assets/Typora Git  云笔记.assets/image-20200815115251679.png)





![image-20200815114809214](../assets/Typora Git  云笔记.assets/image-20200815114809214.png)



### 2.2 Markdown语法设置，就按照我图片这里设置就行，把大部分功能打开

![image-20200815114737291](../assets/Typora Git  云笔记.assets/image-20200815114737291.png)

## 3. Git安装 用github云托管我们的笔记

[Git安装](../git使用/git安装.md)

```
git init 
```

```
git add -A  提交所有变化

git add -u  提交被修改(modified)和被删除(deleted)文件，不包括新文件(new)

git add .  提交新文件(new)和被修改(modified)文件，不包括被删除(deleted)文件
```

```
git commit -m "提交注释2113"
```

```
ssh-keygen -t rsa -C "your_email@example.com"

ssh-keygen -t rsa -C "2597400284@qq.com"
代码参数含义：
-t 指定密钥类型，默认是 rsa ，可以省略。
-C 设置注释文字，比如邮箱。
-f 指定密钥文件存储文件名。

接着又会提示你输入两次密码（该密码是你push文件的时候要输入的密码，而不是github管理者的密码）
当然，你也可以不输入密码，直接按回车。那么push的时候就不需要输入密码，直接提交到github上了



```

```
git push  git@gitee.com:yusenyi/typora_notes.git   master

git push   git@github.com:yusenyi123/-.git master
```

![123](./test/1.png)

![1234](../1.png)

![123](./测试/1.png)

![123](../测  试/1.png)

