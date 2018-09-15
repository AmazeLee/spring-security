# spring-security
Spring Security入门例子
# 1、在pom.xml添加依赖

```
 <!-- Spring Security -->
 <dependency>
     <groupId>org.springframework.security</groupId>
     <artifactId>spring-security-web</artifactId>
 </dependency>

 <dependency>
     <groupId>org.springframework.security</groupId>
     <artifactId>spring-security-config</artifactId>
 </dependency>
```
#2、在web.xml添加

```
<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:spring/spring-security.xml</param-value>
	</context-param>
	<listener>
		<listener-class>
			org.springframework.web.context.ContextLoaderListener
		</listener-class>
	</listener>

	<filter>
		<filter-name>springSecurityFilterChain</filter-name>
		<filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
	</filter>
	<filter-mapping>
		<filter-name>springSecurityFilterChain</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
```
#3、在项目的spring目录下添加配置文件spring-security.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans:beans xmlns="http://www.springframework.org/schema/security"
             xmlns:beans="http://www.springframework.org/schema/beans"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
						http://www.springframework.org/schema/security http://www.springframework.org/schema/security/spring-security.xsd">

    <!-- 以下页面不被拦截 -->
    <http pattern="/*.html" security="none"></http>
    <http pattern="/css/**" security="none"></http>
    <http pattern="/img/**" security="none"></http>
    <http pattern="/js/**" security="none"></http>
    <http pattern="/plugins/**" security="none"></http>

    <!-- 页面拦截规则  use-expressions:是否启动SPEL表达式 默认是true -->
    <http use-expressions="false">
        <!-- 当前用户必须有ROLE_USER的角色才可以访问根目录以及所属的子目录的资源 -->
        <intercept-url pattern="/**" access="ROLE_ADMIN"/>
        <!-- 开启表单登录功能 -->
        <form-login login-page="/login.html" default-target-url="/admin/index.html"
                    authentication-failure-url="/login.html"/>

        <csrf disabled="true"/>
    </http>

    <!-- 认证管理器 -->
    <authentication-manager>
        <authentication-provider>
            <user-service>
                <user name="admin" password="123456" authorities="ROLE_ADMIN"/>
                <user name="root" password="123456" authorities="ROLE_ADMIN"/>
            </user-service>
        </authentication-provider>
    </authentication-manager>

</beans:beans>
```
