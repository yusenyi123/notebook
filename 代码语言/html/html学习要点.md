# html学习要点



## 1.`<meta charset="UTF-8" />` 作用

```
<meta charset="UTF-8"/> 或者是 <meta charset="utf-8"/>
html属性是不区分大小写的，2个写法都可以正常使用

<meta charset="UTF-8"/>
作用：在http响应头中添加信息(很多服务器并不会因为html文档添加了这句话在响应头中添加charset=utf-8,)，更多的时候是http响应头中没有携带charset 参数，这时候浏览器就会扫描html标签，根据meta标签中的设置来解码字符集去解码html文件


这里要注意的是，这只是告知浏览器采取哪种方式对我们的文档进行解码，如果我们的html文件用了其他方式进行编码比如ANSI，然后我们在文档中添加了<meta charset="UTF-8"/>，那么浏览器就会对我们的文档使用UTF-8的编码方式进行解码，但是文档是ANSI进行编码的，那么就会乱码了

所以：html文档的编码方式和<meta>中charset属性的值要一致。否则浏览器会解析错误造成乱码

<meta charset="UTF-8">是html5后新增的告知浏览器文档编码的属性，原先采用http-equiv="content-type" 和content="text/html; charset=UTF-8" 键值对形式（旧的方式依然可用）

HTML 4.01： <meta http-equiv="content-type" content="text/html; charset=UTF-8">
HTML5： <meta charset="UTF-8">
```





## 2.单标签结束是否需要添加/，如`<img>和<img/> <link>和<link/>`

```
According to the HTML5 spec, tags that cannot have any contents (known as void elements) can be self-closing*. This includes the following tags:
根据HTML5规范，不能包含任何内容的标签（称为void元素）可以自动关闭*。
其中包括以下标签：
area, base, br, col, embed, hr, img, input, 
keygen, link, meta, param, source, track, wbr

在上面的标签上，“ /”是完全可选的，因此<img />与<img>没什么不同，


但XHTML中,标签元素必须正确关闭，必须加上/



```

### 结论：在html5规范中两者没有区别，但是在XHMTL中标签元素必须关闭，为了更好的支持以后的语法，建议加上/





## 3. `<html><head><body>`标签在html文档中是否可以省略

### 结论：可以省略,但是`<title>`标签不能省略

### 可以省略的原理：

```
<html><head><body>都可选标签,当然可以省略（在HTML5标往下事实上，让我们扔掉<html>/<head>/<body>,这也是一个合法的HTML5 document

浏览器会自动始终创建一个head元素，并自上而下依次检查页面源码中的各标签，能够加入到head元素的标签都加入到head元素中，随后将剩余的元素分配到自动创建的body元素中。

```



## 4.`<a></a>`标签5大作用

```
1.外部链接：例如<a href=http://www.baidu.com">百度</a>
2.内部链接：网站内部页面之间的相互链接直接链接内部页面名称即可，例如< a href="index.html">首页</a>
3.本页锚点链接：< a href="#h1">首页</a>
4.下载链接：如果href里面地址是一个文件或者压缩包，会下载这个文件
<a href="img,zip">下载文件</a>
5.网页元素链接在网页中的各种网页元素，如文本、图像、表格、音频、视频等都可以添加超链接
<a href="http://www.baidu.com"> <img src="img.jpg/> </a>


可点击又不跳转超链接
<a  href="javascript:;"></a>
超链接可点击不跳转但地址栏会多一个参数#
http://localhost:63342/webworkspace/webproject3/web/index.html?_ijt=jmgtj4rj0j0tc5bgs3l7u8j6v1#
<a  href="#"></a>
```



## 5.浏览器判断html编码的流程

```
前文描述了浏览器在发送HTTP请求时选取编码的行为。那么从服务器端返回HTTP响应时，浏览器又是如何判断该响应使用了何种编码的呢？

浏览器判断返回的HTTP响应消息所使用的编码遵循以下一系列规则。

首先，浏览器会检查HTTP响应中的“Content-type”消息头。如“text/html; charset=UTF-8”，表明该消息所包含的内容是纯文本的HTML文档，采用UTF-8编码。但在很多情况下，服务器返回的Content- type消息头并不包含“charset”信息。

当响应消息不包含“charset”信息时，浏览器会尝试自动探测编码。第一个步骤是检查响应消息体的开头是否包含UTF-8的BOM（字节顺序标记，ByteOrder Marker）。BOM是一种用来判断文件编码的特定字节标记，如果一个文件的开头几个字节包含了UTF-8的BOM，那么浏览器就可以断定这个HTML 文件是采用UTF-8编码的。

如果该HTML中不包含BOM，那么，浏览器就会尝试寻找HTML页面中的<meta>标记，如：
<meta http-equiv="Content-Type"  content="text/html; charset=UTF-8" />

 
如果页面中又不包含<meta>标记，那么，浏览器将采用默认的编码来解析。在中文的IE和Firefox里就是采用GBK或GB2312编码。

因此，要使服务器端返回的响应消息能够正确地被浏览器解析，最简单有效的方法就是在响应的“Content-type”消息头中设置charset属性。在Servlet编程中可以在doGet()或doPost()方法中调用：

response.setCharacterEncoding("UTF-8")
```

### 请求tomcat中的html文档的时候，默认response的响应头中的 Content-Type: text/html; 没有携带charset ，如何让response的响应头携带charset的设置

```
在web应用的web.xml中添加如下配置

<mime-mapping>
    <extension>html</extension>
    <mime-type>text/html;charset=GBK</mime-type>
</mime-mapping>
```





## 6. 复合选择器总结

| 选择器         | 作用                     | 特征                 | 使用情况 | 隔开符号及用法                                               |
| :------------- | :----------------------- | :------------------- | :------- | :----------------------------------------------------------- |
| 后代选择器     | 用来选择元素后代         | 是选择所有的子孙后代 | 较多     | 符号是**空格** .nav a                                        |
| 子代选择器     | 选择 最近一级元素        | 只选亲儿子           | 较少     | 符号是**> **   .nav>p                                        |
| 交集选择器     | 选择两个标签交集的部分   | 既是 又是            | 较少     | **没有符号**   p.one                                         |
| 并集选择器     | 选择某些相同样式的选择器 | 可以用于集体声明     | 较多     | 符号是**逗号**   .nav, .header                               |
| 链接伪类选择器 | 给链接更改状态           |                      | 较多     | 重点记住 a{} 和 a:hover 实际开发的写法，除了a标签，其他标签也可以使用伪类 |





## 7.emmt快速生成html标签

![image-20210105230313608](C:\Users\sen\AppData\Roaming\Typora\typora-user-images\image-20210105230313608.png)

```
p#a${你是个傻逼$}*5  Tab键

<p id="a1">你是个傻逼1</p>
<p id="a2">你是个傻逼2</p>
<p id="a3">你是个傻逼3</p>
<p id="a4">你是个傻逼4</p>
<p id="a5">你是个傻逼5</p>

```





## 8.uri  url  urn 三个的关系

### 参考：

https://routinepanic.com/questions/what-is-the-difference-between-a-uri-a-url-and-a-urn

```
uri:Uniform Resource Identifier (统一资源标识符) 
url:Uniform Resource Locator (统一资源定位符)
urn:Uniform Resource Name (统一资源名)

URI可以分为URL,URN或同时具备locators 和names特性的一个东西。URN作用就好像一个人的名字，URL就像一个人的地址。换句话说：URN确定了东西的身份，URL提供了找到它的方式

关于URL：
URL是URI的一种，不仅标识了Web 资源，还指定了操作或者获取方式，同时指出了主要访问机制和网络位置
URL 定义了用户所需特定资源的位置以及获取方式，可以指向因特网上的任意资源。




关于URN：
URN是URI的一种，用特定命名空间的名字标识资源。使用URN可以在不知道其网络位置及访问方式的情况下讨论资源。



URL 是一种强有力的工具，但 URL 并不完美，它们表示的是实际的地址，而不是准确的名字，这意味着当资源被移走了，URL 就无法对对象进行定位。如果有了对象的准确名称，不论其位于何处都可以找到这个对象。URN 就有为对象提供一个稳定的名称的。

　　PURL：永久统一资源定位符（Persistent Uniform Resource Locators）。是用 URL 来实现 URN 功能的例子。基本思想是，在搜索资源的过程中引入另一个中间层，通过一个中间资源定位符（resource locator）服务器对资源的实际 URL 进行等级和跟踪。客户端向定位符请求一个永久 URL，定位符可以以一个资源作为响应，将客户端重定向到资源当前实际的 URL 上去。

由于从 URL 转换成 URN 是一项巨大的工程，标准化工作的进程很缓慢，URN 现在都没有投入使用。

```



![image-20210110112937829](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20210110112945.png)







### URL 语法

```
URL  语法会随着方案（如 HTTP、FTP、SMTP）的不同而有所不同，但大部分 URL 都遵循通用的 URL 语法。

　　<scheme>://<user>:<password>@<host>:<port>/<path>;<params>?<query>#<frag>

　　方案（sheme）：告诉解析 URL 的应用程序，使用什么协议；方案名是大小写无关的。

　　主机（host）：标识因特网上能够访问资源的宿主机器。可以用主机名 域名 IP地址

　　端口（port）：标识服务器正在监听的网络端口。常用默认端口，请参考：http://www.wusiwei.com/?post=109

　　用户名（user）和密码（password）：很多服务器会要求输入用户名和密码才允许用户访问数据，如 FTP，若用户没有提供，则会插入一个默认的用户名和密码。如 ftp://anonymous:my_passwd@ftp.prep.ai.mlt.edu/pub/gnu。

　　路径（path）：说明资源位于服务器的特定地方。

　　参数（params）：为了正确地与服务器进行交互，向负责解析 URL 的应用程序提供所需的协议参数。名值对列表。HTTP URL 的路径组件可以分成若干路径段，每段都可以有自己的参数，例如：http://www.joes-haniware.com/hammers;sale=false/index.html;graphocs=true

　　字符串（query）：通过提问题或进行查询缩小所请求资源类型范围。查询字符串通常为一系列的“名/值”对的形式出现，名值对之间用字符“&”分隔。

　　片段（frag）：引用部分资源或资源的一个片段。HTTP 服务器通常只处理整个对象，也就是说改变片段值，不会向 HTTP 服务器发送请求，因此 URL 片段仅有客户端使用。html中的页内锚点
```







