# cpu详解

## 参考

https://sspai.com/post/54788

http://www.lotpc.com/yjzs/7051_2.html

https://www.pc841.com/article/20171222-86592_all.html



https://gamingsnap.com/what-are-intel-processor-suffixes/



官方解读:https://www.intel.cn/content/www/cn/zh/processors/processor-numbers.html



芯片天梯：http://www.ppnames.com/html/363.html



https://wenku.baidu.com/view/2a0c5c0c804d2b160b4ec07d.html

https://www.urtech.ca/2017/06/solved-suffix-letters-intel-processors-mean/



视频

https://www.youtube.com/watch?v=DCEJMK32o8I

https://www.youtube.com/watch?v=4a9DBU4sROA

## 因特尔（Intel）的cpu系列

https://www.intel.cn/content/www/cn/zh/processors/processor-numbers.html

```
Core（TM）中文名：酷睿，这个是系列的名称，除了这个还有奔腾（PenTIum）系列、
赛扬（Celeron）系列、至强（Xeon）系列、安腾（Itanium）系列、凌动（Atom）系列、Quark系列
```





### 英特尔CPU命名规则详解(品牌，品牌标识符，Gen标识，SKU数值，产品线后缀)

一款Intel CPU的命名,一般由5个部分组成：**品牌，品牌标识符，Gen标识，SKU数值，产品线后缀**，如下图：

![img](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210501115514.jpeg)



![image-20210430145810959](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210430145811.png)



#### ①英特尔处理器的品牌

首先我们来看下英特尔处理器的分类，英特尔作为老牌的芯片商，英特尔旗下处理器有许多子品牌

**酷睿、奔腾、赛扬**：主要应用于台式机和笔记本电脑

> 1.酷睿系列(intel core系列)（中高端 ）
>
> ######    酷睿系列下，又有i3、i5、i7等，代表着不同的产品线，每个产品线又分为不同版本，比如i5-1135G7就是酷睿i5的第十一代CPU
>
> 2.奔腾系列 (intel Pentium)（中端入门）
>
> 3.赛扬系列 (intel Celeron)（低端入门）



**凌动**：主要应用于平板、智能手机

**Quark Soc**：主要应用于可穿戴设备

**至强、安腾**：主要应用于企业级，服务器和工作站

![img](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210501120308.jpeg)

#### ②英特尔处理器的品牌标识

品牌标识其实就是用于区分产品的定位，以酷睿为例，有i3, i5, i7, i9之分，举个通俗的例子，这就好比宝马车的的X1，X3，X5，X7分别代表了品牌定位低、中、高端，**<strong style="color:red;">通常来说在性能方面i7＞i5＞i3, 但是不意味着一定是i7＞i5＞i3</strong>**

因为当到某一款具体的CPU，可能会有性能方面低段位cpu>高段位cpu的情况，举个极端一点的例子，在性能上，i5 7600K > i7 2600K，所以i7＞i5＞i3并不是绝对正确的





#### ③英特尔处理器的Gen(Generation  世代)标识 

Gen是Generation的缩写，也就是平常我们所说的“**第几代**”。截止目前，Intel最新的CPU是**第十代**。跟旧的一代相比，新的一代意味着更好的制作工艺、设计，所以也意味着更强的性能。但是当到某一款具体的CPU，不意味着前一代的CPU一定弱于后一代，比如 i5 6600K > i5 7500， 我们一起来看下第十代CPU什么样的



#### ④英特尔处理器的SKU (Stock Keeping Unit 库存量单元 )

SKU（Stock Keeping Unit）的意思是库存量单元，其实就是用来进行库存管理的标识数，区分不同的产品，有点类似于图书的ISBN号，或者超市里商品的扫描条码，有了这个才能方便的计数，统计不同的产品的库存量是多少，方便管理大量库存呀。Intel官方并没有对SKU数值进行更多的解释，**但是根据实际情况，在Gen标识和产品后缀相同的情况下，SKU数值越大，代表性能越强**

```text
SKU是Stock Keeping Unit的缩写，指定是库存量单元，一般来说在Gen标识相同的情况下，SKU数值越大
代表性能越强


intel(R)  intel是商标  R (Registered trademark  注册商标)
表示intel这个商标是注册商标

TM(TradeMark  商标） 
既包含注册商标R (Registered trademark)，也包含直接未经中国商标局核准注册的未注册商标。
商标标注TM，能够起到一定的保护作用，但若该商标未经商标局核准注册，其受法律保护的力度不大，只有当该未注册商标达到一定知名度的情况下，才能够一定程度上获得法律保护商标法第58条


```

#### ⑤英特尔处理器的产品线后缀名  

这一位用字母表示，不同的字母代表了不同的含义，以酷睿品牌为例：补充一个背景知识：超频与多核心意味着更高的性能，但会有更高的功耗(耗电)，更高的热量；低功耗意味着较低的功耗，但会性能方面会差一些



<strong style="color:red;">按照业内的划分，台式机用CPU叫做桌面平台，笔记本的CPU叫移动平台；</strong>

笔记本电脑专用的CPU英文称Mobile CPU（移动CPU），它除了追求性能，也追求低热量和低耗电，最早的笔记本电脑直接使用[台式机](https://baike.baidu.com/item/台式机/529292)的CPU，但是随[CPU主频](https://baike.baidu.com/item/CPU主频/3519849)的提高， 笔记本电脑狭窄的空间不能迅速散发CPU产生的热量，还有笔记本电脑的电池也无法负担台式CPU庞大的耗电量， 所以开始出现专门为笔记本设计的Mobile CPU，它的制造工艺往往比同时代的台式机[CPU](https://baike.baidu.com/item/CPU)更加先进。



##### 台式机方面的后缀

> **K代表可以超频；T代表功耗优化**





##### 笔记本电脑方面的后缀



###### 笔记本中的intel  core  CPU后缀详解

> XM:    X(Extreme  Mobile)  XM表示笔记本使用高电压CPU,并且CPU的频率是允许超频的，如Intel Core i7-3920XM。
>
> M： M(Mobile)   代表笔记本用标准电压,PGA封装，可以更换CPU。例如Intel Core i5-4200M
> 
>
> H：H(High performance graphics 高性能图形  )    H表示代表笔记本用标准电压,BGA封装，不可以更换CPU，该cpu带有一块高性能的集成显卡，例如Intel Core i5-8300H、i7 4720HQ。
>
> 
>
> U：U(Ultra Low Voltage  低电压版)  U表示笔记本用低电压CPU，BGA封装，不可以更换CPU,例如i5 3317U。  (第3代酷睿i及之后出现的U后缀标志)
>
> Y：Y(Mobile extremely low power  超低电压     <strong style="color:red;">因为后缀E已经有其他含义了，所以使用Y后缀</strong> )   Y表示笔记本用超低电压CPU，TDP(Thermal design power 热设计功率  )低于15W，BGA封装，例如Intel Core i5-10210Y。
> 
>
> 性能上 XM>M=H>U>Y
>
> 
>
> 
> 
>
> G： 第十代和第十一代智能英特尔® 酷睿™ 处理器家族开始出现G标志，G表示CPU采用Radeon RX Vega显卡，包含 “G” 的处理器编号针对基于显卡的应用进行了优化，并提供更新的显卡技术。G后面还有数字后缀，数字后缀表示处理器提供的显卡级别；数字较大（例如 G7）表示显卡性能相对于较小的数字（例如 G1）有所提升。  例如:Intel Core i7-1160G7
>
> 
>
> K:  K(Unlocked Frequency Multiplier  解锁超频器)   K表示笔记本使用的CPU可以超频
>
> 
>
> Q：Q(Quad-core 四核心)   Q表示笔记本CPU有四个核心<strong style="color:red;">（一般只标注在默认2核心的CPU上；道理很简单，如果一款CPU的默认版本就是4核，也就不需要标注了）</strong>
>
>
> E：E(Embedded  嵌入式)    E表示该cpu应用于嵌入式设备  例如 Intel Core i5-9500TE
>
> T:  T( Power-optimized lifestyle)  T表示CPU经过电源优化。这些处理器的TDP (Thermal design power  散热设计功率)较低，它们散发的热量更少，消费者的功耗也更少  例如 Intel Core i5-9500TE
>
> HF:  HF表示CPU可传输大规格数据，但也缺少图像处理设备，即没有集成显卡，例如 Intel Core i7-9750HF
>
> S：S(Special Edition    特殊版本)   S表示CPU是特殊版本     比如 Intel Core I9-9900KS 



```
M后缀:now it bears noting that Intel  has an M suffix to make it clear that a  chip is mobile but currently it's only  being used on Xeon chips for mobile  workstations


现在值得注意的是，英特尔有一个M后缀来明确表明芯片是移动的，但目前M后缀只用来形容intel  Xeon系统的芯片

```





> - CPUs with the letter K: this indicates that the chips multiplier is unlocked, meaning that it can be easily overclocked if you have a similarly enabled motherboard. Non-K chips have very limited overclocking functionality, so make sure you look for that K if you want to tweak your system.
> - CPUs with the letters HK: Intel doesn't talk about it as much, but the K in HK CPUs that you occasionally see in high-end laptops also means the same thing as the letter K referenced above.
> - CPUs with the letter H: H stands for High-Performance Graphics and is used to designate Intel's higher-end offerings in the mobile segment that consumes more power.
> - CPUs with the letters HQ: This designation is the same as that of the letter H in H CPUs. It is another mobile-specific letter denotation. Many of those higher power chips also have a Q on the end that stands for Quad-Core, which is why you'll often see HQ on more expensive laptops.
> - CPUs with the letters U and Y: U stands for Ultra-Low Power, and Y represents Extremely Low Power.
> - CPUs with the letter T: These processors still fit in a standard LGA Desktop socket, but they are low-power, so you'll often see them in small form factor or all-in-one computers that are designed with smaller power supplies or less aggressive cooling.
> - CPUs with the letter P: These have some interesting graphics options, so if you see a chip with a P on the end, this indicates a desktop processor without integrated graphics, which can save you a few bucks if you're planning to use a discrete video card.
> - CPUs with the letter G: The newer G CPUs feature Radeon RX Vega graphics built from Intel's biggest non-competitor: AMD (Advanced Micro Devices), specifically its division Radeon Technologies Group, which is a different company.
> - CPUs with the letters R and C: Now, of course, we'd be remiss if we didn't give a quick shout out to R and C, which we last saw on the now several generations old Broadway line to designate a soldered-on CPU and then an unlocked desktop CPU, respectively. R stands for High-end Mobile, similar to "H". C stands for unlocked, and is the same as "K" in other generations.
> - CPUs with the letter X: This indicates a very high-end unlocked consumer CPU with the most cores and the highest prices. It's sitting atop the pile in the Core i9 7980XE for Extreme Edition (not to be confused with E for ECC memory, as I mentioned earlier).





> - C – Desktop processor based on the LGA 1150 package with high performance graphics
> - H – High performance graphics
> - K – Unlocked
> - M – Mobile
> - Q – Quad-core
> - R – Desktop processor based on BGA1364 (mobile) package with high performance graphics
> - S – Performance-optimized lifestyle
> - T – Power-optimized lifestyle
> - U – Ultra-low power
> - X – Extreme edition
> - Y – Extremely low power







NUC 本身是一台小巧的「准台式机」，内部集成有 CPU、主板和必要的 I/O 接口，加上自己购买的内存和硬盘，就成为一台完整可用的 PC 了。下面就是NUC的外观

![img](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210501124420.jpeg)





##### CPU封装的类型主要为三种：LGA，PGA，BGA

###### 1.LGA(  land grid array 平面网格阵列封装)   (主板上的cpu方便更换)





###### 2.PGA(pin grid array 插针网格阵列封装)     (主板上的cpu方便更换)





###### 3.BGA(ball grid array  球柵网格阵列封装)    (主板上的cpu不经过专业仪器不能更换)

> 目前绝大部分的 Intels笔记本CPU和智能手机CPU都使用了这种封装方式。
>
> 比如:intel所有产品后缀以H,HQ,U,Y等结尾，包括但不限低压的处理器；AMD低压移动处理器；所有的手机处理器等。
>
> 
>
> BGA可以是LGA,PGA的极端产物，和他们可以随意置换的特性不同，BGA一旦封装了，除非通过专业仪器，否则普通玩家根本不可能以正常的方式拆卸更换，但是因为是一次性做好的，因此BGA可以做的更矮，体积更小。





###### 三种封装方式对比：

> 严格来说，大家各有胜负，并没有谁最好。
>
> LGA:相比较于PGA而言，体积更小，相比于BGA而言具有更换性。但是对于更换过程中的操作失误要求更严格。
>
> PGA:在三种封装中体积最大，但是更换方便，而且更换的操作失误要求低
>
> BGA:三种封装中体积最小，但是更换接近于0,同时由于封装工艺问题，BGA的触点如果在封装过程中没有对准或者结合，极有可能意味着报废，所以相比于LGA,PGA成品率更低。
>
> 
>
> 亳无疑问，LGA和PGA是推动着我们DY爱好者发展的主要道路。但是随着处理器的发展，特别是移动领域。但是随着移动处理器的发展， Intel自四代过后逐漸新放弃推出采用PGA封装的移动处理器，改用BGA封装，无疑这样的举动将会大挫未来的笔记本DY玩家。而BGA封装的处理器，极有可能随着主板一起报废，对于热爱捡垃圾的垃圾佬来说，这简直是一场浪费行为。
>
> 
>
> 不过话说回来封装方式，这三种封装方式仅仅是CPU和主板交互的一种方式而已。也正是因为他只是一种方式，这也就意味着，BGA的CPU通过一个简单的 PGA PCB板，让本来只是BGA的CPU变成PGA。
>
> 