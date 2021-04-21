# RationalRose安装使用

## 参考

https://www.youtube.com/watch?v=vBqcJa_EVZQ

https://www.youtube.com/watch?v=PEe-szBxPXA

https://blog.csdn.net/gao454917848/article/details/25368095



## 安装

https://www.youtube.com/watch?v=vBqcJa_EVZQ

https://www.youtube.com/watch?v=PEe-szBxPXA





## 使用



### RationalRose保存的文件类型是.mdl （models缩写）





### RationalRose浏览界面中的三个横向箭头Associations作用

![image-20210419200108453](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210419200108.png)



### 关联关系的一对多和聚合关系



![image-20210419200645433](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210419200645.png)

![image-20210419201440327](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210419201440.png)



### uml关系比较

#### 1.聚合与组合

（1）聚合与组合都是一种结合关系，只是额外具有整体-部分的意涵。

（2）部件的生命周期不同

聚合关系中，整件不会拥有部件的生命周期，所以整件删除时，部件不会被删除。再者，多个整件可以共享同一个部件。

组合关系中，整件拥有部件的生命周期，所以整件删除时，部件一定会跟着删除。而且，多个整件不可以同时间共享同一个部件。

（3）聚合关系是"has-a"关系，组合关系是"contains-a"关系。

#### 2.关联和聚合

（1）表现在代码层面，和关联关系是一致的，只能从语义级别来区分。

（2）关联和聚合的区别主要在语义上，关联的两个对象之间一般是平等的，例如你是我的朋友，聚合则一般不是平等的。

（3）关联是一种结构化的关系，指一种对象和另一种对象有联系。

（4）关联和聚合是视问题域而定的，例如在关心汽车的领域里，轮胎是一定要组合在汽车类中的，因为它离开了汽车就没有意义了。但是在卖轮胎的店铺业务里，就算轮胎离开了汽车，它也是有意义的，这就可以用聚合了。

#### 3.关联和依赖

（1）关联关系中，体现的是两个类、或者类与接口之间语义级别的一种强依赖关系，比如我和我的朋友；这种关系比依赖更强、不存在依赖关系的偶然性、关系也不是临时性的，一般是长期性的，而且双方的关系一般是平等的。

（2）依赖关系中，可以简单的理解，就是一个类A使用到了另一个类B，而这种使用关系是具有偶然性的、临时性的、非常弱的，但是B类的变化会影响到A。

#### 4.综合比较

这几种关系都是语义级别的，所以从代码层面并不能完全区分各种关系；但总的来说，后几种关系所表现的强弱程度依次为：

组合>聚合>关联>依赖





### RationalRose各种线条表示的关系





![image-20210419201215231](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210419201215.png)







## 用例图(Use Case View)制作

### 用例图包含角色(Actor)和用例(Use Case)



#### 角色之间可以有泛化关系，即继承关系，如超级管理员继承普通管理员的权限，在普通管理员权限的基础上增加新的功能





#### 用例之间的关系



##### 1.角色和用例之间的关联关系





##### 2.角色和角色之间的泛化关系，用例和用例之间的泛化关系



##### 3.用例和用例之间的包含关系(大功能包小功能，比如查询详细信息功能包含查询用户)

![image-20210419204410302](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210419204410.png)

![image-20210419203839654](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210419203839.png)

##### 4.用例和用例之间的扩展关系(功能上进一步扩展，比如数据分析功能和导出分析数据功能，导出分析数据是在数据分析的基础上进行扩展)

![image-20210419203902010](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210419203902.png)





## 时序图(Sequence Diagram)制作

![image-20210420210400280](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210420210407.png)