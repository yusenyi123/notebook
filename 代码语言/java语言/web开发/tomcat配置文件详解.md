# tomcat配置文件详解

## 参考：

https://www.cnblogs.com/kismetv/p/7228274.html#title2-2

https://blog.csdn.net/xuheng8600/article/details/81661039





## 1.server.xml文件

### 基本配置结构

```XML
<Server>

    <Listener />
    
    <GlobaNamingResources>
    
    </GlobaNamingResources>
    
    
    <Service>
        <Connector />
        
        <Engine>
        
            <Logger />
            
            <Realm />
            
               <host>
                   <Logger />
                   
                   <Context />
                   
               </host>
        </Engine>
        
    </Service>
    
</Server>
```

### 核心配置结构(必须配置的节点，配置完就可正常运行，其他节点是辅助节点让tomcat服务器提供额外的功能)

```
<Server>
    <Service>
        <Connector />
        <Connector />
        <Engine>
            <Host>
                <Context /><!-- 现在常常使用自动部署，不推荐配置Context元素，Context小节有详细说明 -->
            </Host>
        </Engine>
    </Service>
</Server>
```



### 核心节点详细介绍

#### 1.`<Sever> </Server>`节点

| 节点常用属性 | 属性作用                                                     |
| ------------ | ------------------------------------------------------------ |
| port         | 指定一个端口，这个端口负责监听关闭tomcat的请求，设为-1可以禁掉该端口。 |
| shutdown     | shutdown指定终止Tomcat服务器运行时,发给Tomcat服务器的shutdown监听端口的字符串.该属性必须设置 |

##### Server节点作用:

```
Server节点作用:它代表整个容器,是Tomcat实例的顶层元素.由org.apache.catalina.Server接口来定义.，它可以包含一个或多个“Service”实例。服务器在指定的端口上监听shutdown命令

例子：
<Server port="8005" shutdown="SHUTDOWN">

</Server>



```



#### 2.`<Service></Service>`节点(服务节点，把Connector和Engine组合在一起对外提供服务)

| 节点常用属性 | 属性作用                                                     |
| ------------ | ------------------------------------------------------------ |
| className    | 指定实现org.apahce.catalina.Service接口的类.默认为org.apahce.catalina.core.StandardService |
| name         | 定义Service的名字                                            |

##### Service节点作用:

```
Service节点作用:是在Connector节点和Engine节点外面包了一层，把它们组装在一起，对外提供服务。一个Service可以包含多个Connector节点，但是只能包含一个Engine节点；其中Connector节点的作用是从客户端接收请求，Engine节点的作用是处理接收进来的请求。
```



#### 3.`<Connector><Connector/>`节点(连接器节点,设置tomcat实例监听的端口和协议)

| 节点常用属性      | 属性作用                                                     |
| ----------------- | ------------------------------------------------------------ |
| port              | 配置连接器监听的端口(0-65535)。如果设置成0，将随机生成(通常只用于嵌入式和测试应用程序)。 |
| protocol          | 配置连接器处理类, 即使用的网络协议。表示该连接器处理哪种协议的请求,"HTTP/1.1"是默认值,等效于"org.apache.coyote.http11.Http11Protocol";还有熟悉的"AJP/1.3";关于HTTP和AJP两种方式的区别和性能优劣可以参见其他文档. |
| connectionTimeout | 当client与tomcat建立连接之后,在"connectionTimeout"时间之内,仍然没有得到client的请求数据,此时连接将会被断开.此值的设定需要考虑到网络稳定型,同时也有性能的考虑.                                                                  它和tcp的配置选项中的"socket_timeout"仍有区别,connectionTimeout只会在链接建立之后,得到client发送http-request信息前有效.                                                                                                                                                      默认值为60000，即60秒；对于互联网应用，此值我们应该设置合理，比如20000 |
| redirectPort      | 配置指定端口来 ssl连接，当用户用http请求某个资源，而该资源本身又被设置了必须要https方式访问，此时Tomcat会自动重定向到这个redirectPort设置的https端口。一般默认配置是8443，但是浏览器默认的是443端口请求ssl服务器，所以在https 下将8443改为443. |
| address           | 配置绑定的服务器ip，当物理server上绑定了多个IP地址时，可以通过“address”来指定tomcat-server需要bind的地址.默认将port关联到所有的ip上。 |
|                   |                                                              |
|                   |                                                              |
|                   |                                                              |
| 更多属性查看      | 查看链接：https://blog.csdn.net/cuiyaoqiang/article/details/52184888 |

##### redirectPort发生作用情景

```
redirectPort属性：配置指定端口来 ssl连接，当用户用http请求某个资源，而该资源本身又被设置了必须要https方式访问，此时Tomcat会自动重定向到这个redirectPort设置的https端口


怎么理解：当用户用http请求某个资源，而该资源本身又被设置了必须要https方式访问，此时Tomcat会自动重定向到这个redirectPort设置的https端口

在web.xml中添加下列配置，则这个web应用的资源只能使用https进行访问，那么当我们使用http协议访问这个资源的时候，就会根据<Connector/>中设置的redirectPort进行重定向


<login-config>
<!-- Authorization setting for SSL -->
<auth-method>CLIENT-CERT</auth-method>
<realm-name>Client Cert Users-only Area</realm-name>
</login-config>
<security-constraint>
<!-- Authorization setting for SSL -->
<web-resource-collection >
<web-resource-name >SSL</web-resource-name>
<url-pattern>/*</url-pattern>
</web-resource-collection>
<user-data-constraint>
<transport-guarantee>CONFIDENTIAL</transport-guarantee>
</user-data-constraint>
</security-constraint>
```

![6](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20210103232548.gif)





##### Connector节点作用:

```
Connector节点作用:
1.接收连接请求，创建Request和Response对象用于和请求端交换数据；然后分配线程让Engine节点来处理这个请求，并把产生的Request和Response对象传给Engine。
2.通过配置Connector节点，可以控制请求Service节点的协议及端口号。
```



```
1 <Connector port="8080" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443" />
2 <Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />

（1）通过配置第1个Connector，客户端可以通过8080端口号使用http协议访问Tomcat。其中，protocol属性规定了请求的协议，port规定了请求的端口号，redirectPort表示当被请求资源强制要求https而请求是http时，重定向至端口号为8443的Connector，connectionTimeout表示连接的超时时间。

在这个例子中，Tomcat监听HTTP请求，使用的是8080端口，而不是正式的80端口；实际上，在正式的生产环境中，Tomcat也常常监听8080端口，而不是80端口。这是因为在生产环境中，很少将Tomcat直接对外开放接收请求，而是在Tomcat和客户端之间加一层代理服务器(如nginx)，用于请求的转发、负载均衡、处理静态文件等；通过代理服务器访问Tomcat时，是在局域网中，因此一般仍使用8080端口。

（2）通过配置第2个Connector，客户端可以通过8009端口号使用AJP协议访问Tomcat。AJP协议负责和其他的HTTP服务器(如Apache)建立连接；在把Tomcat与其他HTTP服务器集成时，就需要用到这个连接器。之所以使用Tomcat和其他服务器集成，是因为Tomcat可以用作Servlet/JSP容器，但是对静态资源的处理速度较慢，不如Apache和IIS等HTTP服务器；因此常常将Tomcat与Apache等集成，前者作Servlet容器，后者处理静态资源，而AJP协议便负责Tomcat和Apache的连接。
```

Tomcat与Apache等集成的原理如下图：

![img](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20210102134258.png)









#### 4.`<Engine></Engine>`节点(引擎节点,处理请求)

| 节点常用属性 | 属性作用                                                     |
| ------------ | ------------------------------------------------------------ |
| className    | 指定实现Engine接口的类,默认值为StandardEngine                |
| defaultHost  | 指定处理客户的默认主机名,当发往本机的请求指定的host名称不存在时，一律使用defaultHost指定的host进行处理；因此，defaultHost的值，必须与Engine中的一个Host节点的name属性值匹配。 |
| name         | 定义Engine的名字，name属性用于日志和错误信息                 |
|              |                                                              |
|              |                                                              |
|              |                                                              |
|              |                                                              |
|              |                                                              |



##### Engine节点作用:

```
每个Service元素只能有一个Engine元素.处理在同一个<Service>中所有<Connector>元素接收到的客户请求.
由org.apahce.catalina.Engine接口定义. 

在<Engine>可以包含如下元素<Logger>, <Realm>, <Value>, <Host>
```





#### 5.`<Host></Host>`节点(虚拟主机节点，Engine收到请求后根据请求中的主机名(域名或者ip地址) 找到对应的Host节点)

| 节点常用属性    | 属性作用                                                     |
| --------------- | ------------------------------------------------------------ |
| name            | 指定虚拟主机的主机名，一个Engine节点中有且仅有一个Host节点的name属性与Engine节点的defaultHost属性相匹配（必须在Engine节点下配置一个这样的Host节点）；一般情况下，主机名需要是在DNS服务器中注册的网络名，但是Engine指定的defaultHost不需要。原因:客户端通常使用主机名来标识它们希望连接的服务器；该主机名也会包含在HTTP请求头中。Tomcat从HTTP头中提取出主机名，寻找名称匹配的主机。如果没有匹配，请求将发送至默认主机。因此默认主机不需要是在DNS服务器中注册的网络名，因为任何与所有Host名称不匹配的请求，都会路由至默认主机 |
| defaultHost     | 指定处理客户的默认主机名,当发往本机的请求指定的host名称不存在时，一律使用defaultHost指定的host进行处理；因此，defaultHost的值，必须与Engine中的一个Host节点的name属性值匹配。 |
| unpackWARs      | 指定了是否将代表Web应用的WAR文件解压；如果为true，通过解压后的文件结构运行该Web应用，如果为false，直接使用WAR文件运行Web应用。 |
| appBase         | 指定Web应用所在的目录，默认值是webapps，这是一个相对路径，代表Tomcat根目录下webapps文件夹。 |
| xmlBase         | 指定该Host节点下Web应用的XML配置文件(这里的web应用的xml配置文件不是web.xml文件，而是进行配置web项目虚拟路径映射的配置文件content.xml文件)所在的目录，默认路径为conf/<Engine_name>/<Host_name>，如果没有该文件夹，会在tomcat运行的时候根据配置的xmlBase路径进行创建，该路径下存放该Host的总体配置文件content.xml和该Host下每个web 应用自己的 (该web应用名称).xml文件 |
| deployOnStartup | deployOnStartup为true时，Tomcat在启动时检查Web应用所在的目录，将目录下检测到的所有Web应用视作新应用 |
| autoDeploy      | autoDeploy为true时，Tomcat在运行时定期检查Web应用所在的目录，查看是否有新的Web应用或Web应用的更新。 |
|                 |                                                              |



##### Host节点作用:

```

Engine与Host
Host是Engine的子容器。Engine组件中可以内嵌1个或多个Host组件，每个Host组件代表Engine中的一个虚拟主机。Host组件至少有一个，且其中一个的name必须与Engine组件的defaultHost属性相匹配。

Host虚拟主机的作用，是运行多个Web应用（一个Context代表一个Web应用），并负责安装、展开、启动和结束每个Web应用。

Host组件代表的虚拟主机，对应了服务器中一个网络名实体(如”www.test.com”，或IP地址”116.25.25.25”)；为了使用户可以通过网络名连接Tomcat服务器，这个名字应该在DNS服务器上注册。

客户端通常使用主机名来标识它们希望连接的服务器；该主机名也会包含在HTTP请求头中。Tomcat从HTTP头中提取出主机名，寻找名称匹配的主机。如果没有匹配，请求将发送至默认主机。因此默认主机不需要是在DNS服务器中注册的网络名，因为任何与所有Host名称不匹配的请求，都会路由至默认主机。
```

##### 检查Web应用更新

一个Web应用可能包括以下文件：XML配置文件，WAR包，以及一个应用目录(该目录包含Web应用的文件结构)；其中XML配置文件位于xmlBase指定的目录，WAR包和应用目录位于appBase指定的目录。

Tomcat按照如下的顺序进行扫描，来检查应用更新：

A、扫描虚拟主机指定的xmlBase下的XML配置文件

B、扫描虚拟主机指定的appBase下的WAR文件

C、扫描虚拟主机指定的appBase下的应用目录





#### 5.`<Context/>`节点(web应用配置节点，手动配置web应用地址)

| 节点常用属性 | 属性作用                                                     |
| ------------ | ------------------------------------------------------------ |
| docBase      | 指定了该Web应用使用的WAR包路径，或应用目录。需要注意的是，在自动部署场景下，docBase不在appBase目录中，才需要指定；如果docBase指定的WAR包或应用目录就在appBase中，则不需要指定，因为Tomcat会自动扫描appBase中的WAR包和应用目录，指定了反而会造成问题。 |
| path         | 指定了访问该Web应用的上下文路径，当请求到来时，Tomcat根据Web应用的 path属性与URI的匹配程度来选择Web应用处理相应请求。例如，Web应用app1的path属性是”/app1”，Web应用app2的path属性是”/app2”，那么请求/app1/index.html会交由app1来处理；而请求/app2/index.html会交由app2来处理。                                                   如果一个Context元素的path属性为””，那么这个Context是虚拟主机的默认Web应用；当请求的uri与所有的path都不匹配时，使用该默认Web应用来处理。                                                                                                                                          但是，需要注意的是，在自动部署场景下，不能指定path属性，path属性由配置文件的文件名、WAR文件的文件名或应用目录的名称自动推导出来。如扫描Web应用时，发现了xmlBase目录下的app1.xml或appBase目录下app1.WAR或app1应用目录，则该Web应用的path属性是”app1”。如果名称不是app1而是ROOT，则该Web应用是虚拟主机默认的Web应用，此时path属性推导为“”。 |
| reloadable   | 指示tomcat是否在运行时监控在WEB-INF/classes和WEB-INF/lib目录下class文件的改动。如果值为true，那么当class文件改动时，会触发Web应用的重新加载。在开发环境下，reloadable设置为true便于调试；但是在生产环境中设置为true会给服务器带来性能压力，因此reloadable参数的默认值为false。 |





##### Context的作用

```
Context元素代表在特定虚拟主机上运行的一个Web应用。Context、应用或Web应用，它们指代的都是Web应用。每个Web应用基于WAR文件，或WAR文件解压后对应的目录（这里称为应用目录）。

Context是Host的子容器，每个Host中可以定义任意多的Context元素。

server.xml配置文件中可以没有Context元素的配置。这是因为，Tomcat开启了自动部署，Web应用没有在server.xml中配置静态部署，而是由Tomcat通过特定的规则自动部署。
```









### Tomcat实例接受到请求处理顺序，如何确定请求由谁处理？

当请求被发送到Tomcat所在的主机时，如何确定最终哪个Web应用来处理该请求呢？

### （1）根据协议和端口号选定Service和Engine

Service中的Connector组件可以接收特定端口的请求，因此，当Tomcat启动时，Service组件就会监听特定的端口。在第一部分的例子中，Catalina这个Service监听了8080端口（基于HTTP协议）和8009端口（基于AJP协议）。当请求进来时，Tomcat便可以根据协议和端口号选定处理请求的Service；Service一旦选定，Engine也就确定。

通过在Server中配置多个Service，可以实现通过不同的端口号来访问同一台机器上部署的不同应用。

### （2）根据域名或IP地址选定Host

Service确定后，Tomcat在Service中寻找名称与域名/IP地址匹配的Host处理该请求。如果没有找到，则使用Engine中指定的defaultHost来处理该请求。在第一部分的例子中，由于只有一个Host（name属性为localhost），因此该Service/Engine的所有请求都交给该Host处理。

### （3）根据URI选定Context/Web应用

这一点在Context一节有详细的说明：Tomcat根据应用的 path属性与URI的匹配程度来选择Web应用处理相应请求，这里不再赘述。

### （4）举例

以请求http://localhost:8080/app1/index.html为例，首先通过协议和端口号（http和8080）选定Service；然后通过主机名（localhost）选定Host；然后通过uri（/app1/index.html）选定Web应用。





## 2.context.xml文件(配置web应用虚拟路径映射，除了在server.xml中配置外的另一种方式)

位于conf文件夹下的context.xml用于定义所有Web应用均需要加载的 Context 配置，如果Web应用指定了自己的context.xml，那么该文件的配置将被覆盖

不推荐在server.xml中进行配置，而是在/conf/context.xml中进行独立的配置。因为server.xml是不可动态重加载的资源，服务器一旦启动了以后，要修改这个文件，就得重启服务器才能重新加载。而context.xml文件则不然，tomcat服务器会定时去扫描这个文件。一旦发现文件被修改（时间戳改变了），就会自动重新加载这个文件，而不需要重启服务器。



### sever.xml配置中讲Host节点中的deployOnStartup设置为true，context.xml文件配置才会生效

```
 注意需要在sever.xml配置中讲Host节点中的deployOnStartup设置为true(默认deployOnStartup属性值就是true),context.xml中的配置才会生效
 
 <Host name="localhost"  appBase="webapps"
           unpackWARs="true" autoDeploy="true" deployOnStartup="true" >
           
 </Host>
```







context.xml的三个作用范围：

1. tomcat server级别：

在/conf/context.xml里配置。

2. Host级别：

在/conf/Catalina/${hostName}文件夹下里添加context.xml，继而进行配置。

3. web app 级别：

在/conf/Catalina/${hostName}文件夹下里添加​{webAppName}.xml，继而进行配置。

