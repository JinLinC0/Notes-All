# `SpringBoot`

## 基本概念

2014年4月，`SpringBoot 1.0.0`发布，作为`Spring`的顶级项目之一

`SpringBoot`就是一种快速构建`Spring`项目的技术，在企业中应用广泛；在微服务的体系下，`SpringBoot`是一个不可或缺的技术

`SpringBoot`提供了一种快速使用`Spring`的方式，基于约定优于配置的思想（将`Spring`需要配置的内容配置好了），可以让开发人员不必在配置于逻辑业务之间进行思维的切换，集中精力投入到业务逻辑的代码编写，从而大大提高了开发的效率

`Spring`的缺点：

- 配置繁琐：虽然`Spring`的组件代码是轻量级的，但是它的配置确是重量级的。一开始，`Spring`用`XML`配置，而且是很多`XML`配置。`Spring2.5`引入了基于注解的组件扫描，消除了大量针对应用程序自身的显示`XML`配置。`Spring3.0`引入了基于`Java`的配置，是一种类型安全的可重构配置方式，代替了`XML`。但是，所有的这些配置都代表了开发时的损耗，导致在思考`Spring`特性配置和解决业务问题之间需要进行思维切换，挤占了编写应用程序逻辑的时间。总之，`Spring`实用，但它要求的回报也不少
- 依赖繁琐：项目的依赖管理是一件耗时耗力的事情，在环境搭建时，需要分析要导入哪些库的坐标，而且还需要分析导入与之有依赖关系的其他库坐标，一旦选错了依赖的版本，随之而来的不兼容问题就会严重阻碍项目的开发进度

`SpringBoot`的功能：

- 自动配置：`SpringBoot`的自动配置是一个运行时（更准确的说是一个应用程序启动时）的过程，考虑了众多因素，才决定`Spring`配置应该使用哪个，不该使用哪个，这个过程是`SpringBoot`自动完成的
- 起步依赖：起步依赖本质上是一个`Maven`项目对象模型（`Project Object Model`，`POM`），定义了对其他库的传递依赖，这些东西加在一起即支持某项功能，简单来说，起步依赖就是将具备某种功能的坐标打包到一起，并提供一些默认的功能（只需引入外面包裹的包即可）
- 辅助功能：提供了一些大型项目中常见的非功能性特性，如嵌入式服务器、安全、指标、健康监测和外部配置等

`SpringBoot`并不是对`Spring`进行功能上的增强，而是提供了一种快速使用`Spring`的方式



## 快速入门

快速的搭建`SpringBoot`的入门级工程，使用`SpringBoot`来构建`Spring`项目

需求：搭建`SpringBoot`工程，定义`HelloController.hello()`方法，返回`Hello SpringBoot`

***

### 传统方式构建`SpringBoot`工程

实现步骤：

1. 创建`Maven`项目

   打开`IDEA`，点击`File`-->`New`-->`Module`-->`Maven`-->`Next`

   在`GroupId`中输入：`com.jlc`；在`ArtifactId`中输入：`springboot-helloworld`；再点击`Next`

   在`Module name`中输入：`springboot-helloworld`；再点击`Finish`

2. 导入`SpringBoot`起步依赖

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd"> 
       <modelVersion>4.0.0</modelVersion>
       
       <groupId>com.jlc</groupId>
       <artifactId>springboot-helloworld</artifactId>
       <version>1.0.SNAPSHOT</version>
       
       <!--springboot工程需要继承的父工程-->
       <parent>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-parent</artifactId>
           <version>2.1.8.RELEASE</version>
       </parent>
       
       <dependencies>
           <!--Web开发的起步依赖-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
       </dependencies> 
   </project>
   ```

3. 定义`Controller`

   在`src/main/java`中创建

   ```java
   package com.jlc.controller;
   
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   
   @RestController
   public class HelloController {
       @RequestMapping("/hello")
       public String hello() {
           return "Hello SpringBoot";
       }
   }
   ```

4. 编写引导类（引导类可以理解为`SpringBoot`项目的入口，其后缀一般都是`Application`结尾的）

   ```java
   package com.jlc;
   
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.boot.SpringApplication;
   
   @SpringBootApplication    // 编写引导类需要添加该注解
   public class HelloApplication {
       public static void main(String[] args) { // 后续运行SpringBoot项目直接运行main方法即可
           SpringApplication.run(HelloApplication.class, args);
       }
   }
   ```

5. 启动测试和访问

   在浏览器中输入：`localhost:8080/hello`进行访问，页面显示：`Hello SpringBoot`

小结：

- `SpringBoot`在创建项目时，使用`jar`的打包方式
- `SpringBoot`的引导类，是项目的入口，运行`main`方法就可以启动项目
- 使用`SpringBoot`和`Spring`构建项目，业务代码编写方式完全一样

***

### 使用`IDEA`快速构建`SpringBoot`工程

使用`IDEA`快速构建`SpringBoot`工程是需要进行网络连接的，在没有网络的情况下是不能构建的

定义`HelloController.hello()`方法，返回`Hello SpringBoot`

打开`IDEA`，点击`File`-->`New`-->`Module`-->`Spring Initializr`-->选择`SDK`（`JDK`）版本-->`Next`

在`Group`中输入：`com.jlc`；在`Artifact`中输入：`springboot-init`；在`Type`中选择`Maven Project`（表示选择的是`Maven`的项目）；在`Language`中选择`Java`；打包方式`Packaging`中选择：`jar`；`Java Version`中选择对应的版本，其他配置可以默认，最后点击`Next`

如果我们要做`Web`开发，我们选择`Web`，勾选相关的依赖：`Spring Web`-->`Next`

输入工程的名称，可以使用默认的-->`Finish`

通过`IDEA`快速构建`SpringBoot`工程，会从网络中下载相关的依赖，在`pom.xml`中会自动的引入相关的坐标

同时在`src/main/java/com/jlc/springbootinit`文件夹中，会自动创建相应的引导类（不需要我们进行手动的编写）：`SpringbootInitApplication`，其内容为：

```java
package com.jlc.springbootinit;

import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.SpringApplication;

@SpringBootApplication
public class SpringbootInitApplication {
    public static void main(String[] args) {
        SpringApplication.run(SpringbootInitApplication.class, args);
    }
}
```

我们只需要编写`Controller`文件即可：

```java
package com.jlc.springbootinit;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {
    @RequestMapping("/hello")
    public String hello() {
        return "Hello SpringBoot";
    }
}
```



## 起步依赖

`SpringBoot`起步依赖的原理分析：起步依赖可以简化`Maven`导入坐标的过程

`SpringBoot`有两个常见的起步依赖：（这两个起步依赖在使用`IDEA`快速构建`SpringBoot`工程是会自动配置）

1. `spring-boot-starter-parent`（`SpringBoot`工程要依赖的父工程）

   定义了各种技术的版本信息，组合了一套最优搭配的技术版本

   ```xml
   <parent>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-parent</artifactId>
       <version>2.1.8.RELEASE</version>
       <relativePath/>
   </parent>
   ```

   我们可以跳转到依赖中，查看系统给我们定义了什么包（包含了一下技术的版本信息），同时定义了版本锁定，后续我们的子过程就会继承这些版本信息去使用，防止有些包的版本冲突

2. `spring-boot-starter-web`（包含了进行`Web`开发需要使用到的起步坐标）系统进行统一配置管理

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-web</artifactId>
   </dependency>
   ```

   起步依赖具有依赖传递的特性，导入封装好的一个坐标，就可以使用该包内部封装的内容

在各种`starter`中，定义了完成该功能需要的坐标合集，其中大部分版本信息来自于父工程

我们的工程继承`parent`，引入`starter`后，通过依赖传递，就可以简单方便的获得需要的`jar`包，并不会存在版本冲突等问题



## 配置

`SpringBoot`是基于约定的，所以很多配置都有默认值，但如果想要使用自己的配置替换默认配置的话，可以使用`application.properties`或者`application.yml`（`application.yaml`）进行配置（配置文件名字是固定的）

`.properties`配置文件通过键值对的方式进行编写：（等号两边没有空格）

```properties
server.port=8080
```

`yml`编写配置文件的方式：（冒号和值之间是有空格的，如果不加空格，语法会报错）

```yaml
server:
  port: 8080
```

系统默认创建的配置文件是`application.properties`配置文件，在根目录`resources`文件夹下

我们可以在配置文件中修改系统定义的属性，如端口号`server.port`（默认值是8080）；也可以配置我们自定义的属性，如`name`属性，但是自定义的属性配置是不会被程序自动识别的，需要我们进行手动加载，从而读取配置

如果我们在`application.properties`、`application.yml`和`application.yaml`（这三个配置文件在同一级目录中都存在）配置文件中都配置了同样的内容，生效优先级从高到低依次是`.properties`、`.yml`和`.yaml`（当一个属性同时存在在这些配置文件中时，低优先级的属性将被忽略掉，不会进行识别配置）

***

### `YAML`

`YAML`是一种直观的能够被电脑直接识别的数据序列化格式（不是一个标记语言），并且容易被人类阅读，容易和脚本语言交互，可以支持`YAML`库的不同的编程语言程序导入。`YAML`文件是以数据为核心的，比传统的`xml`方式更加简洁

`YAML`文件的拓展名可以使用`.yml`或者`.yaml`

使用`YAML`文件进行内容的配置，和`xml`配置文件一样，能够体现具体的层级关系（但是写法上比`xml`文件简洁）

![image-20250606212838704](../images/image-20250606212838704.png)

#### 基本语法

- 大小写敏感
- 数据值前面必须有空格，作为分隔符
- 使用缩进表示层级关系
- 缩进时不允许使用`Tab`键，只允许使用空格（各个系统`Tab`对应的空格数可能不同，导致层级混乱）
- 缩进的空格数目不重要，只要相同层级的元素左侧对齐即可
- `#`表示注释，从这个字符一直到行尾，都会被解析器忽略

#### 数据格式

`YAML`语法有三种可以使用的数据格式：

- 对象(`map`)：键值对的集合

  ```yaml
  person:
    name: jlc
  # 行内写法
  person: {name: jlc}
  ```

- 数组：一组按次序排列的值

  ```yaml
  address:
    - beijing
    - shanghai
  # 行内写法
  address: [beijing,shanghai]
  ```

- 纯量：单个的、不可再分的值

  ```yaml
  msg1: 'hello \n world'   # 单引号忽略转义字符  \n 会被原样的输出
  msg2: "hello \n world"   # 双引号识别转义字符  \n 被识别为换行  hello和world分两行输出
  ```

  



实现自动配置，不需要我们进行手动的编写相应的`xml`配置文件
