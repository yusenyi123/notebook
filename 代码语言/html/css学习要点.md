# css学习要点

## 1.font复合写法要按照顺序

```
font: font-style font-weight font-size/line-height  font-family

使用font属性时，必须按上面语法格式中的序书写。不能更换顺序，且各个属性间以空格隔开
不需要设置的属性可以省略（取默认值），但必须保留font-size和 font-family属性，否则font属性将不起作用



font-style:设置字体的样式如倾斜
font-weight:设置字体的粗细 
font-size/line-height设置字体的大小和行高
font-family:设置字体系列:是黑体还是宋体等


```

## 2.html元素显示模式(块元素,行元素,行内块元素)



### 元素模式详细讲解

| 元素模式                         | 元素行内排列                                                 | 宽度                                                         | 高度                                                         | margin,padding,和元素包含性质                                | 特殊情况                                                     |
| -------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 块元素(display:block)            | 一行只能放一个块元素。渲染块元素会进行换行。                 | 1.默认宽度是父元素的100%宽度    2.可以设置width属性进行调整  | 1.默认高度由内部子元素的高度决定 2.可以设置height属性进行调整 | 1.可以容纳任何其他标签  2.对margin,padding各个方位均可生效   | 文字类的元素内不能使用块级元素，比如：`<h1>~<h6>和<p>`等都是文字类块级元素，里面都不能放其他块级元素(写html的时候语法不会有问题,但是在浏览器渲染的时候就会出现问题,这些文字类块元素中包含的标签在浏览器渲染的时候会被拿到标签外) |
| 行元素(display:inline)           | 一行可以放多个行内元素。渲染的时候不进行换行。在原先的行继续渲染。 | 1.默认宽度由内部子元素宽度决定          2.设置width属性没有作用 | 1.默认高度由内部子元素高度(如font-size line-)决定     2.设置height属性没有作用 | 1.可以容纳文本或者其他行内元素标签  2.对margin的设置只有left 和right生效 3.对于padding 对于left和right生效，但在视觉效果看上下的padding设置也生效,但是上下的padding并不占位置 | 1、链接里面不能再放其他链接； 2、特殊情况，<a>里面可以放块级元素，但是还是转换为块级元素是最安全的； 3、行内元素水平方向的内外边距可以生效，但竖直方向则没有效果 |
| 行内块元素(display:inline-block) | 一行可以放多个行内块元素。渲染的时候不进行换行在原先的行继续渲染。 | 1.默认宽度由内部子元素宽度决定2.可以设置width属性进行调整    | 1.默认高度由内部子元素高度决定    2.可以设置height属性进行调整 | 1.可以容纳任何其他标签   2.对margin,padding各个方位均可生效  |                                                              |
|                                  |                                                              |                                                              |                                                              |                                                              |                                                              |
|                                  |                                                              |                                                              |                                                              |                                                              |                                                              |
|                                  |                                                              |                                                              |                                                              |                                                              |                                                              |
|                                  |                                                              |                                                              |                                                              |                                                              |                                                              |
|                                  |                                                              |                                                              |                                                              |                                                              |                                                              |
|                                  |                                                              |                                                              |                                                              |                                                              |                                                              |

### 常见元素标签分类

| 元素显示模式                     | 标签                                                         |      |
| -------------------------------- | ------------------------------------------------------------ | ---- |
| 块元素(display:block)            | 常见的块元素有`<h1>~<h6>、<p>、<div>、<ul>、<ol>、<i>`等，其中`<div>`标签是最典型的块元素 |      |
| 行元素(display:inline)           | 常见的行内元素有`<a>、<strong>、<b>、<em>、<i>、<del>、<s>、<ins>、<u>、<span>`等，其中`<br/> <span>标签`是最典型的行内元素。有的地方也将行内元素称为内联元素。 |      |
| 行内块元素(display:inline-block) | 在行内元素中有几个特殊的标签`<img/>、< input/>、<td>`,它们同时具有块元素和行内元素的持点， 有些资料称它们为行内块元素 |      |





## 3. margin(外边距)的一些问题

### 1.相邻元素的外边距合并问题

margin(外边距)不是盒子的一部分，处于公共部分表示相邻两个盒子(左右相邻,上下相邻)之间的间隔，所以当两个相邻盒子都设置margin值的时候，取值大的一方作为两者的间隔





### 2.父元素和子元素外边距合并问题

当一个元素包含在另一个元素中时（假设没有内边距或边框把外边距分隔开），它们的上和/或下外边距也会发生合并。请看下图：

![CSS 外边距合并实例 2](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20210110124519.gif)

```
解决方案：
①可以为父元素定义上边框，
②可以为父元素定义上内边距。
①可以为父元素添加 overflow: hidden
```



### 3.margin取负值的效果(padding不可以为负数)



```
为了方便理解负值margin，我们引入参考线的定义，参考线就是就是margin移动的基准点，而margin的值就是移动的数值。
margin的参考线有两类，一类是top、left，它们以外元素作为参考线，另一类是right、bottom，它们以自身作为参考线。


top负值就是以父元素内容区域的上边或者 相邻元素margin 的下边为参考线;
left负值就是以父元素内容区域的左边或者  左方相邻元素 margin 的右边为参考线;

right负值就是以元素本身border的右边为参考线；
bottom负值就是以元素本身border的下边为参考线；



margin-top和margin-left为负值起到的作用是移动自身元素

margin-bottom和margin-right为负值起到的作用是让其他元素向自身靠近，其他元素可以覆盖当前原酸

```



#### 使用margin负数的效果设置水平居中和垂直居中(前提:知道需要居中的元素的width height)

```
<div style="width: 300px;height: 300px;background-color: red;position: relative">

   <span style="width:30px;position: absolute;left: 50%;margin-left:-15px;z-index: 1">123</span>

    <div style="width: 100px;height: 100px;background-color: yellow;position: absolute;left: 50%;margin-left: -50px;"></div>
</div>


```







## 4.css属性书写顺序(代码规范)

```

建议道循以下顺序
1.布局定位属性： display/ position/Ioat/ clear/ visibility/ overflow(建议 display第一个写，毕境关系到模式)
2.自身属性； width/ height/ margin/ padding/ border/ background
3.文本属性：color/font/text-decoretion/text-align/vertical-align/white-space/break-word
4.其他性(CSS3):content/uror/prderfadlus/boxshadow/texshadow/backgroundtineargradent

```

