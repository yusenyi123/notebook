# win10系统激活那点事情

## 1. Retail key 零售秘钥即正规购买的微软激活码激活  行为正版  功能正版

这是最官方的激活方式。这里有个知识点，那就是输入这个类型的密钥会转换为数字权利激活。

数字权利激活是什么呢？

就是直接把你的CPU和主板信息直接记录到微软的服务器里面。只要刷入对应的版本，那么就可以自动联网激活。这也是我们目前最推荐的一种激活方式。

 



## 2. MSDN订阅号所得的密钥，订阅号自己使用正版，商用售卖-行为盗版，功能正版

### 自己购买两种office零售秘钥

#### 1.卖家说可以绑定微软账号，他说的绑定账号是下面这个样子的绑定，也不知道是什么情况，经过测试可以反复激活，是msdn订阅号的密钥激活的

![image-20200904234309012](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200904234309.png)

![image-20200904234045231](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200904234045.png)

------

![image-20200904234215260](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200904234215.png)





#### 2.购买的office2019家庭版本，商家也是说可以绑定账号，绑定完之后可以在服务和订阅处看到，这个应该比较可靠

![image-20200904234456203](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200904234456.png)

## 3. OEM（Original Equipment Manufacturer，原始设备制造商）序列号激活  行为正版  功能正版 

OEM序列号分为三大类：OEMSlp、OEMCoa和OemNonslpOEMSlp、 

1、OEMSlp (System Locked Pre-installation)系统锁定的预装

　　这种key是OEM厂商预装系统使用的，slp key可以用来安装任何OEMSlip系统，和OEM的厂商无关。OEMSlp key不需要联网到微软激活，只要Slp key和你安装的windows 7版本相符(旗舰版或是专业版或是家庭版)，并且品牌电脑bios中含有slic 2.1表，操作系统中有和这个slic 2.1表相匹配的证书，即可自动激活。并且不用担心OEMSlp key被封，因为大批品牌机用户都是使用的这个key，封了误杀后果很严重。

2、OEMCoa(Certificate of Authentication) 验证证书

　在预装了windows操作系统的品牌机笔记本电脑上，机身后都会有一个COA标签，上面有product key，这个key是OEM厂商送给你使用OEM系统光盘重装系统时使用的，OEMCoa key和普通的RTl key (零售版)key激活机制是一样的，需要联网到微软激活，当然也可以电话联系微软激活。

COA又称正版证明标签或防伪证明书，对于品牌机用户来说主要作用还是证明你的系统带有正版windows。



3、OemNonslp(Non System Locked Pre-installation)非系统锁定的预装

激活机制和普通的RTl key (零售版)激活机制是一样的，需要联网到微软激活，当然也可以电话联系微软激活，只是由OEM发布和提供支持。

## 4.   MAK（批量授权）行为正版  功能正版（大部分淘宝/拼多多卖家都出售这种key）

MAK是一些电脑大厂和企业购买的批量授权所获得的Key。比如购买500个点的激活。那么就会输入key后微软的激活服务器扣除一个点。激活后只要不重装就是永久激活，如果再给另外一台PC用一次。或者换了主板硬盘等再重装。那么再扣除一个点。直到5000点用完为止。那么这个key就算彻底废掉了。网络上流传的各种神key。就是这种。不过一般都会被快速的消耗干净。想要找到靠谱的还是得tb。

优点：快速激活，并且是永久激活（只要这个mak key的主人不停用）。

 ==mak key是批量激活的，但这个key其实不是你的，批量激活的key都有一个主人，他可以随时决定要不要停用掉你的激活，但 mak key停用激活不能针对一台机器，一旦停用是使用当前mak key激活的计算机都会停用==

缺点：可以对office进行激不过只能激活vol版本的office，不能重装、重装了就要重新购买。属于快速消耗品。市面上出现的不可能是正版。理论上就是给企业用的。





## 5.KMS（**Key Management Service**）激活工具激活  行为盗版，功能正版

特点：有时间限制，往往是180天，到期之后需要再次执行KMS激活工具

 

KMS的工作原理
 大客户首先购买微软正版“CSVLK”密钥（CSVLK是 Customer service volume licensing key 的缩写，即客户服务批量许可证密钥），然后将一台计算机永久激活（联网或者拨打微软客服电话），经过设置将此台计算机设置为激活服务器。这台计算机作为服务器（KMS Host），始终保持开机联网（局域网连接通畅即可）的状态。局域网内的其他计算机（KMS Client）向服务器请求激活（请求激活的信息中需要有GLVK密钥GVLK，英文全称Generic Volume License Key，表示批量授权许可密钥，用于kms客户端的通用激活序列号，凡是使用kms激活的windows系统还是Office，使用的都是GVLK密钥，GVLK密钥是微软官方免费提供给用户的，如果你要使用GVLK密钥kms激活，你需要安装VL版的系统，kms激活一般是45天或180天，到期后继续使用kms激活续期），当达到一定数量（激活Windows需要25台计算机、激活Office需要5台计算机）的客户端计算机请求激活后，服务器就会向客户端注入激活信息。微软规定：KMS Client 每隔180天需要向KMS Host请求激活一次。 KMS激活适用的范围是VOL（Volume Licensing for Organizations 面向组织的批量许可）版的Windows或者Office，例如Windows8企业版、Office 2013 Professional Plus VOL。此外，例如Windows 8专业版既有零售版又有VOL版，且二者可以通过安装Retail密钥或GVLK。 

密钥进行转化，因此可以用KMS激活；例如Windows 7旗舰版为零售版系统且无法通过安装GVLK密钥转化为VOL版，因此无法使用KMS激活。综上所述，KMS只可以激活VOL版本的Windows或Office。



KMS的利用

能够通过KMS进行激活的一般称为VL版,即VOLUME授权版，一般不会单独在零售市场进行发售，一般是直接向企业提供电子ISO映像进行批量授权安装，基于对KMS原理研究成果，我们可以自行搭建KMS激活服务器，实现每180天一次的自动激活，使得系统一直保持激活状态。

 



https://www.reneelab.com.cn/m/win10-activation-crack-free.html#c

 

## 5.数字权利激活工具  行为盗版，功能正版

特点：永久激活，电脑重装后联网也能自动激活

 

HWIDGEN（修改系统内核）

国外Nsane论坛会员s1ave77制作的一款win10的数字权利激活工具。在激活的时候要开启Windows Update（升级功能）。本来很复杂的工具在中国众多大佬的优化下变得简单。他是通过修改系统内核实现的。是目前最牛原理的激活工具，没有之一。具体的代码可以自己去Github上看。适用于佛系人群。

缺点：远景有谣言说他勒索病毒，当然第三方90%的激活工具都会报毒。可以先用这个激活工具激活，然后重装系统登录微软账号激活

 