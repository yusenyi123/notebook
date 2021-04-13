# c语言语法要点

## 声明 定义 初始化的概念

### 1.声明

**声明**（declaration ）指定了一个变量的标识符，用来描述变量的类型，是类型还是对象，或者函数等。声明，用于编译器(compiler)识别变量名所引用的实体。以下这些就是声明：

extern int bar;

extern int g(int, int);

double f(int, double); // 对于函数声明，extern关键字是可以省略的。

class foo; // 类的声明，前面是不能加class的。

struct  Student { char cName[20]; char cSex; int iGrade; };  //结构体的声明

### 2.定义

**定义**是对声明的实现或者实例化。连接器(linker)需要它（定义）来引用内存实体。与上面的声明相应的定义如下：

int bar;   //定义一个int类型的变量，占用内存中一块区域，但没有初始化

int g(int lhs, int rhs) {return lhs*rhs;} 

double f(int i, double d) {return i+d;} 

struct  Student  student1；  //定义了一个结构体变量student1

class foo {};// foo 这里已经拥有自己的内存了，对照上面两个函数，你就应该明白{}的用处了吧？

### 3.初始化

**初始化**指将声明的变量设置初始值

int a=1;   //定义了一个变量a，并进行了初始化，将a的值设置为1





## 全局变量和局部变量

```c
int x,y; //全局变量x,y
int f1(int a){
    int b,c;  //a,b,c仅在函数f1()内有效,局部变量a,b,c
    return a+b+c;
}
int main(){
    int m,n;  //m,n仅在函数main()内有效,局部变量m,n
    return 0;
}
```

### 全局变量和局部变量的默认值(定义一个变量(任何变量，包括指针变量等)就会占据内存中的一块区域，如果没有初始化，那么这块区域中存储的值为何值)

```
1、局部变量。

局部变量在没有显式初始化时，其值C语言规范没做要求，可以是随机值，也可以是编译器随意给定的值。

比如gcc编译器的局部变量就是随机值，可能为任何值(那就是以前残留在堆栈里的随机值)。而微软的编译器，如VC或VS，则会初始化为全c，即0xCCCCCCCC。

2、全局变量或静态局部变量。
所有的全局变量，即定义在函数外的变量，默认值为0。

所有的静态局部变量，即定义在函数内部的static int name形式的，默认初始化为0。

```

```
如果只声明指针变量, 是没有默认值的, 是碰巧内存分配的一个地址, 不是默认为NULL
所以好的编程习惯, 总是声明是手动为之赋值
int *p=NULL;

C中对NULL的预定义有两个：

　　#define NULL 0
　　#define NULL ((void *)0)
并且标准C规定，在初始化、赋值或比较时，如果一边是变量或指针类型的表达式，则编译器可以确定另一边的常数0为空指针，并生成正确的空指针值。即在指针上下文中“值为0的整型常量表达式”在编译时转换为空指针。那么也就是上面的两个的定义在指针上下文中是一致的了。

我们经常在声明一个指针时，为避免野指针的情况常用的int pi = NULL;中的NULL，是会被自动转换为(void )0的。所以下面的代码也是合法的：


声明一个指针类型 int *p;，则是为存储指针p分配空间，而并不会为p所指向的内存做任何动作，这就是野指针的原因。如下代码，p就是一个未指向任何已知内存的指针，为*p赋值，自然会出现错误：
　　int *p;
　　*p = 1;
```

## C语言中的NULL和0地址

https://lxiange.com/posts/null-in-c.html



![image-20210412213516002](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210412213516.png)

```
为0的逻辑地址如果进行使用，如下方代码
int *p=0;
*p=111;
程序运行是会报错的，所以0地址通常被我们用来做一些特殊的事情，比如当定义指针变量时设置初始值为NULL(NULL是一个宏定义,其值为0)
当我们对这个设置为NULL的指针就行操作时就会报错






C语言中对NULL的预定义：
#undef NULL
#if defined(__cplusplus)
#define NULL 0
#else
#define NULL ((void *)0)
#endif

并且标准C规定，在初始化、赋值或比较时，如果一边是变量或指针类型的表达式，则编译器可以确定另一边的常数0为空指针，并生成正确的空指针值。即在指针上下文中“值为0的整型常量表达式”在编译时转换为空指针。那么也就是上面的两个的定义在指针上下文中是一致的了。
我们经常在声明一个指针时，为避免野指针的情况常用的int pi = NULL;此处的NULL，是会被自动转换为(void *)0






```



## C语言函数指针

```

尽管函数并不是变量，但它在内存中仍有其物理地址。每个函数都有一个入口地址，由函数名指向这个入口地址，函数名相当于一个指向其函数入口的指针常量。

可以将函数名赋值给一个指针，使该指针指向这个函数的入口，即是函数指针

定义一个函数指针的语法:
int (*p)(int x, int  y);  //参数名可以去掉，并且通常都是去掉的。这样指针p就可以保存函数类型为两个整型参数，返回值是整型的函数地址了。

int (*p)(int, int); 





举例子
int Max(int a, int b);
int Min(int a, int b);

int (*p)(int a, int b);
int max, min;
p = Max;
max = (*p)(3, 5);   //进行调用时，也要记得带括号(*p)
p = Min;
```





## 奇怪的 void* 指针

https://blog.popkx.com/c%E8%AF%AD%E8%A8%80%E9%99%B7%E9%98%B1%E4%B8%8E%E6%8A%80%E5%B7%A7%E7%AC%AC31%E8%8A%82-%E9%83%BD%E8%AF%B4void%E6%8C%87%E9%92%88%E6%98%AF%E4%B8%87%E8%83%BD%E6%8C%87%E9%92%88/

事实上，C语言标准库提供了非常丰富的成熟函数供程序员使用，不过不知道读者注意到没，有些库函数的参数是 void * 类型的，例如：

```c
void *memcpy(void *dest, const void *src, size_t n);
void *memmove(void *dest, const void *src, size_t n);
```

前面的章节在讨论C语言指针时，提到指针从某种程度上来说，无非就是一个地址，它的类型只是用于说明数据结构的。例如 int * 型指针告诉编译器该地址处紧接着的 4 字节按照 int 型数据解释，double * 型指针高速编译器接下来的 8 字节按照 double 型数据解释。

> 这里假定int型变量占用4字节内存空间，double 型变量占用 8 字节内存空间。

对于 void * 型指针，之前的分析似乎就不再适用了。因为 void 类型是一个特殊的类型，常被称作“空类型”，C语言中没有 void 类型的变量，所以在遇到 void * 指针时，**编译器根本不知道如何解释接下来的内存，甚至编译器都不知道接下来多少内存属于它**。

正因为如此，在C语言程序开发中，遇到 void * 指针时，如果需要访问它指向的内存，需要重新指定类型，否则就会报错。例如下面这段C语言代码：

```c
#include <stdio.h>
void myprint(void *p)
{
    char c = p[0];
    printf("c=%c\n", c);
}
int main()
{
    char buf[] = "hello world";
    myprint(buf);

    return 0;
}
```

![img](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210412211918.png)

编译这段C语言代码，得到如下输出：

![img](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210412212020.png)

可以看出，这段代码在编译阶段就出错了，原因我们已经分析过：编译器不知道该如何解释 void * 型指针 p。所以在使用 void * 型指针时，要将其先转换为 char * 型，相应C语言代码如下：

```c
void myprint(void *p)
{
    char c = ((char*)p)[0];
    printf("c=%c\n", c);
}
```

有时候为了简便，常常使用中间变量：

```c
void myprint(void *p)
{
    char *cp = (char*)p;
    char c = cp[0];
    printf("c=%c\n", c);
}
```

这时再编译执行就一切正常了。







## 结构体变量名和结构体变量指针

```
C语言的数组类型中，数组变量名就是地址

但是在结构体中结构体变量名不是结构体的地址
```

