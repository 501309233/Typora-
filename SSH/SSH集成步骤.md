1. 创建项目

2. pom.xml编写，把相关依赖写入

3. web.xml

   ```xml
   <web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
            id="WebApp_ID" version="3.1">
     <display-name>user-manager</display-name>
     <welcome-file-list>
       <welcome-file>index.jsp</welcome-file>
     </welcome-file-list>
   
       <!--加载Spring的配置文件到上下文中去 -->
       <context-param>
           <param-name>contextConfigLocation</param-name>
           <param-value>
               classpath*:/applicationContext.xml
           </param-value>
       </context-param>
   
       <!-- Spring监听器 -->
       <listener>
           <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
       </listener>
   
     <!-- 字符集过滤 -->
     <filter>
       <filter-name>encodingFilter</filter-name>
       <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
       <init-param>
         <param-name>encoding</param-name>
         <param-value>UTF-8</param-value>
       </init-param>
       <init-param>
         <param-name>forceEncoding</param-name>
         <param-value>true</param-value>
       </init-param>
     </filter>
     <filter-mapping>
       <filter-name>encodingFilter</filter-name>
       <url-pattern>/*</url-pattern>
     </filter-mapping>
   
     <!-- 以下是 Spring MVC 的相关配置 -->
     <servlet>
       <servlet-name>spring</servlet-name>
       <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
       <init-param>
         <param-name>contextConfigLocation</param-name>
         <!-- 此处指向的的是SpringMVC的配置文件 -->
         <param-value>classpath*:/spring-mvc.xml</param-value>
       </init-param>
       <!--配置容器在启动的时候就加载这个servlet并实例化 -->
       <load-on-startup>1</load-on-startup>
     </servlet>
     <servlet-mapping>
       <servlet-name>spring</servlet-name>
       <url-pattern>/</url-pattern>
     </servlet-mapping>
     <!-- Spring MVC 的配置结束 -->
   
   </web-app>
   
   ```

4. springmvc的配置文件

   ```xml
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
       <!-- 自动扫描且只扫描 @Controller -->
       <context:component-scan base-package="online.shixun"
                               use-default-filters="false">
           <context:include-filter type="annotation"
                                   expression="org.springframework.stereotype.Controller" />
           <context:include-filter type="annotation"
                                   expression="org.springframework.web.bind.annotation.ControllerAdvice" />
       </context:component-scan>
   
       <!-- 对模型视图名称的解析,在请求时模型视图名称添加前后缀 -->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
           <property name="prefix" value="/WEB-INF/views/" />
           <property name="suffix" value=".jsp" />
       </bean>
   
       <!-- 构建一个默认的 Serlvet 来额外的处理一切静态资源 -->
       <mvc:annotation-driven />
       <mvc:default-servlet-handler />
   </beans>
   ```

5. Spring 的配置文件

   ```xml
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
       <!-- 自动扫描 -->
       <context:component-scan base-package="online.shixun">
           <!-- 扫描时跳过 @Controller 注解的JAVA类（控制器） -->
           <context:exclude-filter type="annotation"
                                   expression="org.springframework.stereotype.Controller" />
           <context:exclude-filter type="annotation"
                                   expression="org.springframework.web.bind.annotation.ControllerAdvice" />
       </context:component-scan>
   
       <!--扫描配置文件(这里指向的是之前配置的那个config.properties) -->
       <context:property-placeholder location="classpath*:/application.properties" />
   
       <!--配置数据源 -->
       <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource"
             destroy-method="close">
           <property name="driverClass" value="${jdbc.driver}" />  <!--数据库连接驱动 -->
           <property name="jdbcUrl" value="${jdbc.url}" />     <!--数据库地址 -->
           <property name="user" value="${jdbc.username}" />   <!--用户名 -->
           <property name="password" value="${jdbc.password}" />   <!--密码 -->
           <property name="maxPoolSize" value="40" />      <!--最大连接数 -->
           <property name="minPoolSize" value="1" />       <!--最小连接数 -->
           <property name="initialPoolSize" value="10" />      <!--初始化连接池内的数据库连接 -->
           <property name="maxIdleTime" value="20" />  <!--最大空闲时间 -->
       </bean>
   
       <!--配置session工厂 -->
       <bean id="sessionFactory"
             class="org.springframework.orm.hibernate4.LocalSessionFactoryBean">
           <property name="dataSource" ref="dataSource" />
           <property name="packagesToScan" value="online.shixun.model" />
           <property name="hibernateProperties">
               <props>
                   <prop key="hibernate.hbm2ddl.auto">${hibernate.hbm2ddl.auto}</prop> <!--hibernate根据实体自动生成数据库表 -->
                   <prop key="hibernate.dialect">${hibernate.dialect}</prop>   <!--指定数据库方言 -->
                   <prop key="hibernate.show_sql">${hibernate.show_sql}</prop>     <!--在控制台显示执行的数据库操作语句 -->
                   <prop key="hibernate.format_sql">${hibernate.format_sql}</prop>     <!--在控制台显示执行的数据哭操作语句（格式） -->
               </props>
           </property>
       </bean>
   
       <!-- 事务管理器配置 -->
       <bean id="transactionManager"
             class="org.springframework.orm.hibernate4.HibernateTransactionManager">
           <property name="sessionFactory" ref="sessionFactory" />
       </bean>
   </beans>
   ```

6. 顺便把application.properties引入

```properties
#数据库连接相关
jdbc.driver = com.mysql.jdbc.Driver
jdbc.url = jdbc:mysql://localhost:3306/shixun?useUnicode=true&characterEncoding=utf-8
jdbc.username = root
jdbc.password = Aiyou1314

#hibernate的配置项
hibernate.dialect = org.hibernate.dialect.MySQLDialect
hibernate.show_sql = true
hibernate.format_sql = true
hibernate.hbm2ddl.auto = update
```

