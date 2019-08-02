[TOC]

#HttpBasic 方式的权限实现

##编写springmvc的视图解析器。扫描controller以及开启注解

~~~xml
<?xmlversion="1.0"encoding="UTF-8"?>
<beansxmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:context="http://www.springframework.org/schema/context"
xmlns:mvc="http://www.springframework.org/schema/mvc"
xsi:schemaLocation="
http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd
http://www.springframework.org/schema/mvc
http://www.springframework.org/schema/mvc/spring-mvc.xsd">
<!--扫描Controller类-->
<context:component-scanbase-package="com.mofeng"/>
<!--注解方式处理器映射器和处理器适配器-->
<mvc:annotation-driven/>
<!--视图解析器-->
<beanclass="org.springframework.web.servlet.view.InternalResourceViewResolver">
<!--前缀-->
<propertyname="prefix"value="/WEB-INF/jsp/"/>
<!--后缀-->
<propertyname="suffix"value=".jsp"/>
</bean>
</beans>

~~~

## 编写Controller

~~~java
package com.mofeng.controller;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping("/product")
public class UserController {
    @RequestMapping("/index")
    public String index(){
        return "index";
    }
    /**
     * 商品添加
     */
    @RequestMapping("/add")
    public String add(){
        return "product/productAdd";
    }
    /**
     * 商品修改
     */
    @RequestMapping("/update")
    public String update(){
        return "product/productUpdate";
    }
    /**
     * 查询
     */
    @RequestMapping("/list")
    public String list(){
        return "product/productList";
    }
    /**
     * 商品删除
     */
    @RequestMapping("/delete")
    public String delete(){
        return "product/productDelete";
    }
}

~~~

## 编写springmvc

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <!--扫描Controller类-->
    <context:component-scan base-package="com.mofeng"/>
    <!--注解方式处理器映射器和处理器适配器-->
    <mvc:annotation-driven/>
    <!--视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!--前缀-->
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <!--后缀-->
        <property name="suffix" value=".jsp"/>
    </bean>
</beans>
~~~

## 补充spring-security.xml

**注意事项，spring-security.xml比需添加http和manager，项目才能顺利进行**

~~~xml
	

	<security:http>
        <security:http-basic/>
    </security:http>
    <security:authentication-manager>
    </security:authentication-manager>
~~~

###  \<security:http-basic/\>：使用HttpBasic方式进行登录

###\<security:http\>过滤器链配置的作用

==（在web.xml中已经引入了过滤器链）==

​		\<security:http\>spring过滤器链配置：

* 需要拦截什么资源

* 什么资源什么角色权限

* 定义认证方式：HttpBasic,FormLogin(*) 

* 定义登录页面，定义登录请求地址

### \<security:authentication-manager\>:认证管理器

* 认证信息提供方式（账户名，密码，当前用户权限）

### \<security:authentication-manager\>认证管理器

##写测试页面

**结构如下**

![1564709640540](E:\Typora笔记\spring_security\assets\1564709640540.png)

**运行项目，看有无报错**

## 继续写spring-security.xml的配置

在security:http中添加

```xml
 <security:intercept-url pattern="/**" access="isFullyAuthenticated()"/>
```

* pattern:需要拦截的资源
* access:拦截方式
  * isFullyAuthenticated()	需要认证才可以访问（有用户名、密码、身份就可以）
  * 待补充。。。。

==认证信息需要securityManager来提供==

在security:authentication-manager中添加

~~~XML
<!--这个格式是固定的-->
<security:authentication-manager>
        <security:authentication-provider>
            <security:user-service>
               
                <security:user name="admin" authorities="ROLE_USER" 								password="123456"/>
                
            </security:user-service>
        </security:authentication-provider>
    </security:authentication-manager>
~~~

**security:user**

* name→用户名
* password→密码
* authorities→身份（权限）

登录效果：

![1564713700087](E:\Typora笔记\spring_security\assets\1564713700087.png)