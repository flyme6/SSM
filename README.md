# SSM
用IDEA搭建SSM框架（Spring+SpringMVC+Mybatis）
# 使用IDEA搭建ssm框架

## 环境

> 工具：IDEA 2018.1
>
> jdk版本：jdk1.8.0_171
> 
> Maven版本：apache-maven-3.5.3
> 
> Tomcat版本：apache-tomcat-8.5.30

# 配置步骤

> 1、新建maven项目

> 2、编写pom.xml文件

> 3、新建各种Package包

> 4、添加db.properties数据库配置、以及日志log4j.properties配置文件

> 5、添加mybatis配置文件

    > 5.1、为java po类起类别名

> 6、添加Spring配置文件

    > 6.1、applicationContext-dao.xml
    
    > 6.1.1、加载数据库配置文件
    
    > 6.1.2、配置数据源c3p0连接池
    
    > 6.1.3、配置SqlSessionFactory
    
    > 6.1.4、配置Mapper接口扫描器

    > 6.2、applicationContext-service.xml

    > 6.2.1、添加组件扫描，类必须加上注解（扫描service.impl）

    > 6.3、applicationContext-trsaction.xml
    
    > 6.3.1、配置数据源
    
    > 6.3.2、开启注解（通知）
    
    > 6.3.3、AOP

> 7、添加springmvc配置文件

    > 7.1、springmvc.xml

    > 7.1.1、加载静态资源
    
    > 7.1.2、组件扫描（扫描 controller）
    
    > 7.1.3、开启注解驱动
    
    > 7.1.4、视图解析器

> 8、重新编写web.xml文件

    > 8.1.1、加载Spring容器
    
    > 8.1.2、加载Spring监听器
    
    > 8.1.3、加载springMVC前端控制器
    
    > 8.1.4、加载spring提供乱码过滤器

# 配置详细步骤

## 新建maven项目
![image](http://paz8yo8tt.bkt.clouddn.com/newmaven.png)

## 项目基本展示
![image](http://paz8yo8tt.bkt.clouddn.com/maven.png)

## 编写pom.xml文件,导入相关jar包

```
    <dependencies>
        <!--添加测试类-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <!--log4j-->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
        <!--springWEB-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>4.3.8.RELEASE</version>
        </dependency>

        <!--springMVC-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>4.3.7.RELEASE</version>
        </dependency>

        <!--spring tx 事务处理-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
            <version>4.3.8.RELEASE</version>
        </dependency>

        <!--spring aop-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aop</artifactId>
            <version>4.3.9.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.8.10</version>
        </dependency>

        <!--spring-jdbc-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>4.2.5.RELEASE</version>
        </dependency>

        <!--jstl-->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
        <!--mybatis-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.1</version>
        </dependency>

        <!--mybatis逆向工程-->
        <dependency>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-core</artifactId>
            <version>1.3.5</version>
        </dependency>

        <!--mybatis spring整合包-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>1.3.0</version>
        </dependency>

        <!--hibernate 数据校验器包-->
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-validator</artifactId>
            <version>5.4.1.Final</version>
        </dependency>

        <!--c3p0链接池-->
        <dependency>
            <groupId>com.mchange</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.5.2</version>
        </dependency>

        <!--Mysql数据库驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.41</version>
        </dependency>
        <dependency>
            <groupId>org.jetbrains</groupId>
            <artifactId>annotations-java5</artifactId>
            <version>RELEASE</version>
        </dependency>
    </dependencies>

```
## 新建各种Package包，其中有：

在java文件下新建以下文件：

![image](http://paz8yo8tt.bkt.clouddn.com/package.png)

 com.flyme.controller

 com.flyme.mapper

 com.flyme.po

 com.flyme.service
 
 com.flyme.service.impl



## 添加db.properties数据库配置、以及日志log4j.properties配置文件

![image](http://paz8yo8tt.bkt.clouddn.com/maven-4.png)

在resources文件下新建以下文件


##### db.properties

```
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/examination_system
jdbc.username=root
jdbc.password=root
```

##### log4j.properties

```
### direct log messages to stdout ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=.err
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n
### direct messages to file mylog.log ###
log4j.appender.file=org.apache.log4j.FileAppender
log4j.appender.file.File=../logs/mylog.log
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n
### set log levels - for more verbose logging change 'info' to 'debug' ###
log4j.rootLogger=info, stdout
```

## 添加mybatis配置文件

在resources文件下新建mybatis文件夹，并且在其下新建以下文件

![image](http://paz8yo8tt.bkt.clouddn.com/maven-5.png)

##### mybatis.cfg.xml

```
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--MyBatis主配置文件-->
<configuration>
    <!--为java po类起类别名-->
    <typeAliases>
        <package name="com.flyme.po"/>
    </typeAliases>
</configuration>
```

## 添加Spring配置文件

在recources文件下新建spring文件夹，并且在其下新建下文件

![image](http://paz8yo8tt.bkt.clouddn.com/maven-6.png)

##### applicationContext-dao.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd">

    <!--加载数据库配置文件-->
    <context:property-placeholder location="classpath:db.properties"/>

    <!--配置数据源c3p0连接池-->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driver}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="user" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>

    <!--配置SqlSessionFactory-->
    <bean id="sessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!--加载mybatis配置文件-->
        <property name="configLocation" value="classpath:mybatis/mybatis.cfg.xml"/>
        <!--数据源-->
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <!--Mapper批量扫描，从Mapper包扫描接口，自动创建代理对象，并在Spring容器中自动注册
     使用 Mybatis与Spring整合包的这个 Mapper 扫描器后， Mybatis 配置文件里的扫描器，就可以取消掉了
     遵循的规范 不变
     自动扫描出来的Mapper的bean的id为Mapper类名（首字母小写）
     -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!--如果需要扫描多个包下的mapper,每个包中间使用半角逗号分开-->
        <property name="basePackage" value="com.flyme.mapper"/>
        <property name="sqlSessionFactoryBeanName" value="sessionFactory"/>
    </bean>
</beans>
```

##### applicationContext-service.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd">

    <!--组件扫描，如果想要类被组件扫描，扫描到，并在Spring容器中注册的话
    必须在类名上添加上注解 @Repository、@Service、@Controller、@Component （这四个注解功能一样，名字不同只是为了区分不同功能）
    @Component 是通用组件
    -->
    <context:component-scan base-package="com.flyme.service.impl"></context:component-scan>
</beans>
```

##### applicationContext-trsaction.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	
	
	http://www.springframework.org/schema/aop
	http://www.springframework.org/schema/aop/spring-aop.xsd
	http://www.springframework.org/schema/tx
	http://www.springframework.org/schema/tx/spring-tx.xsd">

    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!--配置数据源-->
        <property name="dataSource" ref="dataSource"></property>
    </bean>

    <!--开启注解（通知）-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <tx:attributes>
            <!--传播行为-->
            <tx:method name="save*" propagation="REQUIRED"/>
            <tx:method name="delete*" propagation="REQUIRED"/>
            <tx:method name="insert*" propagation="REQUIRED"/>
            <tx:method name="update*" propagation="REQUIRED"/>
            <tx:method name="find*" propagation="SUPPORTS" read-only="true"/>
            <tx:method name="get*" propagation="SUPPORTS" read-only="true"/>
            <tx:method name="select*" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>

    <!--AOP-->
    <aop:config>
        <!--设置切入点-->
        <aop:advisor advice-ref="txAdvice" pointcut="execution(* com.flyme.service.impl.*.*(..))"/>
    </aop:config>

</beans>
```

## 添加springmvc配置文件

在resources/spring文件下，新建springmvc.xml文件

![image](http://paz8yo8tt.bkt.clouddn.com/maven-8.png)

##### springmvc.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd
	http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!--加载静态资源-->
    <mvc:default-servlet-handler/>

    <!--1.组件扫描，可以扫描 controller、Service、...
    并注册添加到 spring 容器中
    这里扫描 controller，指定controller的包
    -->
    <context:component-scan base-package="com.flyme.controller"/>

    <!--开启注解驱动-->
    <mvc:annotation-driven/>

    <!--视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
</beans>
```

## 重新编写web.xml文件

![image](http://paz8yo8tt.bkt.clouddn.com/maven-9.png)

##### web.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <welcome-file-list>
        <welcome-file>index.jsp</welcome-file>
    </welcome-file-list>

    <!--加载Spring容器-->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:spring/applicationContext-*.xml</param-value>
    </context-param>

    <!--加载Spring监听器-->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

    <!--springMVC前端控制器加载-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--contextConfigLocation配置SpringMVC加载的配置文件（配置处理器，映射器等等）
        如果不配置contextConfigLocation，默认加载的是：/WEB-INF/servlet名称-servlet.xml(springmvc-servlet.xml)
        -->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring/springmvc.xml</param-value>
        </init-param>
    </servlet>
    <!--
             1. /*  拦截所有   jsp  js png .css  真的全拦截   建议不使用
             2. *.action *.do 拦截以do action 结尾的请求     肯定能使用   ERP
             3. /  拦截所有 （不包括jsp) (包含.js .png.css)  强烈建议使用     前台 面向消费者  www.jd.com/search   /对静态资源放行
          -->
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <!--spring提供 乱码过滤器-->
    <filter>
        <filter-name>CharacterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>CharacterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

</web-app>
```

## SSM框架就此已经搭建完成，下面进行测试

## mybatis进行逆向工程

生产po、mapper相关文件，看图。

![image](http://paz8yo8tt.bkt.clouddn.com/maven-19.png)

如何通过mybatis逆向工程生成，见[链接](https://www.baidu.com/s?ie=utf-8&f=8&rsv_bp=1&rsv_idx=1&tn=baidu&wd=mybatis%E8%BF%9B%E8%A1%8C%E9%80%86%E5%90%91%E5%B7%A5%E7%A8%8B&oq=ssm%25E6%25A1%2586%25E6%259E%25B6&rsv_pq=a7a9316a00043e66&rsv_t=10c1a2VhVPTvHmEWquNqJ7a6G0WoP7fPQxLaRUZFJqOvYbPybwCwDLhI2Nw&rqlang=cn&rsv_enter=1&sug=ssm%25E6%25A1%2586%25E6%259E%25B6&rsv_sug1=4&rsv_sug7=100&rsv_n=2&rsv_sug3=3&rsv_sug2=0&inputT=20373264&rsv_sug4=20373264)。后续更新

### 


## 测试类

### mybatis框架测试

在src/test/java下面进行创建包com.flyme.service，在此下面进行创建CourseServiceImplTest测试类，以用来测试mybatis框架是否搭建成功。

![image](http://paz8yo8tt.bkt.clouddn.com/maven-10.png)

##### CourseServiceImplTest.java

```
package com.flyme.service;

import com.flyme.po.Course;
import org.junit.Before;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;


public class CourseServiceImplTest {


    private ApplicationContext applicationContext;

    @Before
    public void setUp() throws Exception {
        applicationContext = new ClassPathXmlApplicationContext(new String[]{"spring/applicationContext-dao.xml",
                "spring/applicationContext-service.xml"});
    }

    @Test
    public void findById() throws Exception {

        CourseService courseService = (CourseService) applicationContext.getBean("courseServiceImpl");
        Course course = courseService.findById(1);
        System.out.println(course.toString());
    }

}
```

运行findById()方法，出现以下图，证明搭建成功
![image](http://paz8yo8tt.bkt.clouddn.com/maven-11.png)

### SpringMVC框架测试

##### AdminController.java

在src/main/java，com.flyme.controller包下面创建AdminController.java，以用来测SpringMVC框架是否搭建成功。

![image](http://paz8yo8tt.bkt.clouddn.com/maven-12.png)

```
package com.flyme.controller;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/myController")
public class AdminController extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String name = req.getParameter("name");
        req.setAttribute("name", name);
        System.out.println(name);
        req.getRequestDispatcher("index.jsp").forward(req, resp);
    }
}

```

##### idex.jsp

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<form action="myController" method="post">
    <input name="name">
    return:${name}
    <input value="提交" type="submit">
</form>
</body>
</html>
```

**运行项目**

### 配置Tomcat

![image](http://paz8yo8tt.bkt.clouddn.com/maven-15.png)

![image](http://paz8yo8tt.bkt.clouddn.com/maven-16.png)

![image](http://paz8yo8tt.bkt.clouddn.com/maven-17.png)

![image](http://paz8yo8tt.bkt.clouddn.com/maven-18.png)

最后选择OK，即可。

浏览器输入 http://localhost:8010/ （因为笔者改动了端口号，灵活应用）

---
输入：“我是”

![image](http://paz8yo8tt.bkt.clouddn.com/maven-13.png)


---

结果：

![image](http://paz8yo8tt.bkt.clouddn.com/maven-14.png)

---

## 配置成功

## 问题集锦

如有问题欢迎评论
...
