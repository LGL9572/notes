# SpringBoot整合Mybatis

## 1.配置文件

###一、pom配置文件

>```xml
><!--加载mybatis整合springboot-->
>        <dependency>
>            <groupId>org.mybatis.spring.boot</groupId>
>            <artifactId>mybatis-spring-boot-starter</artifactId>
>            <version>1.3.1</version>
>        </dependency>
>        <!--MySql的jdbc驱动-->
>        <dependency>
>            <groupId>mysql</groupId>
>            <artifactId>mysql-connector-java</artifactId>
>        </dependency  
>```

### 二、核心配置文件application.properties

> ```properties
> #配置Mybatis的Mapper.xml文件的所在位置
> mybatis.mapper-locations=classpath:org/lgl/springboot/mapper/*.xml
> 
> #配置数据源
> spring.datasource.username=root
> spring.datasource.password=123456
> spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
> spring.datasource.url=jdbc:mysql://localhost:3306/test?userUnicode=true&characterEncoding=utf8&useSSL=false&serverTimezone=UTC
> ```

### 三、配置文件中存在问题

>```properties
>#配置Mybatis的Mapper.xml文件的所在位置时,不能是
>mybatis.mapper-locations=classpath:org.lgl.springboot.mapper.*.xml
>
>#用于执行DML脚本的数据库的用户名
>spring.datasource.data-username=root
>#执行DML脚本的数据库的密码
>spring.datasource.data-password=123456 
>
>#指定字符的编码、解码格式。
>?userUnicode=true&characterEncoding=utf8
>#MySQL在高版本需要指明是否进行SSL连接
>?useSSL=false 
>#时差问题
>?serverTimezone=UTC
>```

## 2.GeneratorMapper.xml代码自动生成器

>```xml
><!--GeneratorMapper.xml-->
><?xml version="1.0" encoding="UTF-8"?>
><!DOCTYPE generatorConfiguration
>        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
>        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
>
><generatorConfiguration>
>
>    <!-- 指定连接数据库的JDBC驱动包所在位置，指定到你本机的完整路径 -->
>    <classPathEntry location="D:/maven-repo/mysql/5.1.41/mysql-connector-java-5.1.41.jar"/>
>
>    <!-- 配置table表信息内容体，targetRuntime指定采用MyBatis3的版本 -->
>    <context id="tables" targetRuntime="MyBatis3">
>        <!--序列化-->
>        <plugin type="org.mybatis.generator.plugins.SerializablePlugin"/>
>
>        <!--以下需要插件  -->
>
>        <!--
>            插入成功后返回ID
>           <plugin type="cn.doity.common.generator.plugin.InsertAndReturnKeyPlugin"/>
>
>           分页查询功能
>           <plugin type="cn.doity.common.generator.plugin.SelectByPagePlugin"/>
>
>           生成带有for update后缀的select语句插件
>           <plugin type="cn.doity.common.generator.plugin.SelectForUpdatePlugin"/> -->
>
>
>        <!-- 抑制生成注释，由于生成的注释都是英文的，可以不让它生成 -->
>        <commentGenerator>
>            <property name="suppressAllComments" value="true" />
>        </commentGenerator>
>
>        <!-- 配置数据库连接信息 -->
>        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
>                        connectionURL="jdbc:mysql://localhost:3306/test"
>                        userId="root"
>                        password="123456">
>        </jdbcConnection>
>
>        <!-- 生成model类，targetPackage指定model类的包名， targetProject指定生成的model放在idea的哪个工程下面-->
>        <javaModelGenerator targetPackage="org.lgl.springboot.model" targetProject="src/main/java">
>            <property name="enableSubPackages" value="false" />
>            <property name="trimStrings" value="false" />
>        </javaModelGenerator>
>
>        <!-- 生成MyBatis的Mapper.xml文件，targetPackage指定mapper.xml文件的包名， targetProject指定生成的mapper.xml放在idea的哪个工程下面 -->
>        <sqlMapGenerator targetPackage="org.lgl.springboot.mapper" targetProject="src/main/java">
>            <property name="enableSubPackages" value="false" />
>        </sqlMapGenerator>
>
>        <!-- 生成MyBatis的Mapper接口类文件,targetPackage指定Mapper接口类的包名， targetProject指定生成的Mapper接口放在idea的哪个工程下面 -->
>        <javaClientGenerator type="XMLMAPPER" targetPackage="org.lgl.springboot.mapper" targetProject="src/main/java">
>            <property name="enableSubPackages" value="false" />
>        </javaClientGenerator>
>
>        <!-- 数据库表名及对应的Java模型类名 -->
>        <table tableName="user"
>               domainObjectName="User"
>               enableCountByExample="false"
>               enableUpdateByExample="false"
>               enableDeleteByExample="false"
>               enableSelectByExample="false"
>               selectByExampleQueryId="false"/>
>    </context>
></generatorConfiguration>
>```

> ```xml
> <build>
>         <plugins>
>             <!--mybatis代码自动生成插件-->
>             <plugin>
>                 <groupId>org.mybatis.generator</groupId>
>                 <artifactId>mybatis-generator-maven-plugin</artifactId>
>                 <version>1.3.6</version>
>                 <configuration>
>                     <!--配置文件的位置 -->
>                     <configurationFile>GeneratorMapper.xml</configurationFile>
>                     <verbose>true</verbose>
>                     <overwrite>true</overwrite>
>                 </configuration>
>             </plugin>
>         </plugins>
> </build>
> ```

## 3.idea中存在的问题

###一、可能存在.xml文件编译后找不到的问题

> ```xml
> 	<build>
> 		 <!--告诉SpringBoot编译后的位置-->
>         <resources>
>             <!--将src/main/java目录下的*.xml文件也编译到一个目录下-->
>             <resource>
>                 <directory>src/main/java</directory>
>                 <includes>
>                     <include>**/*.xml</include>
>                 </includes>
>             </resource>
>             <resource>
>                 <directory>src/main/resources</directory>
>                 <includes>
>                     <include>**/*.*</include>
>                 </includes>
>             </resource>
>             <!--src/main/WEB-INF目录下的所有资源，编译后都在META-INF/resources目录下-->
>             <resource>
>                 <directory>src/main/WEB-INF</directory>
>                 <targetPath>META-INF/resources</targetPath>
>                 <includes>
>                     <include>**/*.*</include>
>                 </includes>
>             </resource>
>         </resources>
>     </build>         
> ```

### 二、注解问题

>需要在生成的代码XXXMapper.java文件中加`@Mapper`
>
>或者在SpringbootWebApplication类名上加`@MapperScan(basePackages="org.lgl.springboot.mapper")`



