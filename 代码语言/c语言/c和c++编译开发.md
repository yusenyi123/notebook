# c和c++编译开发



## C语言源代码到可执行程序的生成过程

```
从源代码到可执行程序需要四个步骤

预处理 gcc -E hello.c -o helle.i
汇编 gcc -S hello.i -o hello.s
编译 gcc -c hello.s -o hello.o
链接 gcc hello.o -o hello

上面四个命令可以一个命令一步到位 
gcc hello.c -o hello
```





Makefile模板

![image-20200912114726708](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200912114726.png)