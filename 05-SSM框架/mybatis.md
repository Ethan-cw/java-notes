# Mybatis

## 1. 简介

### 1.1 简介和下载

* MyBatis 是一款优秀的持久层框架，它支持自定义 SQL、存储过程以及高级映射。

* MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。

* MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（实体类）（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。

下载：

* maven仓库：[Maven Repository: org.mybatis » mybatis (mvnrepository.com)](https://mvnrepository.com/artifact/org.mybatis/mybatis)

  ```xml
  <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
  <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.7</version>
  </dependency>
  ```

* github：[Releases · mybatis/mybatis-3 (github.com)](https://github.com/mybatis/mybatis-3/releases)

* 中文文档：[mybatis – MyBatis 3 | 简介](https://mybatis.org/mybatis-3/zh/index.html)

### 1.2 持久层

数据持久化

* 内存断电既失
* 数据库（jdbc），io文件持久化

### 1.3 持久层

Dao层，Service层，Controller层

持久层：完成持久化工作的代码块

### 1.4 为什么需要mybatis

方便，简化JDBC代码。框架。自动化。

不用mybatis也可以，但是学了更容易上手。

优点：

* sql和代码分离

* 提供映射标签，支持对象与数据库的orm字段关系映射。
* 提供对象关系映射标签，支持对象关系组建维护。
* 提供xml标签，支持编写动态sql。

## 2. 第一个Mybatis程序

思路：搭建环境-->导入Mybatis-->编写代码-->测试

### 2.1 搭建数据库

```java
CREATE DATABASE `mybatis`;
USE `Mybatis`;

CREATE TABLE `user`(
	`id` INT(20) NOT NULL PRIMARY KEY,
	`name` VARCHAR(30) DEFAULT NULL,
	`pwd` VARCHAR(30) DEFAULT NULL
)ENGINE=INNODB DEFAULT CHARSET=utf8;


INSERT INTO `user`(`id`, `name`, `pwd`) VALUES
(1,'chenwen','123456'),
(2,'chenwen1','123456'),
(3,'chenwen2','123456')
```

新建项目：

1. 新建普通maven项目

2. 删除src目录

3. 导入maven依赖

   ```xml
   <!--导入依赖-->
       <properties>
           <maven.compiler.source>8</maven.compiler.source>
           <maven.compiler.target>8</maven.compiler.target>
       </properties>
   
       <dependencies>
           <!--mysql驱动-->
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
               <version>5.1.47</version>
           </dependency>
           <!--mybatis-->
           <dependency>
               <groupId>org.mybatis</groupId>
               <artifactId>mybatis</artifactId>
               <version>3.5.7</version>
           </dependency>
           <!--junit-->
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>4.12</version>
           </dependency>
       </dependencies>
   ```

### 2.2 创建一个模块

1. 编写mybatis核心配置文件

   ```xml
   <!--核心配置文件-->
   <configuration>
       <environments default="development">
           <environment id="development">
               <transactionManager type="JDBC"/>
               <dataSource type="POOLED">
                   <property name="driver" value="com.mysql.jdbc.Driver"/>
                   <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
                   <property name="username" value="root"/>
                   <property name="password" value="123456"/>
               </dataSource>
           </environment>
       </environments>
   </configuration>
   ```

2. 编写mybatis的工具类

   ```java
   public class MybatisUtils {
       private static SqlSessionFactory sqlSessionFactory;
       static {
           try {
               // 使用mybatis第一步获取sqlSessionFactory对象
               String resource = "mybatis-config.xml";
               InputStream inputStream = Resources.getResourceAsStream(resource);
               sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
           } catch (IOException e) {
               e.printStackTrace();
           }
       }
   
       //既然有了 SqlSessionFactory，顾名思义，我们可以从中获得 SqlSession 的实例。
       //SqlSession 提供了在数据库执行 SQL 命令所需的所有方法。
       public static SqlSession getSqlSession(){
           return sqlSessionFactory.openSession();
       }
   }
   ```

### 2.3  编写代码

1. 实体类

```java
public class User {
    private int id;
    private String name;
    private String pwd;

    public User() {
    }
    public User(int id, String name, String pwd) {
        this.id = id;
        this.name = name;
        this.pwd = pwd;
    }

    public int getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public String getPwd() {
        return pwd;
    }

    public void setId(int id) {
        this.id = id;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setPwd(String pwd) {
        this.pwd = pwd;
    }
}
```

2. Dao接口

```java
public interface UserDao {
    List<User> getUserList();
}
```

3. 接口实现类由原来的UserDaoImp转换为UserMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--namespace绑定一个对应的Dao/Mapper接口-->
<mapper namespace="com.chen.dao.UserDao">
    <!--select id="getUserList" 对应Dao里方法的名字-->
    <select id="getUserList" resultType="com.chen.pojo.User">
        select * from mybatis.user
    </select>
</mapper>
```

4. 测试

org.apache.ibatis.binding.BindingException: Type interface com.chen.dao.UserDao is not known to the MapperRegistry.

>  每一个mapper.xml都需要再核心配置文件中注册



