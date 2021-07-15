# shell脚本学习阶段1

## 参考

https://halysl.github.io/2020/04/02/Login-shell-%E5%92%8C-Non-Login-shell-%E7%9A%84%E5%8C%BA%E5%88%AB/

http://www.ruanyifeng.com/blog/2013/08/linux_boot_process.html



linux命令查询网址

https://man.linuxde.net/cp

打开文件 文件描述符等知识

https://blog.csdn.net/wwwlyj123321/article/details/100298377

https://zhuanlan.zhihu.com/p/143430585

## 1.在shell 中执行程序的3 种方法 

### 1.使文件具有可执行权限，直接运行文件 (会fork出子进程进行执行,子进程对应的程序由文件第一行#! 后面指向的路径确定)

echo.sh文件的内容

```shell
#! /bin/sh
cd /tmp 
echo "hello world!"
```



```shell
# 给echo.sh文件赋予可执行权限
chmod +x echo.sh
# 运行当前文件夹下的echo.sh文件
./echo.sh
```

### 2.直接调用命令解释器(如sh,bash)执行程序 (会fork出子进程进行执行,子进程对应的程序对应调用的命令解释器)

```shell
# 调用bash命令解释器 解释执行echo.sh文件，会在原先shell父进程下fork出一个bash的子进程进行解释执行 
bash echo.sh
```



### 3.使用 source 执行文件 （在原先的shell中执行，不fork子进程执行）

```sh
chmod +x echo.sh

# 在当前shell中解释执行echo.sh文件，所以echo.sh中的命令会影响当前shell的环境变量
source echo.sh
```

## 2.shell 中程序运行的过程

```
当Shell解释我们输入的命令然后 执行程序时，首先判断是否程序有执行权限。如果没有足够的权限，则系统会提示用户：“权限不够”。从安全角度考虑，任何程序要在机器上执行时，必须判断执行这个程序的用户是否具有相应权限。在第一种方法中，我们直接执行文件，则需要文件具有可执行权限。
chmod 命令可以修改文件的权限。+x 参数使程序文件具有可执行权限。

命令行 Shell 接收到我们的执行命令，并且判定我们有执行权限后，则调用 Linux 内核命令新建（fork）一个进程，在新建的进程中调用我们指定的命令。如果这个命令文件是编译型的（二进制文件），则 Linux 内核知道如何执行文件。不幸的是，我们的 echo.sh 程序文件并不是编译型的文件，而是文本文件，内核并不知道如何执行，于是，内核返回“not executable format file”（不是可执行的文件类型）出错信息。Shell 收到这个信息时说：“内核不知道怎么运行，我知道，这一定是个脚本！”Shell 知道这是个脚本后，启动了一个新的 Shell 进程来执行这个程序。但是现在的 Linux 系统往往拥有好几个种类的shell(如sh，bash等)，到底挑选哪个夫婿呢？这就要看脚本中意哪个了。在第一行中，脚本通过“#! /bin/sh”告诉命令行：“我只和他好，让他来执行吧!”

这种选婿方法有助于执行方式的通用化。用户在编写脚本时，在程序的第一行通过#!来设置运行 Shell 创建一个什么样的进程来执行此脚本。
在我们的 echo.sh 中，Shell 创建了一个/bin/sh（标准 Shell）进程来执行脚本。命令行在扫过第一行，发现#!时，开始试图读取#!之后的字符，搜寻解释器的完整路径。如果在第一行中的解释器也有参数，则一并读取。例如，我们可以这样来引用我们的解释器：
#! /bin/bash -l 
这样，命令行 Shell 会启用一个新的 bash 进程来执行程序的每一行。并且，-l 参数使得这个bash 进程的反应与登录 Shell 相似。


这种选婿方法，使得我们可以调用任何的解释器，并不局限于 Linux Shell。例如，我们可以创建这样一个 python程序：

#! /usr/bin/python 
print“hello world!” 

当这个文件被赋予可执行权限，并且用第一种方式运行时，就像调用了 python 解释器来执行一样。
```



```
简单来讲就是当使用方法1直接执行shell脚本文件的时候，原先的shell父进程会fork出一个子进程，至于子进程对应哪个程序则是由shell父进程读取shell脚本文件开头#！ 后面路径所指向的程序


```



## 3. linux shell中环境变量和普通变量的区别

```
1.环境变量会被子进程继承.普通变量不会
2.Linux下所有进程都有环境变量
```







## 4.在linux 命令参数中对应文件夹参数   文件夹名 和文件名/ 效果是一样的  ，举个例子，假设test1是个文件夹  test2是一个文件夹  那么命令 mv test1 test2 和mv test1/ test2/  效果一样



## 5.每个进程默认都会open()三个文件，即标准输入文件，标准输出文件，标准错误文件，对应的进程打开文件表中的文件描述符分别是0,1,2

标准输入（Standard Input）的文件描述符是 0，标准输出（Standard Output）是 1，标准错误（Standard Error）是 2。尽管这种习惯并非 UNIX 内核的特性，但是因为一些 Shell 和很多应用程序都使用这种习惯，因此，如果内核不遵循这种习惯的话，很多应用程序将不能使用。
这也是当我们重定向标准错误时，使用（2>）的原因。



## 6.命令中的io重定向详解

https://www.runoob.com/linux/linux-shell-io-redirections.html

### 1.命令输入重定向演示(命令的默认输入 是读取标准输入 ，标准输入文件描述符为0)

#### test.sh脚本

```sh
#! /bin/bash
echo "输入你的名字"
read name
echo "你的名字是"
echo  "$name"
```



#### 测试命令

```sh
nano test.sh
# 给脚本赋予执行权限
chmod a+x test.sh
# 直接执行脚本，脚本默认从标准输入进行读取，也就是从用户键盘输入
./test.sh

nano myname.txt

# 将脚本的输入从默认的标准输入即键盘输入  该为从myname.txt中读取
./test.sh < myname.txt
```

![image-20210604140900628](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210604140907.png)

### 2.命令输出重定向(命令的默认输出 是输出到标准输出 ， 标准输出文件描述符1)

```sh
# 原先执行echo "测试一下" 命令  命令的输出会打印在控制台上，现在将命令的输出定向到1.txt文件，使用>会覆盖1.txt文件原先的内容
echo "测试一下" > 1.txt


# 使用>> 是将输出追加到1.txt文件的末尾，不进行覆盖
echo "测试一下" >> 1.txt
```







### 重定向深入讲解

一般情况下，每个 Unix/Linux 命令运行时都会打开三个文件：

- 标准输入文件(stdin)：stdin的文件描述符为0，Unix程序默认从stdin读取数据。
- 标准输出文件(stdout)：stdout 的文件描述符为1，Unix程序默认向stdout输出数据。
- 标准错误文件(stderr)：stderr的文件描述符为2，Unix程序会向stderr流中写入错误信息。

默认情况下，command > file 将 stdout 重定向到 file，command < file 将stdin 重定向到 file。

如果希望 stderr 重定向到 file，可以这样写：

```
command 2>file
```

如果希望 stderr 追加到 file 文件末尾，可以这样写：

```
command 2>>file
```

**2** 表示标准错误文件(stderr)。

如果希望将 stdout 和 stderr 合并后重定向到 file，可以这样写：

```sh
#2>&1 讲解  2表示标准错误的文件描述符  1表示表示输出的文件描述  &1 通过文件描述符引用文件 即&1 表示 标准输出的文件   2 > &1 就表示将标准错误输出定向到标准输出文件中
# 下面的命令就表示 将文件的标准输出 定向到 file文件中，但是因为执行了2>&1 标准错误的输出被定向到了标准输出，那么整个命令最后的结果就是将命令执行产生的标准错误和标准输出  定向输出到file文件中去   >表示覆盖原先file文件的内容
command > file  2>&1

# >>表示在原先file文件的末尾进行追加 不进行覆盖
command >> file 2>&1
```

如果希望对 stdin 和 stdout 都重定向，可以这样写：

```sh
#command 命令将 stdin 重定向到 file1，将 stdout 重定向到 file2。
command < file1   >file2
```



## 7. 使用| 创建管道  (管道可以将前一个命令的输出 作为后一个命令的输入)

```
从理论上讲，管道的功效完全可以用<和>实现。通过创建临时文件，而输入和输出重定向可以分别读取临时文件和写入临时文件。但是管道相较这种做法更高效，它可以直接连接程序的输入、输出，并且没有程序使用个数限制，只要你尚未获得最终处理结果，都可以在命令后继续添加管道

管道的数据共享在 Linux 内核中是通过内存复制实现的。相较于 CPU 的运算，数据的移动往往更消耗时间。因此，在设计管道时，尽量把能够减少数据量的操作置于管道的前端。这样一来数据复制快速，二来程序运算量减少。
```

```sh
#这条命令读取/etc/passwd 文件中的前 10 行，将读取的内容从输出端（管道）输出到 grep 命令的输入端，grep 读取内容后，在其中检索包含文本“prince”的行。

head–n10 /etc/passwd | grep "prince" 
```



## 8. 文件末尾添加

https://en.wikipedia.org/wiki/Here_document#Unix_shells

https://cloud.tencent.com/developer/article/1421154

### Here Document(一种特殊的重定向方式)

Here Document 是 Shell 中的一种特殊的重定向方式，用来将输入重定向到一个交互式 Shell 脚本或程序。

它的基本的形式如下：

```
command << delimiter
    document
delimiter
```

它的作用是将两个定界符( delimiter )之间的内容(document) 作为输入传递给 command。

> 注意：
>
> - 结尾的delimiter 一定要顶格写，前面不能有任何字符，后面也不能有任何字符，包括空格和 tab 缩进。
> - 开始的delimiter前后的空格会被忽略掉。

### 实例

在命令行中通过 **wc -l** 命令计算 Here Document 的行数：

```sh
$ wc -l << EOF
    欢迎来到
    菜鸟教程
    www.runoob.com
EOF
3          # 输出结果为 3 行
$
```

我们也可以将 Here Document 用在脚本中，例如：

```sh
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com

cat << EOF
欢迎来到
菜鸟教程
www.runoob.com
EOF
```

执行以上脚本，输出结果：

```
欢迎来到
菜鸟教程
www.runoob.com
```





### 使用注意点

```sh
 cat <<EOF >  /etc/sysconfig/modules/ipvs.modules
#!/bin/bash
modprobe -- ip_vs
modprobe -- ip_vs_rr
modprobe -- ip_vs_wrr
modprobe -- ip_vs_sh
modprobe -- nf_conntrack_ipv4
EOF
```

```javascript
cat > checkServer.sh << EOF
#! /bin/bash
status=\`cat ${keepalived_conf}/xmha/keepalived_status\`
if [ \$status == "master" ];then
        ps -ef | grep ambari-server | grep -v grep >> /dev/null 2>&1
        if [ \$? -ne 0 ];then
                sh /etc/keepalived/xmha/checkFile.sh
                echo "backup" > "${keepalived_conf}/xmha/keepalived_status"
                # 停止keepalived服务，使VIP转移
                /bin/sudo -u root pkill keepalived
                # 再次检查keepalived进程，防止停止失败
                ps -ef | grep /opt/gvmysql/keepalived/sbin/keepalived | grep -v grep
                if [ \$? -eq 0 ];then
                        # 如果keepalived服务未成功停止，则手动kill
                        ps -ef | grep /opt/gvmysql/keepalived/sbin/keepalived | grep -v grep | awk  '{print \$2}' | xargs kill -9
                fi
        fi
elif [ \$status == "backup" ];then
        ps -ef | grep ambari-server | grep -v grep >> /dev/null 2>&1
        if [ \$? -eq 0 ];then
                ps -ef | grep ambari-server | grep -v grep | awk  '{print \$2}' | xargs kill -9
        fi
fi
sh /etc/keepalived/xmha/check_brain_split.sh
EOF
```

- <strong style="color:red;">如上述代码所示，将内容批量输入至checkServer.sh文件中。其中**没有加转义符 `\` 的变量会在脚本中被解释为真实值；加转义符 `\` 的变量会将变量原样地输入至目标文本中。**</strong>
- 涉及到变量操作，如果需要保留该变量到文件中的话，需要转义符`\`。否则，shell脚本将会解释这些变量。
- `cat` 追加内容用 `>>`，覆盖内容用 `>` 。
- 远程主机执行 `cat EOF` 命令，需要使用引号将 `cat至文件的部分` 括起来，下面是举例。

```sh
# 远程主机执行cat EOF命令
ssh root@${host2} "cat > ${keepalived_conf}/xmha/checkFile.sh" << EOF
# 代码
EOF
```







## 9.模式匹配运算符

https://blog.csdn.net/halazi100/article/details/40652575

http://www.ruanyifeng.com/blog/2018/09/bash-wildcards.html

https://www.cnblogs.com/pengdonglin137/p/3524471.html

```
path1=/home/prince/desktop/long.file.name

echo ${path1#/*/}
prince/desktop/long.file.name


echo ${path1##/*/}
long.file.name


echo  ${path1%.*}
/home/prince/desktop/long.file

echo  ${path1%%.*}
/home/prince/desktop/long



var=http://blog.csdn.net/halazi100

echo ${var#*/}
/blog.csdn.net/halazi100

```



```shell
#如果需要取出的开头的字符串不匹配，则命令会直接输出变量名的值
echo ${变量名#需要先删除的开头的字符串*需要匹配的字符串}



Var=/home/firefox/MyProgram/fire.login.name

# #*/ #表示从左边开始找  */ 表示找到第一个 / ，删除第一个/前面所有的字符(包括/)，返回经过删除操作后的字符串
echo ${Var#*/}
home/firefox/MyProgram/fire.login.name

# #/*/  #表示从左边开始找  /*/  删除字符串开头的/  然后开始匹配第一个/，删除找到的第一个/前面所有的字符(包括/)，返回经过删除操作后的字符串
 echo ${Var#/*/}
firefox/MyProgram/fire.login.name
 
#  
echo ${Var#*.}

login.name


```



##  10. shell中的函数

http://cw.hubwiz.com/card/c/56d906a0ecba4b4d31cd28ef/1/5/2/

https://blog.csdn.net/USBdrivers/article/details/8572470

![image-20210604171030386](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210604171030.png)



### test 测试命令使用

```sh
test  是否取反(!)  需要测试的表达式   是否进行多条件拼接(-a|-o)   需要测试的表达式 

# 判断/home/1.txt 文件是否存在并且不为null,如果存在且不为null test命令返回0，其余情况返回非0
test -s  /home/1.txt

#加了! 返回的结果是上面的命令取反
test ! -s  /home/1.txt

# -a   a表and  多条件测试
#/home/1.txt不存在或者为null 且 /home/2.txt不存在或者为null ，test命令返回0，其余情况返回非0
test ! -s /home/1.txt  -a  ! -s /home/2.txt


```



## 命令详解

### ①mv (mv  move 移动文件，重命名文件命令)

```shell
-b: 当目标文件或目录存在时，在执行覆盖前，会为其创建一个备份。
-i: 如果指定移动的源目录或文件与目标的目录或文件同名，则会先询问是否覆盖旧文件，输入 y 表示直接覆盖，输入 n 表示取消该操作。(简单讲就是，不加-i参数 默认采取覆盖策略，使用-i参数后  遇到移动时目标已经存在相同文件名的文件夹或者目录，会询问是否覆盖还是取消操作)
-f: 如果指定移动的源目录或文件与目标的目录或文件同名，不会询问，直接覆盖旧文件。
-n: 不要覆盖任何已存在的文件或目录。
-u：当源文件比目标文件新或者目标文件不存在时，才执行移动操作。


#将源文件名 source_file 改为目标文件名 dest_file
mv source_file(文件) dest_file(文件)  

#将文件 source_file 移动到目标目录 dest_directory 中
mv source_file(文件) dest_directory(目录)

#1.若目录dest_directory 存在，将source_directory 移动到dest_directory 目录下
#2.若目录dest_directory 不存在，将source_directory重命名为dest_directory
mv source_directory(目录) dest_directory(目录)



举例子 test3和test4都是文件夹

#1.若test4文件夹存在，将test3文件夹移动到test4文件夹下
#2.若test4文件夹不存在，将test3重命名为test4
mv test3 test4

# 效果和mv test3 test4 相同  即test3/就是test3
mv test3/ test4

# 效果和mv test3 test4 相同   即test4/ 就是test4
mv test3 test4/
# 效果和mv test3 test4 相同 
mv test3/ test4/

#将test3文件夹下全部的文件 移动到test4文件夹下
mv test3/* test4
```

### ②chmod命令 (chmod change mode 控制用户对文件的权限的命令)

```
-c : 若该文件权限确实已经更改，才显示其更改动作
-f : 若该文件权限无法被更改也不要显示错误讯息
-v : 显示权限变更的详细资料
-R : 对目前目录下的所有文件与子目录进行相同的权限变更(即以递归的方式逐个变更)
--help : 显示辅助说明
--version : 显示版本


```

#### 1.符号模式写法

![image-20210527141543408](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210527141550.png)

```
#将文件 file1.txt 设为所有人皆可读取
chmod ugo+r file1.txt

#将文件 file1.txt 设为所有人皆可读取
chmod a+r file1.txt

#将文件 file1.txt 与 file2.txt 设为该文件拥有者，与其所属同一个群体者可写入，但其他以外的人则不可写入 
chmod ug+w,o-w file1.txt file2.txt


#为 ex1.py 文件拥有者增加可执行权限:
chmod u+x ex1.py


#将目前目录下的所有文件与子目录皆设为任何人可读取 
chmod -R a+r *
```



#### 2.八进制模式写法

![image-20210527145215612](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210527145215.png)

```
# 对a.sh文件赋予该文件所有者读写执行权限，赋予和文件所有者同组的用户读写执行权限，赋予其他用户读写执行权限
chmod 777  a.sh

# 对b.sh文件赋予文件所有者读写执行权限，赋予和文件所有者同组的用户读执行权限，赋予其他用户执行权限
chmod 751 b.sh
```



### ③set 命令 (set 设置) set指令能设置所使用shell的执行方式，可依照不同的需求来做设置。

```shell
# 查看系统全部变量(包括环境变量和普通变量)
set

#-e参数表示只要shell脚本中发生错误，即命令返回值不等于0，则停止执行并退出shell。
#set -e在shell脚本中经常使用。默认情况下，shell脚本碰到错误会报错，但会继续执行后面的命令。
set -e


#-u参数表示shell脚本执行时如果遇到不存在的变量会报错并停止执行。
#默认不加-u参数的情况下，shell脚本遇到不存在的变量不会报错，会继续执行
set -u
```



### ④unset命令  unset为shell内建指令，可删除变量或函数

```shell
sen@sen-VirtualBox:~$ 
# 创建一个名为myvartest的变量，赋值为1
sen@sen-VirtualBox:~$ myvartest=1
# 输出myvartest变量的值
sen@sen-VirtualBox:~$ echo $myvartest
1

# 查看myvartest变量是否存在
sen@sen-VirtualBox:~$ set | grep myvartest
myvartest=1

#删除myvartest变量
sen@sen-VirtualBox:~$ unset myvartest

#删除后再次输出myvartest变量
sen@sen-VirtualBox:~$ echo $myvartest


#删除后再次查看myvartest变量
sen@sen-VirtualBox:~$ set | grep myvartest
sen@sen-VirtualBox:~$ 

```



### ⑤cp命令 (cp copy  复制)  cp命令主要用于复制文件或目录

**参数说明**：

```
-a：此参数的效果和同时指定"-dpR"参数相同；
-d：当复制符号连接时，把目标文件或目录也建立为符号连接，并指向与源文件或目录连接的原始文件或目录；
-f：强行复制文件或目录，不论目标文件或目录是否已存在；
-i：覆盖既有文件之前先询问用户；
-l：对源文件建立硬连接，而非复制文件；
-p：保留源文件或目录的属性；
-R/r：递归处理，将指定目录下的所有文件与子目录一并处理；
-s：对源文件建立符号连接，而非复制文件；
-u：使用这项参数后只会在源文件的更改时间较目标文件更新时或是名称相互对应的目标文件并不存在时，才复制文件；
-S：在备份文件时，用指定的后缀“SUFFIX”代替文件的默认后缀；
-b：覆盖已存在的文件目标前将目标文件备份；
-v：详细显示命令执行的操作。
```



```shell

#将当前文件夹下的1.txt文件复制到test1文件夹下，命令结束后test1文件夹下会有一个1.txt文件
cp 1.txt test1

# 同cp 1.txt test1效果一样
cp 1.txt  test1/



# 复制文件夹的时候必须添加-r 或者-R参数
#将test文件夹复制到test2文件夹下，test2文件夹下会有一个test1文件夹
cp –r test1  test2    

# 和cp –r test1  test2  作用一样
cp -r test1/ test2
# 和cp –r test1  test2  作用一样
cp –r test1  test2/

# 和cp –r test1  test2  作用一样
cp –r test1/  test2/



#将test1文件夹下全部文件复制到test2文件夹下,test2文件夹下不会有test1文件夹，会有test1文件夹中的全部文件
cp  -r  test1/*   test2/




# -s 参数 创建快捷链接(软链接)文件
# 如果源文件相对路径，创建的链接文件只能位于和源文件同一文件夹下
#为3.txt文件创建一个快捷链接(软链接)文件3.link 
cp -s 3.txt 3.link

# 源文件使用相对路径，生成的文件没有位于同一文件夹下，命令报错
cp -s 3.txt test2/3.link


#源文件使用绝对路径 快捷链接文件
cp -s /home/sen/cptest/3.txt /home/sen/cptest/test2/3.link

# 和cp -s /home/sen/cptest/3.txt /home/sen/cptest/3.link 作用相同
ln -s /home/sen/cptest/3.txt /home/sen/cptest/test2/3.link

```

### ⑥ln命令(ln  link files 链接文件)  ln命令可以用来创建文件的软链接和硬链接,不添加参数默认是硬链接

https://man.linuxde.net/ln

https://blog.csdn.net/wwwlyj123321/article/details/100298377

```
-b或--backup：删除，覆盖目标文件之前的备份；
-d或-F或——directory：建立目录的硬连接；
-f或——force：强行建立文件或目录的连接，不论文件或目录是否存在；
-i或——interactive：覆盖既有文件之前先询问用户；
-n或--no-dereference：把符号连接的目的目录视为一般文件；
-s或——symbolic：对源文件建立符号连接，而非硬连接；
-S<字尾备份字符串>或--suffix=<字尾备份字符串>：用"-b"参数备份目标文件后，备份文件的字尾会被加上一个备份字符串，预设的备份字符串是符号“~”，用户可通过“-S”参数来改变它；
-v或——verbose：显示指令执行过程；
-V<备份方式>或--version-control=<备份方式>：用“-b”参数备份目标文件后，备份文件的字尾会被加上一个备份字符串，这个字符串不仅可用“-S”参数变更，当使用“-V”参数<备份方式>指定不同备份方式时，也会产生不同字尾的备份字符串；
--help：在线帮助；
--version：显示版本信息。
```



#### 硬链接和软链接区别

```shell
每个文件的数据位置信息都保存在这个文件所属文件夹的数据中
首先，我们要知道文件夹 也称文件目录   目录类型的文件 的数据由一个个目录项组成   该文件夹下的每个文件都会对应一个目录项，目录项中保存了文件名 和索引节点在硬盘中的位置， 索引节点中保存了该文件在硬盘中的哪个位置等其他非常多的信息


创建一个文件的硬链接就是创建一个文件，也就是在这个文件所在的文件夹保存的数据中，添加一个目录项，这个目录项中的索引节点指向要创建硬链接文件对应的索引节点



创建一个文件的软链接的作用是，创建一个文件，这个文件的数据内容记录了需要创建软链接的文件的具体位置，通过这个软链接文件我们能够找到原来文件的具体位置，然后进行加载


# 为3.txt 文件创建一个 硬链接文件3.dlink
ln 3.txt 3.dlink

# 和ln 3.txt 3.dlink作用相同
ln -d 3.txt 3.dlink

# 为3.txt 文件创建一个 软链接文件3.slink
ln -s 3.txt 3.slink
```

### ⑦grep命令  (grep  Global Regular Expression Print 全局正则表达式打印)







### cat 命令 （cat  concatenate  连接）





### ⑧tar 命令 (tar  tape archive 磁带规定)  tar 是用来建立，还原备份文件的工具程序，它可以加入，解开备份文件内的文件。

https://www.runoob.com/linux/linux-comm-tar.html

https://linux.cn/article-6679-1.html

```sh
-f <备份文件>或--file=<备份文件>  指定备份文件。
-v或--verbose 显示指令执行过程。
-x或--extract或--get 从备份文件中还原文件。


-z或--gzip或--ungzip 通过gzip指令处理备份文件一般格式为xx.tar.gz或xx. tgz
-j使用bzip2压缩，一般格式为xxx.tar.bz2
#常用解压命令就是
tar -xzf  test.tar.gz
tar -xzvf test.tar.gz


-c或--create 建立新的备份文件。

#常用的创建压缩文件的命令
#将a.c 压缩成 test.tar.gz文件
tar -czvf test.tar.gz  a.c  


-t或--list 列出备份文件的内容。



-C<目的目录>或--directory=<目的目录> 切换到指定的目录。使用-C可以解压到指定的目录
#把根目录下的openstack_test.tar解压到/tmp下。
tar -xvf /openstack_test.tar -C /tmp

注意事项：

1：在上面的参数中， c/x/t 仅能存在一个！不可同时存在！例如，不可能同时压缩与解压缩。
2：-f: 指定打包的文件名，切记，这个参数是最后一个参数（不能再接其它参数），后面只能接打包文件名。即多参数合并使用的时候f必须是最后一个
3：参数可以合并在一起，也可以单独分开。
4：tar 是将 规定文件中的内容 放到当前文件夹下
```

### ⑨exec 命令 (exec  execute 执行)  在原先父进程的基础上执行脚本，但是不会继续执行原先父进程的后续脚本

https://wiki.jikexueyuan.com/project/13-questions-of-shell/exec-source.html

#### 深入理解exec命令,进行试验测试

#### 1.sh脚本

```sh
#!/bin/bash
A=B 
echo "PID for 1.sh before exec/source/fork:$$"
export A
echo "1.sh: \$A is $A"
case $1 in
        exec)
                echo "using exec..."
                exec ./2.sh ;;
        source)
                echo "using source..."
                . ./2.sh ;;
        *)
                echo "using fork by default..."
                ./2.sh ;;
esac
echo "PID for 1.sh after exec/source/fork:$$"
echo "1.sh: \$A is $A"
```

#### 2.sh脚本

```sh
#!/bin/bash
echo "PID for 2.sh: $$"
echo "2.sh get \$A=$A from 1.sh"
A=C
export A
echo "2.sh: \$A is $A"
```

#### 执行脚本,根据脚本运行结果理解exec命令 sourc命令  正常fork的区别

```sh
nano 1.sh
nano 2.sh
chmod 777 ./1.sh
chmod 777 ./2.sh
./1.sh fork
./1.sh source
./1.sh exec
```



```sh
[root@localhost ~]# nano 1.sh
[root@localhost ~]# nano 2.sh
[root@localhost ~]# chmod 777 ./1.sh
[root@localhost ~]# chmod 777 ./2.sh
[root@localhost ~]# ./1.sh fork
PID for 1.sh before exec/source/fork:8944
1.sh: $A is B
using fork by default...
PID for 2.sh: 8945
2.sh get $A=B from 1.sh
2.sh: $A is C
PID for 1.sh after exec/source/fork:8944
1.sh: $A is B



[root@localhost ~]# ./1.sh source
PID for 1.sh before exec/source/fork:8946
1.sh: $A is B
using source...
PID for 2.sh: 8946
2.sh get $A=B from 1.sh
2.sh: $A is C
PID for 1.sh after exec/source/fork:8946
1.sh: $A is C




[root@localhost ~]# ./1.sh exec
PID for 1.sh before exec/source/fork:8947
1.sh: $A is B
using exec...
PID for 2.sh: 8947
2.sh get $A=B from 1.sh
2.sh: $A is C

#可以看到exec命令 没有继续执行1.sh中在exec 命令后带命令,exec命令的进程id还是和父进程相同

[root@localhost ~]# 

```

### ⑩ mount/umount命令 (mount  挂载  umount 取消挂载)

```sh
mount [-t vfstype] [-o options]  device  dir
-a 安装在/etc/fstab文件中类出的所有文件系统。
-f 伪装mount，作出检查设备和目录的样子，但并不真正挂载文件系统。
-n 不把安装记录在/etc/mtab 文件中。
-r 讲文件系统安装为只读。
-v 详细显示安装信息。
-w 将文件系统安装为可写，为命令默认情况。
-t vfstype 指定文件系统的类型，通常不必指定，mount 会自动选择正确的类型
光盘或光盘镜像：iso9660 
DOS fat16文件系统：msdos 
Windows 9x fat32文件系统：vfat 
Windows NT ntfs文件系统：ntfs 
Mount Windows文件网络共享：smbfs 
UNIX(LINUX) 文件网络共享：nfs
-o options 主要用来描述设备或档案的挂接方式。
loop：用来把一个文件当成硬盘分区挂接上系统 
ro：采用只读方式挂接设备 
rw：采用读写方式挂接设备 
iocharset：指定访问文件系统所用字符集


device  要挂接(mount)的设备。
dir设备在系统上的挂接点(mount point)


#查看已经挂载的情况
mount
#将/home/sunky/mydisk.iso文件作为硬盘分区，以文件系统iso9660 挂载到/mnt/vc/drom下
mount -o loop -t iso9660 /home/sunky/mydisk.iso /mnt/vcdrom

mount -o loop -t iso9660 /iso/windows.iso  /test

#通过指定挂载的文件夹取消挂载 
umount /test
#通过指定挂载的文件取消挂载
umount  /iso/windows.iso


mount -o loop /iso/CentOS-7.9-x86_64-DVD-2009.iso  /test
```

#### 挂载u盘

https://www.cnblogs.com/lifexy/p/7891883.html

```sh
#查看全部的存储设备
fdisk -l

[root@localhost test]# fdisk -l
WARNING: fdisk GPT support is currently new, and therefore in an experimental phase. Use at your own discretion.

磁盘 /dev/sda：21.5 GB, 21474836480 字节，41943040 个扇区
Units = 扇区 of 1 * 512 = 512 bytes
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节
磁盘标签类型：gpt
Disk identifier: 79DB8992-BC27-4A52-8134-B39F310D67F1


#         Start          End    Size  Type            Name
 1         2048       976895    476M  EFI System      EFI System Partition
 2       976896     41940991   19.5G  Microsoft basic 

磁盘 /dev/sdb：67.1 GB, 67108864000 字节，131072000 个扇区
Units = 扇区 of 1 * 512 = 512 bytes
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节
磁盘标签类型：dos
磁盘标识符：0xcad4ebea

   设备 Boot      Start         End      Blocks   Id  System
/dev/sdb4   *         256   131071999    65535872    7  HPFS/NTFS/exFAT
[root@localhost test]# 

#可以看到插入的u盘被分配的设备号为/dev/sdb4
mount -t ntfs /dev/sdb4  /test


```





