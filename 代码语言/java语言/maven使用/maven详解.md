# maven详解

## 参考：

http://yangtao309.github.io/blog/2014/11/14/mavenyuan-ma-fen-xi-2/

https://www.cnblogs.com/laoyin666/p/9204278.html



https://www.cnblogs.com/a-ray-of-sunshine/p/5011111.html

https://blog.csdn.net/killerover84/article/details/83586838



https://blog.csdn.net/zmh458/article/details/102486135





http://www.blogjava.net/jianyue/articles/227932.html





https://swenfang.github.io/2018/06/03/Maven-Priority/

## 1.maven介绍

### maven本质:

Maven 翻译为"专家"、"内行"，是 Apache 下的一个纯 Java 开发的开源项目。

Maven的本质是⼀个由java语音开发的项⽬管理⼯具，该工具将项⽬开发构建和管理过程抽象成⼀个项⽬对象模型(POM   project obecjt model）。开发⼈员只需做⼀些简单的配置，就可以批量完成项⽬的构建、报告和⽂档的⽣成⼯作。

### 两大功能：

1.帮助程序员完成项目构建(构建(build)   **构建就是以我们编写的 Java 代码、框架配置文件、国际化等其他资源文件、JSP 页 面和图片等静态资源作为“原材料”，去“生产”出一个可以运行的项目的过程。**)

2.帮助程序员管理依赖包







## 2.mvn 命令执行的流程





### pom 的继承(pom的继承是单继承,所有pom.xml都继承自super pom.xml)

 super pom.xml位于在 maven-model-builder-{version}.jar 包的org/apache/maven/model 中的 pom-4.0.0.xml 这个文件



如果要自定义继承的pom.xml，那个这个pom.xml中packaging属性配置必须为pom





### mvn命令运行时根据effective pom中的配置信息，执行不同的插件目标(effective pom包含了当前项目POM对应的PO对象，直到Super POM对应的PO对象中的信息)



#### effective pom是由当前项目中的pom.xml加上当前项目pom.xml继承的pom.xml整合生成(注意所有的pom都继承super pom )



#### 查看effective pom

mvn help:effective-pom



#### effective pom例子

```
  <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.example</groupId>
    <artifactId>test2</artifactId>
    <version>1.0-SNAPSHOT</version>
    
    <properties>
      <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
      <maven.compiler.source>1.8</maven.compiler.source>
      <maven.compiler.target>1.8</maven.compiler.target>
    </properties>
    
    <repositories>
      <repository>
        <snapshots>
          <enabled>false</enabled>
        </snapshots>
        <id>central</id>
        <name>Central Repository</name>
        <url>https://repo.maven.apache.org/maven2</url>
      </repository>
    </repositories>
    
    <pluginRepositories>
      <pluginRepository>
        <releases>
          <updatePolicy>never</updatePolicy>
        </releases>
        <snapshots>
          <enabled>false</enabled>
        </snapshots>
        <id>central</id>
        <name>Central Repository</name>
        <url>https://repo.maven.apache.org/maven2</url>
      </pluginRepository>
    </pluginRepositories>
    
    <build>
      <sourceDirectory>G:\codespace\mavenspace\test2\src\main\java</sourceDirectory>
      <scriptSourceDirectory>G:\codespace\mavenspace\test2\src\main\scripts</scriptSourceDirectory>
      <testSourceDirectory>G:\codespace\mavenspace\test2\src\test\java</testSourceDirectory>
      <outputDirectory>G:\codespace\mavenspace\test2\target\classes</outputDirectory>
      <testOutputDirectory>G:\codespace\mavenspace\test2\target\test-classes</testOutputDirectory>
     
     <resources>
        <resource>
          <directory>G:\codespace\mavenspace\test2\src\main\resources</directory>
        </resource>
      </resources>
      
      <testResources>
        <testResource>
          <directory>G:\codespace\mavenspace\test2\src\test\resources</directory>
        </testResource>
      </testResources>
      
      <directory>G:\codespace\mavenspace\test2\target</directory>
      <finalName>test2-1.0-SNAPSHOT</finalName>
      
      <pluginManagement>
        <plugins>
          <plugin>
            <artifactId>maven-antrun-plugin</artifactId>
            <version>1.3</version>
          </plugin>
          <plugin>
            <artifactId>maven-assembly-plugin</artifactId>
            <version>2.2-beta-5</version>
          </plugin>
          <plugin>
            <artifactId>maven-dependency-plugin</artifactId>
            <version>2.8</version>
          </plugin>
          <plugin>
            <artifactId>maven-release-plugin</artifactId>
            <version>2.5.3</version>
          </plugin>
        </plugins>
      </pluginManagement>
      
      
      <plugins>
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>2.5</version>
          <executions>
            <execution>
              <id>default-clean</id>
              <phase>clean</phase>
              <goals>
                <goal>clean</goal>
              </goals>
            </execution>
          </executions>
        </plugin>
        
        <plugin>
          <artifactId>maven-resources-plugin</artifactId>
          <version>2.6</version>
          <executions>
          
            <execution>
              <id>default-testResources</id>
              <phase>process-test-resources</phase>
              <goals>
                <goal>testResources</goal>
              </goals>
            </execution>
            
            <execution>
              <id>default-resources</id>
              <phase>process-resources</phase>
              <goals>
                <goal>resources</goal>
              </goals>
            </execution>
            
          </executions>
        </plugin>
        
        <plugin>
          <artifactId>maven-jar-plugin</artifactId>
          <version>2.4</version>
          <executions>
            <execution>
              <id>default-jar</id>
              <phase>package</phase>
              <goals>
                <goal>jar</goal>
              </goals>
            </execution>
          </executions>
        </plugin>
        <plugin>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.1</version>
          <executions>
            <execution>
              <id>default-compile</id>
              <phase>compile</phase>
              <goals>
                <goal>compile</goal>
              </goals>
            </execution>
            <execution>
              <id>default-testCompile</id>
              <phase>test-compile</phase>
              <goals>
                <goal>testCompile</goal>
              </goals>
            </execution>
          </executions>
        </plugin>
        <plugin>
          <artifactId>maven-surefire-plugin</artifactId>
          <version>2.12.4</version>
          <executions>
            <execution>
              <id>default-test</id>
              <phase>test</phase>
              <goals>
                <goal>test</goal>
              </goals>
            </execution>
          </executions>
        </plugin>
        <plugin>
          <artifactId>maven-install-plugin</artifactId>
          <version>2.4</version>
          <executions>
            <execution>
              <id>default-install</id>
              <phase>install</phase>
              <goals>
                <goal>install</goal>
              </goals>
            </execution>
          </executions>
        </plugin>
        <plugin>
          <artifactId>maven-deploy-plugin</artifactId>
          <version>2.7</version>
          <executions>
            <execution>
              <id>default-deploy</id>
              <phase>deploy</phase>
              <goals>
                <goal>deploy</goal>
              </goals>
            </execution>
          </executions>
        </plugin>
        <plugin>
          <artifactId>maven-site-plugin</artifactId>
          <version>3.3</version>
          <executions>
            <execution>
              <id>default-site</id>
              <phase>site</phase>
              <goals>
                <goal>site</goal>
              </goals>
              <configuration>
                <outputDirectory>G:\codespace\mavenspace\test2\target\site</outputDirectory>
                <reportPlugins>
                  <reportPlugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-project-info-reports-plugin</artifactId>
                  </reportPlugin>
                </reportPlugins>
              </configuration>
            </execution>
            <execution>
              <id>default-deploy</id>
              <phase>site-deploy</phase>
              <goals>
                <goal>deploy</goal>
              </goals>
              <configuration>
                <outputDirectory>G:\codespace\mavenspace\test2\target\site</outputDirectory>
                <reportPlugins>
                  <reportPlugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-project-info-reports-plugin</artifactId>
                  </reportPlugin>
                </reportPlugins>
              </configuration>
            </execution>
          </executions>
          <configuration>
            <outputDirectory>G:\codespace\mavenspace\test2\target\site</outputDirectory>
            <reportPlugins>
              <reportPlugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-project-info-reports-plugin</artifactId>
              </reportPlugin>
            </reportPlugins>
          </configuration>
        </plugin>
      </plugins>
    </build>
    
    
    <reporting>
      <outputDirectory>G:\codespace\mavenspace\test2\target\site</outputDirectory>
    </reporting>
    
  </project>
```



#### maven插件

```
maven插件本质上就是一个jar包，里面包含了一些特殊的java类，被成为mojo  maven plain old java object
一个特殊的java类就是一个目标
```

##### 单独执行插件命令

```
当Maven解析到compiler:compile命令后，它⾸先基于默认的groupId归并所有插件仓库的元数据org/apache/maven/plugins/maven-metadata.xml；接着检查归并后的元数据，找到对应的artifactId为maven-compiler-plugin；接下来再结合当前元数据的groupId为org.apache.maven.plugins；最后找到仓库中最新的version，从⽽就可
以得到⼀个插件的完整坐标信息。

在pom.xml 目录下执行下列命令

mvn <插件名称即groupId:artifactId:vesrsion(可以不用填写完整,写artifactId,maven根据他的查找机制也能找到)  |插件前缀(将在对应groupId的插件目录下原配置文件中将插件的artifactId对应一个前缀，方便调用)>:<插件中的目标>  [-D参数名=参数值(参数值可以没有)]


举例
//使用前缀调用
mvn  compiler:compile

//最完整版
mvn org.apache.maven.plugins:maven-archetype-plugin:2.2:create -Dgroupid-cn.com.mvnbook.demo
-Dartifactid=MVNBOOKTPO1 -Dpackagename=cn.com.mvnbook.demo.tpol
```





#### effective pom中包含了mvn项目构建的生命周期的不同阶段和与这些阶段相对应的插件的绑定配置



mvn 将项目的构建过程抽象成了几个生命周期，每个生命周期都有不同的阶段，不同的阶段完成不同的事情，Maven只是对项⽬的构
建过程进⾏了统⼀的抽象定义和管理。⾄于每个阶段由谁来做，Maven⾃⼰不去实现，⽽是让对应的插件去完成。(抽象过程)



每个阶段都绑定了对应的插件，本质上由对应的插件实现每个阶段要完成的事情(插件具体实现相应的功能，完成不同的事情)

⽐如maven-compile-plugin就可以完成在compile阶段Java源代码的编译任务。



在effective pom包含了mvn项目构建的生命周期的不同阶段和与这些阶段相对应的插件的绑定配置

在执行mvn命令的时候，我们输入的参数是mvn所抽象的生命周期中的阶段，如执行mvn compile ，但本质上执行effective pom配置中与compile阶段绑定的maven插件





```
通过对Maven⽣命周期的了解，可以知道Maven只是对项⽬的构建过程进⾏了统⼀的抽象定义和管理。⾄于每个阶段由谁来做，
Maven⾃⼰不去实现，⽽是让对应的插件去完成。这就是插件的作⽤。⽐如maven-compile-plugin就可以完成在compile阶段Java源代码的编译任务。

但是从插件本身来说，⼀个插件可以实现⽣命周期多个阶段的任务，⽐如maven-dependency-plugin就可以实现⼗多个功能：分析项⽬的依赖功能；列出项⽬的依赖树；分析依赖的来源等。为⽅便指定执⾏插件的某个功能，将插件的每个功能叫⽬标。这样就可以实现在哪个阶段，执⾏哪个插件，达到哪个⽬标。⽐如“dependency:analyze”，表⽰maven-dependency-plugin的分析⽬标；“dependency:tree”表⽰
maven-dependency-plugin列出依赖的⽬标
```



#### 将一个插件同maven项目构建的生命周期阶段绑定

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



```


@Parameter注解之前的部分是参数的描述，这个注解将变量标识为mojo参数。注解的defaultValue参数定义变量的默认值，此值maven的属性值，例如“${project.version}”（更多信息可以看上一篇文章中的 target="_blank">maven属性部分），property参数可用于通过引用用户通过-D选项设置的系统属性，即通过从命令行配置mojo参数，如mvn ... -Dsayhi.greeting=路人甲Java可以将路人甲Java的值传递给greeting参数，这个注解还有几个属性大家有兴趣的可以自己去研究一下。
```



## profile标签(不同应用开发激活不同的配置)



```
一个pom.xml中的多个profile只能被激活一个

setting.xml 可以配置profile的激活，可以配置多个profile激活，这些激活的profile可以来自setting.xml，也可以是项目pom.xml 中的profile，也可以是外部profile.xml中的profile
<activeProfiles>
    <activeProfile>p1</activeProfile>
	   <activeProfile>p2</activeProfile>
  </activeProfiles>
```

```
     <plugin>
        <artifactId>maven-plugin-plugin</artifactId>
        <version>3.5</version>
        <executions>
        
          <execution>
            <id>default-addPluginArtifactMetadata</id>
            <phase>package</phase>
            <goals>
              <goal>addPluginArtifactMetadata</goal>
            </goals>
          </execution>
          
          <execution>
            <id>default-descriptor</id>
            <phase>process-classes</phase>
            <goals>
              <goal>descriptor</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
```





## plugin相关

```
对于不同的<packaging>值，即不同的项目打包方式，maven内置的生命周期不同阶段绑定的插件不同

如使用<packaging>maven-plugin</packaging>时，会使用maven-plugin-plugin插件中三个目标plugin:descriptor，plugin:addPluginArtifactMetadata，和plugin:updateRegistry。这些目标生成一个描述文件，对仓库数据执行一些修改。


plugin:descriptor 
生成 Plugin Descriptor(插件描述符) 即plugin.xml


plugin:addPluginArtifactMetadata 
生成maven-metadata-local.xml

Description:
Inject any plugin-specific artifact metadata to the project's artifact, for subsequent installation and deployment. It is used:
将任何特定于插件的工件元数据注入到项目的工件中，以进行后续的安装和部署。
它用于：
1. to add the latest metadata (which is plugin-specific) for shipping alongside the plugin's artifact
添加最新的元数据（特定于插件）以与插件的工件一起发送
2. to define plugin mapping in the group
在组中定义插件映射



plugin:updateRegistry

Description:

Update the user plugin registry (if it's in use) to reflect the version we're installing.
Attributes:

Requires a Maven project to be executed.
The goal is thread-safe and supports parallel builds.
Since version: 2.0.
Binds by default to the lifecycle phase: install.
```



### 老版本maven-plugin-plugin 中的目标和构建自定义maven插件的生命周期阶段绑定表



会使用maven-plugin-plugin插件中三个目标plugin:descriptor，plugin:addPluginArtifactMetadata，和plugin:updateRegistry

![image-20210202094634926](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210206113204.png)







```
<pluginManagement>声明插件,在<pluginManagement>声明后，在<bulid>下的<plugins>使用插件的时候，可以不指定版本号


<dependencyManagement> 
```

