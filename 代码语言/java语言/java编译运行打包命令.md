# java编译运行打包命令





## [参数]  {参数}  <参数>中[] <> {}三个括号的含义

```
1. []：内的内容意思是：可写可不写
   例如：/home下就一个list 文件，使用ls --help中的 Usage: ls [OPTION]... [FILE]...
2. {}：那就必须要在{}内给出的选择里选一个。
3. <>：表示必选
```



## 1.设置环境变量PATH和CLASSPATH



### PATH设置和作用

```
设置JAVA_HOME变量 D:\software\coderunenv\java8

再设置PATH变量
%JAVA_HOME%\bin
%JAVA_HOME%\jre\bin

```



```
计算机如何查找命令呢？Windows 操作系统根据 Path 环境变量来查找命令。Path 环境变量的值是一系列路径，Windows 操作系统将在这一系列的路径中依次查找命令，如果能找到这个命令，则该命令是可执行的；否则将出现“'xxx'不是内部或外部命令，也不是可运行的程序或批处理文件”的提示。而Linux 操作系统则根据 PATH 环境变量来查找命令，PATH 环境变量的值也是一系列路径。
提示：
Windows操作系统不区分大小写，设置 Path 和 PATH 并没有区别；而 Linux 系统是区分大小写的，设置 Path 和
PATH 是有区别的，因此对比两个操作系统我们只需要设置 PATH 环境变量即可。

```



### CLASSPATH设置和作用

```
.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar
```



```
那么 CLASSPATH 环境变量的作用是什么呢？
使用“java 全类名(包名.类名)”命令来运行 Java 程序时，
JRE 到哪里去搜索 Java 类呢？可能有读者会回答，在当前路径下搜索啊。这个回答很聪明，但 1.4 以前版本的 JDK 都没有设计这个功能，这意味着即使当前路径已经包含了 HelloWorld.class，并在当前路径下执行“java HelloWorld”，系统将一样提示找不到 HelloWorld 类。
如果使用 1.4 以前版本的 JDK，则需要在 CLASSPATH 环境变量中添加一点（.），用以告诉 JRE 需要在当前路径下搜索 Java 类。
除此之外，编译和运行 Java 程序还需要 JDK 的 lib 路径下 dt.jar 和 tools.jar 文件中的 Java 类，因此还需要把这两个文件添加到 CLASSPATH 环境变量里。
```



```
用于编译 Java 程序所使用的 javac.exe 命令是使用 Java 编写的，这个类就是 lib 路径下 tools.jar 文件中 sun\tools\javac 路径下的 Main 类，JDK 的 bin 路径下的 javac.exe 命令实际上仅仅是包装了这个 Java 类。不仅如此，bin 路径下的绝大部分命令都是包装了 tools.jar文件里的工具类。
```







## 2.安装的JDK下的文件夹作用详解











## 3. java编译 运行  文档注释  打包

### 3.1  javac命令(编译java文件)

#### 参考：

https://blog.csdn.net/wenxindiaolong061/article/details/103397574



#### 常用参数

```
-d  d就是 destination，用于指定.class文件的生成目录，在eclipse中，源文件都在src中，编译的class文件都是在bin目录中
如果某个类是一个包的组成部分，则 javac 将把该类文件放入反映包名的子目录中，必要时创建目录。
使用-d如果编译的java文件是某个包下的，还会在指定目录下创建包文件夹

例如，如果指定 -d c:\myclasses 并且该类名叫 com.mypackage.MyClass，那么在c:\myclasses目录下编译的时候就会创建包文件夹,最终该类的字节码被存放在
c:\myclasses\com\mypackage\MyClass.class


编译java源文件的时候,对于这个文件所依赖的类需要指定他的位置(这个位置可以是目录，JAR 归档文件位置或 ZIP 归档文件位置)，如果指定的这个依赖类的源代码文件位置，那么编译器会先将源代码编译

-classpath(-cp)  指定编译所依赖的类的class文件的查找位置(这个位置可以是目录，JAR 归档文件位置或 ZIP 归档文件位置)。在Linux中，用“:”分隔classpath，而在windows中，用“;”分隔。
注意:如果是指定目录，那么指定的目录是该class所在包的包的目录，而不是class类所在的目录

     
-sourcepath    指定编译所依赖的类的java源代码文件的查找位置(这个位置可以是目录，JAR 归档文件位置或 ZIP 归档文件位置)





-encoding <编码>  指定源文件使用的字符编码，如果源文件使用的UTF-8编码，而此参数被设定为GBK，则编译将出现乱码

-verbose  输出详细的编译信息，包括：classpath、加载的类文件信息。

-source 使用指定版本的JDK编译，比如：-source 1.4表示用JDK1.4的标准编译，如果在源文件中使用了泛型，则用JDK1.4是不能编译通过的。
-target 指定生成的class文件要运行在哪个JVM版本，以后实际运行的JVM版本必须要高于这个指定的版本。



如果有文件为A.java（其中有类A），且在类A中使用了类B，类B在B.java中，则编译A.java时，默认会自动编译B.java，且生成B.class。
implicit:none    不自动生成隐式引用的类文件。
implicit:class（默认）  自动生成隐式引用的类文件。




@<文件名>
如果同时需要编译数量较多的源文件(比如1000个)，一个一个编译是不现实的（当然你可以直接 javac *.java ），比较好的方法是：将你想要编译的源文件名都写在一个文件中（比如sourcefiles.txt），其中每行写一个文件名，如下所示：
在windows下配合下面命令，实现编译整个工程
dir /s /B *.java > sources.txt

/s      显示指定目录和所有子目录中的文件
/B      输出的信息中没有标题信息或摘要


在linux下配合下面命令，实现编译整个工程
find -name "*.java" > sources.txt

编译sources.txt中的java文件，将编译后的字节码输出到当前目录下
javac -d .  @sources.txt


-bootclasspath、-extdirs
-bootclasspath和-extdirs 几乎不需要用的，因为他是用来改变 “引导类”和“扩展类”。

引导类(组成Java平台的类)：Java\jdk1.7.0_25\jre\lib\rt.jar等，用-bootclasspath设置。
扩展类：Java\jdk1.7.0_25\jre\lib\ext目录中的文件，用-extdirs设置。


-g：在生成的class文件中包含所有调试信息（行号、变量、源文件）
-g:none ：在生成的class文件中不包含任何调试信息。

-J<标志>  直接将 <标志> 传递给运行时系统
```





#### javac命令完整参数

```
用法: javac <options> <source files>

其中, 可能的选项包括:

  -g                         在生成的class文件中包含所有调试信息（行号、变量、源文件）

  -g:none                    在生成的class文件中不生成任何调试信息

  -g:{lines,vars,source}     只生成某些调试信息，eclipse的断点调试功能，就是通过设置此参数实现的。

  -nowarn                    不生成任何警告

  -verbose                   输出有关编译器正在执行的操作的消息，包括：classpath、加载的类文件信息

  -deprecation               输出使用已过时的 API 的源位置，如果你在源文件中使用了“已过时的API”，则生成详细警告信息

  -classpath(-cp)  指定编译所依赖的类的class文件的查找位置(它们可以是目录、JAR 归档文件或 ZIP 归档文件)。在Linux中，用“:”分隔classpath，而在windows中，用“;”分隔。
   
   
  -sourcepath    指定编译所依赖的类的java源代码文件的查找位置(它们可以是目录、JAR 归档文件或 ZIP 归档文件)

  -bootclasspath <路径>        覆盖引导类文件的位置

  -extdirs <目录>              覆盖所安装扩展的位置

  -endorseddirs <目录>         覆盖签名的标准路径的位置

  -proc:{none,only}          控制是否执行注释处理和/或编译。

  -processor <class1>[,<class2>,<class3>...] 要运行的注释处理程序的名称; 绕过默认的搜索进程

  -processorpath <路径>        指定查找注释处理程序的位置

  -d <目录>                    指定放置生成的类文件的位置

  -s <目录>                    指定放置生成的源文件的位置

  -implicit:{none,class}     指定是否为隐式引用文件生成类文件

  -encoding <编码>             指定源文件使用的字符编码，如果源文件使用的UTF-8编码，而此参数被设定为GBK，则编译将出现乱码

  -source <发行版>              使用指定版本的JDK编译，比如：-source 1.4表示用JDK1.4的标准编译，如果在源文件中使用了泛型，则用JDK1.4是不能编译通过的。

  -target               指定生成的class文件要运行在哪个JVM版本，以后实际运行的JVM版本必须要高于这个指定的版本。

  -version                   版本信息

  -help                      输出标准选项的提要

  -A关键字[=值]                  传递给注释处理程序的选项

  -X                         输出非标准选项的提要

  -J<标记>                     直接将 <标记> 传递给运行时系统

  -Werror                    出现警告时终止编译

  @<文件名>                     从文件读取选项和文件名
```







### 3.2   java命令(运行java程序     java  全类名)





#### -classpath(--cp)参数

```
指定文件夹路径 zip文件所在路径 jar文件所在路径  在这些路径下查找运行的类所在的字节码文件。
注意点:如果运行的字节码文件是在一个包路径下的，指定的搜索文件夹路径是这个包所在的文件夹路径，而不是这个字节码所在的文件路径
                          
举例说明：假设在E:\test下有一个dog.java文件,代码为

package cn.itany.test;

public class dog{
	
static	int a=2;

public static void main(String [] args)
{
	System.out.println(a);
	
}
		
}

使用javac -d . dog.java进行编译
编译后 dog.class 文件被保存在E:\test\cn\itany\test下

我们要运行这个dog类
在cmd中执行的命令是 
java -cp E:\test  cn.itany.test.dog

注意这里的运行的类名是全类名(包名.类名)，原因是多个包下可能有同名的类,这时候就不知道运行哪个包下的类了

这里-cp 指定的参数是E:\test  是cn.itany.test这个包所在的路径

如果执行
java -cp E:\test\cn\itany\test   cn.itany.test.dog

会出现下列错误
E:\test\cn\itany\test>java -cp E:\test\cn\itany\test   cn.itany.test.dog
错误: 找不到或无法加载主类 cn.itany.test.dog
```







#### java命令完整参数

```
java
用法: java [-选项] 全类名(包名.类名)   [给main函数传递的参数...]   
         (执行一个类)   
 或者 java [-选项] -jar jar文件     [给main函数传递的参数...]
         (执行一个jar文件)
         
         
 其中，可能的选项包括：
 
     -classpath(简写-cp)  指定class类所属包的包文件夹路径 zip文件所在路径 jar文件所在路径  在这些路径下查找运行的类所在的字节码文件
                          注意点，如果运行的字节码文件是在一个包路径下的，指定的搜索文件夹路径是这个包所在的文件夹路径，
                          而不是这个字节码所在的文件路径
                          
                          
                          
                        
      
     -client       选择 "client" VM(ginger547:应该是指Virtual Machine)
     -server       选择 "server" VM
     -hotspot      与 "client" VM同义  [不赞成]
                   默认情况的VM是client.
                   
                   
                 
     -D<名字>=<值>
                   设置一个系统属性
     -verbose[:class|gc|jni]
                   输出详细信息
                   
     -version      打印产品版本然后退出
     -version:<值>
                   只运行指定版本
     -showversion  打印产品版本后继续
     -jre-restrict-search | -jre-no-restrict-search
                   在版本搜索的时候，包含/排除用户私人的JRE
     -? -help      打印帮助信息
     -X            打印非标准选项帮助
     -ea[:<包名>...|:<类名>]
     -enableassertions[:<包名>...|:<类名>]
                  使断言可用
     -da[:<包名>...|:<类名>]
     -disableassertions[:<包名>...|:<类名>]
                   是断言不可用
     -esa | -enablesystemassertions
                   使系统级断言可用
     -dsa | -disablesystemassertions
                   使系统级断言不可用
     -agentlib:<库名>[=<选项>]
                   加载本地代理库<库名>,例如. -agentlib:hprof
                   同时可查看, -agentlib:jdwp=help和 -agentlib:hprof=help
     -agentpath:<路径名>[=<选项>]
                   通过全路径名来加载本地代理库
     -javaagent:<jar路径>[=<选项>]
                  加载Java编程语言代理，可查看 java.lang.instrument
```





### 3.3 javadoc命令(根据java源码中的文档注释生成api说明文档)

#### 参考：

https://www.cnblogs.com/bluestorm/archive/2012/10/04/2711329.html

https://www.cnblogs.com/maokun/p/7067580.html

https://www.xdnote.com/javadoc/

https://my.oschina.net/u/4385577/blog/3483405

https://www.iteye.com/blog/strong-life-126-com-806246

https://blog.csdn.net/u013084266/article/details/105948544

#### 文档注释的格式

生成的文档是 HTML 格式，而这些 HTML 格式的标识符并不是 javadoc 加的，而是我们在写注释的时候写上去的。
比如，需要换行时，不是敲入一个回车符，而是写入 `<br>`，如果要分段，就应该在段前写入 `<p>`。
文档注释的正文并不是直接复制到输出文件 (文档的 HTML 文件)，==而是读取每一行后，删掉前导的 * 号及 * 号以前的空格，再输入到文档的==。

#### 编写注意点：

1.简述开头的第一句话，使用`.或。`表示简述结束

2.简述后面就是详细描述了

3.使用javadoc标记描述参数，返回值，异常等信息



```java
  /**
   * show 方法的简述 。
   * <p>show 方法的详细说明第一行<br>
   * show 方法的详细说明第二行
   * @param a
   * 
   * 
   * 参数a的描述 就是这么多
   * 
   * 
   * 
   * @param b
   * 
   * 
   * 参数b的描述 就是这么多
   * 
   * @return
   * 
   * 返回值的描述，就是这么多
   * 
   */
  public int  show(int a,int b)
  {
	  return 1;
  }
```



#### java代码中的文档注释中使用 javadoc 标记

```


javadoc 标记由"@"及其后所跟的标记类型和专用注释引用组成
javadoc 标记有如下一些：
@author 标明开发该类模块的作者
@version 标明该类模块的版本
@see 参考转向，也就是相关主题
@param 对方法中某参数的说明
@return 对方法返回值的说明
@exception 对方法可能抛出的异常进行说明

@author 作者名
@version 版本号
其中，@author 可以多次使用，以指明多个作者，生成的文档中每个作者之间使用逗号 (,) 隔开。@version 也可以使用多次，只有第一次有效

使用 @param、@return 和 @exception 说明方法
这三个标记都是只用于方法的。@param 描述方法的参数，@return 描述方法的返回值，@exception 描述方法可能抛出的异常。它们的句法如下：
@param 参数名参数说明
@return 返回值说明
@exception 异常类名说明

```



#### 包注释(package.html和package-info.java)

##### package.html的模板(`<p>换段 <br> 换行  换段比换行间隔更大`)

```
<html>
<body>
一句话简介，使用.或者。结尾

<p>
包详细描述。
<p>产品模块名称和版本
<br>公司版权信息
</body>
</html>

```

##### 使用模板

```
<html>
<body>
test包的作用是提供一些测试工具类。


<p>
下面是test包的详细说明
<p>
包含集合框架、遗留的 collections 类、事件模型、日期和时间设施、国际化和各种实用工具类（字符串标记生成器、随机数生成器和位数组、日期Date类、堆栈Stack类、向量Vector类等）。集合类、时间处理模式、日期时间工具等各类常用工具包
<p>
⑴Collection （）接口，扩展了Iterable接口，位于集合层次结构的顶部，因此所有的集合都实现Collection接口，并提供了iterator（）方法来返回一个迭代器。用add（）方法添加对象，remove（）方法删除元素，clear（）删除集合所有元素（size=0），contains（）方法查看集合是否包含对象，toArray（）方法返回集合元素数组，equals（）方法比较两个集合是否相等，size（）方法返回集合中元素的数目，isEmpty（）判断集合是否为空，hashCode（）返回调用集合的散列码，iterator（）返回调用集合的迭代器。
<p>
⑵List（）接口，扩展了Collection接口，存储一个序列的元素（列表基于0的索引），可以包含重复的元素，但不能有null值。获得特定位置的对象调用get（）方法，用set（）方法给特定位置元素赋值，用indexOf（）或lastIndexOf（）方法分别获得对象的第一个实例或最后一个实例所在的位置，subList（）方法取子列表，listIterator（）返回一个迭代器。
<p>
⑶Set接口，扩展了Collection接口，该集合不允许存在相同的元素（包括唯一null值）。SortedSet接口，扩展了Set接口并声明自已是升序的集合。First（）或Last（）方法分别获得第一或最后一个对象，subSet（）获得子集，headSet（）和tailSet（）方法分别获得从头开始或直到末尾的子集。

<br>公司版权信息
<br>
<br>
<br>
<br>
</body>
</html>

```

##### package-info.java的模板











#### javadoc完整参数

```
用法：
　　javadoc [options] [packagenames] [sourcefiles]

选项：

-public 仅显示 public 类和成员
-protected 显示 protected/public 类和成员 (默认)
-package 显示 package/protected/public 类和成员
-private 显示所有类和成员


-d <directory> 输出文件的目标目录
-version 包含 @version 段
-author 包含 @author 段
-splitindex 将索引分为每个字母对应一个文件
-windowtitle <text> 文档的浏览器窗口标题



javadoc    -encoding UTF-8 -charset UTF-8  -d apidoc  -windowtitle 测试  -doctitle 学习javadoc工具的测试 API 文档   -header 我的类   *.java 

```



### 3.4 jar命令(打包归档)

#### 参考:

https://www.jb51.net/article/53601.htm

https://wenku.baidu.com/view/e9c054d26ad97f192279168884868762cbaebbc3.html

https://www.cnblogs.com/Gandy/p/7290069.html

#### jar完整命令参数

```

 
jar命令格式：jar {c t x u f }[ v m e 0 M i ][-C 目录]文件名...
 
其中{ctxu}这四个参数必须选选其一。[v f m e 0 M i ]是可选参数，文件名也是必须的。
 
-c  创建一个jar包
-t 显示jar中的内容列表
-x 解压jar包
-u 添加文件到jar包中


-f 指定jar包的文件名
-v  生成详细的报造，并输出至标准设备
-m 指定manifest.mf文件.(manifest.mf文件中可以对jar包及其中的内容作一些一设置)
-0 产生jar包时不对其中的内容进行压缩处理
-M 不产生所有文件的清单文件(Manifest.mf)。这个参数与忽略掉-m参数的设置
-i    为指定的jar文件创建索引文件
-C 表示转到相应的目录下执行jar命令,相当于cd到那个目录，然后不带-C执行jar命令       
```

#### jar命令使用例子

```
将当前命令执行时所在的文件夹下全部文件归档进一个test.jar文件
写法1
jar cf  test.jar  . 
写法2
jar cf  test.jar  ./






```



#### MANIFEST.MF配置文件

##### MANIFEST.MF配置文件编写格式

```
【MANIFEST.MF格式说明】
1. 文件中的内容以键值对的形式出现，键值对之间采用"冒号+空格"进行分隔(注意：冒号后的空格必须有，否则格式有错误)
如：
Manifest-Version: 1.0

2. 文件每行最多72个字符，可以分多行写，但是在行的末尾必须加上空格符作为续行符(注意：末尾的续行符不能少)
3. 文件的最后必须要空两行，并且这两行都必须顶格(最后一行写完，要回车两次，而且要确保回车的两行都是顶格，这个很重要，否则最后一个配置会被丢弃)
4. 通常指定Class-Path时会采用每一行一个jar包的方法，因为每一行的长度有限制，当jar较多时会超过一行限制的字符数，所以我们通常采用一行一个jar包的方式，在编写的时候一行一个jar，每行末尾加上空格符作为续行符，并且由于多个Classpath之间有空格符进行分隔，所以每行的开头也要加上空格代表是下一个jar包(如示例所示)







```

##### 示例

```
Manifest-Version: 1.0
Created-By: 1.6.0_10-rc2 (Sun Microsystems Inc.)
Premain-Class: itracer.ITracer
Can-Redefine-Classes: true
Can-Retransform-Classes: true
Can-Set-Native-Method-Prefix: true
Class-Path: lib/asm-4.0.jar 
 lib/asm-util-4.0.jar 
 lib/log4j.jar


```

注：示例中指定Class-Path时采用了分行的方法，则每行的后面（除最后一行外）都有一个空格，该空格是作为换行的续行符，并且由于多个Classpath之间有空格符进行分隔，所以在每行的开头有一个空格符。整个文件的最后空两行，并且这两行都必须顶格





```

```







### 3.5 keytool命令(生成密钥和证书)

keytool 是个密钥和证书管理工具。它使用户能够管理自己的公钥/私钥对及相关证书，用于（通过数字签名）自我认证（用户向别的用户/服务认证自己）或数据完整性以及认证服务。

#### 参考：

https://blog.csdn.net/suo082407128/article/details/51982119



#### keytool完整参数

```




-genkey      在用户主目录中创建一个默认文件".keystore",还会产生一个mykey的别名，mykey中包含用户的公钥、私钥和证书
(在没有指定生成位置的情况下,keystore会存在用户系统默认目录，如：对于window xp系统，会生成在系统的C:/Documents and Settings/UserName/文件名为“.keystore”)

-genkeypair：生成一对非对称密钥;

-alias       指定密钥对的别名，该别名是公开的;
-keystore    指定密钥库的路径及名称，不指定的话，默认在操作系统的用户目录下生成一个".keystore"的文件
-keyalg      指定密钥的算法 (如 RSA  DSA（如果不指定默认采用DSA）)
-validity    指定创建的证书有效期多少天
-keysize     指定密钥长度
-storepass   指定密钥库的密码(获取keystore信息所需的密码)
-keypass     指定别名条目的密码(私钥的密码)
-dname       指定证书拥有者信息 例如：  "CN=名字与姓氏,OU=组织单位名称,O=组织名称,L=城市或区域名称,ST=州或省份名称,C=单位的两字母国家代码"


-list        显示密钥库中的证书信息    
keytool -list -v -keystore 指定keystore -storepass 密码

-v           显示密钥库中的证书详细信息


-export      将别名指定的证书导出到文件  
keytool -export -alias 需要导出的别名 -keystore 指定keystore -file 指定导出的证书位置及证书名称 -storepass 密码


-file        参数指定导出到文件的文件名


-delete      删除密钥库中某条目       
keytool -delete -alias 指定需删除的别  -keystore 指定keystore  -storepass 密码

-printcert   查看导出的证书信息         
keytool -printcert -file yushan.crt


-keypasswd   修改密钥库中指定条目口令    
keytool -keypasswd -alias 需修改的别名 -keypass 旧密码 -new  新密码  -storepass keystore密码  -keystore sage


-storepasswd  修改keystore口令     
keytool -storepasswd -keystore e:/yushan.keystore(需修改口令的keystore) -storepass 123456(原始密码) -new yushan(新密码)


-import      将已签名数字证书导入密钥库 
keytool -import -alias 指定导入条目的别名 -keystore 指定keystore -file 需导入的证书
```

