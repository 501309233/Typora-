[TOC]

# 一、简介

Spring Boot来简化Spring应用开发，约定大于配置，去繁从简，just run就能创建一个独立的，产品级别的应用

* 背景

J2EE笨重的开发、繁多的配置、低下的开发效率、复杂的部署流程、第三方技术集成难度大

* 解决

“Spring全家桶”时代

Spring Boot→J2EE一站式解决方案

Spring Cloud→分布式整体解决方案

* 优点
  * 快速创建独立运行的Spring项目以及与主流框架集成
  * 使用嵌入式的Servlet容器，应用无需打成WAR包
  * starters自动依赖于版本控制
  * 大量的自动配置，简化开发，也可修改默认值
  * 无需配置XML，无代码生成，开箱即用
  * 准生产环境的运行时应用监控
  * 与云计算的天然集成

# 二、微服务

2014年，Martin Fowler提出

微服务：架构风格

一个应用应该是一组小型服务；可以通过HTTP的方式进行互通；

每一个功能元素最终都是一个可独立替换和独立升级的软件单元



# 三、环境约束

* Jdk1.8
* maven3.x
* Intellij IDEA 
* Spring Boot 1.5.9
* Spring框架的使用经验
* 熟练使用Maven进行项目构建和依赖管理
* 熟练使用Eclipse或者IDEA

谷粒学院

# 四、Spring Boot HelloWorld

一个功能：

​	浏览器发送hello请求，服务器接受请求并处理，响应HelloWorld字符串；

## 1. 配置maven

### 配置镜像

~~~xml
<mirror>
        <id>alimaven</id>
        <name>aliyun maven</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
        <mirrorOf>central</mirrorOf> 
      </mirror>
~~~

### 配置jdk版本

~~~xml
<profile>
      <id>jdk-1.8</id>

      <activation>
        <activeByDefault>true</activeByDefault>
        <jdk>1.8</jdk>
      </activation>
      <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
      </properties>
    </profile>
~~~

## 2、创建maven项目，导入依赖

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.0.0.RELEASE</version>
</parent>

<dependencies>
    <dependency>
        <groupId> org.springframework.boot </groupId>
        <artifactId> spring-boot-starter-web </artifactId>
    </dependency>
</dependencies>

<!-- Package as an executable jar -->
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

## 3、创建一个主程序来启动SpringBoot

```Java
/**
 * @SpringBootApplication 来标注一个主程序类，用来说明这是一个SpringBoot应用
 */
@SpringBootApplication
public class HelloWorldApplication {
    public static void main(String[] args) {
        //SpringBoot应用启动方法
        SpringApplication.run(HelloWorldApplication.class,args);
    }
}
```

## 4、创建一个controller来处理请求

```java
@Controller
public class HelloController {

    @RequestMapping("/hello")
    @ResponseBody
    public String hello(){
        return "Hello World ";
    }
}
```

## 5、启动主方法来把应用启动起来

进行测试

## 6、将项目打包放在服务器上面运行

在maven中

```xml
<!-- Package as an executable jar -->
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

运行这个插件，得到一个可执行的jar包

将这个jar包放在服务器上（服务器有Java1.8的运行环境）

然后用命令java -jar jar包名启动这个工程，就可以了

# 五、探究SpringBoot

## 1、pom文件

### 父项目

maven中有一个父项目

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.0.0.RELEASE</version>
</parent>
```

这个父项目中还有一个父项目

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.0.0.RELEASE</version>
    <relativePath>../../spring-boot-dependencies</relativePath>
</parent>
```

这个项目真正管理着Spring Boot应用中所有的依赖的版本

Spring Boot的版本仲裁中心

以后导入依赖默认是不需要写版本号的（没有在dependencies中依赖的还是要声明版本号）

### 导入的依赖

```xml
<dependency>
        <groupId> org.springframework.boot </groupId>
        <artifactId> spring-boot-starter-web </artifactId>
</dependency>
```

**spring-boot-starter**-web:

​	spring-boot-starter:spring-boot场景启动器；帮我们导入了web模块正常运行所依赖的组件

Spring Boot将所有的功能场景都抽取出来，做成一个个的starters（启动器），只需要在项目里面引入这些starter相关场景的所有依赖都会导入进来。要用什么功能就导入什么场景的启动器

## 2、主程序类，主入口类

```java
/**
 * @SpringBootApplication 来标注一个主程序类，用来说明这是一个SpringBoot应用
 */
@SpringBootApplication
public class HelloWorldApplication {
    public static void main(String[] args) {
        //SpringBoot应用启动方法
        SpringApplication.run(HelloWorldApplication.class,args);
    }
}
```

**@SpringBootApplication**:

​	Spring Boot 应用标注主某个类上说明这个类是SpringBoot的主配置类，SpringBoot就应该运行这个类的main方法来启动SpringBoot应用

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
```

**@SpringBootConfiguration**:Spring Boot的配置类：

​		标注在某个类上，表示这是一个Spring Boot的配置类；

​		**@Configuration**：配置类上来标注这个注解；

​				配置类----配置文件；配置类也是容器中的一个组件

**@EnableAutoConfiguration**：开启自动配置功能；

​		以前我们需要配置的东西，Spring Boot帮我们自动配置；@EnableAutoConfiguration告诉Spring Boot开启自动配置功能；这样自动配置才能生效；

```Java
@AutoConfigurationPackage
@Import({AutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {
```

​		**@AutoConfigurationPackage**:自动配置包

​				@**Import**({AutoConfigurationPackages.Registrar.class});

​				Spring的底层注解@Import，给容器中导入一个组件；导入的组件由AutoConfigurationPackages.Registrar.class；

==将主配置类(@SpringBootApplication标注的类)的所在包及下面所有子包里面的所有组件扫描到Spring容器中 ；==

```java
@Import({EnableAutoConfigurationImportSelector.class})
```

​			给容器中导入组件

​			EnableAutoConfigurationImportSelector；导入哪些组件

​			将所有需要导入的组件以全类名的方式返回，这些组件就会被添加到容器中

​			会给容器中导入非常多的自动配置类（xxxAutoConfiguration）,就是给这个容器中导入这个场景所需要的所有组件，并配置好这些组件；

有了自动配置类，免去了我们手动配置注入功能组件等工作；

​			SpringFactoriesLoader.loadFactoryNames(EnableAutoConfiguration.class,classLoader);

==SpringBoot在启动的时候从类路径下的META-INF/spring.factories中获取EnableAutoConfiguration指定的值，将这些值作为自动配置类导入到容器中，自动配置类就生效，帮我们进行自动配置工作；以前需要我们自己配置的东西，自动配置类都帮我们配置好了；==

​			J2EE的整体整合解决方案和自动配置都在spring-boot-autoconfigure-1.5.9.RELEASE.jar



# 六、使用Spring Initializer快速创建 SpringBoot项目

IDE都支持使用Spring的项目创建向导来快速创建项目

选择我们需要的模块，向导会联网创建SpringBoot项目；

默认生成的SpringBoot项目：

* 主程序已经生成好了，我们只需要我们自己的逻辑
* resources文件夹中的目录结构
  * static：保存所有的静态资源；js css images...
  * Templates:保存所有的模板页面；（SpringBoot默认jar包使用嵌入式的Tomcat，默认不支持jsp页面）可以使用模板引擎（freemarker、thymeleaf）
  * application.properties:SpringBoot应用的配置文件；可以修改一些默认设置

SpringBoot在启动的时候从类路径下的META-INF/spring

.factories中获取EnableAutoConfiguration指定的值，将这些值