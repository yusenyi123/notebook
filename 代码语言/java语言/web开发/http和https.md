# http和https

## 参考：

https://www.hi-linux.com/posts/7040.html

https://www.huaweicloud.com/articles/391b2918649b40ee3e55d73121304e8e.html

https://www.jianshu.com/p/29e0ba31fb8d



## 1.http(HyperText Transfer Protocol 超文本传输协议)



### 1.简介

HTTP协议（Hyper Text Transfer Protocol，超文本传输协议）,是用于从万维网（WWW:World Wide Web ）服务器传输超文本到本地浏览器的传送协议。

HTTP基于TCP/IP通信协议来传递数据。

HTTP基于客户端/服务端（C/S）架构模型，通过一个可靠的链接来交换信息，是一个无状态的请求/响应协议。

### 2.特点

（1）HTTP是无连接：无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间。

（2）HTTP是媒体独立的：只要客户端和服务器知道如何处理的数据内容，任何类型的数据都可以通过HTTP发送。客户端以及服务器指定使用适合的MIME-type内容类型。

（3）HTTP是无状态：无状态是指协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。

 

### 3.HTTP请求报文构成(HTTP请求报文由3部分组成（请求行+请求头+请求体）)
![img](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210316115756.jpeg)

#### 请求行：

①是请求方法，GET和POST是最常见的HTTP方法，除此以外还包括DELETE、HEAD、OPTIONS、PUT、TRACE。

②为请求对应的URL地址，它和报文头的Host属性组成完整的请求URL。

③是协议名称及版本号。

#### 请求头：

④是HTTP的报文头，报文头包含若干个属性，格式为“属性名:属性值”，服务端据此获取客户端的信息。

与缓存相关的规则信息，均包含在header中

#### 请求体：

⑤是报文体，它将一个页面表单中的组件值通过param1=value1&param2=value2的键值对形式编码成一个格式化串，它承载多个请求参数的数据。不但报文体可以传递请求参数，请求URL也可以通过类似于“/chapter15/user.html? param1=value1&param2=value2”的方式传递请求参数。 

### 4.HTTP响应报文组成

![image-20210319224402430](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210319224409.png)





## 2.https(HyperText Transfer Protocol Secure  超文本传输安全协议)     https也常称为HTTP over TLS、HTTP over SSL或HTTP Secure



### 我们为什么需要HTTPS？

1. 保护隐私(Privacy)：所有信息都是加密传播，第三方无法窃听数据。如果使用HTTP明文传输数据的话，很可能被第三方劫持数据，那么所输入的密码或者其他个人资料都被暴露在他人面前，后果可想而知。

2. 不使用SSL/TLS的HTTP通信，就是不加密的通信。所有信息明文传播，带来了三大风险。

   > （1） **窃听风险**（eavesdropping）：第三方可以获知通信内容。
   >
   > （2） **篡改风险**（tampering）：第三方可以修改通信内容。
   >
   > （3） **冒充风险**（pretending）：第三方可以冒充他人身份参与通信。

3. 数据完整性(Integraty)：一旦第三方篡改了数据，接收方会知道数据经过了篡改，这样便保证了数据在传输过程中不被篡改 —— 数据的完整性。

4. 身份认证(Identification)：第三方不可能冒充身份参与通信，因为服务器配备了由证书颁发机构（Certificate Authority，简称CA）颁发的安全证书，可以证实服务器的身份信息，防止第三方冒充身份。（也有少数情况下，通信需要客户端提供证书，例如银行系统，需要用户在登录的时候，插入银行提供给用户的USB，就是需要客户端提供证书，用来验证客户的身份信息。）

   



![image-20210103220250312](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20210103220508.png)













### SSL(Secure Sockets Layer 安全套接字层)和TLS(Transport Layer Security 传输层安全性)的关系



#### 发展历史

```
SSL 1.0、2.0和3.0 
Netscape开发了原始的SSL协议，Netscape Communications的首席科学家Taher Elgamal从1995年到1998年被称为“ SSL之父”。SSL版本1.0从未公开发布，因为该协议存在严重的安全漏洞。1995年2月发布的2.0版包含许多安全漏洞，因此必须设计3.0版。SSL版本3.0于1996年发布，代表了Paul Kocher产生的协议的完全重新设计。与Netscape工程师Phil Karlton和Alan Freier合作，以及Consensus Development的Christopher Allen和Tim Dierks的参考实现。较新的SSL / TLS版本基于SSL 3.0。IETF在RFC  6101中以历史文档的形式发布了SSL 3.0 1996年草案。

RFC 6176在2011年弃用了SSL 2.0 。2014年，发现SSL 3.0容易受到POODLE攻击的影响，该攻击会影响SSL中的所有分组密码。RC4是SSL 3.0支持的唯一非块密码，也可以像SSL 3.0中所使用的那样被破坏。 RFC 7568于2015年6月弃用了SSL 3.0 。


TLS 1.0 
TLS 1.0最初是在1999年1月的RFC 2246中定义为SSL 3.0版的升级版，并由Consensus Development的Christopher Allen和Tim Dierks编写。如RFC中所述，“此协议与SSL 3.0之间的差异并不明显，但它们之间的差异足以阻止TLS 1.0与SSL 3.0之间的互操作性”。蒂姆·迪尔克斯（Tim Dierks）后来写道，这些更改以及从“ SSL”重命名为“ TLS”，对Microsoft来说都是面子大方的手势，“因此，看起来IETF不会只是在戳戳Netscape的协议”。

在PCI委员会建议机构迁移从TLS 1.0到TLS 1.1或更高版本2018年6月30日前在2018年10月，苹果，谷歌，微软和Mozilla的共同宣布，他们将弃用TLS 1.0和1.1三月2020

```

#### 简略描述发展历史(关系结论:TLS 1.0通常被标示为SSL 3.1)

```
1994年，NetScape公司设计了SSL协议（Secure Sockets Layer）的1.0版，但是未发布。

1995年，NetScape公司发布SSL 2.0版，很快发现有严重漏洞。

1996年，SSL 3.0版问世，得到大规模应用。

1999年，互联网标准化组织ISOC接替NetScape公司，发布了SSL的升级版TLS 1.0版。

2006年和2008年，TLS进行了两次升级，分别为TLS 1.1版和TLS 1.2版。最新的变动是2011年TLS 1.2的修订版。


目前，应用最广泛的是TLS 1.0，接下来是SSL 3.0。但是，主流浏览器都已经实现了TLS 1.2的支持。

TLS 1.0通常被标示为SSL 3.1，TLS 1.1为SSL 3.2，TLS 1.2为SSL 3.3。
```



### CA(Catificate Authority  认证机构 )证书

CA，Catificate Authority，它的作用就是提供证书（即服务器证书，由域名、公司信息、序列号和签名信息组成）加强服务端和客户端之间信息交互的安全性，以及证书运维相关服务。任何个体/组织都可以扮演 CA 的角色，只不过难以得到客户端的信任，能够受浏览器默认信任的 CA 大厂商有很多，其中 TOP5 是 Symantec、Comodo、Godaddy、GolbalSign 和 Digicert。



### HTTPS认证过程

![image-20210103222136525](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20210103222136.png)

、





## tomcat使用https协议

### 生成秘钥

```
keytool -genkeypair -alias "tomcat" -keyalg "RSA" -keystore "E:\runenv\tomcat.keystore"
```

```
C:\Users\sen>keytool -genkeypair -alias "tomcat" -keyalg "RSA" -keystore "E:\runenv\tomcat.keystore"
输入密钥库口令:
密钥库口令太短 - 至少必须为 6 个字符   
输入密钥库口令:      //此处密码对应 Connector节点中的 keystorePass属性
再次输入新口令:
您的名字与姓氏是什么?
  [Unknown]:
您的组织单位名称是什么?
  [Unknown]:
您的组织名称是什么?
  [Unknown]:
您所在的城市或区域名称是什么?
  [Unknown]:
您所在的省/市/自治区名称是什么?
  [Unknown]:
该单位的双字母国家/地区代码是什么?
  [Unknown]:
CN=Unknown, OU=Unknown, O=Unknown, L=Unknown, ST=Unknown, C=Unknown是否正确?
  [否]:  y



输入 <tomcat> 的密钥口令            //此处密码对应 Connector节点中的 keyPass属性
        (如果和密钥库口令相同, 按回车):
密钥口令太短 - 至少必须为 6 个字符
输入 <tomcat> 的密钥口令
        (如果和密钥库口令相同, 按回车):
再次输入新口令:

C:\Users\sen>
```

### tomcat的server.xml的配置

```
  <Connector port="8443" protocol="org.apache.coyote.http11.Http11Protocol"
               maxThreads="150" SSLEnabled="true" scheme="https" secure="true"
               clientAuth="false" sslProtocol="TLS" 
               keystoreFile="E:\runenv\tomcat.keystore"  
               keystorePass="123456"  keyPass="1234567"/>	
               
               
               
keystorePass是keyStore文件的密码,

keyPass是"证书私钥"的密码,

对于这两个密码相同时，就可以不配置  keyPass属性 因为tomcat默认keyPass的值和keystorePass的值相同             
```

