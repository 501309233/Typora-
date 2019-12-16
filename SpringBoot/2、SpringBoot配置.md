[toc]

# 一、配置文件

* SpringBoot使用一个全局的配置文件，配置文件名是固定的
  * application.properties
  * application.yml

*  配置文件放在src/main/resources目录或者类路径/config下

* .yml是YAML(YAML Ain't Markup Language)语言的文件，以数据为中心，比json、xml等更适合做配置文件

  * http://www.yaml.org/参考语法规范

  * 标记语言：

    * 以前的配置文件；大多都使用的是xxx.xml文件
    * 配置例子

    ```yaml
    server:
    	port:8081
    ```

    Xml:

    ```xml
    <server>
    	<port>8081</port>
    </server>
    ```

    

* 全局配置文件可以对一些默认配置值进行修改

# 二、YAML语法

## 1、YAML基本语法

* 使用缩进表示层级关系
* 缩进时不允许使用Tab键，只允许使用空格
* 缩进的空格数目不重要，只要相同层级的元素左侧对齐即可
* 大小写敏感

~~~yaml
server:
  port: 8081
  path: /hello
~~~



## 2、YAML支持的三种数据结构

* 对象：键值对的集合(Map，属性和值，键值对)

  * k: v：对象还是k: v的方式，在下一行来写对象的属性和值，注意缩进

    ​	friends:

    ​			lastName: zhangsan

    ​			age: 20

    行内写法：

    ```yaml
    friends: {lastName: zhangsan,age: 18}
    ```

    

* 数组：一组按次序排列的值(list，set)

  用-值表示数组中的一个元素

  ```yaml
  pets:
   - cat
   - dog
   - panda
  ```

  行内写法：

  ```yaml
  pets: [cat,dog,pig]
  ```

  

* 字面量：单个的、不可再分的值(数字，字符串，布尔值)

  * k: v：字面直接来写；字符串默认不用加上单引号或者双引号；

  * ""：双引号；不会转义字符串里面的特殊字符；特殊字符会作为本身想表示的意思

    name: "zhangsan \n lise"：输出：zhangsan 换行 lisi

  * ''：单引号：会转义特殊字符，特殊字符最终只是一个普通的字符串数据

    name: 'zhangsan \n lisi'：输出：zhangsan \n lisi

# 三、配置文件值注入

## maven中的依赖

```xml
 <!--读取配置中的信息注入到类中-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-configuration-processor</artifactId>
        <optional>true</optional>
    </dependency>
</dependencies>
```

将配置文件中配置的每一个属性的值，映射到这个组件中

## 写配置文件

配置文件中的属性和值的规则参照要映射的类

* yaml版本

```yaml
person:
  name: 宋拓
  age: 23
  birthday: 1996/8/24
  dog:
    name: xiaoyellow
    age: 1
#注：大写字母可以写成- n比如lastName--->last-name
```

* properties版本

idea，properties配置文件utf-8

```properties
#配置person的值
person.age=18
person.name=张三
person.birthday=2019/12/10
person.boss=true
person.maps.k1=v1
person.maps.k2=v2
person.lists=a,b,c
person.dog.name=dog
person.dog.age=10
```

## 在类中添加注解

```Java
@Component
//默认从全局配置文件（application.properties或application.yml）中获取值
@ConfigurationProperties(prefix = "person")
public class Person {
    private String name;
    private int age;
    private Date birthday;
    private Dog dog;

```

@ConfigurationProperties：告诉SpringBoot将本类中的所有属性和配置文件中相关的配置进行绑定

prefix = "person" , 配置文件中哪个下面的所有属性进行一一映射

只有这个组件是容器中的组件，才能用容器提供的@ConfigurationProperties功能，所以要加@Component

##SpringBootTest单元测试

在项目目录的test目录中找

可以在测试期间很方便的类似编码一样进行自动注入

```Java
@SpringBootTest
class HelloworldApplicationTests {
    @Autowired
    Person person;
  
    @Test
    void contextLoads() {
        System.out.println(person);
    }
}
```

## @Value 和@ConfigurationProperties获取值比较

|                      | @ConfigurationProperties | @Value     |
| -------------------- | ------------------------ | ---------- |
| 功能                 | 批量注入配置文件中的属性 | 一个个指定 |
| 松散绑定（松散语法） | 支持                     | 不支持     |
| SpEL                 | 不支持                   | 支持       |
| JSR303数据校验       | 支持                     | 不支持     |
| 复杂类型封装         | 支持                     | 不支持     |

配置文件yml还是properties他们都能获取到值

如果我们只是在某个业务逻辑中需要获取一下配置文件中的某项值，使用@Value

如果我们专门编写了一个javabean来和配置文件进行映射，我们就直接使用@ConfigurationProperties

### 属性名匹配规则（松散绑定-Relaxed binding）

* person.firstName : 使用标准方式
* person.first-name : 大写用-
* person.first_name : 大写用_
* PERSON_FIRST_NAME : 推荐系统属性使用这种写法



### JSR303数据校验

例子：

```java
@Component
@ConfigurationProperties(prefix = "person")
@Validated
public class Person {
    @Email
    private String Email;
  //可以校验Email是否是邮箱格式
```

### @PropertySource

```java
@PropertySource(value = {"classpath:person.properties"})
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
```

加载指定的配置文件（默认为主配置文件application）

### @ImportResource

导入Spring配置文件，让配置文件里面的内容生效

SpringBoot里面没有Spring的配置文件，我们自己编写的配置文件，也不能自动识别

想让Spring的配置文件生效，加载进来；需要@ImportResource

 ~~~Java
@ImportResource(locations = {"classpath:beans.xml"})
导入Spring的配置文件让其生效
 ~~~



SpringBoot推荐给容器中添加组件的方式：

1、配置类==Spring配置文件

@Configuration:指明当前类是一个配置类；就是来替代之前的Spring配置文件

2、使用bean标签给容器中添加组件

```Java
/**
 * @Configuration:指明当前类是一个配置类；就是来替代之前的Spring配置文件
 *
 * 在配置文件中用<bean></bean>标签添加组件
 */
@Configuration
public class MyAppConfig {
    //将方法的返回值添加到容器中，容器中这个组件默认的ID就是方法名
    @Bean
    public HelloService helloService(){
        return new HelloService();
    }
}
```

# 四、配置文件占位符

## 1、随机数

\${random.value},​\${random.int},\${random.long},

\${random.int(10)},\${random.int[1024,65536]}

## 2、占位符获取之前配置的值，如果没有，可以使用:指定默认值

可以在配置文件中引用前面配置过的属性（优先级前面配置过的这里都可以用）

${app.name:默认值}来指定找不到属性时的默认值

~~~properties
person.lastName=张三${random.uuid}
person.age=${random.int}
person.dog.name=${person.hello:hello}_dog
~~~

person.dog.name会输出：hello_dog

# 五、profile

Profile是Spring对不同环境提供不同配置功能的支持，可以通过激活、指定参数等方式快速切换环境

## 1、多profile文件形式：

* 格式：application-{profile}.properties:
  * application-dev.properties、application-prod.properties

我们在主配置文件编写的时候，文件名可以是application-{profile}.properties(yml)

默认使用application.properties的配置；

## 2、多profile文档块模式(yml)：

```yaml
spring:
  profiles:
    active: prod
server:
  port: 8081
---
server: 
  port: 8082
spring:
  profiles: dev
---
server:
  port: 8083
spring:
  profiles: prod	#指定属于哪个环境
```

## 3、激活方式：

* 命令行  --spring.profiles.active=dev
* 配置文件(application.properties)中指定 spring.profiles.active=dev
* jvm参数 -Dspring.profiles.active=dev

# 六、配置文件加载位置

SpringBoot启动会扫描一下位置的application.properties或者application.yml文件作为SpringBoot的默认配置文件

* File:./config/
* File:./
* classpath:/config/
* classpath:/
* 以上是按照优先级从高到低的顺序，所有位置的文件都会被加载，**高优先级配置内容会覆盖低优先级配置内容**，还可以高优先级和低优先级互补配置
* 我们也可以通过配置spring.config.location来改变默认配置：

项目打包好以后，我们可以使用命令行参数的形式，启动项目的时候来指定配置文件的新位置；指定配置文件和默认加载的这些配置文件共同起作用形成互补配置



# 七、外部配置加载顺序

SpringBoot支持多种外部配置方式

这些方式优先级从高到低：

1. **命令行参数**

   多个配置参数用空格分开；--配置项=值 

2. 来自java:comp/env的JNDI属性

3. Java系统属性（System.getProperties()）

4. 操作系统环境变量

5. RandomValuePropertySource配置的random.*属性值

6. **jar包外部的application-{profile}.properties或application.yml(带spring.profile)配置文件**

7. **jar包内部的application-{profile}.properties或application.yml(带spring.profile)配置文件**

8. **jar包外部的application.properties或application.yml(不带spring.profile)配置文件**

9. **jar包内部的application.properties或application.yml(不带spring.profile)配置文件**

10. @Configuration注解类上的@PropertySource

11. 通过SpringApplication.setDefaultProperties指定的默认属性

==高优先级的配置覆盖低优先级的配置，所有的配置会形成互补配置==

==优先加载带profile；由jar包外向jar包内进行寻找==

# 八、自动配置的原理

1、可以查看HttpEncodingAutoConfiguration



2、通用模式

* xxxAutoConfiguration:自动配置类
* xxxProperties:属性配置类
* yml/properties文件中能配置的值就来源于[属性配置类]

3、几个重要的注解

* @Bean
* @Conditional

4、--debug查看详细的自动配置报告

# 八、自动配置原理

配置文件到低能写什么？怎么写？自动配置原理：

配置文件能配置的属性：

https://docs.spring.io/spring-boot/docs/2.2.2.RELEASE/reference/pdf/spring-boot-reference.pdf

10. Appendices



自动配置原理：

1）SpringBoot启动的时候加载主配置类，开启了自动配置功能@EnableAutoConfiguration

2）@EnableAutoConfiguration作用：

* 利用EnableAutoConfigurationImportSelector给容器中导入一些组件

* 可以查看selectImports()方法的内容；

* List<String>configurations = getCandidateConfigurations(annotationMetadata,attributes);获取候选的配置

  * ```java
    SpringFactoriesLoader.loadFactoryNames()
    扫描所有jar包类路径下META-INF/spring.factories
    把扫描到的这些文件的内容包装成properties对象
    从properties中获取到EnableAutoConfiguration.class类（类名）对应的值，然后把他们添加到容器中
    ```

    将类路径下META-INF/spring.factories 里面配置的所有EnableAutoConfiguration的值加入到了容器中；

    ```properties
    # PropertySource Loaders
    org.springframework.boot.env.PropertySourceLoader=\
    org.springframework.boot.env.PropertiesPropertySourceLoader,\
    org.springframework.boot.env.YamlPropertySourceLoader
    
    # Run Listeners
    org.springframework.boot.SpringApplicationRunListener=\
    org.springframework.boot.context.event.EventPublishingRunListener
    
    # Error Reporters
    org.springframework.boot.SpringBootExceptionReporter=\
    org.springframework.boot.diagnostics.FailureAnalyzers
    
    # Application Context Initializers
    org.springframework.context.ApplicationContextInitializer=\
    org.springframework.boot.context.ConfigurationWarningsApplicationContextInitializer,\
    org.springframework.boot.context.ContextIdApplicationContextInitializer,\
    org.springframework.boot.context.config.DelegatingApplicationContextInitializer,\
    org.springframework.boot.rsocket.context.RSocketPortInfoApplicationContextInitializer,\
    org.springframework.boot.web.context.ServerPortInfoApplicationContextInitializer
    
    # Application Listeners
    org.springframework.context.ApplicationListener=\
    org.springframework.boot.ClearCachesApplicationListener,\
    org.springframework.boot.builder.ParentContextCloserApplicationListener,\
    org.springframework.boot.cloud.CloudFoundryVcapEnvironmentPostProcessor,\
    org.springframework.boot.context.FileEncodingApplicationListener,\
    org.springframework.boot.context.config.AnsiOutputApplicationListener,\
    org.springframework.boot.context.config.ConfigFileApplicationListener,\
    org.springframework.boot.context.config.DelegatingApplicationListener,\
    org.springframework.boot.context.logging.ClasspathLoggingApplicationListener,\
    org.springframework.boot.context.logging.LoggingApplicationListener,\
    org.springframework.boot.liquibase.LiquibaseServiceLocatorApplicationListener
    
    # Environment Post Processors
    org.springframework.boot.env.EnvironmentPostProcessor=\
    org.springframework.boot.cloud.CloudFoundryVcapEnvironmentPostProcessor,\
    org.springframework.boot.env.SpringApplicationJsonEnvironmentPostProcessor,\
    org.springframework.boot.env.SystemEnvironmentPropertySourceEnvironmentPostProcessor,\
    org.springframework.boot.reactor.DebugAgentEnvironmentPostProcessor
    
    # Failure Analyzers
    org.springframework.boot.diagnostics.FailureAnalyzer=\
    org.springframework.boot.diagnostics.analyzer.BeanCurrentlyInCreationFailureAnalyzer,\
    org.springframework.boot.diagnostics.analyzer.BeanDefinitionOverrideFailureAnalyzer,\
    org.springframework.boot.diagnostics.analyzer.BeanNotOfRequiredTypeFailureAnalyzer,\
    org.springframework.boot.diagnostics.analyzer.BindFailureAnalyzer,\
    org.springframework.boot.diagnostics.analyzer.BindValidationFailureAnalyzer,\
    org.springframework.boot.diagnostics.analyzer.UnboundConfigurationPropertyFailureAnalyzer,\
    org.springframework.boot.diagnostics.analyzer.ConnectorStartFailureAnalyzer,\
    org.springframework.boot.diagnostics.analyzer.NoSuchMethodFailureAnalyzer,\
    org.springframework.boot.diagnostics.analyzer.NoUniqueBeanDefinitionFailureAnalyzer,\
    org.springframework.boot.diagnostics.analyzer.PortInUseFailureAnalyzer,\
    org.springframework.boot.diagnostics.analyzer.ValidationExceptionFailureAnalyzer,\
    org.springframework.boot.diagnostics.analyzer.InvalidConfigurationPropertyNameFailureAnalyzer,\
    org.springframework.boot.diagnostics.analyzer.InvalidConfigurationPropertyValueFailureAnalyzer
    
    # FailureAnalysisReporters
    org.springframework.boot.diagnostics.FailureAnalysisReporter=\
    org.springframework.boot.diagnostics.LoggingFailureAnalysisReporter
    
    ```

    每一个这样的xxxAutoConfiguration类都是容器中的一个组件，都加入到容器中；用他们来做自动配置

3）每一个自动配置类进行自动配置功能；

4）以**HttpEncodingAutoConfiguration**（Http编码自动配置）为例解释自动配置原理

```java
@Configuration(		//表示这是一个配置类，以前编写的配置文件一样，也可以给容器中添加组件
    proxyBeanMethods = false
)
//启动ConfigurationProperties功能，将配置文件中对应的值和HttpEncodingProperties绑定起来；并把HttpEncodingProperties加入到ioc容器中
@EnableConfigurationProperties({HttpProperties.class})
//Spring底层@Conditional注解，根据不同的条件，如果满足指定的条件，，整个配置类里面的配置就会生效；判断当前应用是否是web应用，如果是，当前配置类生效
@ConditionalOnWebApplication(
    type = Type.SERVLET
)
//判断当前项目有没有这个类；CharacterEncodingFilter；SpringMVC中进行乱码解决的过滤器
@ConditionalOnClass({CharacterEncodingFilter.class})
//判断配置文件中是否存在某个配置 spring.http.encoding.enabled；如果不存在，判断也是成立的
//即使我们配置文件中不配置spring.http.encoding.enabled=true，也是默认生效的
@ConditionalOnProperty(
    prefix = "spring.http.encoding",
    value = {"enabled"},
    matchIfMissing = true
)
public class HttpEncodingAutoConfiguration {
  //它已经和SpringBoot的配置文件映射了
   private final Encoding properties;

  //只有一个有参构造器的情况下，参数的值就从容器中拿
    public HttpEncodingAutoConfiguration(HttpProperties properties) {
        this.properties = properties.getEncoding();
    }

  //给容器中添加组件，这个组件中的某些值要从properties中获取
    @Bean
    @ConditionalOnMissingBean
    public CharacterEncodingFilter characterEncodingFilter() {
        CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();
        filter.setEncoding(this.properties.getCharset().name());
        filter.setForceRequestEncoding(this.properties.shouldForce(org.springframework.boot.autoconfigure.http.HttpProperties.Encoding.Type.REQUEST));
        filter.setForceResponseEncoding(this.properties.shouldForce(org.springframework.boot.autoconfigure.http.HttpProperties.Encoding.Type.RESPONSE));
        return filter;
    }

```

根据当前不同的条件判断，决定这个类是否生效

一旦这个配置类生效；这个配置类就会给容器中添加各种组件；这些组件的属性是从对应的properties类中获取的，这些类里面的每一个属性又是和配置文件绑定的



5）在配置文件中能配置的属性都是在xxxProperties类中封装着；配置文件能配置什么就可以参照某个功能对应的这个属性类

```java
//从配置文件中获取指定的值和bean的属性进行绑定
@ConfigurationProperties(
    prefix = "spring.http"
)
public class HttpProperties {
    private boolean logRequestDetails;
    private final HttpProperties.Encoding encoding = new HttpProperties.Encoding();
```

```properties
#我们能配置的属性都是来源于这个功能的properties
spring.http.encoding.enabled=true
spring.http.encoding.charset=utf-8
spring.http.encoding.force=ture
```



## **精髓：**

* **SpringBoot启动会加载大量的自动配置类**
* **我们看我们需要的功能有没有SpringBoot默认写好的自动配置类**
* **我们再来看这个自动配置类中到底配置了哪些组件；（只要我们要用的组件由，我们就不需要再来配置了）**
* **给容器中自动配置类添加组件的时候，会从properties类中获取某些属性，我们就可以在配置文件中指定这些属性的值**

xxxAutoConfiguration；自动配置类；它给容器中添加组件

xxxProperties：封装配置文件中相关属性；

## 细节：

### @Conditional

作用：必须是@Conditional指定的条件成立，才给容器中添加组件，配置里面的所有内容才生效

| @Conditional扩展注解            | 作用（判断是否满足当前指定条件）                 |
| ------------------------------- | ------------------------------------------------ |
| @ConditionalOnJava              | 系统的Java版本是否符合要求                       |
| @ConditionalOnBean              | 容器中存在指定Bean                               |
| @ConditionalOnMissiongBean      | 容器中不存在指定Bean                             |
| @ConditionalOnExpression        | 满足SpEL表达式指定                               |
| @ConditionalOnClass             | 系统中有指定的类                                 |
| @ConditionalOnMissiongClass     | 系统中没有指定的类                               |
| @ConditionalOnSingleCandidate   | 容器中只有一个指定的Bean，或者这个Bean是首选Bean |
| @ConditionalOnProperty          | 系统中指定的属性是否有指定的值                   |
| @ConditionalOnResource          | 类路径下是否存在指定资源文件                     |
| @ConditionalOnWebApplication    | 当前是web环境                                    |
| @ConditionalOnNotWebApplication | 当前不是web环境                                  |
| @ConditionalOnJndi              | JNDI存在指定向                                   |

**自动配置类必须在一定的条件下才能生效**

在配置文件中写

```properties
#开启SpringBoot的debug
debug=true
```

控制台会打印自动配置报告，这样我们就会很方便的知道那些自动配置类生效

```java
============================
CONDITIONS EVALUATION REPORT
============================
  Positive matches:
-----------------
//启用的
   AopAutoConfiguration matched:
      - @ConditionalOnProperty (spring.aop.auto=true) matched (OnPropertyCondition)
        
  Negative matches:
-----------------
//没有启用的
   ActiveMQAutoConfiguration:
      Did not match:
         - @ConditionalOnClass did not find required class 'javax.jms.ConnectionFactory' (OnClassCondition)

   AopAutoConfiguration.AspectJAutoProxyingConfiguration:
      Did not match:
         - @ConditionalOnClass did not find required class 'org.aspectj.weaver.Advice' (OnClassCondition)


```

