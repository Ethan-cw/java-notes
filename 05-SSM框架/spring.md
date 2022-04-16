# 1 Spring

## 1.1 简介

Spring：整合了现在有的技术框架

核心部分：

* IOC：控制反转，把创建对象的过程交给Spring来管理
* AOP：面向切面，不修改源代码的进行功能增强。

特点：

* 方便解耦
* AOP编程支持
* 方便程序支持
* 方便和其他框架进行整合
* 方便进行事务的管理
* 降低API的使用难度

## 1.2 组成

![img](img/1010726-20190908042152777-1895820426.png)

## 1.3 扩展

![image-20211202201640658](img/image-20211202201640658.png)

* Spring Boot
  * 快速开发的脚手架
  * 可以快速开发单个微服务
  * 约定大于配置
* Spring Cloud
  * 基于Spring Boot实现

学习Spring Boot的前提是Spring及SpringMVC

弊端：发展太久，违背了原来的理念！配置十分繁琐，人称配置地狱。

# 2 IOC理论推导

[Spring：IOC本质分析探究 (copyfuture.com)](https://copyfuture.com/blogs-details/20190726113449466pf49hbm0qrn8wpe)

使用Set接口实现

```java
//利用set动态实现值的注入
public void setUserDao(UserDao userDao) {
    this.userDao = userDao;
}
```

从本质上解决了问题，不用再去管理对象的创建。系统的耦合性大大降低，专注于业务的实现。

这是IOC的原型。

**IOC本质**

控制反转IoC(Inversion of Control)，是一种设计思想，DI(依赖注入)是实现IoC的一种方法，也有人认为DI只是IoC的另一种说法。没有IoC的程序中 , 我们使用面向对象编程 , 对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，个人认为所谓控制反转就是：获得依赖对象的方式反转了。

IoC是Spring框架的核心内容，使用多种方式完美的实现了IoC，可以使用XML配置，也可以使用注解，新版本的Spring也可以零配置实现IoC。

**Spring容器在初始化时先读取配置文件，根据配置文件或元数据创建与组织对象存入容器中，程序使用时再从Ioc容器中取出需要的对象。**

控制 : 谁来控制对象的创建 , 传统应用程序的对象是由程序本身控制创建的 , 使用Spring后 , 对象是由Spring来创建的 .

反转 : 程序本身不创建对象 , 而变成被动的接收对象 .

依赖注入 : 就是利用set方法来进行注入的.

# 3 HelloSpring

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 使用Spring创建对象，这些都被称为Bean
    bean = 对象 new Hello();

    id 变量名
    class = new的对象
    property 相当于对象属性设置了值

    -->
    <bean id="hello" class="com.cw.pojo.Hello">
        <property name="name" value="Spring"></property>
    </bean>
</beans>
```

```java
package com.cw.pojo;

public class Hello {
    private String name;

    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }

    public void show(){
        System.out.println("Hello"+ name );
    }
}
```

```java
import com.cw.pojo.Hello;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MyTest {
    public static void main(String[] args) {
        //获取Spring的上下文对象
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        //我们的对象都在Spring中管理了，要用直接取出来。
        Hello hello = (Hello)context.getBean("hello");
        System.out.println(hello.toString());
    }
}
```

# 4 IOC创建对象的方式

1. 使用无参构造创建对象，默认

2. 假设想使用有参构造

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <bean id="user" class="com.cw.pojo.User">
   <!--        无参构造-->      
   <!--        <property name="name" value="cw"/>-->
   <!--        通过下标有参构造-->
   <!--        <constructor-arg index="0" value="cw"/>-->
   <!--        通过类型有参构造-->
   <!--        <constructor-arg type="java.lang.String" value="cw"/>-->
   <!--        直接通过参数名-->
           <constructor-arg name="name" value="cw"/>
       </bean>
   </beans>
   ```

   总结：配置文件加载时候，容器中管理的对象就已经开始初始化了！

# 5 Spring的配置

## 5.1 别名

```xml
<alias name="user" alias="user2"/>
```

## 5.2 Bean的配置

 ```xml
 <!--
     id 唯一标识符
     class 包名+类名
     name 别名 别名
 -->
 <bean id="user" class="com.cw.pojo.User" name="user3 u2, u3">
         <property name="name" value="cw"/>
 </bean>
 ```

## 5.3 import

一般用于团队开发，多个配置文件导入为一个

```xml
<import resource="beans2.xml"/>
```

# 6 DI 依赖注入

## 6.1 构造注入

前面说过了

## 6.2 Set方式注入

* 依赖注入：Set注入！
  * 依赖：bean对象的创建依赖于容器
  * 注入：bean对象的所有属性由容器来注入

【环境搭建】

1. 复杂类型

   ```java
   package com.cw.pojo;
   
   public class Address {
       private String address;
   
   
   
       public String getAddress() {
           return address;
       }
   
       public void setAddress(String address) {
           this.address = address;
       }
   
       @Override
       public String toString() {
           return "Address{" +
                   "address='" + address + '\'' +
                   '}';
       }
   }
   ```

   

2. 真实对象

   ```java
   package com.cw.pojo;
   
   import java.util.*;
   
   public class Student {
       private String name;
       private Address address;
       private String[] books;
       private List<String> hobbys;
       private Map<String,String> card;
       private Set<String> game;
       private String wife;
       private Properties info;
   ```

3. beans.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <bean id="address" class="com.cw.pojo.Address"/>
   
   
       <bean id="student" class="com.cw.pojo.Student">
           <!--普通值注入 value-->
           <property name="name" value="cw"/>
           <!--Bean注入 ref-->
           <property name="address" ref="address"/>
           <!--数组注入 ref-->
           <property name="books">
               <array>
                   <value>红楼梦</value>
                   <value>西游记</value>
                   <value>三国演义</value>
               </array>
           </property>
           <!--List注入 -->
           <property name="hobbys">
               <list>
                   <value>看电影</value>
                   <value>唱歌</value>
                   <value>写代码</value>
               </list>
           </property>
           <!--Map注入 -->
           <property name="card">
               <map>
                   <entry key="身份证" value="123"/>
                   <entry key="学生证" value="321"/>
               </map>
           </property>
   
           <!--Set注入 -->
           <property name="game">
               <set>
                   <value>王者</value>
                   <value>LOL</value>
               </set>
           </property>
           <!--空值注入 -->
           <!--<property name="wife" value=""/>-->
           <!--null注入 -->
           <property name="wife">
               <null></null>
           </property>
   
           <property name="info">
               <props>
                   <prop key="学号">123456789</prop>
                   <prop key="年龄">30</prop>
               </props>
           </property>
       </bean>
   </beans>
   
   ```

   

4. 测试类

   ```java
   import com.cw.pojo.Student;
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   
   public class MyTest {
       public static void main(String[] args) {
           ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
           Student student = (Student)context.getBean("student");
           System.out.println(student.toString());
       }
   }
   ```



## 6.3 扩展方式注入

[Spring Framework 中文文档 - Core Technologies | Docs4dev](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--p命名空间的，可以直接注入-->
    <bean id="user" class="com.cw.pojo.User" p:name="cw" p:age="18"/>
    <!--c命名空间的，可以通过构造器注入 construct-args-->
    <bean id="user2" class="com.cw.pojo.User" c:age="18" c:name="wqj"/>
</beans>
```

注意点：p命名空间和c命名空间 需要导入约束

```xml
xmlns:p="http://www.springframework.org/schema/p"
xmlns:c="http://www.springframework.org/schema/c"
```

## 6.4 bean的作用域

![image-20211203144411647](img/image-20211203144411647.png)

1. 单例模式（默认模式）

   ```xml
   <bean id="accountService" class="com.something.DefaultAccountService" scope="singleton"/>
   ```

2. 原型模式 get时候都会产生一个新对象

   ```xml
   <bean id="accountService" class="com.something.DefaultAccountService" scope="prototype"/>
   ```

3. 其余的在web开发中使用到

# 7 Bean的自动装配

Spring会在上下文中自动寻找，并自动给bean装配属性！

在Spring中有三种装配的方式：

1. 在xml中显示的配置
2. 在java中显示的配置
3. 隐式的自动装配【重要】

## 7.1 测试

环境搭建：一个人有两个宠物

## 7.2 根据名字自动装配

自动装配 autowire="byName" 会自动查找跟对象set的相同的beanid

保证所有bean的id唯一，并且和自动注入的属性的set方法的值一样

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="cat" class="com.cw.jopo.Cat"/>
    <bean id="dog" class="com.cw.jopo.Dog"/>

    <bean id="people" class="com.cw.jopo.People" autowire="byName">
        <property name="name" value="cw"/>
<!--        <property name="cat" ref="cat"/>-->
<!--        <property name="dog" ref="dog"/>-->
    </bean>
</beans>
```

## 7.3 根据类型自动装配

自动装配 autowire="byType" 会自动查找跟对象set的相同的类型bean

id可以省略

保证所有bean的class唯一，并且和自动注入的属性的set方法的类型一样

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="cat" class="com.cw.jopo.Cat"/>
    <bean class="com.cw.jopo.Dog"/>

    <bean id="people" class="com.cw.jopo.People" autowire="byType">
        <property name="name" value="cw"/>
<!--        <property name="cat" ref="cat"/>-->
<!--        <property name="dog" ref="dog"/>-->
    </bean>
</beans>
```

## 7.4 使用注解实现自动装配

需要导入约束 context

配置注解的支持

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>
    <bean id="cat" class="com.cw.jopo.Cat"/>
    <bean id="dog" class="com.cw.jopo.Dog"/>
    <bean id="dog222" class="com.cw.jopo.Dog"/>
    <bean id="people" class="com.cw.jopo.People"/>

</beans>
```

```java
// 直接在属性上或者set方法上用就行
// 属性上加 @Autowired 可以省略set方法
public class People {
	@Autowired(required = false)
    private Cat cat;
    @Autowired
    @Qualifier(value = "dog222")
    private Dog dog;
}
```

科普：

``` 
@Nullable：表示特定参数，返回值或字段可以是null的 注解。
@Autowired(required = false): 说明这个对象可以为null
```

# 8 使用注解开发

自动装配的约束

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>
</beans>
```

1. bean

2. 属性如何注入

   ```java
   package com.cw.pojo;
   
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.stereotype.Component;
   
   //等价于在bean里 <bean id="user" class="com.cw.pojo.User"/>
   //@Component 组件
   @Component
   public class User {
       //相当于 value = "cw"
       @Value("cw")
       public String name;
   }
   ```

3. 衍生的注解 这几个注解，都是组件的意思，代表某个类注册到容器中

   @Component有几个衍生注解，在web开发中会按照mvc三层架构分层

   * dao 【@Repository】
   * service【@Service】
   * controller 【@Controller】

4. 自动装配置

5. 作用域

   ```java
   @Scope("prototype")
   ```

   

6. 小结

   xml和注解：

   * xml万能 适用于各种场合
   * 注解：维护比较难 不是自己的类使用不了

   最佳实践：

   * xml来完成管理bean

   * 注解：完成属性的注入

   * 注意必须让注解生效，需要开启注解支持

     ```xml
     <!-- 指定要扫描的包   这个包下的注解就会生效 -->
     <context:component-scan base-package="com.cw"/>
     <context:annotation-config/>
     ```

# 9 使用java的方式配置spring

完全不使用xml

JavaConfig是Spring的子项目，现在成为了核心功能

在SpringBoot中随处可见

```java
package com.cw.pojo.config;

import com.cw.pojo.User;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

// 也会被容器托管，配置类 类似于beans.xml
@Configuration
@ComponentScan("com.cw.pojo")
@Import(MyConfig2.class)
public class MyConfig {

    //相当于注册一个bean
    //id = getUser class = 返回值
    @Bean
    public User getUser(){
        return new User();
    }
}

```

```java
package com.cw.pojo.config;

import org.springframework.context.annotation.Configuration;

@Configuration
public class MyConfig2 {
}
```

```java
package com.cw.pojo;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

//会被容器托管
@Component
public class User {
    private String name;

    public String getName() {
        return name;
    }
    @Value("cw")
    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

```java
import com.cw.pojo.User;
import com.cw.pojo.config.MyConfig;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(MyConfig.class);
        User user = context.getBean("getUser", User.class);
        System.out.println(user.getName());
    }
}
```

# 10 代理模式

为什么学习代理模式？因为这是SpringAOP的底层【面试容易问】

分类：

* 静态代理
* 动态代理

## 10.1 静态代理

角色分析：

* 抽象角色：用接口或者抽象类来解决

  ```java
  package com.cw.demo01;
  
  //租房
  public interface Rent {
      public void rent();
  }
  ```

  

* 真实角色：被代理的角色

  ```java
  package com.cw.demo01;
  
  //房东
  public class Host implements Rent{
      @Override
      public void rent() {
          System.out.println("房东要出租房子");
      }
  }
  ```

  

* 代理角色：代理真实角色，一般做一些附属操作

  ```java
  package com.cw.demo01;
  
  public class Proxy implements Rent{
      private Host host;
  
      public Proxy() {
      }
  
      public Proxy(Host host) {
          this.host = host;
      }
  
      @Override
      public void rent() {
          fare();
          host.rent();
      }
  
      //看房
      public void fare(){
          System.out.println("收中介费");
          System.out.println("签合同");
      }
  }
  ```

  

* 客户：访问代理对象的人

  ```java
  package com.cw.demo01;
  
  public class Client {
      public static void main(String[] args) {
          Host host = new Host();
          //代理
          Proxy proxy = new Proxy(host);
          proxy.rent();
      }
  }
  ```



代理模式的好处：

* 使真实的角色操作更加存粹
* 公共的交给代理角色
* 公告业务发生扩展时候，方便集中管理

缺点：一个真实角色会产生一个代理角色，代码量翻倍，效率低

## 10.2 动态代理

* 角色和静态代理一样
* 动态代理的代理是动态生成的
* 动态代理分为两大类
  * 基于接口：JDK 动态代理
  * 基于类的： cglib
  * java字节码

需要了解两个类：Proxy 代理，InvocationHandler 调用处理程序

**InvocationHandler **

```java
package com.cw.demo02;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

//会用这个类自动生成代理类
public class ProxyInvocationHandler implements InvocationHandler {

    //被代理的接口
    private Rent rent;

    public void setRent(Rent rent) {
        this.rent = rent;
    }

    //生成得到代理类
    public Object getProxy() {
        return Proxy.newProxyInstance(
                this.getClass().getClassLoader(),
                rent.getClass().getInterfaces(),
                this);
    }


    //处理代理实例并返回结果
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        //动态代理的本质，就是使用反射机制实现
        Object result = method.invoke(rent, args);
        return result;
    }
}
```

```java
package com.cw.demo02;


//房东
public class Host implements Rent {

    @Override
    public void rent() {
        System.out.println("房东要出租房子");
    }
}
```

```java
package com.cw.demo02;

//租房
public interface Rent {
    public void rent();
}
```

```java
package com.cw.demo02;

public class Client {
    public static void main(String[] args) {
        //真实角色
        Host host = new Host();

        //代理角色 不存在
        ProxyInvocationHandler pih = new ProxyInvocationHandler();

        //通过调用程序处理角色来处理我们要调用的接口对象
        pih.setRent(host);//设置代理对象

        Rent proxy = (Rent)pih.getProxy();// 动态生成代理类
        proxy.rent();
    }
}
```

# 11 AOP

## 11.1 什么是AOP

面向切面编程（AOP是Aspect Oriented Program的首字母缩写） ，我们知道，面向对象的特点是继承、多态和封装。而封装就要求将功能分散到不同的对象中去，这在软件设计中往往称为职责分配。实际上也就是说，让不同的类设计不同的方法。这样代码就分散到一个个的类中去了。这样做的好处是降低了代码的复杂程度，使类可重用。
但是人们也发现，在分散代码的同时，也增加了代码的重复性。什么意思呢？比如说，我们在两个类中，可能都需要在每个方法中做日志。按面向对象的设计方法，我们就必须在两个类的方法中都加入日志的内容。也许他们是完全相同的，但就是因为面向对象的设计让类与类之间无法联系，而不能将这些重复的代码统一起来。
也许有人会说，那好办啊，我们可以将这段代码写在一个独立的类独立的方法里，然后再在这两个类中调用。但是，这样一来，这两个类跟我们上面提到的独立的类就有耦合了，它的改变会影响这两个类。那么，有没有什么办法，能让我们在需要的时候，随意地加入代码呢？**这种在运行时，动态地将代码切入到类的指定方法、指定位置上的编程思想就是面向切面的编程。**
一般而言，我们管切入到指定类指定方法的代码片段称为切面，而切入到哪些类、哪些方法则叫切入点。有了AOP，我们就可以把几个类共有的代码，抽取到一个切片中，等到需要时再切入对象中去，从而改变其原有的行为。
这样看来，AOP其实只是OOP的补充而已。OOP从横向上区分出一个个的类来，而AOP则从纵向上向对象中加入特定的代码。有了AOP，OOP变得立体了。如果加上时间维度，AOP使OOP由原来的二维变为三维了，由平面变成立体了。从技术上来说，AOP基本上是通过代理机制实现的。
AOP在编程历史上可以说是里程碑式的，对OOP编程是一种十分有益的补充。

![preview](img/v2-e777957e808c92fefcbcbec3945a2f91_r.jpg)

## 11.2 AOP在Spring中的作用

**提供声明式事务:允许用户自定义切面**

- 横切关注点:跨越应用程序多个模块的方法或功能.既是,与我们业务逻辑无关,但是我们需要关注的部分,就是横切关注点.如日志,安全,缓存,事务等…
- 切面（ASPECT）：横切关注点 被模块化 的特殊对象。即，它是一个类。
- 通知（Advice）：切面必须要完成的工作。即，它是类中的一个方法。
- 目标（Target）：被通知对象。
- 代理（Proxy）：向目标对象应用通知之后创建的对象。
- 切入点（PointCut）：切面通知 执行的 “地点”的定义。
- 连接点（JointPoint）：与切入点匹配的执行点。

![在这里插入图片描述](img/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDkyNzQzNg==,size_16,color_FFFFFF,t_70.png)

SpringAOP中，通过Advice定义横切逻辑，Spring中支持5种类型的Advice:

![20200219154304911](img/20200219154304911.png)

## 11.3 使用Spring实现AOP

需要增加一个依赖

```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependencies>
    <dependency>
        <groupId>org.aspectj</groupId>
        <artifactId>aspectjweaver</artifactId>
        <version>1.9.7</version>
    </dependency>
</dependencies>
```

方式一：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">
    <bean id="userService" class="com.cw.service.UserServiceImpl"/>
    <bean id="log" class="com.cw.log.Log"/>
    <bean id="afterlog" class="com.cw.log.AfterLog"/>

    <!--方式一 使用原生的Spring API接口    -->
    <!--配置aop 需要导入aop的约束-->
    <aop:config>
        <!--切入点: expression:表达式，
        execution(要执行的位置！ 修饰符 返回值 类名 方法名 参数)-->
        <aop:pointcut id="pointcut" expression="execution(* com.cw.service.UserServiceImpl.*(..))"/>
        <!--执行环绕增加-->
        <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>
        <aop:advisor advice-ref="afterlog" pointcut-ref="pointcut"/>
    </aop:config>
</beans>
```

```java
package com.cw.service;

public interface UserService {
    public void add();
    public void delete();
    public void update();
    public void query();
}
```

```java
package com.cw.service;

public class UserServiceImpl implements UserService{
    @Override
    public void add() {
        System.out.println("增加了用户");
    }

    @Override
    public void delete() {
        System.out.println("删除了用户");
    }

    @Override
    public void update() {
        System.out.println("更新了用户");
    }

    @Override
    public void query() {
        System.out.println("查找了用户");
    }
}
```

```java
package com.cw.log;

import org.springframework.aop.AfterReturningAdvice;

import java.lang.reflect.Method;

public class AfterLog implements AfterReturningAdvice {
    //Object o 返回值
    @Override
    public void afterReturning(Object o, Method method, Object[] objects, Object o1) throws Throwable {
        System.out.println("执行了"+method.getName()+"方法，返回结果为"+o);
    }
}
```

```java
package com.cw.log;

import org.springframework.aop.MethodBeforeAdvice;

import java.lang.reflect.Method;

public class Log implements MethodBeforeAdvice {

    //要执行的目标对象的方法
    //objects 参数
    //o 目标
    @Override
    public void before(Method method, Object[] objects, Object o) throws Throwable {
        System.out.println(o.getClass().getName()+"的"+method.getName()+"被执行了");
    }
}
```

方式二：使用自定义类实现AOP

```xml
<aop:config>
<!--        自定切面 ref引用的类-->
        <aop:aspect ref="diy">
            <!--切入点-->
            <aop:pointcut id="pointcut" expression="execution(* com.cw.service.UserServiceImpl.*(..))"/>
            <!--通知-->
            <aop:before method="before" pointcut-ref="pointcut"/>
            <aop:after method="after" pointcut-ref="pointcut"/>
        </aop:aspect>
    </aop:config>
```

```java
package com.cw.diy;

public class DiyPointCut {
    public void before(){
        System.out.println("方法执行前==========");
    }

    public void after(){
        System.out.println("方法执行后==========");
    }
}
```

方式三：使用注解实现AOP

```xml
<!--    方式三 开启注解支持-->
    <context:component-scan base-package="com.cw"/>
    <context:annotation-config/>
    <aop:aspectj-autoproxy/>
```

```java
package com.cw.diy;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.Signature;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.stereotype.Component;

@Component //注册进了bean
@Aspect //标注这个类是切面
public class AnnotationPointCut {

    @Before("execution(* com.cw.service.UserServiceImpl.*(..))")
    public void before(){
        System.out.println("方法执行前==========");
    }
    @After("execution(* com.cw.service.UserServiceImpl.*(..))")
    public void after(){
        System.out.println("方法执行后==========");
    }
    //在环绕增强中，可以给定一个参数，代表要切入点的位置
    @Around("execution(* com.cw.service.UserServiceImpl.*(..))")
    public void around(ProceedingJoinPoint joinPoint) throws Throwable {
        System.out.println("环绕前==========");
        //获得签名
        Signature signature = joinPoint.getSignature();
        System.out.println(signature);
        //执行方法
        Object proceed = joinPoint.proceed();
        System.out.println("环绕后==========");

    }
}
```

```java
import com.cw.service.UserService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MyTest {
    public static void main(String[] args) {

        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        //动态代理代理的是接口
        UserService userService = (UserService)context.getBean("userService");
        userService.add();
    }
}
```

