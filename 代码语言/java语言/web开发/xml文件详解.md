# xml文件详解

## 命名空间(XML 命名空间提供避免元素命名冲突的方法。)

在 XML 中，元素名称是由开发者定义的，当两个不同的文档使用相同的元素名时，就会发生命名冲突。

### 冲突情景：

XML1:

```xml
<table>
<tr>
<td>Apples</td>
<td>Bananas</td>
</tr>
</table>
```

XML2:

```xml
<table>
<tr>
<td>Apples</td>
<td>Bananas</td>
</tr>
</table>
```

假如这两个 XML 文档被一起使用，由于两个文档都包含带有不同内容和定义的` <table> `元素，就会发生命名冲突。

XML 解析器无法确定如何处理这类冲突

### XML 命名空间 - xmlns 属性

当命名空间被定义在元素的开始标签中时，所有带有相同前缀的子元素都会与同一个命名空间相关联。

命名空间，可以在他们被使用的元素中或者在 XML 根元素中声明：

```xml
<root xmlns:h="http://www.w3.org/TR/html4/"
xmlns:f="http://www.w3cschool.cc/furniture">

<h:table>
<h:tr>
<h:td>Apples</h:td>
<h:td>Bananas</h:td>
</h:tr>
</h:table>

<f:table>
<f:name>African Coffee Table</f:name>
<f:width>80</f:width>
<f:length>120</f:length>
</f:table>

</root>
```



### 默认的命名空间(无前缀时的命名空间)（在父元素中声明工作空间，子元素可以省去前缀）

为元素定义默认的命名空间可以让我们省去在所有的子元素中使用前缀的工作。它的语法如下：

==table中的子元素命名空间均为`http://www.w3.org/TR/html4/`==

```
<table xmlns="http://www.w3.org/TR/html4/">
<tr>
<td>Apples</td>
<td>Bananas</td>
</tr>
</table>
```

