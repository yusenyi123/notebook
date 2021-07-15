# linux系统引导后发生的事情

## 参考

http://www.ruanyifeng.com/blog/2013/08/linux_boot_process.html

https://unix.stackexchange.com/questions/307990/is-gnome-terminal-a-type-of-non-login-shell

https://cloud.tencent.com/developer/article/1130493

https://github.com/CheungChan/blog/blob/master/linux/shell%E7%99%BB%E5%BD%95%E8%AF%BB%E5%8F%96%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%9A%84%E9%A1%BA%E5%BA%8F.md

https://halysl.github.io/2020/04/02/Login-shell-%E5%92%8C-Non-Login-shell-%E7%9A%84%E5%8C%BA%E5%88%AB/

https://ithelp.ithome.com.tw/articles/10232226

https://www.cnblogs.com/pu20065226/p/12340584.html



https://forums.centos.org/viewtopic.php?t=65464



http://blog.leanote.com/post/gaunthan/shell%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E8%AF%BB%E5%8F%96%E6%B5%81%E7%A8%8B

https://leaderli.github.io/2020/05/11/linux%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F/

电脑开机运行引导装入程序，引导装入程序将引导程序装入内存进行运行

引导程序运行将系统内核加载到内存中



对应linux系统，系统的内核文件被存放在/boot文件夹下



系统内核被加载后，会运行第一个进程init进程，init进程会运行一堆开机启动的程序(这些开机启动的程序，它们在Windows叫做"服务"（service），在Linux就叫做"[守护进程](http://zh.wikipedia.org/wiki/守护进程)"（daemon）)



每次开机启动有不同的运行级别，在不同运行级别下，init进程需要启动的程序是不同的，不同的系统设置开机启动级别的方式也不一样



每个运行级别需要启动的程序保存在/etc目录下面，都有一个对应的子目录，指定要加载的程序。(上面目录名中的"rc"，表示run command（运行程序），最后的d表示directory（目录）)

> ```bash
> 　　/etc/rc0.d
> 　　/etc/rc1.d
> 　　/etc/rc2.d
> 　　/etc/rc3.d
> 　　/etc/rc4.d
> 　　/etc/rc5.d
> 　　/etc/rc6.d
> ```

下面让我们看看 /etc/rc2.d 目录中到底指定了哪些程序。

```
$ ls  /etc/rc2.d

README
S01motd
S13rpcbind
S14nfs-common
S16binfmt-support
S16rsyslog
S16sudo
S17apache2
S18acpid
...
　　
```

可以看到，除了第一个文件README以外，其他文件名都是"字母S+两位数字+程序名"的形式。字母S表示Start，也就是启动的意思（启动脚本的运行参数为start），如果这个位置是字母K，就代表Kill（关闭），即如果从其他运行级别切换过来，需要关闭的程序（启动脚本的运行参数为stop）。后面的两位数字表示处理顺序，数字越小越早处理，所以第一个启动的程序是motd，然后是rpcbing、nfs......数字相同时，则按照程序名的字母顺序启动，所以rsyslog会先于sudo启动。

前面提到，七种预设的"运行级别"各自有一个目录，存放需要开机启动的程序。不难想到，如果多个"运行级别"需要启动同一个程序，那么这个程序的启动脚本，就会在每一个目录里都有一个拷贝。这样会造成管理上的困扰：如果要修改启动脚本，岂不是每个目录都要改一遍？

Linux的解决办法，就是七个 /etc/rcN.d 目录里列出的程序，都设为链接文件，指向另外一个目录 /etc/init.d ，真正的启动脚本都统一放在这个目录中。init进程逐一加载开机启动程序，其实就是运行这个目录里的启动脚本。

```
$ ls -l /etc/rc2.d

README
S01motd -> ../init.d/motd
S13rpcbind -> ../init.d/rpcbind
S14nfs-common -> ../init.d/nfs-common
S16binfmt-support -> ../init.d/binfmt-support
S16rsyslog -> ../init.d/rsyslog
S16sudo -> ../init.d/sudo
S17apache2 -> ../init.d/apache2
S18acpid -> ../init.d/acpid
...
```

/etc/init.d 这个目录名最后一个字母d，是directory的意思，表示这是一个目录，用来与程序 /etc/init 区分。













## 两种shell打开方式(方式不同读取的配置文件也不同)

### 查看当前shell是哪种shell

```
echo $0
输出 bash, su  这样的为 Non Login shell

输出-bash, -su 这样的为 Login shell
```



### login shell 



#### 命令行登录过程

```
1.读取系统配置，首先读入 /etc/profile，这是对所有用户都有效的配置  在ubuntu中/etc/profile执行会调用/etc/bash.bashrc文件

2.读取用户配置，然后依次寻找下面三个文件，这是针对当前用户的配置。
　  ~/.bash_profile
　　~/.bash_login
　　~/.profile
　需要注意的是，这三个文件只要有一个存在，就不再读入后面的文件了。比如，要是 ~/.bash_profile 存在，就不会再读入后面两个文件了。
　
　ubuntu系统默认存在　~/.profile文件，不存在　  ~/.bash_profile和　~/.bash_login这个文件
　所以会读取　~/.profile文件
　
3. ~/.profile中存在执行～/.bashrc的命令，所以会执行～/.bashrc文件
　
　因此各脚本执行顺序为：/etc/profile -> (~/.bash_profile | ~/.bash_login | ~/.profile) -> ~/.bashrc -> ~/.bash_logout
```







#### ssh登录过程(和命令行登录相同)



#### 图形化界面登录过程

```
1.首先读入 /etc/profile，这是对所有用户都有效的配置

2.只查找  ~/.profile
```





### non-login shell

```
1.执行 /etc/bashrc 文件 或/etc/bash.bashrc文件
2.执行～/.bashrc文件(~/.bashrc：该文件包含专用于你的 bash shell 的 bash 信息，当登录时以及每次打开新的 shell 时，该该文件被读取。)



 /etc/bashrc或/etc/bash.bashrc对所有用户新打开的bash都生效，但~/.bashrc只对当前用户新打开的bash生效。 



ubuntu图像界面系统中 右键打开终端 默认为non-login shell，可以设置成login shell方式

gnome-terminal打开的shell默认为non-login shell，但该shell的环境变量继承了图形化界面登录时的环境变量，所以输出该shell的环境变量的时候，会看到/etc/profile文件中设置的环境变量会被输出，但是该non-login shell打开时并没有执行/etc/profile文件




```



```
#在终端输入su 用户名 以non-login shell打开
su  用户名


#在终端输入su - 用户名  以login shell 打开
```

