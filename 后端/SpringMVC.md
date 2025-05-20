# `SpringMVC`

## 基本概念

`SpringMVC`是一种基于`Java`实现的`MVC`设计模型（`M`：模型，主要用于数据封装和业务逻辑处理；`V`：视图，主要用于数据的展示；`C`：控制器，主要用于分发指派工作）的请求驱动类型的轻量级`Web`框架，属于`SpringFrameWork`的后续产品，已经融合在`Spring Web Flow`中

`SpringMVC`已经成为目前最主流的`MVC`框架之一，并且随着`Spring3.0`的发布，全面超越`Struts2`，成为最优秀的`MVC`框架。它通过一套完善的注解机制，让一个简单的`Java`类成为处理请求的控制器，而无须实现任何接口。同时它还支持`RESTful`编程风格的请求

对于`Web`层，在实际开发中，会出现很多的`Servlet`，用于处理不同功能的`Web`实现。对于每一个`Servlet`，其执行的行为有很多都是一致的（重复的）（一般情况下`Servlet`其内部执行的动作为接收请求参数、封装实体、访问业务层、接收返回结果和指派页面等），对于一致的行为，我们需要对其进行功能的抽取，让一个组件去完成这些通用共有行为的操作（`Web`层相应的框架去完成），同时，具体`Web`层的组件去完成一些特有的行为操作

`SpringMVC`流程图：

![image-20250520214617142](../images/image-20250520214617142.png)

`SpringMVC`的基本开发步骤：

1. 导入`SpringMVC`相关坐标
2. 配置`SpringMVC`核心控制器`DispathcerServlet`，每个请求都要通过共有行为的前端控制器
3. 编写`POJO`（即控制器`Controller`，内部负责调用业务层，指派视图等）和视图页面
4. 将`Controller`使用注解（`@Controller`）配置到`Spring`容器中（业务方法的映射地址）
5. 配置`spring-mvc.xml`文件（`SpringMVC`核心文件），主要配置组件扫描
6. 执行访问测试（客户端发起请求）



## 快速入门

需求：客户端发起请求，服务器端接收请求，执行逻辑并进行视图跳转

1. 在`pom.xml`中额外导入`SpringMVC`相关坐标

   ```xml
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-webmvc</artifactId>
       <version>5.0.5.RELEASE</version>
   </dependency>
   ```

2. 在`web.xml`额外配置`SpringMVC`的前端控制器

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app version="3.0" 
       xmlns="http://java.sun.com/xml/ns/javaee" 
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee 
           http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">
   	<!--配置SpringMVC的前端控制器-->
       <servlet>
           <servlet-name>DispatcherServlet</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <load-on-startup>1</load-on-startup>   <!--服务器启动的时候，就创建对象，如果不配置，则为第一次访问的时候加载对象-->
       </servlet>
       <!--配置映射地址-->
       <servlet-mapping>
           <servlet-name>DispatcherServlet</servlet-name>
           <url-pattern>/</url-pattern>  <!--每次在请求时，都要经过这个部分-->
       </servlet-mapping>
   </web-app>
   ```

3. 编写`Controller`和视图页面

4. 将`Controller`使用注解（`@Controller`）配置到`Spring`容器中（业务方法的映射地址）

   在`com.jlc`中创建一个包`controller`（在使用`SpringMVC`时，`Web`层的包一般都是`controller`）

   在包中创建`UserController`

   ```java
   package com.jlc.controller;
   
   import org.springframework.stereotype.Controller;
   
   @Controller   // 放到Spring容器中
   public class UserController {
       @RequestMapping("/quick")  // 请求映射，访问/quick时，就会映射到save()方法
       public String save() {
           System.out.println("Controller save running");
           return "success.jsp";   // 跳转到具体的视图
       }
   }
   ```

   具体的视图在`webapp`文件夹中进行创建，如创建`success.jsp`

   ```jsp
   <%@page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
       <h1>Success!</h1>
   </body>
   </html>
   ```

5. 配置`spring-mvc.xml`文件（`SpringMVC`核心文件），主要配置组件扫描

   在`resources`文件夹中创建`spring-mvc.xml`配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xmlns:context="http://www.springframework.org/schema/context"
   xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans.xsd
   http://www.springframework.org/schema/context
   http://www.springframework.org/schema/context/spring-context.xsd">
       
   	<!-- Controller的组件扫描 -->
       <context:component-scan base-package="com.jlc.controller"/>
       
   </beans>
   ```

   同时对`web.xml`额外配置`SpringMVC`的前端控制器进行修改，告知配置文件的位置

   ```xml
   <!--配置SpringMVC的前端控制器-->
       <servlet>
           <servlet-name>DispatcherServlet</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <init-param>   <!--加载spring-mvc.xml配置文件-->
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:spring-mvc.xml</param-value>
           </init-param>
           <load-on-startup>1</load-on-startup>   
       </servlet>
   ```

6. 执行访问测试（客户端发起请求）

   在浏览器中输入`localhost:8080/quick`，回车，页面中会出现`Success!`

   同时，终端会打印`Controller save running`

   

## 组件解析

`SpringMVC`底层的组件是如何进行实现的，我们需要对组件进行解析

在`SpringMVC`框架的内部，很多功能都由对应的组件帮助我们去完成（组件间分工明确）

`SpringMVC`的执行流程图：

![image-20250520215058663](../images/image-20250520215058663.png)

1. 用户发送请求至前端控制器`DispatcherServlet`（该前端控制器主要负责调度，进行相应的组件调用）
2. `DispatcherServlet`收到请求调用`HandlerMapping`处理器映射器（该组件用于找资源，解析资源，对请求进行解析，最终具体找哪个，返回处理器执行链，返回的是一串资源的地址，内部封装了具体资源执行的顺序）
3. 处理器映射器找到具体的处理器（可以根据`xml`配置、注解进行查找），生成处理器对象及处理器拦截器（如果有则生成）一并返回给`DispatcherServlet`
4. `DispatcherServlet`调用`HandlerAdapter`处理器适配器（前端控制器拿到返回的处理器执行链后，调用处理器适配器，让其处理调用哪些要被执行的资源）
5. `HandlerAdapter`经过适配器调用具体的处理器（`Controller`，也叫后端控制器，一般是我们自己写的资源封装的对象）
6. `Controller`执行完返回`ModelAndView`（模型和视图对象）
7. `HandlerAdapter`（处理器适配器）将`controller`执行结果`ModelAndView`返回给`DispatcherServlet`（前端控制器）
8. `DispatcherServlet`将`ModelAndView`传给`ViewReslover`视图解析器（从`ModelAndView`中将视图`View`对象解析出来）
9. `ViewReslover`解析后返回具体的`View`
10. `DispatcherServlet`根据`View`进行渲染视图（即将模型数据填充至视图中），`DispatcherServlet`响应用户

***

### 注解解析

对应`SpringMVC`中的注解进行解析，常用的注解有：

- `@RequestMapping`：请求映射到具体的某个方法上

  作用：用于建立请求`URL`和处理器请求方法之间的对应关系（对请求的虚拟地址进行映射到具体的某个方法上）

  使用位置：

  - 在类上，请求`URL`的第一级访问目录，此处不写的话，就相当于应用的根目录

  - 在方法上，请求`URL`的第二级访问目录，与类上的使用`@RequestMapping`标注的一级目录一起组成访问虚拟路径

    ```java
    @Controller   // 放到Spring容器中
    @RequestMapping("/new")
    public class UserController {
        @RequestMapping("/quick")  // 请求映射，访问/quick时，就会映射到save()方法
        public String save() {
            System.out.println("Controller save running");
            return "/success.jsp";   // 跳转到具体的视图，使用的是相对地址，加上/表示从当前Web下找.jsp资源
        }
    }
    ```

    > 此时的访问地址为`http://loacalhost:8080/new/quick`

  属性：`@RequestMapping`的具体属性参数有：

  - `value`：用于指定请求的`URL`，它和`path`属性的作用是一样的

    如：`@RequestMapping(value="/quick")`，只写一个`value`属性，`value`可以省略

  - `method`：用于指定请求的方式

    如：`@RequestMapping(value="/quick", method=RequestMethod.POST)`

    当前请求方式必须是`POST`的请求方式才能访问到

  - `params`：用于指定限制请求参数的条件，它支持简单的表达式，要求参数请求的`key` 和`value`必须和配置的一样

    如：`params={"accountName"}`：表示请求参数必须有`accountName`

    `params={"money!100"}`：表示请求参数中`money`不能是100
