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

# 配置文件占位符

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

