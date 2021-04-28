# 伪终端(ptm)原理

## 参考

https://www.cnblogs.com/sparkdev/p/11605804.html

http://www.freeoa.net/osuport/intronix/linux-tty-ptys-console_3316.html

https://evoa.me/archives/20/

https://zdyxry.github.io/2020/04/25/%E5%9C%A8%E7%BB%88%E7%AB%AF%E8%BE%93%E5%85%A5%E5%91%BD%E4%BB%A4%E5%90%8E%E7%B3%BB%E7%BB%9F%E5%81%9A%E4%BA%86%E4%BB%80%E4%B9%88/



https://blog.csdn.net/w1857518575/article/details/82670522

https://segmentfault.com/a/1190000016129862



https://i.linuxtoy.org/docs/guide/ch19s03.html



https://docs.huihoo.com/homepage/shredderyin/x.html

https://caifu.zol.com.cn/204/2045957.html

```
[root@localhost ~]# ll /proc/6321/fd
总用量 0
lrwx------. 1 root root 64 4月  22 11:49 0 -> /dev/null
lrwx------. 1 root root 64 4月  22 11:49 1 -> /dev/null
lrwx------. 1 root root 64 4月  22 11:52 10 -> /dev/ptmx
lrwx------. 1 root root 64 4月  22 11:52 14 -> /dev/ptmx
lrwx------. 1 root root 64 4月  22 11:52 15 -> /dev/ptmx
lrwx------. 1 root root 64 4月  22 11:49 2 -> /dev/null
lrwx------. 1 root root 64 4月  22 11:49 3 -> socket:[70767]
lrwx------. 1 root root 64 4月  22 11:49 4 -> socket:[72283]
lr-x------. 1 root root 64 4月  22 11:49 5 -> pipe:[72286]
l-wx------. 1 root root 64 4月  22 11:52 6 -> /run/systemd/sessions/11.ref
l-wx------. 1 root root 64 4月  22 11:52 7 -> pipe:[72286]
lrwx------. 1 root root 64 4月  22 11:52 8 -> socket:[70877]
lrwx------. 1 root root 64 4月  22 11:52 9 -> socket:[70878]
[root@localhost ~]# 

```

## lsof命令

lsof(list open files)是一个列出当前系统打开文件的工具。



```

#查看/dev/ptmx被多个进程打开的情况
$ sudo lsof /dev/ptmx
```



## ssh远程连接伪终端工作流程

### 登录阶段

1.当进程ssh client请求与sshd建立登陆连接的时候，经过TCP握手以及tls握手之后，确认是一个合法的请求，sshd会fork()一个子进程出来专门服务于这条连接

```
[root@localhost ~]# ps -ef | grep sshd
root       894     1  0 Nov25 ?        00:00:00 /usr/sbin/sshd -D
root      3126   894  0 Nov25 ?        00:00:00 sshd: root@pts/0

```

2.可以看出sshd`894`进程创建了一个子进程`3126`用来处理这条TCP连接。子进程`3126`对`/dev/ptmx`设备文件调用open()操作即打开文件，那么这个进程就会在内存的该进程打开文件表中添加关于`/dev/ptmx`文件的文件描述符，子进程`3126`在`/dev/pts`下创建了一个字符设备文件`/dev/pts/0`，将关于`/dev/ptmx`文件的文件描述符中的内容指向这个创建的字符设备`/dev/pts/0`，后续对`/dev/ptmx`进行read()，write()操作时，根据查找文件描述符上的对应关系，其实是对`/dev/pts/0`进行read(),write()操作



（3）子进程`3126`会再fork()一个子进程`3128`这个一个bash程序运行创建的进程，子进程`3128`的3个描述符（标准输入，标准输出，标准错误）都定向到/dev/pts/0 这个设备文件，并开始循环读取标准输入描述符，即根据设置的映射关系，循环读取/dev/pts/0 这个设备文件，当有其他进程向/dev/pts/0 这个设备文件写入数据时，就会被该`3128`这个bash程序的进程所接收到，这个bash进程就会解释读到的数据执行相应的命令





### 执行命令

（1）当ssh client发出一个`ls`命令，通过TCP连接将该字符串发送给`3126`进程，`3126`进程接收`ls`字符串后，对`/dev/ptmx`文件调用write()操作，这时查找该进程下`/dev/ptmx`文件的文件描述符能够得知需要向`/dev/pts/0`这个文件进行write()操作，那么该字符串就被写入到`/dev/pts/0`这个设备文件

(2)`3128`bash进程是在循环读取标准输入文件描述符的，标准输入文件描述符被定向到了`/dev/pts/0`这个设备文件，即`3128`bash进程不断在循环读取`/dev/pts/0`设备文件中的内容，那么当其他进程向写入/dev/pts/0 这个设备文件数据时，就会被该`3128`这个bash程序的进程所接收到，这个bash进程就会解释读到的数据执行相应的命令，执行命令后会将运行结果对标准输出描述符所对应的文件执行write()操作，因为bash进程的标准输出描述符也会定向到了`/dev/pts/0`设备文件，即会对`/dev/pts/0`这个设备文件调用write()操作

(3)）`3126`进程对`/dev/ptmx`文件调用read()操作，根据前面设定的对应关系，其实是对`/dev/pts/0`调用read()操作，那么`3126`进程就能得到bash进程运行命令后的结果，然后返回给远程客户端





## 图形化界面打开终端(也是伪终端)

```
ubuntu图形化界面中打开终端的gnome-terminal
```

![img](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210422171029.png)







![image-20210422172403388](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210422172403.png)





![image-20210422172440984](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210422172441.png)

```
问题：添加了个用户，出现了一个情形，有的添加用户home目录下有.bashrc等相关文件，有的则没有，机型基本一样，添加命令是同一个，烦恼已来了。
系统启动过程，略去关注一个文件/etc/inittab，这里关注这几行：
# Run gettys in standard runlevels
1:2345:respawn:/sbin/mingetty tty1
2:2345:respawn:/sbin/mingetty tty2
3:2345:respawn:/sbin/mingetty tty3
4:2345:respawn:/sbin/mingetty tty4
5:2345:respawn:/sbin/mingetty tty5
6:2345:respawn:/sbin/mingetty tty6

这里告诉我们有6个字符终端，可以供我们使用，从tty1到tty6，第一个字段是终端序号，第二个代表系统运行级别，我现在运行在3下面，那我ps就可以看到可以登录的终端：
root 3385 0.0 0.0 1664 472 tty1 Ss+ 19:31 0:00 /sbin/mingetty
root 3386 0.0 0.0 1664 472 tty2 Ss+ 19:31 0:00 /sbin/mingetty
root 3387 0.0 0.0 1664 468 tty3 Ss+ 19:31 0:00 /sbin/mingetty
root 3388 0.0 0.0 1664 476 tty4 Ss+ 19:31 0:00 /sbin/mingetty
root 3389 0.0 0.0 1664 476 tty5 Ss+ 19:31 0:00 /sbin/mingetty
root 3390 0.0 0.0 1664 476 tty6 Ss+ 19:31 0:00 /sbin/mingetty

当我们使用终端登录进去后我们发现其中的一个进程消失了。
root 3993 1 0 May14 tty2 00:00:00 /sbin/mingetty tty2
root 3994 1 0 May14 tty3 00:00:00 /sbin/mingetty tty3
root 3995 1 0 May14 tty4 00:00:00 /sbin/mingetty tty4
root 3996 1 0 May14 tty5 00:00:00 /sbin/mingetty tty5
root 3997 1 0 May14 tty6 00:00:00 /sbin/mingetty tty6

tty1哪去了，实际上tty1 进程调用了login程序，tty1退出了，我们通过ps我们可以看到login进程。
root 7735 1 0 11:09 ? 00:00:00 login -- root
root 7781 7735 0 11:17 tty1 00:00:00 \_ -bash

我们看到login使用root用户，并且产生了子进程bash。
首先login的过程中，如果我们输入的用户名或者密码错误，那登陆就不会成功，这是显而易见的。
所以登陆过程分为3步：
1）、验证用户名密码
2）、获取初始化环境
3）、为用户启动shell

初始化shell环境

初始化shell环境主要做的事情：
1）、转到我们的home目录(chdir)
2）、把终端的所有权改成用户的(chown)
3）、改变终端的访问权限,以使用户可以读写
4）、通过setgid和initgroup来设置用户的group IDs
5）、用login所有的信息初始化环境: HOME, SHELL, USER LOGNAME, PATH
6）、改变我们的user ID(通过setuid), 同时触发登录的shell------execl("/bin/sh", "-sh", (char *)0);
```







## 没有图形化界面直接进入的终端(不是伪终端,使用的是/dev/tty设备文件)



```
当我们键盘输出时，就会调用键盘驱动程序，键盘驱动程序就会对 /dev/ttyn (n为0到6的数字)这个设备进行write()操作

当前的设备文件是/dev/tty2


由下图可知bash进程pid伪2807
这个bash进程会一直对/dev/stdin这个文件进行read()，/dev/stdin指向了/pro/2807/fd/0 
/pro/2807/fd/0 又指向了/dev/tty2




bash进程的工作流程就是永远在读取/dev/stdin文件，当读取到回车字符后，开始执行相应命令（程序），然后等待程序执行完成将对/dev/stdout文件进行write()将结果写入，不过下图可知/dev/stdout其实也是指向了/dev/tty2文件
伪代码：
while (! end_of_input)
    get command
    execute command
    wait for command to finish


所以当我们使用键盘敲击一个个字符的时候，键盘驱动运行，对/dev/tty2这个设备文件执行write()操作，
bash进程一直监视/dev/stdin文件，但/dev/stdin文件根据前面的说法是被指向了/dev/tty2文件



```

![image-20210422175216780](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210422175216.png)





![image-20210422175430964](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210422175431.png)









```
当我们在gui界面的操作系统上，比如ubutnu，右键点击打开终端这时候发生了什么事情？


linux最核心概念，一切皆文件

1.首先创建了一个终端设备文件假设这个文件是/dev/pts/0(后面的数字会变)，创建了一个bash进程，bash一个软件(这个软件作用是不断的从他的标准输入设备即/dev/stdin文件中获取字符(即对/dev/stdin文件不断执行read()操作)，
其实/dev/stdin文件是一个超链接文件，他指向/proc/self/fd/0这个文件，

这边的/proc/self 路径不是真实的路径 是一种特指  他对应的真实的文件路径是/proc/当前进程pid


所以/dev/stdin指向了   /proc/当前进程pid/fd/0这个文件


但是/proc/当前进程pid/fd/0这个文件也是一个超链接文件，他指向了/dev/pts/0


当bash进程读取到回车字符时，就将回车字符前的字符串解析 然后执行对应的程序



2.当我们键盘敲击一个字符，就会执行一此键盘驱动程序，键盘驱动程序就会向对/dev/stdin这个文件执行一次write()操作
但/dev/stdin 这个文件根据上面的讲解其实是指向/dev/pts/0这个文件的，所以是对/dev/pts/0这个文件进行一此write()操作
bash进程一直在读取/dev/pts/0这个文件



```



```

每个进程启动的时候，都会执行
open("/dev/stdin")
open("/dev/stdout")
open("/dev/stderror")

执行一此open操作，就会在该进程的打开文件表中加入一个对应文件的文件描述符，记录该文件的一些信息，可以下次执行write，read操作的时候不需要在查找表



如果这个进程执行print()，print()的本质就是向/dev/stdout这个文件执行一此write()操作，





```







## 最终总结

```
#  在桌面应用程序中打开终端模拟器后，做了如下一些操作：

1 模拟器主进程启动，在内核中动态创建伪终端设备对，比如 主设备/dev/ptmx 从设备 /dev/pts1

2 模拟器主进程和X服务器建立连接，创建模拟器窗口，接收和处理从X服务器发过来的鼠标和键盘事件，把键盘事件写给主设备ptmx，同时负责读取ptmx主设备的输出，将输出内容渲染到模拟器窗口上（同样需要和X服务器交互）

3 模拟器主进程fork出子进程，将子进程的stdin、stdout、stderr文件描述符定位到从设备/dev/pts1，子进程通过exec系统调用装入shell程序（如bash），此时bash正式登场，从终端（从设备）读取命令执行，bash完全感觉不到自己是运行在伪终端上的～

# 上面的过程我们可以发现一个问题：每打开一个终端模拟器就要启动两个进程，这是很浪费资源的，实际上通过我分析Ubuntu上的gnome-terminal模拟器发现它做了如下优化处理：把模拟器主进程做成一个本地服务器，服务器监听和处理多个主设备，同时负责和X服务器打交道来完成窗口的显示～ 这样每次打开新终端的时候，只会fork出一个新shell进程
```

![image-20210422214859418](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210422214907.png)

```
终端模拟器 Terminal Emulator 也叫终端仿真器。它加上键盘和显示器共同构建了以前的终端。它的工作流程如下：

捕获键盘输入（ STDIN ）
将输入发送给命令行程序（ SHELL ）
拿到命令行程序的输出结果（ STDOUT 和 STDERR ）
调用图形接口，将输出结果渲染到显示器上
```



## gnome-terminal-server如何运行

```
 Unix 98接口：使用一个/dev/ptmx作为master设备，在每次打开操作时会得到一个master设备fd，并在/dev/pts/目录下得到一个slave设备（如 /dev/pts/3和/dev/ptmx），这样就避免了逐个尝试的麻烦。由于可能有好几千个用户登陆，所以/dev/pts/*是动态生成的，不象其他设备文件是构建系统时就已经产生的硬盘节点(如果未使用devfs、udev、mdev等) 。第一个用户登陆，设备文件为/dev/pts/0，第二个为/dev/pts/1，以此类推。它们并不与实际物理设备直接相关。现在大多数系统是通过此接口实现pty。

在gui界面每次打开一个终端模拟器，gnome-terminal-server进程打开一个/dev/ptmx
即gnome-terminal-server进程的打开文件表中会多一个/dev/ptmx文件的描述符
也会创建一个设备  /dev/pts/数字




```

```
4.讲讲现象背后的故事
当ubuntu系统创建一个新的terminal时(比如上面的pts/3)
4.1 首先执行ptm = open('/dev/ptmx',...)操作
4.2 接下来fork(),然后child将打开'/dev/pts/3',dup2到0,1和2句柄上,随后执行execl启动一个shell.
pts = open('/dev/pts/3',...);
dup2(pts, 0); // 对应lib库中stdin
dup2(pts, 1); // 对应lib库中stdout
dup2(pts, 2); // 对应lib库中stderr
close(pts);
execl("/system/bin/sh", "/system/bin/sh", NULL);
// 这样sh输入数据将全部来自pts,
// sh的输出数据也都全部输送到pts,也就直接送到了打开ptmx的新terminal中.
4.3 新terminal将启动GUI,捕获按键数据,然后写入ptm,这样pts将收到数据,进而sh将从stdin中获得数据,
于是sh将作进一步运算,将结果送给stdout或stderr,进而送给pts,于是ptm获得数据,然后terminal的GUI
将数据显示出来.

具体流程图如下[luther.gliethttp]：
terminal捕获到key按键值 <--> ptm <--> pts/3 <--> stdin <--> shell读到数据
shell数据结果 <--> stdout <--> pts/3 <--> ptm <--> terminal显示
4.4 好了,正常的启动流程图已经有了,来看看,我们试验时数据出现显示异常的现象背后到底是怎么发生的.
与上面正常流程不同的时,我们在另外一个地方执行了ca
```

https://www.onitroad.com/jc/linux/man-pages/linux/man4/pts.4.html

https://evoa.me/archives/20/

http://flyblog.tech/posts/terminal/

https://www.shuzhiduo.com/A/xl56pDW4zr/

http://blog.chinaunix.net/uid-12873540-id-2912802.html

https://wenku.baidu.com/view/53d0daf8aef8941ea76e05d2

https://zhuanlan.zhihu.com/p/61369678

ptmx(pseudo-terminal master)
ptmx是伪终端的master端。在/dev下仅有2个ptmx文件，其信息如下：

```bash
ubuntu@VM-32-73-ubuntu:/dev$ ll /dev/ptmx
crw-rw-rw- 1 root tty 5, 2 Jan 16 16:38 /dev/ptmx
ubuntu@VM-32-73-ubuntu:/dev$ ll /dev/pts/ptmx
c--------- 1 root root 5, 2 Mar 17  2018 /dev/pts/ptmx
```

Bash

Copy

讲讲现象背后的故事
当ubuntu系统创建一个新的terminal时(比如上面的pts/1)
首先执行ptm = open('/dev/ptmx',...)操作
接下来fork(),然后child进程将打开'/dev/pts/1',dup2到0,1和2句柄上,随后执行execl启动一个shell.
pts = open('/dev/pts/1',...);
dup2(pts, 0); // 对应lib库中stdin
dup2(pts, 1); // 对应lib库中stdout
dup2(pts, 2); // 对应lib库中stderr
close(pts);
execl("/system/bin/sh", "/system/bin/sh", NULL);
// 这样sh输入数据将全部来自pts,
// sh的输出数据也都全部输送到pts,也就直接送到了打开ptmx的新terminal中.

新terminal将启动GUI,捕获按键数据,然后写入ptm,这样pts将收到数据,进而sh将从stdin中获得数据,
于是sh将作进一步运算,将结果送给stdout或stderr,进而送给pts,于是ptm获得数据,然后terminal的GUI
将数据显示出来.

terminal捕获到key按键值 <--> ptm <--> pts/1 <--> stdin <--> shell读到数据
shell数据结果 <--> stdout <--> pts/1 <--> ptm <--> terminal显示

因为是master - slaver，所以ptm只有一个，pts可以有多个
我们用一个ssh的图来看

```
+----------+       +------------+
 | Keyboard |------>|            |
 +----------+       |  Terminal  |
 | Monitor  |<------|            |
 +----------+       +------------+
                          |
                          |  ssh protocol
                          |
                          ↓
                    +------------+
                    |            |
                    | ssh server |--------------------------+
                    |            |           fork           |
                    +------------+                          |
                        |   ↑                               |
                        |   |                               |
                  write |   | read                          |
                        |   |                               |
                  +-----|---|-------------------+           |
                  |     ↓   |                   |           ↓
                  |   +--------+   +-------+    |       +-------+  fork   +-------------+
                  |   |  ptmx  |<->| pts/0 |<---------->| shell |-------->| tmux client |
                  |   +--------+   +-------+    |       +-------+         +-------------+
                  |   |        |                |                               ↑
                  |   +--------+   +-------+    |       +-------+               |
                  |   |  ptmx  |<->| pts/2 |<---------->| shell |               |
                  |   +--------+   +-------+    |       +-------+               |
                  |     ↑   |  Kernel           |           ↑                   |
                  +-----|---|-------------------+           |                   |
                        |   |                               |                   |
                        |w/r|   +---------------------------+                   |
                        |   |   |            fork                               |
                        |   ↓   |                                               |
                    +-------------+                                             |
                    |             |                                             |
                    | tmux server |<--------------------------------------------+
                    |             |
                    +-------------+
```

需要注意的是，由于pts是slave端，所以不支持一对多，如果我们在linux中开启两个终端分别是pts1 和 pts2
如果我们再pts2中执行 cat /dev/pts/1命令，然后我们在pts1终端中输入字符，可以发现一部分字符会回显再pts1端上，另一部分的字符会会显在pts2上。我画个图就很好理解为什么了

当我们在pts1中输入数据时，输入流从ptmx传递给pts1在传递给bash，bash会把用户输入原样返回给输出流。这时候pts1接收到bash返还给的输出，但此时有两个应用程序在等待pts1的返回。一个是ptmx，一个是pts2下的cat进程（其实应该是pts2下bash的子进程）。于是此时就发生了数据争夺。linux内核调度器根据当时情况随时都会将他们中的一个调出或者调入,因此数据就出现了一部分被送到了pts/2的cat命令,另一部分被送到了pts1的shell,









## 在ubuntu图形化界面中右键按下打开终端发生了哪些事情







```
#  在桌面应用程序中打开终端模拟器后，做了如下一些操作：

1 模拟器主进程启动，在内核中动态创建伪终端设备对，比如 主设备/dev/ptmx 从设备 /dev/pts1

2 模拟器主进程和X服务器建立连接，创建模拟器窗口，接收和处理从X服务器发过来的鼠标和键盘事件，把键盘事件写给主设备ptmx，同时负责读取ptmx主设备的输出，将输出内容渲染到模拟器窗口上（同样需要和X服务器交互）

3 模拟器主进程fork出子进程，将子进程的stdin、stdout、stderr文件描述符定位到从设备/dev/pts1，子进程通过exec系统调用装入shell程序（如bash），此时bash正式登场，从终端（从设备）读取命令执行，bash完全感觉不到自己是运行在伪终端上的～

# 上面的过程我们可以发现一个问题：每打开一个终端模拟器就要启动两个进程，这是很浪费资源的，实际上通过我分析Ubuntu上的gnome-terminal模拟器发现它做了如下优化处理：把模拟器主进程做成一个本地服务器，服务器监听和处理多个主设备，同时负责和X服务器打交道来完成窗口的显示～ 这样每次打开新终端的时候，只会fork出一个新shell进程












```



### 先介绍三个程序

### 

#### 1.X server介绍(图形化界面实现程序)

```
X server程序监听键盘输入，鼠标等的输入,将这些输入发送给当前选中的窗口程序，接收窗口程序发来的数据将这些数据渲染到显示屏上
```

#### 2.终端模拟程序(一个图形化界面程序)

```
终端模拟程序是一个图形化界面程序，他的运行原理是，因为他是一个图形化界面程序，所以在图形化界面程序实现原理的概念下，当他的图形界面窗口被选中时他作为x client会接受x sever程序发送给他的键盘输入信息（x sever 程序的进程不断监听键盘 鼠标等的输入 然后确定要转发给哪个x client进程），收到键盘输出的字符后向/dev/pts/0设备文件进行写入，同时也会读取dev/pts/0设备文件中的数据发送给x  sever进程要求x sever进程将数据渲染到显示屏上
```

#### 3.bash程序(命令解释程序)

```
bash程序（命令解释程序）的运行原理，不断的从/dev/stdio文件读取字符，当读取到回车字符后，将回车字符之前输入的字符做完命令进行运行，然后讲运行命令的结果输出到/dev/stdout文件 
```



### 究竟发生了哪些事情

#### 详细讲解

```
1. 每次右键打开终端  都会先打开一个终端模拟程序，这是一个图形化界面程序，这个程序运行会打开设备文件/dev/ptmx,在进程的打开文件表中添加一个文件描述符，并且创建一个设备文件/dev/pts/数字  文件描述符和这个创建的设备文件绑定，假设这里的设备文件是/dev/pts/0   (每进行一次右键打开终端，就会打开一次/dev/ptmx文件和创建一个/dev/pts/数字 文件)

2. 终端模拟程序是一个图形化程序，会创建模拟器窗口也就是我们在ubutnu界面中看到的输入命令的图形界面，之后终端模拟程序会接收和处理从X server 发来的鼠标和键盘事件(X server是图形化界面的实现程序,在图形化系统中监控键盘等的输入，确定这些输入要往哪些图形化窗口进行发送，然后接收图形化窗口程序发来的数据将数据渲染到显示屏上，图形化窗口程序就是X clinet),终端处理程序会将收到的键盘输入的字符，对该进程打开的/dev/ptmx设备文件 进行write()操作，在进行write()操作时，会查看打开文件表中该文件对应的文件描述符中的信息，根据前面讲解的，该进程的打开文件表中/dev/ptmx这个设备文件的文件描述符中的信息是指向了/dev/pts/0文件，也就是说会将键盘输入的字符写入到/dev/pts/0这个设备文件


3. 终端模拟程序之后会fork出一个子进程，将子进程的stdin、stdout、stderr文件描述符定位到从设备/dev/pts/0

(这里要补充的是，每个进程运行的时候都会打开三个设备文件即/dev/stdin,/dev/stdout,/dev/stedrr三个设备文件，作为进程的标准输入，标准输出，错误输出，这些有什么作用，举个例子调c语言调用printf()函数，printf函数的作用就是对/dev/stdout进行一次write()操作，write()操作的流程是，1.先获取 打开的/dev/stdout 这个设备文件他对应的文件描述符中存储的信息，这个信息存储了到底这个打开的文件指向了哪个物理文件2.获得到这个信息后就会向真正的物理文件进行写入)

子进程会使用子进程通过exec系统调用装入shell程序（如bash，是命令解释程序），根据前面的描述也就说shell程序的三个stdin、stdout、stderr文件描述符也是被定位到从设备/dev/pts/0，shell程序是一个命令解释程序，他的运行原理是不断的对/dev/stdin文件进行read()操作，当读取到回车字符后，将回车字符前面的字符组成命令找到对应的程序进行运行，之后将运行的结果对/dev/stdout文件进行write()操作，这里/dev/stdout文件，/dev/stderr文件在终端模拟程序运行时被指向了/dev/pts/0文件，所以其实是不断对/dev/pts/0文件进行read()操作，命令运行结束后将结果对/dev/pts/0文件进行write()操作


4. 终端模拟程序是一个图形化程序，他会对/dev/pts/0文件不断进行read()操作，然后要求x sever程序将他读取到的信息渲染到显示屏上，所以我们能在显示屏上看到我们命令的输出和命令运行的结果

```



#### 简单总结

```
简单总结:
我们在ubuntu图形化界面每次右键打开终端时会先后创建两个进程
先创建终端模拟程序，这是一个图形化界面程序即我们看到的，
后面会创建一个是我们看不到的bash程序

图形化的终端模拟程序和x server程序交互将我们键盘的输出，写入到/dev/pts/0文件(这里的/dev/pts/0文件是举个特例，真实的文件是/dev/pts/数字 文件,这里将数字取0进行讲解)，并且也会让x server程序将/dev/pts/0文件的内容在显示屏上渲染
bash程序一直读取/dev/stdio文件,读取到回车字符后，将回车字符之前输入的字符做完命令进行运行，然后讲运行命令的结果输出到/dev/stdout文件,但这里的bash进程的/dev/stdio文件，/dev/stdout文件，/dev/stderr文件在终端模拟程序运行时被指向了/dev/pts/0文件

所以实际运行的时候就是终端模拟程序不断将我们的键盘输出写到/dev/pts/0文件，然后bash程序不断读取/dev/pts/0文件的内容然后执行命令，执行完成后将结果继续写入到/dev/pts/0文件中，终端模拟程序又会将/dev/pts/0文件的内容让x server程序渲染到显示屏中，所以我们能在显示屏上看到我们命令的输出和命令运行的结果





```



![image-20210424205130867](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210424205138.png)

![image-20210424210348803](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210424210349.png)