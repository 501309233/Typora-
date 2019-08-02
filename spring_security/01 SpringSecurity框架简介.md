官网：https://projects.spring.io/spring-security/

Spring Security是强大的，且容易定制的实现认证与授权的基于spring开发的框架

 

# Spring Security的功能：

Authentivcation:认证，就是用户登录

Authorization:授权，判断用户拥有什么权限，可以访问什么资源，

安全防护，防止跨站请求，session攻击等

非常容易结合springmvc进行使用

 

# Spring Security 与Shiro的区别

相同点

认证、授权、加密、会话管理、缓存支持、rememberMe功能..

不同点

## 优点

SpringSecurity基于spring开发，项目如使用spring作为基础，配合springSecurity 做权限更加方便，而Shiro需要和spring进行整合开发

SpringSecurity功能比shiro更加丰富，如安全防护方面

spring security社区资源相对比Shiro更加丰富

## 缺点：

shiro的配置和使用比较简单，spring security上手复杂

shiro依赖性低，不需要任何框架和容器，可以独立运行，spring security依赖spring容器