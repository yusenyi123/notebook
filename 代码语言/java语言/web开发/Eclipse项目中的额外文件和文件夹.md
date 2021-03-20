# Eclipse项目中的额外文件和文件夹

## 参考：

https://blog.csdn.net/xiongyouqiang/article/details/79609512

http://cache.baiducontent.com/c?m=yFmGCHhE7mwuiSVfPb-ODPDEgZB_iiL6Wh8CJIWpAXzH3bN-_jBo2OJrd2QS5Z81YC260Gl3XtPA2ZTAW03DOyLUwA5GxS-PiXY61nTzjYSBIkiBrQLcMWgIg_sG3mRkS1e1IsVRpCnizuH66hyYz_&p=c6769a4799934eab5bb7c8224d47&newp=99759a41d5d012a05aa6e62c534c92695d0fc20e38d2db01298ffe0cc4241a1a1a3aecbf2c22120ed6c078600aad485bedf137723d0034f1f689df08d2ecce7e3c&s=cfcd208495d565ef&user=baidu&fm=sc&query=classpathentry&qid=99cdbe820009c74f&p1=2



https://blog.csdn.net/qq_26264237/article/details/89714823

https://help.eclipse.org/2020-12/index.jsp?topic=%2Forg.eclipse.platform.doc.isv%2Freference%2Fmisc%2Fproject_description_file.html



https://www.cnblogs.com/shihaiming/p/5803957.html



https://www.twblogs.net/a/5c2e3981bd9eee35b3a48e2c



https://blog.csdn.net/roobert_chao/article/details/88884067



https://stackoverflow.com/questions/478985/slim-down-and-or-understand-the-eclipse-files-in-a-dynamic-web-project

https://wiki.eclipse.org/JSDT/Architecture

https://www.eclipse.org/pdt/help/html/setting_the_javascript_build_path.htm

Eclipse项目根目录下通常有两个文件：.project和.classpath，.project是Eclipse项目必须有的文件，而.classpath是Java项目必须有的文件。这两个文件均是XML格式的文本文件，用普通文本编辑器即可打开。

## .project(提供了项目的完整描述，可以用来在工作区中它被导出后再次导入时重新创建项目)

.project文件提供了项目的完整描述，可以用来在工作区中它被导出后再次导入时重新创建项目。你的新项目应该和以下类似：

```xml
<?xml version="1.0"encoding="UTF-8"?> 
<projectDescription> 
    <name>TestProject</name> 
    <comment></comment> 
    <projects> 
    </projects> 
    <buildSpec> 
        <buildCommand> 
            <name>org.eclipse.jdt.core.javabuilder</name> 
            <arguments></arguments> 
        </buildCommand> 
    </buildSpec> 
    <natures> 
        <nature>org.eclipse.jdt.core.javanature</nature> 
    </natures> 
</projectDescription> 
```

### .project配置文件节点说明

详细说明网址

https://help.eclipse.org/2020-12/index.jsp?topic=%2Forg.eclipse.platform.doc.isv%2Freference%2Fmisc%2Fproject_description_file.html&cp=2_1_5_11



https://g.yuque.com/jiujuan/techstack/vgqfxn?language=en-us

```
1、<name></name>：工程名/项目名；
2、<comment></comment>：工程注释描述；
3、<natures></natures>：运行时需要的额外IDE插件；nature标记表示该项目的类型。
这里的nature性质为org.eclipse.jdt.core.javanature表示它是Java项目。
运行时需要的额外Eclipse插件<natures></natures>，及其具体加载方式信息<buildSpec></buildSpec>


4、<buildSpec></buildSpec> （bulidSpec    Build specification  生成规范）
<buildSpec>中包含一系列<buildCommand> 


<buildCommand> 包含两个标签<name>和 <arguments> 
<name>是项目构建命令的名字 
<arguments> 构建命令初始化时需要传递的参数（一般看到的都是空的）

   <buildSpec> 
        <buildCommand> 
            <name>org.eclipse.jdt.core.javabuilder</name> 
            <arguments></arguments> 
        </buildCommand> 
    </buildSpec> 

    
    
    
    
buildSpec
org.eclipse.jdt.core.javabuilder：java工程需要
org.eclipse.m2e.core.maven2Builder：maven工程需要
org.eclipse.wst.common.project.facet.core.builder：
org.eclipse.wst.jsdt.core.javascriptValidator：验证工程的js文件，去掉会取消js验证
org.eclipse.wst.validation.validationbuilder：

natures
org.eclipse.jdt.core.javanature：java工程需要
org.eclipse.m2e.core.maven2Nature：maven工程需要
org.eclipse.wst.common.project.facet.core.nature：Convert to faceted form
org.eclipse.jem.workbench.JavaEMFNature：Web工程需要
org.eclipse.wst.common.modulecore.ModuleCoreNature：
```

```
对于每个工程，你可以注册了一个nature，它用来关联一个或多个builder。一个builder可以操纵你在Eclipse IDE中的资源，来创建其它的资源文件。

例如Java Builder可以把java 源文件变成class文件。通过org.eclipse.core.resources.builders可以扩展你自己的builder，每一个builder都是IncrementalProjectBuilder的子类，必须实现接口build，当resources有改变时，autobuild会调用build方法。当用户选择“clean” "build"或者“build all”时，都会调用build方法。

builder会包含在.project文件中。
```

![image-20210304192242382](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210304192242.png)



```
The project description file
Description: When a project is created in the workspace, a project description file is automatically generated that describes the project.  The purpose of this file is to make the project self-describing, so that a project that is zipped up or released to a server can be correctly recreated in another workspace.  This file is always called ".project", and is located as a direct member of the project's content area.  The name of the file is exposed through the static field DESCRIPTION_FILE_NAME on org.eclipse.core.resources.IProjectDescription.
描述：在工作空间中创建项目时，将自动生成一个描述该项目的项目描述文件。
此文件的目的是使项目自描述，以便可以在另一个工作空间中正确地创建已压缩或发布到服务器的项目。
该文件始终称为“ .project”，并且是项目内容区域的直接成员。
文件名通过org.eclipse.core.resources.IProjectDescription上的静态字段DESCRIPTION_FILE_NAME公开。

The name, location, and content of this file are part of the workspace API.  This means they are guaranteed not to change in a way that would break existing users of the file.  However, the right to add additional elements and attributes to the markup is reserved for possible future additions to the file.  For this reason, clients that read the description file contents should tolerate unknown elements and attributes.

Clients that modify, delete, or replace the project description file do so at their own risk.  Projects with invalid or missing description files will not be generally usable.  If a project with an invalid description file is discovered on workspace startup, the project is closed, and it will not be possible to open it until the project description file has been repaired.  The workspace will not generally attempt to automatically repair a missing or invalid description file.  One exception is that missing project description files will be regenerated during workspace save and on calls to IProject.setDescription.

Modifications to the project description file have mostly the same effect as changing the project description via IProject.setDescription.  One exception is that adding or removing project natures will not trigger the corresponding nature's configure or deconfigure method.  Another exception is that changes to the project name are ignored.

If a new project is created at a location that contains an existing project description file, the contents of that description file will be honoured as the project description.  One exception is that the project name in the file will be ignored if it does not match the name of the project being created.  If the description file on disk is invalid, the project creation will fail.


 
Configuration Markup:

   <!ELEMENT projectDescription (name, comment, projects, buildSpec, natures, linkedResources)>

   <!ELEMENT name EMPTY>

name - the name of the project.  Ignored if it does match the name of the project using this description.  Corresponds to IProjectDescription.getName().
   <!ELEMENT comment EMPTY>

comment - a comment for the project.  The comment can have arbitrary contents that are not interpreted by the project or workspace.  Corresponds to IProjectDescription.getComment().
   <!ELEMENT projects (project)*>
   <!ELEMENT project EMPTY>
projects - the names of the projects that are referenced by this project.  Corresponds to IProjectDescription.getReferencedProjects().
   <!ELEMENT buildSpec (buildCommand)*>

   <!ELEMENT buildCommand (name, arguments)>

   <!ELEMENT name EMPTY>

   <!ELEMENT arguments (dictionary?)>

   <!ELEMENT dictionary (key, value)*>

   <!ELEMENT key EMPTY>

   <!ELEMENT value EMPTY>

buildSpec - the ordered list of build commands for this project.  Corresponds to IProjectDescription.getBuildSpec().
buildCommand - a single build commands for this project.  Corresponds to org.eclipse.core.resources.ICommand.
name - the symbolic name of an incremental project builder.  Corresponds to ICommand.getBuilderName().
arguments - optional arguments that may be passed to the project builder.  Corresponds to ICommand.getArguments().
dictionary - a list of <key, value> pairs in the argument list.  Analagous to java.util.Map.
   <!ELEMENT natures (nature)*>

   <!ELEMENT nature EMPTY>

natures - the names of the natures that are on this project.  Corresponds to IProjectDescription.getNatureIds().
nature - the name of a single natures on this project.
   <!ELEMENT linkedResources (link)*>

   <!ELEMENT link (name, type, location)>

   <!ELEMENT name EMPTY>

   <!ELEMENT type EMPTY>

   <!ELEMENT location EMPTY>

linkedResources - the list of linked resources for this project.
link - the definition of a single linked resource.
name - the project-relative path of the linked resource as it appears in the workspace.
type - the resource type. Value values are: "1" for a file, or "2" for a folder.
location - the local file system path of the target of the linked resource. Either an absolute path, or a relative path whose first segment is the name of a workspace path variable.
locationURI - if the file is not in the local file system, this attribute contains the absolute URI of the resource in some backing file system.
Examples: The following is a sample project description file.  The project has a single nature and builder configured, and some project references.
<?xml version="1.0" encoding="UTF-8"?>
<projectDescription>
    <name>WinterProject</name>
    <comment>This is a cool project.</comment>
    <projects>
        <project>org.seasons.sdt</project>
        <project>CoolStuff</project>
    </projects>
    <buildSpec>
        <buildCommand>
            <name>org.seasons.sdt.seasonBuilder</name>
            <arguments>
                <dictionary>
                    <key>climate</key>
                    <value>cold</value>
                </dictionary>
            </arguments>
        </buildCommand>
    </buildSpec>
    <natures>
        <nature>org.seasons.sdt.seasonNature</nature>
    </natures>
</projectDescription>

API Information: The contents of the project description file map to the org.eclipse.core.resources.IProjectDescription interface. The project description file can be overwritten by the method IProject.setDescription().
```



## .classpath

### 作用：

.classpath文件用于记录项目编译环境的所有信息，包括：源文件路径、编译后class文件存放路径、依赖的jar包路径、运行的容器信息、依赖的外部project等信息。如果把该文件删除，则eclipse不能讲该工程识别为一个正常的java工程，仅仅当做普通的文件夹而导致不能正常运行。



### 举例说明

```xml
<?xml version="1.0" encoding="UTF-8"?>
<classpath>
    <classpathentry kind="src" path="src"/>
    <classpathentry kind="src" path="resource"/>
    <classpathentry kind="con" path="org.eclipse.jdt.launching.JRE_CONTAINER/org.eclipse.jdt.internal.debug.ui.launcher.StandardVMType/jdk1.7">
        <attributes>
            <attribute name="owner.project.facets" value="java"/>
        </attributes>
    </classpathentry>
    <classpathentry kind="con" path="org.eclipse.jst.server.core.container/org.eclipse.jst.server.tomcat.runtimeTarget/学习 8080">
        <attributes>
            <attribute name="owner.project.facets" value="jst.web"/>
        </attributes>
    </classpathentry>
    <classpathentry kind="con" path="org.eclipse.jst.j2ee.internal.web.container"/>
    <classpathentry kind="con" path="org.eclipse.jst.j2ee.internal.module.container"/>
    <classpathentry kind="output" path="WebContent/WEB-INF/classes"/>
</classpath>
```



①以”classpath”为根节点，每个“classpathentry”节点代表一个说明信息。 
②每个“classpathentry”以“kind”属性指明作用类型，“**path**”指明路径。 
③以上文件的所有内容，都是依赖项目中的“Java Build Path“ 内容改变而改变的，即对“Java Build Path“ 的所有操作都会反应到.classpath的文件内容中。 

![image-20210127182449506](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210206113021.png)

### 属性解析

#### kind="src"

```
kind="src"
src：即source 源文件，代表的是一个源文件，path=”src”是一个相对路径，相对.classpath文件本身，即path=”src”表示文件夹src与.classpath在同一个目录，且代表源文件

下列节点表示，相对.claaspath文件的src文件夹，是一个源代码文件夹
<classpathentry kind="src" path="src"/>

kind=”src”的操作对应于“Java Build Path”的“Source”tab页 


另外，当指定属性combineaccessrules=”false”是则代表引入外部project，具体如下
<classpathentry combineaccessrules="false" kind="src" path="/mybatis"/>
对应页面tab，其中path=”/mybatis”，是相对应workspace下的绝对路径。 
```





#### kind="output"

```
kind="output"
output用于指定kind="src"文件夹中的源码文件编译后的class文件存放路径

表示把<classpathentry>节点kind="src" 中设置的path路径下的源码文件编译后的class文件存放到bin目录下
<classpathentry kind="output" path="bin"/>
path：代表存放class文件路径，同样是相对.classpath文件的路径，找到“bin”，可以看到class文件的存放 


```

#### kind="con"

```xml
kind="con"
con即是container,就是程序运行的容器，或者就说是运行环境也OK，它实际上是在Myeclipse最初的时候要配置installed JREs中指定（一般情况下我们指定的是JDK），但是这里实际使用的是JDK下的JRE中的jar包，就是JDK_HOME/jre/lib就是对应的这条语句。具体内容如下

  <classpathentry kind="con" path="org.eclipse.jdt.launching.JRE_CONTAINER/org.eclipse.jdt.internal.debug.ui.launcher.StandardVMType/jdk1.7">
        <attributes>
            <attribute name="owner.project.facets" value="java"/>
        </attributes>
      
    </classpathentry>


   <classpathentry kind="con" path="org.eclipse.jst.server.core.container/org.eclipse.jst.server.tomcat.runtimeTarget/学习 8080">
        <attributes>
            <attribute name="owner.project.facets" value="jst.web"/>
        </attributes>
    </classpathentry>

<classpathentry kind="con" path="org.eclipse.jst.j2ee.internal.web.container"/>
<classpathentry kind="con" path="org.eclipse.jst.j2ee.internal.module.container"/>

```



#### kind="lib"

```
kind="lib"用于指定project依赖的Referenced Libraries，其中path指定了依赖的jar的相对路径。

<classpathentry kind="lib" path="WebContent/WEB-INF/lib/commons-dbcp-1.2.1.jar"/>

```



### 节点加载顺序

.classpath文件中各节点的顺序是通过tab-Order and Export 来控制的，不同的顺序可能会引起加载**class**文件问题，一般是源码放在最前面。 

## .settings文件夹(各种插件的配置文件)



### org.eclipse.jdt.core.prefs (编译相关)

```
org.eclipse.jdt.core.prefs文件指定了一些Java编译的特性，比如Java版本之类的，看文件每一行的key能猜出具体的用处。
典型内容:

eclipse.preferences.version=1
org.eclipse.jdt.core.compiler.codegen.inlineJsrBytecode=enabled
org.eclipse.jdt.core.compiler.codegen.targetPlatform=1.7
org.eclipse.jdt.core.compiler.compliance=1.7
org.eclipse.jdt.core.compiler.problem.assertIdentifier=error
org.eclipse.jdt.core.compiler.problem.enumIdentifier=error
org.eclipse.jdt.core.compiler.problem.forbiddenReference=warning
org.eclipse.jdt.core.compiler.source=1.7
```

在eclipse中修改window->Preferences->Java->Compiler的配置 就会修改该文件

![image-20210304113923905](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210304113923.png)

### org.eclipse.wst.common.project.facet.core.xml(当前项目使用了哪些框架)

```
org.eclipse.wst.common.project.facet.core.xml指示了项目中启用那些facet及facet的版本。
典型内容：


<?xml version="1.0" encoding="UTF-8"?>
<faceted-project>
  <runtime name="Apache Tomcat v8.0"/>
  <fixed facet="wst.jsdt.web"/>
  <installed facet="wst.jsdt.web" version="1.0"/>
  <installed facet="java" version="1.7"/>
  <installed facet="jst.web" version="3.1"/>
</faceted-project>
```

在eclipse中修改项目使用的框架会修改该文件

![image-20210127222042067](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210206113033.png)





### org.eclipse.wst.common.component(如何将web项目文件生成war,和war的部署)

```
org.eclipse.wst.common.component文件规定了项目怎么组装成一个webapp，这里可以玩很多种组装方式。
典型内容:

<?xml version="1.0" encoding="UTF-8"?>
<project-modules id="moduleCoreId" project-version="1.5.0">
    <wb-module deploy-name="inkfish-web">
        <wb-resource deploy-path="/" source-path="/target/m2e-wtp/web-resources"/>
        <wb-resource deploy-path="/" source-path="/src/main/webapp" tag="defaultRootSource"/>
        <wb-resource deploy-path="/WEB-INF/classes" source-path="/src/main/java"/>
        <wb-resource deploy-path="/WEB-INF/classes" source-path="/src/main/resources"/>
        <property name="context-root" value="inkfish-web"/>
        <property name="java-output-path" value="/inkfish-web/target/classes"/>
    </wb-module>
</project-modules>
```



在eclipse中修改项目的Deployment Assembly会修改该文件

![image-20210127223021872](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210206113044.png)







### .jsdtscope(js开发工具的使用范围)

#### jsdt (JavaScript Development Tools scope)

```
.jsdtscope规定了 JS 开发工具的范围。许多 web 项目的配置内容是一样的。可直接拷贝其他web项目中的文件，无需更改。

典型内容
<?xml version="1.0" encoding="UTF-8"?>
<classpath>
    <classpathentry excluding="**/*.min.js|**/bower_components/*|**/custom/*|**/node_modules/*|**/target/**|**/vendor/*" kind="src" path="src/main/webapp"/>
    <classpathentry kind="con" path="org.eclipse.wst.jsdt.launching.JRE_CONTAINER"/>
    <classpathentry kind="con" path="org.eclipse.wst.jsdt.launching.WebProject">
        <attributes>
            <attribute name="hide" value="true"/>
        </attributes>
    </classpathentry>
    <classpathentry kind="con" path="org.eclipse.wst.jsdt.launching.baseBrowserLibrary"/>
    <classpathentry kind="output" path=""/>
</classpath>


配置JS的例外（一般用于让Eclipse不校验第三方JS文件，避免开启JS校验后Eclipse假死）
在.jsdtscope文件的<classpathentry kind="src" path="src/main/webapp"/>增加excluding属性，例如
<classpathentry excluding="**/*.min.js|**/bower_components/*|**/custom/*|**/node_modules/*|**/target/**|**/vendor/*" kind="src" path="src/main/webapp"/>
```



### org.eclipse.wst.jsdt.ui.superType.container(jsdt的一些配置)



```

典型内容
org.eclipse.wst.jsdt.launching.baseBrowserLibrary
或者也见过
org.eclipse.wst.jsdt.launching.JRE_CONTAINER


By default, plain javascript files (.js) inherit members from object Global. HTML files contained in a static/dynamic web project inherit members from object Window.
So if your context is a plain javaScript file it will appear that only Window.window or Window.alert(..) is valid since none of the Window members are inherited. What you really want is to inherit this field + method from an instance of the Window object.

The JSDT supports a configurable super type at the project level. Each .js or .html file within a project inherits all the fields and methods from the projects super type. By default the type is Global for standalone JavaScript projects and Window for Static/Dynamic Web Projects.


You can change the super type for a project from the JavaScript Include Path properties page from Object Global to Object Window to achieve the results you desire... On the Global Order/SuperType page change the Super Type to Window in the ECMA library.


```



### org.eclipse.wst.jsdt.ui.superType.name(jsdt的一些配置)

```
典型内容
Window
或者也见过
Global
```





### org.eclipse.m2e.core.prefs（maven的一些配置）

```
org.eclipse.m2e.core.prefs是一些maven相关的配置。

典型内容
eclipse.preferences.version=1
activeProfiles=dev
resolveWorkspaceProjects=true
version=1


```


一般在Maven项目开发时和生产环境中配置不一样，可以在pom.xml中指定不同的profile来实现，Eclipse项目开发时指定profile的话（比如指定名叫dev的profile），就可以配置这个文件的activeProfiles属性。如果在界面中配置，在这里：

![image-20210127223021872](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210206113055.png)