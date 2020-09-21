# make工具和Makefile

https://seisman.github.io/how-to-write-makefile/overview.html







## 1.make是什么

make 是一个工程管理器

利用make工具可以自动完成编译工作，这些工作包括：

- 如果修改了某几个源文件，则只重新编译这几个源文件
- 如果某个头文件被修改了，则重新编译所有包含该头文件的源文件

利用这种自动编译可以大大简化开发工作，避免不必要的重新编译。make工具通过一个称为Makefile的文件来完成并自动维护编译工作，Makefile文件描述了整个工程的编译、连接规则。



## 2.Makefile是什么

Makefile 文件描述了整个工程的编译、连接等规则。其中包括：工程中的哪些源文件需要编译以及如何编译、需要创建那些中间文件以及如何创建这些中间文件、如何最后产生我们想要得可执行文件。尽管看起来可能是很复杂的事情，但是为工程编写 Makefile 的好处是能够使用一行命令来完成“自动化编译”，一旦提供了正确的 Makefile，编译整个工程你所要做的唯一的一件事就是输入 make 命令。整个工程完全自动编译，极大提高了效率。

==根据Makefile文件的编写格式，其实他就是依赖规则的集合体==



## 3.make和Makefile的关系

make 是工具，Makefile是配置文件，我们在使用make工具的时候会读取Makefile中的内容然后进行程序的编译生成

Makefile和makefile是默认的配置文件名，我们在命令行执行make命令的时候不指定配置文件的名字就会自动调用同一目录下的Makefile

```
不指定配置文件
make

指定配置文件为mymakefile
make -f mymakefile
```

==如果同一个目录下存在Makefile和makefile两个配置文件，那么会优先选择makefile，但我们看到的代码中往往都是Makefile（原因：程序员总是把自己的身份放低一级，所以使用Makefile）==

## 4.make 工作流程

它的具体工作顺序是：当在 shell 提示符下输入 make 命令以后。 make 读取当前目录下的 Makefile 文件，并将 Makefile 文件中的第一个目标作为其执行的“终极目标”，开始处理第一个规则（终极目标所在的规则）。在我们的例子中，第一个规则就是目标 "main" 所在的规则。规则描述了 "main" 的依赖关系，并定义了链接 ".o" 文件生成目标 "main" 的命令；make 在执行这个规则所定义的命令之前，首先处理目标 "main" 的所有的依赖文件（例子中的那些 ".o" 文件）的更新规则（以这些 ".o" 文件为目标的规则）。

对这些 ".o" 文件为目标的规则处理有下列三种情况：

- 目标 ".o" 文件不存在，使用其描述规则创建它；
- 目标 ".o" 文件存在，目标 ".o" 文件所依赖的 ".c" 源文件 ".h" 文件中的任何一个比目标 ".o" 文件“更新”（在上一次 make 之后被修改）。则根据规则重新编译生成它；
- 目标 ".o" 文件存在，目标 ".o" 文件比它的任何一个依赖文件（".c" 源文件、".h" 文件）“更新”（它的依赖文件在上一次 make 之后没有被修改），则什么也不做。


通过上面的更新规则我们可以了解到中间文件的作用，也就是编译时生成的 ".o" 文件。作用是检查某个源文件是不是进行过修改，最终目标文件是不是需要重建。我们执行 make 命令时，只有修改过的源文件或者是不存在的目标文件会进行重建，而那些没有改变的文件不用重新编译，这样在很大程度上节省时间，提高编程效率。小的工程项目可能体会不到，项目工程文件越大，效果才越明显。

当然 make 命令能否顺利的执行，还在于我们是否制定了正确的的依赖规则，当前目录下是不是存在需要的依赖文件，只要任意一点不满足，我们在执行 make 的时候就会出错。所以完成一个正确的 Makefile 不是一件简单的事情。



我们使用make命令默认把第一个目标作为我们终极目标，当然我们也可以指定Makefile文件中的一个目标作为我们执行的终极目标

如下方的Makefile文件

我们在shell 中执行make，默认把edit作为我们的终极目标



如果我们想把其他目标作为我们生成的终极目标呢

执行make  目标名       ，如我们只想生成search.o，那么执行make search.o

```
edit : main.o kbd.o command.o display.o \
        insert.o search.o files.o utils.o
    cc -o edit main.o kbd.o command.o display.o \
        insert.o search.o files.o utils.o

main.o : main.c defs.h
    cc -c main.c
kbd.o : kbd.c defs.h command.h
    cc -c kbd.c
command.o : command.c defs.h command.h
    cc -c command.c
display.o : display.c defs.h buffer.h
    cc -c display.c
insert.o : insert.c defs.h buffer.h
    cc -c insert.c
search.o : search.c defs.h buffer.h
    cc -c search.c
files.o : files.c defs.h buffer.h command.h
    cc -c files.c
utils.o : utils.c defs.h
    cc -c utils.c
clean :
    rm edit main.o kbd.o command.o display.o \
        insert.o search.o files.o utils.o
```



## 5.使用make工具的好处

make工具根据Makefile中的目标文件和依赖文件的关系，查看目标文件和依赖文件修改的时间，根据两者时间的关系采取不同策略

如果目标文件最近修改时间比依赖文件最近修改时间要早，那么就会重新执行生成当前目标文件的命令

如果目标文件最近修改时间比依赖文件最近修改时间要晚，那么就不执行命令



所以这样就能做到，当我们在编写大项目的时候，我们已经编译了一次，但我们又修改了部分代码，使用make工具不会重新全部编译，只会根据Makefile文件中的依赖关系重新编译修改的代码影响到的目标文件







## 6.Makefile编写

### 格式：

```
根据上文的描述,Makefile其实就是一个依赖规则的集合体，每个规则表示了生成一个目标文件需要的依赖文件和命令，所以每个规则的格式如下方

[需要生成的目标文件]: [生成改目标文件所依赖的文件] [生成改目标文件所依赖的文件][生成改目标文件所依赖的文件]...
[TAB键][生成目标文件所用到的命令]


举例子
main:a.o b.o
	gcc a.o b.o -o main


自动化变量
$@ 表示规则中的目标文件集。在模式规则中，如果有多个目标，那么，$@ 就是匹配于目标中模式定义的集合。
$% 当目标是函数库文件时，表示规则中的目标成员名。例如，如果一个目标是 foo.a (bar.o)，那么，$% 就是 bar.o，$@ 就是 foo.a。如果目标不是函数库文件（Unix 下是 .a，Windows 下是 .lib），那么，其值为空。
$< 依赖目标中的第一个目标名字。如果依赖目标是以模式（即 %）定义的，那么 $< 将 是符合模式的一系列的文件集。
$? 所有比目标新的依赖目标的集合。以空格分隔。
$^ 所有的依赖目标的集合。以空格分隔。如果在依赖目标中有多个重复的，那个这个变量 会去除重复的依赖目标，只保留一份。
$+ 这个变量很像 "$^"，也是所有依赖目标的集合。只是它不去除重复的依赖目标。
$* 这个变量表示目标模式中 % 及其之前的部分。如果目标是 dir/a.foo.b，并且目标的模式是 a.%.b，那么，$* 的值就是 dir/a.foo。

```

### Makefile模板

```
tar=main
objs=main.o  tools1.o  tools2.o
cc=gcc
$(tar):$(objs)
	$(cc) $(objs) -o $(tar)
%.o:%.c
	$(cc)  $^ -o $@

.PHONY : clean
clean :
	rm -f *.o 

```

