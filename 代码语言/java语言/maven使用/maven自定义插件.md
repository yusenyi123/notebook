# maven自定义插件

## 参考：

http://www.itsoku.com/article/244

https://blog.csdn.net/chy555chy/article/details/86742353

https://maven.apache.org/guides/plugin/guide-java-plugin-development.html



https://maven.apache.org/plugin-tools/maven-plugin-tools-annotations/index.html



https://cloud.tencent.com/developer/article/1433523

## 插件按照是否依赖项目工程可分成两类

### 1.依赖工程的插件(即执行该插件时需要指定一个pom.xml)

### 2.不依赖工程的插件（可以直接执行,不需要指定pom.xml）



### mvn命令用法 

####  1. mvn  生命周期阶段    [options]   （本质是执行多个插件）

举例：

```
mvn install

mvn clean  
```





####  2.单独执行插件命令： mvn   插件groupId:插件artifactId:[插件version]:插件目标   [options]  

```
注意如果插件需要依赖项目，需要在到有pom.xml 文件下去执行命令，或者使用-f  指定pom.xml文件所在目录

mvn -f G:\codespace\mavenspace\myplugs1  myplugs1:goal2
```



####  3.单独执行插件命令： mvn 前缀:插件目标   [options]  （本质和2相同，只是经过了一定配置，让执行变得简单）

##### mvn命令使用前缀执行原理：

```
为了简化对插件的调⽤，可以在命令⾏中使⽤前缀指明要执⾏的插件，现在解释⼀下Maven是如何根据插件的前缀找到
真正的插件的。插件前缀与groupId:artifactId是⼀⼀对应的。这种对应关系保存在仓库的元数据中，这样的元数据为groupId/maven-metadata.xml文件。要准确地解析到插件，还需要解释⼀下这⾥的groupId。前⾯介绍过，⽬前绝⼤部分插件都是放在http://repo1.maven.org和http://repository.codehaus.org中的，它们的groupId对应的是org.apache.maven.plugins和org.codehaus.mojo。Maven在解析插件仓库元数据的时候，会默认使⽤org.apache.maven.plugins和org.codehaus.mojo两个groupId，也就是说，Maven会⾃动检测http://repo1.maven.org/maven2/org/apache/maven/plugins/maven-metadata.xml和http://repository.codehaus.org/org/codehaus/mojo/maven-metadata.xml中的元数据。
元数据文件会被下载到本地文件中，自定义插件install时也会有元数据文件在本地仓库中
 
当然，也可以告诉Maven从其他仓库中查找，只要在settings.xml中做如下配置。
  <pluginGroups>
	 <pluginGroup>org.example</pluginGroup>
  </pluginGroups>
  
这样配置后，Maven不仅仅会检测本地仓库中
org/apache/maven/plugins/maven-metadata.xml、org/codehaus/mojo/maven-metadata.xml，还会检测org/example/maven-metadata.xml

maven-metadata.xml中的内容


<?xml version="1.0" encoding="UTF-8"?>
<metadata>
  <plugins>
    <plugin>
      <name>testplug4</name>
      <prefix>testplug4</prefix>
       <artifactId>testplug4</artifactId>
    </plugin>
    <plugin>
      <name>myplugs1</name>
      <prefix>myplugs1</prefix>
      <artifactId>myplugs1</artifactId>
    </plugin>
  </plugins>
</metadata>


可以看出前缀和artifactId绑定在一起，再加上在setting.xml中配置的groupid,那么前缀和一组groupid和artifactId即绑定在一起，所以可以通过mvn 前缀:目标 的方式调用插件


```





##### mvn命令可使用的参数选项：

```

参数选项：

 -B，-批处理模式以非交互方式运行（批处理）模式（禁用输出颜色）

 -D，-定义<arg>定义系统属性
mvn -DpropertyName=propertyValue clean package 可以用来临时定义属性和值。如果 pom.xml 中已经有该属性，那么会替换掉pom.xml 中的值。
project>
	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<android.sdk.path>C:\software\android\sdk</android.sdk.path>
		<maven.test.skip>true</maven.test.skip>
		<maven.javadoc.skip>true</maven.javadoc.skip>
	</properties>
</project>
如果需要定义多个变量，可以用空格分隔
mvn -DpropA=valueA -DpropB=valueB -DpropC=valueC clean package


当然这个属性也可以直接在 pom.xml 文件下配置

 -f，-file <arg>强制使用备用POM文件（或带有pom.xml的目录），pom文件路径必须紧跟在 -f 参数后面

 -e，-errors产生执行错误消息

 -X，-debug产生执行调试输出

 -l，-log-file <arg>所有构建输出的日志文件会去（禁用输出颜色）

 -q，-quiet安静的输出-仅显示错误

 -v，-version显示版本信息

 -h，-help显示帮助信息
 -P, --activate-profiles <arg> P表示 Profiles 配置文件，需要在 <profile> 标签中指定 <id> 才能用 -P 使之生效。



mvn -f G:\codespace\mavenspace\myplugs1  myplugs1:goal2  -Dsex=傻逼


```



## 自定义插件开发

### 1.创建一个maven 的java工程,修改pom.xml

#### 1.1修改`<packaging>`标签中的值为maven-plugin

#### 1.2 导入两个开发maven插件需要的依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>myplugs1</artifactId>
    <version>1.0-SNAPSHOT</version>
    
    <!--packaging标签值 为maven-plugin -->
    <packaging>maven-plugin</packaging>

  <!-- 导入两个开发插件需要的依赖-->
     
    <dependencies>
        <!--使用javadoc的方式，即在注释中进行描述的方式进行开发-->
        <dependency>
            <groupId>org.apache.maven</groupId>
            <artifactId>maven-plugin-api</artifactId>
            <version>3.3.9</version>
        </dependency>

        <!--使用注解的方式进行开发-->
        <dependency>
            <groupId>org.apache.maven.plugin-tools</groupId>
            <artifactId>maven-plugin-annotations</artifactId>
            <version>3.5.2</version>
            <scope>provided</scope>
        </dependency>
        
         <!--测试依赖-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>3.8.1</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

</project>
```



### 2. 创建一个类，继承AbstractMojo这个抽象类，实现这个抽象类的方法

#### 我使用注解的方式进行maven插件开发，下面是注解使用介绍



##### @mojo注解 介绍

```
package org.apache.maven.plugins.annotations;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Inherited;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Documented
@Retention(RetentionPolicy.CLASS)
@Target({ElementType.TYPE})
@Inherited
public @interface Mojo {
    String name();  //一个mojo类就是一个插件的目标，这里就是指定目标名

    LifecyclePhase defaultPhase() default LifecyclePhase.NONE; //指定插件的默认生命周期中的阶段，如果这里进行了配置，那么在pom.xml配置插件绑定生命周期中阶段的时候，可以不指定阶段，这时候会选用这里的配置，详情可以查看自定义插件使用中的配置
    
    

    ResolutionScope requiresDependencyResolution() default ResolutionScope.NONE;

    ResolutionScope requiresDependencyCollection() default ResolutionScope.NONE;

    InstantiationStrategy instantiationStrategy() default InstantiationStrategy.PER_LOOKUP;

    String executionStrategy() default "once-per-session";

    boolean requiresProject() default true;//是否需要依赖项目，也就是该插件执行是否需要一个pom.xml的配置文件，默认是需要依赖项目

    boolean requiresReports() default false;

    boolean aggregator() default false;

    boolean requiresDirectInvocation() default false;

    boolean requiresOnline() default false;

    boolean inheritByDefault() default true;

    String configurator() default "";

    boolean threadSafe() default false;
}

```





##### @Parameter注解 介绍

```java
//
// Source code recreated from a .class file by IntelliJ IDEA
// (powered by Fernflower decompiler)
//

package org.apache.maven.plugins.annotations;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Inherited;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Documented
@Retention(RetentionPolicy.CLASS)
@Target({ElementType.FIELD})
@Inherited
public @interface Parameter {
    String name() default "";  //指定当前参数的名字，name值只能是使用该注解的参数的参数名，不填，默认值为使用该注解的参数的变量名
    
    //在运行时指定使用该注解的参数的参数值，在pom.xml中的 <execution>标签下的   <configuration>标签中指定的参数名标签
    
   // 注意maven默认使用的maven-plugin-plugin插件版本过低，无法识别高版本annotation新增加的name方法。
    //需要pom.xml加入如下代码，这样就能成功mvn install 了
     /**
        <pluginManagement>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-plugin-plugin</artifactId>
                    <version>3.5</version>
                </plugin>
            </plugins>
        </pluginManagement>
        **/

    String alias() default "";  //指定当前参数的别名，可以在配置文件中使用另一个名字的标签来指定运行时的参数

    String property() default "";  //指定命令行传参数时给定的参数名  即mvn -D参数名=参数值 就是这里的参数名

    String defaultValue() default "";  //指定参数的默认值

    boolean required() default false;// 当该值为true，在该Mojo运行前该参数就必须要有一个可用的值。如果Maven试图运行该Mojo的时候该参数的值为null，Maven就会抛出一个错误。

    boolean readonly() default false; //参数值只读，用户就只能通过更改POM中finalName的值来更改这个参数的值。
}

```











#### java代码例子

```java
package com.itany.plugin;

import org.apache.maven.plugin.AbstractMojo;
import org.apache.maven.plugin.MojoExecutionException;
import org.apache.maven.plugin.MojoFailureException;
import org.apache.maven.plugins.annotations.Mojo;
import org.apache.maven.plugins.annotations.Parameter;


@Mojo(name="goal4")
public class Myplugin4  extends AbstractMojo {

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    //这种写法错误，name的值必须为使用该注解的参数的参数名，此处正确写法应该是name="name",也可以不填写name属性
    @Parameter(name = "name1",alias = "name2")
    private  String name;

    @Parameter(name="age")
    private  String age;

    @Parameter(property = "sex")
    private  String gender;
    @Override
    public void execute() throws MojoExecutionException, MojoFailureException {

        System.out.println("我的自定义插件");
        System.out.println("name的值"+name);
        System.out.println("sex的值"+gender);
        System.out.println("age的值"+age);

    }
}

```







## 自定义插件使用

### 配置pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <!--插件的信息，在使⽤插件或者在pom中配置插件的时候，如果使⽤的是Maven官⽅插件的话，是可以不指定groupId的，因为     这些插件的groupId都是⼀样的，都是org.apache.maven.plugins -->
    <groupId>org.example</groupId>
    <artifactId>mytest</artifactId>
    <version>1.0-SNAPSHOT</version>



    <!-- 将插件和maven生命周期绑定，调用maven命令时执行绑定的插件-->
    <build>
        <plugins>

            <!--  配置一个插件-->
            <plugin>
                <groupId>org.example</groupId>
                <artifactId>testplug4</artifactId>
                <version>1.0-SNAPSHOT</version>
                <executions>
                    
                    <!--配置一个绑定 -->
                    <!-- 配置生命周期阶段和 插件中目标的绑定，一个阶段可以绑定多个插件中的目标-->
                    <execution>
                        <!--id 标识符,标识每一个execution，不能有相同的execution-->
                        <id>test1</id>
                        <phase>clean</phase>
                        
               <!--设置该阶段插件执行的目标，插件本质是一个jar，里面有多个类，每个类是一个目标，完成相应的功能，可以填写多个目标 -->
                       <goals>
                            <goal>hello</goal>
                            <goal>hello2</goal>
                        </goals>
                        
                   <!--插件目标运行时的参数  标签为参数名，标签内的值为参数值 -->
                  <!--效果就是命令执行插件时  mvn 插件名:插件目标 -D参数名=参数值..  效果一样  -->      
                     <configuration> 
                         <a>123</a>
                         <name>alice</name>
                     </configuration> 
                        
                    </execution>

            <!--如果插件开发中配置了默认绑定的生命周期，可以不写phase-->
                    <execution>
                        <id>test2</id>
                        <goals>
                            <goal>hello3</goal>
                        </goals>
                    </execution>

                </executions>
                
                  <!--整个插件的参数  标签为参数名，标签内的值为参数值 -->
                    <configuration> 
                         <user>123</user>
                         <name>alice</name>
                    </configuration> 
                
            </plugin>

        </plugins>
    </build>

</project>
```









