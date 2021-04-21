# springboot版本水果商场

## 1.idea新建springboot项目，使用快速模板生成方式



### pom.xml(maven项目构建配置文件，依赖配置)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.4.4</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>edu.njau</groupId>
    <artifactId>fruitshop</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>fruitshop</name>
    <description>Demo project for Spring Boot</description>

    <properties>
        <java.version>1.8</java.version>
        <pageHelper.version>4.1.6</pageHelper.version>
        <jsqlparser.version>1.0</jsqlparser.version>
    </properties>



    <dependencies>

        <!-- mybatis依赖，加入了该依赖需要在application配置文件中配置数据源-->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.4</version>
        </dependency>
        <!-- jdbc驱动依赖-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>

        <!-- 分页插件依赖 start-->
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper</artifactId>
            <version>${pageHelper.version}</version>
        </dependency>

        <dependency>
            <groupId>com.github.jsqlparser</groupId>
            <artifactId>jsqlparser</artifactId>
            <version>${jsqlparser.version}</version>
        </dependency>
        <!-- 分页插件依赖 end-->



        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>

```

### application.properties(springboot框架配置文件)

```properties

#web项目路径配置
server.servlet.context-path=/netclass
#静态路径资源配置
#spring.web.resources.static-locations=classpath:static/,classpath:/static
#file:/E:/netclassspace/myspringbootnetclass/static

#file:${user.dir}/static 在IDEA开发测试模式时，得到的地址为：{project所在文件夹}/static
#file:${user.dir}/static 在打包成jar正式发布时，得到的地址为：{发布jar包所在目录}/static/
spring.web.resources.static-locations=file:${user.dir}/static,classpath:/static






#mybatis配置 start
# 数据源配置
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
# 低版本的springboot需要配置serverTimezone=UTC否则会报错，从springboot2.3.9开始修改该问题
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/fruitshop?characterEncoding=utf-8&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=root

# 配置别名
mybatis.type-aliases-package=edu.njau.fruitshop.entity
# 注册mapper
mybatis.mapper-locations=classpath:mapper/*.xml
# mybatis的配置文件路径，一般进行分页插件的配置
mybatis.config-location=classpath:mybatis-config.xml
#mybatis配置 end


```

### mybatis-config.xml(mybatis框架配置文件)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>

    <plugins>
        <!-- 配置mybatis分页插件 -->
        <!-- com.github.pagehelper为PageHelper类所在包名 -->
        <plugin interceptor="com.github.pagehelper.PageHelper">
            <!-- 4.0.0以后版本可以不设置该参数 -->
            <property name="dialect" value="mysql"/>
            <!-- 该参数默认为false -->
            <!-- 设置为true时，会将RowBounds第一个参数offset当成pageNum页码使用 -->
            <!-- 和startPage中的pageNum效果一样-->
            <property name="offsetAsPageNum" value="true"/>
            <!-- 该参数默认为false -->
            <!-- 设置为true时，使用RowBounds分页会进行count查询 -->
            <property name="rowBoundsWithCount" value="true"/>
            <!-- 设置为true时，如果pageSize=0或者RowBounds.limit = 0就会查询出全部的结果 -->
            <!-- （相当于没有执行分页查询，但是返回结果仍然是Page类型）-->
            <property name="pageSizeZero" value="true"/>
            <!-- 3.3.0版本可用 - 分页参数合理化，默认false禁用 -->
            <!-- 启用合理化时，如果pageNum<1会查询第一页，如果pageNum>pages会查询最后一页 -->
            <!-- 禁用合理化时，如果pageNum<1或pageNum>pages会返回空数据 -->
            <property name="reasonable" value="false"/>
            <!-- 3.5.0版本可用 - 为了支持startPage(Object params)方法 -->
            <!-- 增加了一个`params`参数来配置参数映射，用于从Map或ServletRequest中取值 -->
            <!-- 可以配置pageNum,pageSize,count,pageSizeZero,reasonable,orderBy,不配置映射的用默认值 -->
            <!-- 不理解该含义的前提下，不要随便复制该配置 -->
            <property name="params" value="pageNum=pageHelperStart;pageSize=pageHelperRows;"/>
            <!-- 支持通过Mapper接口参数来传递分页参数 -->
            <property name="supportMethodsArguments" value="false"/>
            <!-- always总是返回PageInfo类型,check检查返回类型是否为PageInfo,none返回Page -->
            <property name="returnPageInfo" value="none"/>
        </plugin>
    </plugins>
</configuration>
```





### 主程序配置mybatis注解

```java
package edu.njau.fruitshop;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
// @MapperScan做的是扫描MyBatis接口
// 相当于Spring中的MapperScannerConfigurer配置
// 该注解的值是包名
@MapperScan("edu.njau.fruitshop.dao")
@SpringBootApplication
public class FruitshopApplication {

    public static void main(String[] args) {
        SpringApplication.run(FruitshopApplication.class, args);
    }

}

```







## 2.使用前端easyui框架进行页面制作

### 1.easyui的datagrid和后端mybatis分页配合展示数据

