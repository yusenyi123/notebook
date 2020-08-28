# git GUI 汉化   git各种中文乱码解决



## 1. 汉化

### 1.1 找到你的Git安装目录下的这个路径:Git\mingw64\share\git-gui\lib



![image-20200814195421033](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827162819.png)

### 1.2 在这个目录下的msgs文件夹中放入汉化文件zh_cn.msg(如果没有msgs文件夹就自己新建一个)

![image-20200814195705559](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/image-20200814195705559.png)

## 2. 中文乱码解决（没有设置过编码，导致中文会显示乱码）

![image-20200814195756072](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827162826.png)

```
git config --global core.quotepath false          # 显示 status 编码 
# core.quotepath的作用是控制路径是否编码显示的选项。当路径中的字符大于0x80的时候，如果设置为true，转义显示；设置为false，不转义。

git config --global gui.encoding utf-8            # 图形界面编码 
git config --global i18n.commit.encoding utf-8    # 处理提交信息编码 
git config --global i18n.logoutputencoding utf-8  # 输出 log 编码 
export LESSCHARSET=utf-8                          # 因为 git log 默认使用 less 分页，所以需要 bash 对 less 命令处理时使用 utf-8 编码 

```

以上命令与下面在对应文件中添加相应数据等效：
在Git安装目录下的 **/etc/gitconfig** 文件中添加

```
[core]
    quotepath = false
[gui]
    encoding = utf-8
[i18n]
    commitencoding = utf-8
    logoutputencoding = utf-8
```


在Git安装目录下的   **/etc/profile**文件中添加

```
export LESSCHARSET=utf-8
```



特殊说明

**gui.encoding = utf-8** 是为了解决 git gui 和 gitk 中的中文乱码问题，如果发现代码中的注释显示乱码，可以在所属项目的根目录中 **.git/config** 文件中添加：

```
[gui]
        encoding = utf-8
```

**i18n.commitencoding = utf-8** 是为了设置 commit log 提交时使用 UTF-8 编码。
**i18n.logoutputencoding = utf-8** 是为了保证在 **git log** 时使用 UTF-8 编码。
**export LESSCHARSET=utf-8** 是为了保证 **git log** 翻页时使用 UTF-8 编码，这样就可以正常显示中文了【配合前面的 **i18n.logoutputencoding** 设置】。



