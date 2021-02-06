# cmd命令和dos批处理

## 参考：

http://cache.baiducontent.com/c?m=-U2qldfx7JxGkGhH2sEJKGf3n9IRSAxYM49h0Jotzqt9nZ4gTUvIuEgS3AMYBM3-PtaKQzqknEus3_C5DfMoo2JEymlQ7EyQ8bpNm23gAXvV1lN0FaX_EOgebts-fK2SvnUAL9ji9NOKE6nw3B1P3K&p=8d67841e85cc43ff57ee947d52058f&newp=807fc54ad5c342be0ef3c7710f0d88231610db2151d7d301298ffe0cc4241a1a1a3aecbf2c221107d1c37f6203ad435feff53d763d0034f1f689df08d2ecce7e3ede39&s=1679091c5a880faf&user=baidu&fm=sc&query=%25%25%7E%24PATH%3Ai&qid=b3788d5f00008d8b&p1=11



https://www.cnblogs.com/lizm166/p/11132601.html

## PATHEXT环境变量简介

功能：当你在CMD窗口中，运行某个程序时，可以不输入程序的后缀名，直接运行。

注：当在一个相同的目录结构下，有相同的多个主文件名，不同的文件后缀名时，系统会根据PATHEXT中的后缀名，选择其中顺序最靠前的那一个。



##  字符串截取

对字符串变量的截取操作，

|                           |                |
| ------------------------- | -------------- |
| 前n个字符                 | `%str:~0,n%`   |
| 去掉最后n个字符后的字符串 | `%str:~0,-n%`  |
| 第m个字符开始的n个字符    | `%str:~m-1,n%` |
| 倒数第n个字符为           | `%str:~-n,1%`  |
| 倒数第n个及其之后的字符为 | `%str:~-n%`    |
| 倒数第n个开始的m个字符为  | `%str:~-n,m%`  |

##  goto 和 call 有什么不同

`goto`改变了bat脚本自上而下的执行顺序，将程序的运行跳转到冒号指定的标签处，并从此处往下运行。
`call` 与`qoto`比较类似，也是改变脚本的运行顺序，将程序跳转到指定标签。但是，如果使用的是`call`,则再跳转后遇到`exit`或`eof`时，脚本的运行将回到`call`的调用处，即执行`call`的下边那条指令。
另外，`call·还可以传递参数，在跳转到的标签中，`,`%1`即传递的第一个参数。

## 批文件接收参数(%num %~num)

### **% 批处理变量引导符**

这个百分号严格来说是算不上命令的，它只是批处理中的参数而已（多个%一起使用的情况除外，以后还将详细介绍）。
引用变量用%var%，调用程序外部参数用%1至%9等等
%0 %1 %2 %3 %4 %5 %6 %7 %8 %9 %*为命令行传递给批处理的参数

%0 批处理文件本身，包括完整的路径和扩展名
%1 第一个参数
%9 第九个参数
%* 从第一个参数开始的所有参数
参数%0具有特殊的功能，可以调用批处理自身，以达到批处理本身循环的目的，也可以复制文件自身等等。





当参数以引号开头时，%~1会自动将引号删除。

```
@echo off
call :sub "abc"
pause
call :sub abc"
pause
call :sub "abc
pause
goto :eof
:sub
echo %1 %~1
```

![image-20210131175258586](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210206112904.png)









```bat
@for %%i in (python.exe) do @set py=%%~$PATH:i
@for %%i in ( %py%) do @set py=%%~dpi
@echo %py%
```

这是CALL命令自动翻译的结果，意思是在%PATH%中搜寻%i这个文件，设置个py变量





因为%在bat文件中是特殊字符，连续的两个%表示执行时脱为一个%
在控制台窗口中 把%% 替换为 % 即可正常运行



```
@echo off
set a=c:\windows\system32;c:\windows\;d:\路径可以随便你填
for %%j in (cmd.exe notepad.exe) do echo 在变量a中所列出的路径中寻找j中的文件 %%~$a:j
pause
```

## &和&&

```
&之后的命令无论如何都会被执行。
&&之后的命令只有在前&&之前的命令执行成功オ会被执行。
```

&

## %~dp0更改当前目录为批处理本身的目录 

```
%0 批处理文件本身，包括完整的路径和扩展名

~去除引号

dp字符串扩展

```

## %%~$path:I

```
%%~$path:I——查找path变量设置的目录中是否包含名字为%%I的文件，如果在目录中找到该文件，则输出为该文件的完整磁盘路径。如果path未作定义，或者没找到文件，%%~$path:I的值为空。


@ echo off
for %%I in (explorer.exe) do echo,%path%&echo,%%~$path:I
pause

上面是变量path的值，下面是扩展%%~$path:I的值。
for将explorer.exe赋值给%%I，然后在变量path值中的第一个目录C:\Windows\system32查找explorer.exe，没找到；再到变量path值中的第二个目录C:\Windows查找，找到后就将%%~$path:I扩展为C:\Windows\explorer.exe，并回显。
当然，%%~$path:I可以改成%%~$a:I，但变量a必须事先定义，否则%%~$a:I值为空。
```

