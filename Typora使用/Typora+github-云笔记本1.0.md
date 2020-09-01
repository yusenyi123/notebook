# Typora+github-云笔记1.0版本

## 1. Typora介绍

Typora 是一款**支持实时预览的 Markdown 文本编辑器**。它有 OS X、Windows、Linux 三个平台的版本，并且由于仍在测试中，是**完全免费**的。下载地址：https://www.typora.io/

### 关于 Markdown

Markdown 是用来编写结构化文档的一种纯文本格式，它使我们在双手不离开键盘的情况下，可以对文本进行一定程度的格式排版。

由于目前还没有一个权威机构对 Markdown 的语法进行规范，各应用厂商制作时遵循的 Markdown 语法也是不尽相同的。其中比较受到认可的是 [GFM 标准](https://github.github.com/gfm/)，它是由著名代码托管网站 [GitHub](https://github.com/) 所制定的。Typora 主要使用的也是 GFM 标准。同时，你还可以在 `文件 - 偏好设置 - Markdown 语法偏好 - 严格模式` 中将标准设置为「更严格地遵循 GFM 标准」。具体内容你可以在官方的 [这篇文档](http://support.typora.io/Strict-Mode/) 中查看。



## 2.Typora安装和设置

下载地址：https://www.typora.io/#windows

### 2.1 图像设置，在这里设置插入图片的保存规则，选相对路径，在你的笔记目录下建一个文件夹专门存放图片，然后把放图片那个目录设置隐藏，这样在下面菜单中就不会显示了，都是我们的笔记看着很清爽

==图像设置这样做的目的是：为了让我们这托管的网站查看我们的笔记的时候也能看到笔记中的图片==

![image-20200815115209812](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827135434.png)

![image-20200815115251679](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827135449.png  "我的笔记本文件夹概况")



![image-20200815204132133](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827135505.png)



### 2.2 Markdown语法设置，就按照我图片这里设置就行，把大部分功能打开

![image-20200815114737291](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827135456.png)

## 3. Git安装 用github托管我们的笔记

### 3.1 git安装和设置环境变量

[Git安装](../git使用/git安装.md) 

https://git-scm.com/downloads

==如果我们单纯用来做笔记本，我们就下载便携版本的git,下载完解压就能用==

==把git解压目录下的cmd文件夹路径添加到系统变量中== 

### 3.2 生成ssh公钥,使用ssh公钥不需要每次向github提交都输入密码

```
打开git bash 输入下列代码
设置全局的用户名和邮箱，每次提交的时候的信息
git config --global user.email "2597400284@qq.com"
git config --global user.name "sensen"


生成ssh登录公钥和私钥
ssh-keygen -t rsa -C "your_email@example.com"

ssh-keygen -t rsa -C "2597400284@qq.com"

ssh-keygen -t rsa -f  E:\test   -C "test key"

ssh-keygen -t rsa -f  ~/.ssh/test   -C "test key"
代码参数含义：
-t 指定密钥类型，默认是 rsa ，可以省略。
-C 设置注释文字，比如邮箱。
-f 指定密钥文件存储文件名。

执行命令后需要进行3次或4次确认：

1.确认秘钥的保存路径（如果不需要改路径则直接回车）；
2. 如果上一步置顶的保存路径下已经有秘钥文件，则需要确认是否覆盖（如果之前的秘钥不再需要则直接回车覆盖，如需要则手动拷贝到其他目录后再覆盖）；
3.创建密码（如果不需要密码则直接回车）；（该密码是你push文件的时候要输入的密码，而不是github管理者的密码）
当然，你也可以不输入密码，直接按回车。那么push的时候就不需要输入密码，直接提交到github上了
这里我们选择按回车不输出密码
4. 确认密码；



执行完这个代码后在我们的用户目录下会生成一个.ssh的隐藏文件夹，文件夹里面有两个文件id_rsa和id_rsa.pub，前者是私钥，后者是公钥，复制id_rsa.pub的内容添加到github的ssh公钥处
目录.ssh下的文件说明

id_rsa ：存放私钥的文件
id_rsa.pub ：存放公钥的文件
known_hsots ：可以保存多个公钥文件，每个访问过计算机的公钥(public key)都记录在~/.ssh/known_hosts文件中
authorized_keys ：A机器生成的公钥-->放B的机器.ssh下authorized_keys文件里，A就能免密访问B，但是B不能访问A。如果需要两台电脑互相访问均免密码。则需要重复上面的步骤（机器的配置刚好相反）。

ssh在建立连接的时候不指定-i参数会默认寻找 ~/.ssh/id_rsa
若是省略 -i 参数，则 ssh-copy-id 会将默认的密钥 ~/.ssh/id_rsa 对应的公钥交付给远程主机。

```

## quicker脚本：一键获得ssh的公钥

https://getquicker.net/Sharedaction?code=d49471e9-8176-4f5a-d1c3-08d8423ccd55&fromMyShare=true

```
脚本为下列代码

git config --global user.email "youxiang"
git config --global user.name "user"
ssh-keygen -t rsa -C "sshkey"
$sshtext=(cat ~/.ssh/id_rsa.pub)
echo $sshtext
pause
```

```
一些其他杂项知识记录

echo %path%
set PATH="%PATH%E:\git\cmd" 

setx PATH "%PATH%;E:\git\cmd" /m


set命令：这种语法只能在Cmd Shell环境中有效，关闭运行环境环境变量将不保存。
setx命令：永久设定环境变量。setx设置环境变量后，将在新打开的终端中生效，当前终端不会立即生效。
/m 表示添加到系统环境变量

/m 表示添加到系统环境变量
$Env:path=$Env:Path+";E:\git\cmd"  
```

![image-20200815203025258](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827135726.png  ".ssh文件夹下的文件" )

### 3.3 将ssh公钥添加到github的sshkeys中

![image-20200815203507450](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827135521.png)

![image-20200815203153944](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827135752.png)

![image-20200815203229265](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827135529.png)

### 3.4  在github中新建仓库，获取仓库的ssh链接

![image-20200815203907551](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827135541.png)



![image-20200815203948987](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827135808.png)

![image-20200815204033936](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827135549.png)





## 4. 使用quicker 编写脚本进行一键初始化和远程同步，脚本基于以下命令

```
在笔记文件夹目录下 打开git bash 输入下列命令初始化本地仓库
git init 
```



```
将笔记本文件夹下所有文件进行跟踪，提交所有变化

git add -A  提交所有变化

其他命令：
git add -u  提交被修改(modified)和被删除(deleted)文件，不包括新文件(new)

git add .  提交新文件(new)和被修改(modified)文件，不包括被删除(deleted)文件
```

```
生成一个本地库版本

git commit -m "提交注释2113"
```

```
将本地库版本推送到github中
git push   git@github.com:yusenyi123/notebook.git  master


git push  git@gitee.com:yusenyi/notebook.git master


```

### 4.1 quicker一键脚本链接:

https://getquicker.net/Sharedaction?code=bf3da905-e641-4bc8-668d-08d841787f87&fromMyShare=true

![image-20200816114349589](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827135556.png)

### 4.2 一键同步脚本使用

第一次启动脚本会让你输入远程仓库的ssh链接和选择需要上传的笔记文件夹，第一次运行完之后脚本会记录你输入的远程仓库的ssh链接和你选择的文件夹，下次再运行就不需要设置。

==如果要进行重新设置,按住ctrl再运行该活动，就会进入初始化设置状态重新设置ssh链接和选择要上传的文件夹==



如果笔记文件夹没有本地初始化脚本第一次运行会进行初始化（不会上传内容到远程）

如果文件夹已经进行了本地仓库初始化，再次运行脚本就会直接把本地版本上传到远程仓库



## 5. 注意事项

###  5.1 笔记文件名不要包含空格，如果有空格那么上传到github中预览的时候将无法看到图片

### 5.2 笔记文件名不要太长，太长也会出现上述问题







## 6.额外说明-把图片和笔记分开存放

上面的笔记搭建是图片和笔记都存放在本地，然后一起推送到远程仓库，但这样做有个问题，当我们写了比较多的笔记，这时候图片就会占据大量的空间，github一个仓库1G（超过1G会收到邮件），gitee一个仓库500M（gitee免费用户总容量5G）

### Gitee个人用户仓库容量说明

![image-20200827114325657](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827135606.png)



### github个人用户仓库容量说明

https://docs.github.com/en/github/managing-large-files/what-is-my-disk-quota

![image-20200827120912612](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827135616.png)





所以这里我们可以把图片和笔记分开存放，图片存放在多个图片仓库中，笔记就用单独的笔记仓库，这样1G的笔记仓库能够容纳非常多的文本了基本用不完



但上述方式就需要在写笔记的时候先进行图片的上传，使用picgo工具配合typora把图片上传到github仓库



配置文字版说明：https://www.flighty.cn/html/tutorial/20200217_576.html

视频说明：https://www.bilibili.com/video/BV1TE411w758?t=73

