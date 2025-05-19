# `Spring`

## 基本概念

`Spring`是分层的`Java SE/EE`应用`full-stack`（各层都有解决方案）轻量级开源框架，以`IoC`反转控制和`AOP`面向切面编程为内核

`Spring`提供了展现层`SpringMVC`（`Web`层）和持久层`Spring JDBCTemplate`以及业务层事务管理等众多的企业级应用技术，还能整合开源世界中众多著名的第三方框架和类库，逐渐成为使用最多的`Java EE`企业应用开源框架

`Spring`的优势：

- 方便解耦，简化开发

  通过`Spring`提供的`IoC`容器，可以将对象间的依赖关系交由`Spring`进行控制，避免硬编码造成的过度耦合，用户也不必再为单例模式类，属性文件解析等底层的需求编写代码，可以更专注于上层任务

- `AOP`编程的支持

  通过`Spring`的`AOP`功能，方便进行面向切面编程，许多不容易用传统`OOP`实现的功能可以通过`AOP`实现
  
- 声明式事务的支持

  可以将我们从单调烦闷的事务管理代码（提交事务，回滚等）中解脱出来，通过声明方式灵活的进行事务管理，提高开发效率和质量

- 方便程序的测试

  可以用非容器依赖的编程方式进行几乎所有的测试工作，测试不再是昂贵的操作，而是随手可做的事情

- 方便集成各种优秀的框架

  `Spring`对各种优秀的框架（`Struts`、`Hibemate`、`Hessian`、`Quartz`等框架）有很好的支持

- 降低`Java EE API`的使用难度

  `Spring`对`Java EE API`（如`JDBC`、`JavaMail`、远程调用等）进行了薄薄的封装层（模板），使这些`API`的使用难度大为降低

- `Spring`的源代码设计巧妙，处处体现了`Java`设计模式的灵活运用，其源代码是`Java`技术学习的最好典范

`Spring`的体系结构

![image-20250506202055898](../images/image-20250506202055898.png)

> - 最顶部的两层是数据访问层和`Web`应用层，其实现要借助中间层的工具
> - 中间层包括`AOP`和切面等编程技术
> - `Core Container`：核心容器，主要涉及的是`IoC`的知识点，有主要的四个部分：产生对象的容器、核心、上下文和`Spring`的表达式语言
> - `Test`表示整体的框架都可以进行测试



## 开发步骤

`Spring`框架的基本开发步骤为：

1. 导入`Spring`开发的基本包坐标

   依赖坐标是一种标识符，用来指明我们需要使用的库或框架的版本信息。在`Maven`构建工具中，通过添加依赖坐标来告诉构建工具需要下载并引入哪些库文件

   在具体模块的`pom.xml`文件中导入基本的包坐标

   ```xml
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-context</artifactId>
       <version>5.3.9</version>
   </dependency>
   ```

2. 编写`Dao`接口和实现类（创建`Bean`）

   在`src/main/java`文件夹中创建接口

   ```java
   package com.jlc.dao;
   
   public interface UserDao {
       public void save();
   }
   ```

   为这个接口创建对应的实现：

   ```java
   package com.jlc.dao.impl;
   
   import com.jlc.dao.UserDao
   
   public class UserDaoImpl implements UserDao {
       // 实现接口的save方法
       public void save() {
           System.out.println("save running");
       }
   }
   ```

3. 创建`Spring`核心配置文件（一般叫做`applicationContext.xml`）

4. 在`Spring`配置文件中配置`UserDaolmpl`

   将全包名配置到配置文件的内部，通过`id`去标识这个全限定名（后续可以通过这个`id`标识去获取这个全限定名）

   在`src/main/resources`文件夹中创建`Spring`配置文件`applicationContext.xml`

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xmlns:context="http://www.springframework.org/schema/context"
   xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans.xsd
   http://www.springframework.org/schema/context
   http://www.springframework.org/schema/context/spring-context.xsd">
       
   	<!-- bean定义和其他配置 -->
       <bean id="userDao" class="com.jlc.dao.impl.UserDaoImpl"></bean>
       
   </beans>
   ```

   > `id`标识配置具体的标识；`class`表示配置具体的全限定名

5. 引入`Spring`框架（框架的作用是读取这个`xml`配置文件），使用`Spring`的`API`获取`Bean`实例（获取全包名，通过反射创建`Bean`对象，默认情况下反射是通过无参构造去创建对象的）

   ```java
   package com.jlc.demo;
   
   public class UserDaoDemo {
       public static void main(String[] args) {
           // 获得容器中，由Spring进行创建的userDao
           ApplicationContext app = new ClassPathXmlApplicationContext(configLocation:"applicationContext.xml");
           UserDao userDao = (UserDao) app.getBean(s:"userDao");  // 通过标识去获取具体的对象
           userDao.save();  // 调用save()方法
       }
   }
   ```

![image-20250506203955870](../images/image-20250506203955870.png)

`Spring`框架的作用就是读取`XML`配置文件的，从而获得我们的全包名；

上述的过程完成了解耦，代码可以不进行修改，我们只需要修改其配置文件（在开发阶段和运行阶段是保持不变的），就可以实现相应的功能



## 配置文件

### `Bean`标签

`Bean`标签用于配置对象交由`Spring`来创建

默认情况下它调用的是类中的无参构造器，如果没有无参构造器则不能创建成功

基本属性：

- `id`：`Bean`实例在`Spring`容器中的唯一标识，是不允许重复的

- `class`：`Bean`的全限定名称

- `scope`：`Bean`标签的范围配置，指对象的作用范围，取值如下：

  |     取值范围     |                             描述                             |
  | :--------------: | :----------------------------------------------------------: |
  |   `singleton`    | 默认值，单例的（容器中存在的对象只有一个，即每次在`getBean`实例出对象时，指向的是同一个对象） |
  |   `prototype`    | 多例的（从容器中获取对象有多个对象，即每次在`getBean`时，都会获取新的对象） |
  |    `request`     | `WEB`项目中，`Spring`创建一个`Bean`的对象，将对象存入到`request`域中 |
  |    `session`     | `WEB`项目中，`Spring`创建一个`Bean`的对象，将对象存入到`session`域中 |
  | `global session` | `WEB`项目中，应用在`Portlet`环境，如果没有`Portlet`环境，那么`global session`相当于`session` |

  > - 当`scope`的取值为`singleton`时，`Bean`的实例化个数是一个，其`Bean`的实例化时机为当`Spring`核心文件被加载时（`ApplicationContext app = new ClassPathXmlApplicationContext(configLocation:"applicationContext.xml");`），实例化配置的`Bean`实例；`Bean`的生命周期：
  >   - 对象创建：当应用加载，创建容器时，对象就被创建了
  >   - 对象运行：只要容器在，对象一直活着
  >   - 对象销毁：当应用卸载时，销毁容器时，对象就被销毁了
  > - 当`scope`的取值为`prototype`时，`Bean`的实例化个数有多个，其`Bean`的实例化时机为当调用`getBean()`方法时实例化`Bean`；`Bean`的生命周期：
  >   - 对象创建：当使用对象时，创建新的对象实例
  >   - 对象运行：只要对象在使用中，就一直活着
  >   - 对象销毁：当对象长时间不用时，就会被`Java`的垃圾回收器回收

- `init-method="具体方法"`：对象的初始化方法，即在初始化的时候执行指定的方法（这个指定的方法在对象声明存在）

- `destroy-method="具体方法"`：对象的销毁方法，即在对象销毁时执行指定的方法（这个指定的方法在对象声明存在）

  销毁对象：`app.close();`

#### `Bean`实例化的三种方式

`Bean`实例化的方式默认是找其对象的无参构造器方法进行实例化（是重点），但是我们也可以通过配置，让其不找无参构造，使用工厂静态方法实例化和工厂实例方法实例化

工厂静态方法实例化

首先需要创建一个静态工厂：

```java
package com.jlc.factory;

public class StaticFactory {
    // 编写一个静态方法，返回对象实例
    public static UserDao getUserDao() {
        return new UserDaoImpl();
    }
}
```

在`Spring`配置文件中声明使用工厂静态方法进行实例化

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:context="http://www.springframework.org/schema/context"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd">
    
    <bean id="userDao" class="com.jlc.factory.StaticFactory" factory-method="getUserDao"></bean>
    
</beans>
```

> 找全限定名`class="com.jlc.factory.StaticFactory"`内部指定的工厂方法`getUserDao`返回对应的对象（该方法是静态方法，可以不通过类.的方式进行调用）

工厂实例方法实例化

与工厂静态方法实例化类似，但是不是通过静态方法返回对象

```java
package com.jlc.factory;

public class DynamicFactory {
    // 编写一个方法，返回对象实例
    public UserDao getUserDao() {
        return new UserDaoImpl();
    }
}
```

在`Spring`配置文件中声明使用工厂实例方法进行实例化

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:context="http://www.springframework.org/schema/context"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd">
    
    <bean id="factory" class="com.jlc.factory.DynamicFactory"></bean>
    <bean id="userDao" factory-bean="factory" factory-method="getUserDao">
    
</beans>
```

> 由于不是静态方法，首先需要通过`Spring`产生工厂对象，再调用工厂内部的实例化方法

#### 依赖注入

之前的工作，只是让`Spring`帮助我们实例化出`Dao`层的对象，在实际开发中，业务成和`Web`层也是有对应代码的

创建业务层的代码：在`src/main/java`文件夹中创建：

```java
package com.jlc.service;

// 声明业务层的接口
public interface UserService {
    public void save();
}
```

为接口创建一个实现：

```java
package com.jlc.service.impl;

import com.jlc.service.UserService;

public class UserServiceImpl implements UserService {
    public void save() {
        // 调用Dao中的save方法
        ApplicationContext app = new ClassPathXmlApplicationContext(configLocation:"applicationContext.xml");
        UserDao userDao = (UserDao) app.getBean(s:"userDao");  // 通过标识去获取具体的对象
        userDao.save();  // 调用save()方法
    }
}
```

将`UserServiceImpl`也配置到`Spring`容器中：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:context="http://www.springframework.org/schema/context"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd">
    
	<!-- bean定义和其他配置 -->
    <bean id="userDao" class="com.jlc.dao.impl.UserDaoImpl"></bean>
    <bean id="userService" class="com.jlc.service.impl.UserServiceImpl"></bean>
</beans>
```

编写一个测试文件：

```java
package com.jlc.demo;

public class UserController {
    public static void main(String[] args) {
        ApplicationContext app = new ClassPathXmlApplicationContext(configLocation:"applicationContext.xml");
        UserService userService = (UserService) app.getBean(s:"userService");
        userService.save();  // 调用save()方法
    }
}
```

目前`UserService`实例和`UserDao`实例都存在于`Spring`容器中，但最终程序直接使用的可能是`UserService`，所以可以在`Spring`容器中，上述做法是在`Spring`容器外将`Dao`组装到`Service`内部完成后续的操作

我们可以在`Spring`容器中，将`UserDao`设置到`UserService`内部，这个过程就是依赖注入

![image-20250519161344401](../images/image-20250519161344401.png)

依赖注入是`Spring`框架核心`IOC`的具体实现，在编写程序时，通过控制反转，把对象的创建交给了`Spring`，但是代码中不可能出现没有依赖的情况。`IOC`解耦只是降低他们的依赖关系，但不会消除。

通过依赖注入，业务层和持久层的依赖关系，在使用`Spring`之后，就让`Spring`来配置和维护了，简而言之，就是让框架把持久层对象传入业务从鞥，而不用我们自己去获取

依赖注入有两种方式：

- `set`方法：为某个属性设置值（即注入值）

  ```java
  // 在UserServiceImpl中通过set方法进行依赖注入
  package com.jlc.service.impl;
  
  import com.jlc.service.UserService;
  
  public class UserServiceImpl implements UserService {
      private UserDao userDao;   // 用于接收容器中的userDao
      public void setUserDao(UserDao userDao) {
          this.userDao = userDao;
      }
      
      public void save() {
          userDao.save();  // 调用save()方法
      }
  }
  ```

  在`Spring`配置文件中声明依赖注入

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:context="http://www.springframework.org/schema/context"
  xsi:schemaLocation="http://www.springframework.org/schema/beans
  http://www.springframework.org/schema/beans/spring-beans.xsd
  http://www.springframework.org/schema/context
  http://www.springframework.org/schema/context/spring-context.xsd">
      
  	<!-- bean定义和其他配置 -->
      <bean id="userDao" class="com.jlc.dao.impl.UserDaoImpl"></bean>
      <bean id="userService" class="com.jlc.service.impl.UserServiceImpl">
          <property name="userDao" ref="userDao"></property>
      </bean>
  </beans>
  ```

  > `<property name="userDao" ref="userDao"></property>`中的`name`是`set`方法后面的部分，大写字母变成小写；`ref`表示将具体的唯一标识进行注入（对象的引用用`ref`）

  `set`方法进行依赖注入，有一个简化的方式，即`P`命名空间注入，主要的区别体现在配置文件中（使用属性的方式代替具体的子标签）：

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:context="http://www.springframework.org/schema/context"
  xsi:schemaLocation="http://www.springframework.org/schema/beans
  http://www.springframework.org/schema/beans/spring-beans.xsd
  http://www.springframework.org/schema/context
  http://www.springframework.org/schema/context/spring-context.xsd"
  xmlns:p="http://www.springframework.org/schema/p">
      
  	<!-- bean定义和其他配置 -->
      <bean id="userDao" class="com.jlc.dao.impl.UserDaoImpl"></bean>
      <bean id="userService" class="com.jlc.service.impl.UserServiceImpl" p:userDao-ref="userDao"></bean>
  </beans>
  ```

- 构造方法：通过有参构造，将数据注入到其他对象的内部

  ```java
  // 在UserServiceImpl中通过构造方法进行依赖注入
  package com.jlc.service.impl;
  
  import com.jlc.service.UserService;
  
  public class UserServiceImpl implements UserService {
      private UserDao userDao;   // 用于接收容器中的userDao
      public UserServiceImpl(UserDao userDao) {   // 有参构造
          this.userDao = userDao;
      }
      
      // 使用的有参构造，会覆盖无参构造，我们要重新声明
      public UserServiceImpl() {}
      
      public void save() {
          userDao.save();  // 调用save()方法
      }
  }
  ```

  在`Spring`配置文件中声明依赖注入

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:context="http://www.springframework.org/schema/context"
  xsi:schemaLocation="http://www.springframework.org/schema/beans
  http://www.springframework.org/schema/beans/spring-beans.xsd
  http://www.springframework.org/schema/context
  http://www.springframework.org/schema/context/spring-context.xsd">
      
  	<!-- bean定义和其他配置 -->
      <bean id="userDao" class="com.jlc.dao.impl.UserDaoImpl"></bean>
      <bean id="userService" class="com.jlc.service.impl.UserServiceImpl">
          <constructor-srg name="userDao" ref="userDao"></constructor-srg>
      </bean>
  </beans>
  ```

  > `<constructor-srg name="userDao" ref="userDao"></constructor-srg>`中的`name`是`UserServiceImpl`内部对应的私有属性名；`ref`表示将具体的唯一标识进行注入（对象的引用用`ref`）

##### 依赖注入的数据类型

对于注入的引用`Bean`，除了对象的引用类型可以注入，普通的数据类型、集合等都可以在容器中进行注入

注入数据的三种数据类型：既可以通过`set`方法，也可以通过构造函数的方法，这里只演示`set`方法

- 普通数据类型

  ```java
  // 在UserServiceImpl中通过set方法进行普通数据类型的依赖注入
  package com.jlc.service.impl;
  
  import com.jlc.service.UserService;
  
  public class UserServiceImpl implements UserService {
      private String userName;   // 用于接收容器中的注入的username
      private int age;   // 用于接收容器中的注入的age
      public void setUserName(String userName) {
          this.userName = userName;
      }
      public void setAge(int age) {
          this.age = age;
      }
      
      public void save() {
          System.out.println(userName, age);   // zhangsan, 25
      }
  }
  ```

  在`Spring`配置文件中声明依赖注入

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:context="http://www.springframework.org/schema/context"
  xsi:schemaLocation="http://www.springframework.org/schema/beans
  http://www.springframework.org/schema/beans/spring-beans.xsd
  http://www.springframework.org/schema/context
  http://www.springframework.org/schema/context/spring-context.xsd">
      
  	<!-- bean定义和其他配置 -->
      <bean id="userDao" class="com.jlc.dao.impl.UserDaoImpl"></bean>
      <bean id="userService" class="com.jlc.service.impl.UserServiceImpl">
          <property name="userName" value="zhangsan"></property>
          <property name="age" value="25"></property>
      </bean>
  </beans>
  ```

  > 普通属性值进行依赖注入使用`value`

- 引用数据类型：之前对象引用的注入（对象引用进行依赖注入使用`ref`）

- 集合数据类型

  ```java
  // 在UserServiceImpl中通过set方法进行集合数据类型的依赖注入
  package com.jlc.service.impl;
  
  import com.jlc.service.UserService;
  
  public class UserServiceImpl implements UserService {
      private List<String> strList;
      private Map<String, User> userMap;   // User是一个对象
      private Properties properties;
      
      public void setStrList(List<String> strList) {
          this.strList = strList;
      }
      public void setUserMap(Map<String, User> userMap) {
          this.userMap = userMap;
      }
      public void setProperties(Properties properties) {
          this.properties = properties;
      }
      
      public void save() {
          System.out.println(strList);   // [aa, bb, cc]
          System.out.println(userMap);   // {u1=User{name='jack', addr='hangzhou'}, u2=User{name='mary', addr='shanghai'}}
          System.out.println(properties);   // {p2=PPP, p1=ppp}
      }
  }
  ```

  在`Spring`配置文件中声明依赖注入

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:context="http://www.springframework.org/schema/context"
  xsi:schemaLocation="http://www.springframework.org/schema/beans
  http://www.springframework.org/schema/beans/spring-beans.xsd
  http://www.springframework.org/schema/context
  http://www.springframework.org/schema/context/spring-context.xsd">
      
  	<!-- bean定义和其他配置 -->
      <bean id="userDao" class="com.jlc.dao.impl.UserDaoImpl"></bean>
      <bean id="userService" class="com.jlc.service.impl.UserServiceImpl">
          <property name="strList">
              <List>
                  <value>aa</value>
                  <value>bb</value>
                  <value>cc</value>
              </List>
          </property>
          <property name="userMap" value="25">
              <Map>
                  <entry key="u1" value-ref="user1"></entry>
                  <entry key="u2" value-ref="user2"></entry>
              </Map>
          </property>
          <property name="properties">
              <props>
                  <prop key='p1'>ppp</prop>
                  <prop key='p2'>PPP</prop>
              </props>
          </property>
      </bean>
      
      <bean id="user1" class=com.jlc.domain.User>
          <property name="name" value="jack"></property>
          <property name="addr" value="hangzhou"></property>
      </bean>
      <bean id="user2" class=com.jlc.domain.User>
          <property name="name" value="mary"></property>
          <property name="addr" value="shanghai"></property>
      </bean>
  </beans>
  ```

  > 集合属性值使用内部的具体对应子标签进行依赖的注入，子标签具体注入的值如果是基本数据类型，使用`<value>`标签进行包裹，如果是对象引用类型，使用`<ref>`标签进行包裹

***

### `import `标签

`import `标签用于引入其他配置文件

在实际开发中，`Spring`的配置文件内容一般非常多，不方便后期的维护，所以我们通常将配置文件中的部分配置拆分到其他配置文件中，而在`Spring`主配置文件中通过`import`标签进行加载开发

```xml
<import resource="applicationContext-xxx.xml"/>
```

> 引入之后，加载主配置文件，其引用的分配置文件也会被一起进行加载



## 相关`API`

```java
// Spring相关的API
ApplicationContext app = new ClassPathXmlApplicationContext(configLocation:"applicationContext.xml");
UserService userService = (UserService) app.getBean(s:"userService");
userService.save();  // 调用save()方法
```

> `ApplicationContext`的继承体系：`ApplicationContext`是一个接口，代表应用上下文，可以通过其实例获得`Spring`容器中的`Bean`对象，`ClassPathXmlApplicationContext`是该接口对应的接口实现，使用多态的方式接收，`ApplicationContext`接口的所有实现类有：
>
> - `ClassPathXmlApplicationContext`：从类的根路径（`resources`文件夹）下加载配置文件
>
>   ```java
>   new ClassPathXmlApplicationContext(configLocation:"applicationContext.xml");
>   ```
>
> - `FileSystemXmlApplicationContext`：从磁盘路径上加载配置文件，配置文件可以在磁盘的任意位置
>
>   ```java
>   new FileSystemXmlApplicationContext(configLocation:"D:\\src\\mian\\resources\\applicationContext.xml");
>   ```
>
> - `AnnotationConfigApplicationContext`：使用注解配置容器对象时，需要使用此类来创建`Spring`容器，从而读取注解

### `getBean()`

`getBean()`的参数有两种方式：

- `getBean("id")`

  如：`UserService userService = (UserService) app.getBean(s:"userService");`

  当参数的数据类型是字符串时，表示根据配置文件中`Bean`的`id`从容器中获得`Bean`实例，返回的是`Object`，需要进行强转

- `getBean(class)`

  如：`UserService userService = app.getBean(UserService.class);`

  当参数的数据类型是`Class`类型时，表示根据类型从容器中匹配`Bean`实例，但是当容器中相同类型的`Bean`有多个时（即`id`不同，`class`的内容相同），则此方法会报错。



## 配置数据源

数据源就是`Java`中的连接池，使用连接池去充当数据源

数据源可以提高程序性能，需要事先实例化数据源，从而初始化部分连接资源；当我们要使用连接资源时从数据源中获取；当使用完毕后，将连接资源归还给数据源

常见的数据源（连接池）有：`DBCP`、`C3P0`、`BoneCP`和`Druid`

***

### 传统方式配置数据源

数据源的开发步骤：

1. 导入数据源的坐标和数据库的驱动坐标

   在具体模块的`pom.xml`文件中导入基本的包坐标

   ```xml
   <dependency>
       <dependency>
           <groupId>mysql</groupId>
           <artifactId>mysql-connector-java</artifactId>
           <version>5.1.32</version>
       </dependency>
       <dependency>
           <groupId>c3p0</groupId>
           <artifactId>c3p0</artifactId>
           <version>0.9.1.2</version>
       </dependency>
       <dependency>
       	<groupId>com.alibaba</groupId>
           <artifactId>druid</artifactId>
           <version>1.1.10</version>
       </dependency>
   </dependency>
   ```

2. 创建数据源对象

3. 设置数据源的基本连接数据信息（驱动，数据库地址，用户名和密码）

4. 使用数据源获取连接资源和归还连接资源

```java
package com.jlc.test;

import com.mchange.v2.c3p0.ComboPooledDataSource;

public class DataSourceTest {
    // c3p0数据源的使用
    public void test1() throws Exception {
        // 创建数据源对象
        ComboPooledDataSource dataSource = new ComboPooledDataSource();
        // 设置基本的连接参数
        dataSource.setDriverClass("com.mysql.jdbc.Driver");
        dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/test");
        dataSource.setUser("root");
        dataSource.setPassword("admin");
        // 获取资源
        Connection connection = dataSource.getConnection();
        ...
        // 关闭连接，归还连接资源
        connection.close();
    }
    
    // Druid数据源的使用
    public void test2() throws Exception {
        // 创建数据源对象
        DruidDataSource dataSource = new DruidDataSource();
        // 设置基本的连接参数
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/test");
        dataSource.setUsername("root");
        dataSource.setPassword("admin");
        // 获取资源
        DruidPooledConnection connection = dataSource.getConnection();
        ...
        // 关闭连接，归还连接资源
        connection.close();
    }
}
```

通常，在实际开发中，要对基本的连接参数信息进行抽取，使字符串设置与数据源进行解耦合，将信息配置在`properties`配置文件中（以键值对的形式存放），在`resources`文件夹中新建：`jdbc.properties`

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/test
jdbc.username=root
jdbc.password=admin
```

修改上述的使用数据源的文件，使其读取外部的数据库配置信息文件

```java
package com.jlc.test;

import com.mchange.v2.c3p0.ComboPooledDataSource;
import java.util.ResourceBundle;

public class DataSourceTest {
    // c3p0数据源的使用（通过加载数据库信息配置文件）
    public void test1() throws Exception {
        // 读取数据库配置文件
        ResourceBundle rb = ResourceBundle.getBundle("jdbc");  // 传入的是相对resources文件夹下的文件地址，而且其名称也不需要扩展名，读取的默认是.properties文件
        // 设置读取的配置文件
        String driver = rb.getString("jdbc.driver");
        String url = rb.getString("jdbc.url");
        String username = rb.getString("jdbc.username");
        String password = rb.getString("jdbc.password");
        // 创建数据源对象
        ComboPooledDataSource dataSource = new ComboPooledDataSource();
        // 设置基本的连接参数
        dataSource.setDriverClass(driver);
        dataSource.setJdbcUrl(url);
        dataSource.setUser(username);
        dataSource.setPassword(password);
        // 获取资源
        Connection connection = dataSource.getConnection();
        ...
        // 关闭连接，归还连接资源
        connection.close();
    }
}
```

***

### 通过`Spring`配置文件配置数据源

对于数据源的第三方包，也可以通过`Spring`配置文件进行配置，即将`DataSource`的创建权交给`Spring`容器去完成

基本步骤：

1. 导入数据源的坐标和数据库的驱动坐标，之前的基础上导入`spring-context`

   在具体模块的`pom.xml`文件中导入基本的包坐标

   ```xml
   <dependency>
       <dependency>
           <groupId>mysql</groupId>
           <artifactId>mysql-connector-java</artifactId>
           <version>5.1.32</version>
       </dependency>
       <dependency>
           <groupId>c3p0</groupId>
           <artifactId>c3p0</artifactId>
           <version>0.9.1.2</version>
       </dependency>
       <dependency>
       	<groupId>com.alibaba</groupId>
           <artifactId>druid</artifactId>
           <version>1.1.10</version>
       </dependency>
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-context</artifactId>
           <version>5.3.9</version>
       </dependency>
   </dependency>
   ```

2. 创建`Spring`配置文件，在`resources`文件夹下创建`Spring`配置文件`applicationContext.xml`

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xmlns:context="http://www.springframework.org/schema/context"
   xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans.xsd
   http://www.springframework.org/schema/context
   http://www.springframework.org/schema/context/spring-context.xsd">
       
   	<!-- bean定义和其他配置 -->
       <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
           <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
           <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/test"></property>
           <property name="user" value="root"></property>
           <property name="password" value="admin"></property>
       </bean>
       
   </beans>
   ```

3. 测试数据源

   ```java
   package com.jlc.test;
   
   public class DataSourceTest {
       // Spring容器产生数据源对象
       public void test1() throws Exception {
           // 创建数据源对象
           ApplicationContext app = new ClassPathXmlApplicationContext(configLocation:"applicationContext.xml");
           DataSource dataSource = app.getBean(DataSource.class);
           // 获取资源
           Connection connection = dataSource.getConnection();
           ...
           // 关闭连接，归还连接资源
           connection.close();
       }
   }
   ```

   

   



