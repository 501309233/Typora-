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

##写测试页面

**结构如下**

![1564709640540](E:\Typora笔记\spring_security\assets\1564709640540.png)

**运行项目，看有无报错**

