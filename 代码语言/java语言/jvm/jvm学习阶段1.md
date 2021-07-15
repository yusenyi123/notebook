# jvm学习阶段1

## 参考

https://blog.csdn.net/weixin_43591980/article/details/109593587

https://www.163.com/dy/article/FJ969Q3V0518KQEA.html

http://wangwren.com/2019/11/JVM%E5%86%85%E5%AD%98%E6%A8%A1%E5%9E%8B/#JVM%E7%9A%84%E8%BF%90%E8%A1%8C%E5%8E%9F%E7%90%86

https://www.kancloud.cn/imnotdown1019/java_core_full/1012266

## jvm内存和本地内存



jvm即java虚拟机，java虚拟机通过软件的方式模拟一台硬件电脑，jvm虚拟机专门运行我们的java代码，那么jvm虚拟机就会占用内存中的一部分内存作为这台虚拟机的专用内存即jvm内存，我们的java代码可以操作jvm内存



而本地内存不能通过java代码进行控制，只能使用本地方法(和cpu架构相关的机器语言代码 二进制形式表示的机器语言) ，这些方法是java语言的底层实现