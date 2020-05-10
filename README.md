# IDEA tomcat8.5.51源码剖析环境搭建





+ ### a    源码地址  
  
  Tomcat官网：http://tomcat.apache.org/
Tomcat各版本源码：http://archive.apache.org/dist/tomcat/
  
+ ### b  环境搭建

  - apache-tomcat-8.0.51-src 目录下新建pom.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <project xmlns="http://maven.apache.org/POM/4.0.0"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  
   <modelVersion>4.0.0</modelVersion>
   <groupId>org.apache.tomcat</groupId>
   <artifactId>Tomcat8.5.51</artifactId>
   <name>Tomcat8.5.51</name>
   <version>8.5.51</version>
  
   <build>
    <finalName>Tomcat8.5.51</finalName>
    <sourceDirectory>java</sourceDirectory>
    <testSourceDirectory>test</testSourceDirectory>
    <resources>
     <resource>
      <directory>java</directory>
     </resource>
    </resources>
    <testResources>
     <testResource>
      <directory>test</directory>
     </testResource>
    </testResources>
    <plugins>
     <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>2.3</version>
      <configuration>
       <encoding>UTF-8</encoding>
       <source>1.8</source>
       <target>1.8</target>
      </configuration>
     </plugin>
    </plugins>
   </build>
  
   <dependencies>
    <dependency>
     <groupId>junit</groupId>
     <artifactId>junit</artifactId>
     <version>4.12</version>
     <scope>test</scope>
    </dependency>
    <dependency>
     <groupId>org.easymock</groupId>
     <artifactId>easymock</artifactId>
     <version>3.4</version>
    </dependency>
    <dependency>
     <groupId>ant</groupId>
     <artifactId>ant</artifactId>
     <version>1.7.0</version>
    </dependency>
    <dependency>
     <groupId>wsdl4j</groupId>
     <artifactId>wsdl4j</artifactId>
     <version>1.6.2</version>
    </dependency>
    <dependency>
     <groupId>javax.xml</groupId>
     <artifactId>jaxrpc</artifactId>
     <version>1.1</version>
    </dependency>
    <dependency>
     <groupId>javax.xml.soap</groupId>
     <artifactId>javax.xml.soap-api</artifactId>
     <version>1.4.0</version>
    </dependency>
    <dependency>
     <groupId>org.eclipse.jdt.core.compiler</groupId>
     <artifactId>ecj</artifactId>
     <version>4.5.1</version>
    </dependency>
   </dependencies>
  </project>
  
  ```

  - apache-tomcat-8.0.51-src 目录下创建 catalina-home 目录  将apache-tomcat-8.0.51-src目录下的

    conf  、webapp 目录下移动到  catalina-home
    
- **Ctrl+N** 找到 org.apache.catalina.startup.Bootstrap  tomcat的启动类  然后启动main方法
  
  此时  tomcat运行报错  ，需要配置tomcat的运行环境


+ ### c  运行环境配置

  * 设置启动参数   Edit Configurations  启动类 VM options 增加配置 

    ```properties
    -Dcatalina.home=/home/zyp/IdeaProjects/tomcat8/apache-tomcat-8.0.51-src/catalina-home
    -Dcatalina.base=/home/zyp/IdeaProjects/tomcat8/apache-tomcat-8.0.51-src/catalina-home
    -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager
    -Djava.util.logging.conf=/home/zyp/IdeaProjects/tomcat8/apache-tomcat-8.0.51-src/catalina-home/conf/logging.properties
    ```

    之后运行  test包下有错误  ，打开project structure 快捷键：**ctrl+alt+shift+s**  将 test 包  unmack test source  或者删除 test包  

    之后运行 bootstrap  访问http://localhost:8080/   显示jsp错误

+ ### d   JasperInitializer 解析器初始化

    	**Ctrl+N**   打开 org.apache.catalina.startup.ContextConfig  类 **ctrl+g**  777 行   添加代码

  ​	 

  ```java
          context.addServletContainerInitializer(new JasperInitializer(),null);
  ```

  之后运行  访问 访问http://localhost:8080/   环境配置成功   但是启动日志看到 有些监听器没有找到，之后发现这些监听其在/webapps/examples/... web.xml中配置   对剖析源码无影响 删除即可

  

+ ### e   删除 webapps 包下的  examples
